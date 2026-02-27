Analyst Triage Ticket
Ticket ID: SOC-2025-0001
Detection: SSH_BruteForce_Attempt
Severity: Medium

Alert Summary
Multiple failed SSH login attempts detected from external IP 198.51.100.23 targeting multiple usernames.

Evidence
Failed password for invalid user admin from 198.51.100.23
Failed password for invalid user test from 198.51.100.23
Failed password for invalid user root from 198.51.100.23

Triage Actions
• Reviewed authentication logs in Splunk
• Extracted source IP and usernames
• Counted frequency of login attempts
• Confirmed repeated automated authentication attempts

Analysis
The activity pattern indicates automated password guessing rather than normal user behavior.
No successful login events observed in the dataset.

Conclusion
True Positive — External brute-force authentication attempt.

Recommended Response
• Block IP 198.51.100.23 at firewall
• Monitor for additional attempts
• Consider account lockout threshold
• Recommend MFA for remote authentication
