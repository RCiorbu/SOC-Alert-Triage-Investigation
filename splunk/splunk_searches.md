# Splunk Searches & Detection Examples

## 1) High-level failed login alert (candidate detection)
SPL:
index=main sourcetype=syslog (sshd OR webapp OR sudo) | rex "user=(?<user>\w+)" | eval outcome=if(match(_raw,"Failed password|login failure"),"failed",if(match(_raw,"Accepted password|login success"),"success","other")) | stats count by user,outcome,src_ip | where outcome="failed" AND count>=3

Purpose: detect users or src_ip with 3+ failed attempts in the indexed window (tunable).

---

## 2) Brute-force activity from single IP
SPL: 
index=main sourcetype=syslog (sshd OR webapp) | rex "from (?<src_ip>\d+.\d+.\d+.\d+)" | stats count(eval(match(_raw,"Failed password|login failure"))) AS failed, count(eval(match(_raw,"Accepted password|login success"))) AS success BY src_ip | where failed>=5 AND success<1

Purpose: find IPs attempting many failed logins but no successes — candidate brute-forcers.

---

## 3) Account compromise candidate (failed attempts followed by success)
SPL:
index=main sourcetype=syslog (sshd OR webapp) | rex "user=(?<user>\w+)|for (?<user2>\w+) from (?<src_ip>\d+.\d+.\d+.\d+)" | eval user=coalesce(user,user2) | sort 0 _time | transaction user maxspan=10m startswith="failed" endswith="success" | search eventcount>1

Purpose: find a sequence where a user had failed attempts and then a success within a short window.

---

## 4) Unusual process / post-auth activity
SPL:
index=main sourcetype=syslog "suspicious process" OR "pspy" OR "http POST to https://" | table _time host user _raw

Purpose: surface suspicious process launches or outbound posts after auth.

---

## 5) Example saved-search (alert) configuration (pseudo)
Name: `BruteForce_Detection`
Schedule: run every 5 minutes
Search: [use query #2]
Trigger Condition: if results > 0
Severity: High
Actions: create notable event / send email to SOC queue
