# Splunk Searches & Detection Examples (copyable)

## 1) High-level failed login alert (candidate detection)
SPL:

index=main sourcetype=syslog (sshd OR webapp OR sudo)
| rex "for (invalid user )?(?<user>\w+)(?: from (?<src_ip>\d+.\d+.\d+.\d+))?"
| eval outcome=if(match(_raw,"Failed password|login failure"),"failed",if(match(_raw,"Accepted password|login success"),"success","other"))
| stats count(eval(outcome="failed")) AS failed, count(eval(outcome="success")) AS success BY user, src_ip
| where failed >= 3

Purpose: detect users or src_ip with 3+ failed attempts (tunable). Good early-warning candidate.

---

## 2) Brute-force activity from single IP
SPL:

index=main sourcetype=syslog (sshd OR webapp)
| rex "from (?<src_ip>\d+.\d+.\d+.\d+)"
| stats count(eval(match(_raw,"Failed password|login failure"))) AS failed, count(eval(match(_raw,"Accepted password|login success"))) AS success BY src_ip
| where failed >= 5 AND success < 1

Purpose: find IPs attempting many failed logins but no success — candidate brute-forcers.

---

## 3) Account compromise candidate (failed attempts followed by success)
SPL:

index=main sourcetype=syslog (sshd OR webapp)
| rex "for (invalid user )?(?<user>\w+)(?: from (?<src_ip>\d+.\d+.\d+.\d+))?"
| sort 0 _time
| transaction user maxspan=10m startswith="Failed password" endswith="Accepted password"
| search eventcount>1

Purpose: find a short sequence where a user had failed attempts then a success — higher likelihood of compromise.

---

## 4) Unusual process / post-auth activity
SPL:

index=main sourcetype=syslog ("suspicious process" OR "pspy" OR "http POST to https://")
| table _time host user _raw

Purpose: surface suspicious process launches or outbound posts after authentication.

---

## 5) Notes about saved-search / alert config
- Name: BruteForce_Detection  
- Schedule: run every 5 minutes (cron `*/5 * * * *`)  
- Condition: results > 0 (or use `where failed >= 5`)  
- Severity: High  
- Actions: create notable event, add to SOC queue, email to on-call, optionally auto-block IP with a ticket to IT (use caution).
- Tuning: add whitelists (management IPs, known scanners), add IP reputation lookup to suppress known benign s
