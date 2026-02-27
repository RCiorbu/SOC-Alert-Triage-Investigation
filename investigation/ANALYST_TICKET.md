# SOC Analyst Triage Ticket

Ticket ID: SOC-2025-0001  
Detection Name: SSH_BruteForce_Attempt  
Severity: Medium  
Analyst: Rostislav Ciorbu  

---

## Alert Summary
Multiple failed SSH login attempts were detected from a single external IP address targeting several usernames.

---

### Evidence
![Raw failed logins](SOC-Alert-Triage-Investigation/ActivityScreenshots/rawfailedlogins.png)


---

## Triage Actions
- Reviewed authentication logs in Splunk
- Extracted source IP and targeted usernames
- Counted frequency of login attempts
- Confirmed repeated automated authentication behavior

---

## Analysis
The pattern of authentication attempts indicates automated password guessing rather than legitimate user activity.

No successful login events were observed in the dataset.

---

## Conclusion
True Positive — External brute-force authentication attempt

---

## Recommended Response
- Block IP 198.51.100.23 at firewall
- Continue monitoring for repeated attempts
- Implement account lockout threshold
- Recommend MFA for remote authentication
