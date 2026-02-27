# Analyst Triage Ticket (Sample)

**Ticket ID:** SOC-2025-0001  
**Date/Time:** 2025-08-02 09:20 UTC  
**Detected by:** Saved search `BruteForce_Detection`  
**Initial severity (suggested):** High

## Alert summary
Multiple failed authentication attempts observed from single IP `198.51.100.23` against SSH and webapp endpoints. Later, a successful login for user `jdoe` from same IP observed.

## Evidence (copy/paste relevant raw events)
- 2025-08-02T09:03:15Z Failed password for invalid user admin from 198.51.100.23
- 2025-08-02T09:04:05Z Failed password for invalid user admin from 198.51.100.23
- 2025-08-02T09:19:30Z Accepted password for jdoe from 198.51.100.23
- 2025-08-02T09:30:12Z user jdoe started suspicious process /usr/bin/pspy
- 2025-08-02T09:35:20Z http POST to https://bad.example.com/api/upload by user jdoe

## Triage steps performed
1. Verified repeated failed attempts from `198.51.100.23` (SPL#2).
2. Correlated events: failed → success for `jdoe` (SPL#3).
3. Searched for suspicious post-auth activity: found process `pspy` and outbound POST to `bad.example.com`.
4. Checked geo/IP reputation (document result here — e.g., "198.51.100.23 flagged on threat intel").
5. Checked last login times and asset ownership for `jdoe` (asset owner: marketing team).

## Preliminary determination
Likely **compromised account** for `jdoe` following a credential brute-force or credential stuffing attack. Evidence of post-auth suspicious activity indicates possible data exfiltration.

## Immediate recommended containment
- Reset `jdoe` password and force reauthentication (MFA if available).
- Disable remote access for `jdoe` until verified.
- Isolate host where suspicious `pspy` process ran (take host offline or quarantine).
- Block IP `198.51.100.23` at firewall/IDS.
- Collect full host forensic image and process memory (if SOC policy allows).
- Notify Incident Response team and escalate to IR lead.

## Next actions assigned
- SOC Tier-2 / IR: Forensic collection (Assigned: [name])  
- IT: Block IP at perimeter (Assigned: [name])  
- App owner: Revoke API tokens for suspicious POST endpoint (Assigned: [name])

## Notes
Document timeline and add references to logs and screenshots. Prepare initial draft of notification if data exfiltration suspected.
