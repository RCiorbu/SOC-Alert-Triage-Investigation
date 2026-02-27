# Incident Closeout — Case SOC-2025-0001

## Executive summary
On 2025-08-02, the SOC detected a brute-force style authentication campaign originating from IP `198.51.100.23`. Following multiple failed attempts, the account `jdoe` was observed successful authenticating from that IP, followed by suspicious activity (process `pspy` and outbound POST to `bad.example.com`). The account was contained, credentials rotated, and the host was quarantined. No confirmed large-scale exfiltration was observed in logs beyond the POST; forensic analysis recommended.

## Timeline (key events)
- 09:03–09:06 UTC — repeated failed SSH login attempts from 198.51.100.23 targeting `admin` and other accounts.
- 09:19:30 UTC — Accepted password for `jdoe` from 198.51.100.23.
- 09:30:12 UTC — Host executed suspicious process `/usr/bin/pspy`.
- 09:35:20 UTC — Outbound HTTP POST to https://bad.example.com/api/upload.
- 09:40:00 UTC — Additional failed attempts from same attacker IP observed.

## Root cause
Evidence suggests attacker used credential stuffing or password spraying to obtain valid credentials for `jdoe`. Post-auth activity indicates the attacker executed reconnaissance tools and attempted outbound data transfer.

## Actions taken
- Immediate: Reset `jdoe` password, revoked sessions, disabled account for remote access.
- Host mitigation: Isolated the host and collected forensic image (pending).
- Network: Blocked attacker IP and updated firewall rules.
- Detection improvements: Implemented detection rule `BruteForce_Detection`, increased threshold and added correlation for failed->success sequences.
- User awareness: Notified affected user and required organization-wide phishing and credential hygiene reminders.

## Lessons learned / Recommendations
- Enforce MFA for all remote access — would have prevented credential-only compromise.
- Enforce account lockout or adaptive lockout after threshold failed attempts.
- Add EDR monitoring for abnormal process execution and outbound connections.
- Implement SIEM enrichment with threat-intel for faster triage.

## Attachments / Evidence
- Parser/queries used (see `splunk/splunk_searches.md`)
- Raw logs excerpt (see `logs/auth.log`)
- Analyst ticket (see `investigation/ANALYST_TICKET.md`)

Prepared by: Rostislav Ciorbu  
Role: SOC Analyst (sample project)
