# Incident Closeout Report

Case ID: SOC-2025-0001  
Prepared by: Rostislav Ciorbu  

---

## Summary
Repeated failed SSH authentication attempts were detected originating from IP 198.51.100.23.

No successful authentication occurred.  
The activity was determined to be an automated brute-force login attempt.

---

## Impact
- No accounts compromised
- No unauthorized access observed
- No data exposure

---

## Actions Taken
- Detection rule created for repeated login failures
- Source IP flagged for blocking
- Monitoring recommended for similar behavior

---

## Lessons Learned
Brute-force login attempts are common internet scanning activity but should still generate monitoring alerts for security awareness.

---

## Improvement Opportunities
- Implement MFA for remote login
- Configure lockout threshold after repeated failures
- Add IP reputation enrichment to SIEM alerts
