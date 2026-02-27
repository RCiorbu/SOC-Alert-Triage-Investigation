# SOC Alert Triage & Investigation (Splunk-style)

## TL;DR (read this first — hiring managers will)
Simulated Tier-1 SOC investigation: detected a brute-force authentication campaign that escalated to a confirmed compromised account. The repo includes sample auth logs, Splunk search examples (SPL), a detection rule, an analyst triage ticket, and a final incident closeout that shows containment and recommended next steps.

**Skills demonstrated:** alert triage, SPL search, log analysis, timeline building, containment recommendations, analyst write-ups.

---

## Files in this repo
- `logs/auth.log` — realistic sample authentication logs (SFTP/SSH/web auth style)
- `splunk/splunk_searches.md` — SPL queries and saved search configs
- `splunk/detection_rule.conf` — example detection rule for SIEM
- `investigation/ANALYST_TICKET.md` — triage ticket filled with evidence & actions
- `investigation/CLOSEOUT.md` — final incident write-up (timeline, root cause, remediation)
- `assets/run_in_splunk.txt` — quick instructions to ingest logs into Splunk

---

## How to use
1. Spin up Splunk Free (local) or use an existing Splunk instance.
2. Ingest `logs/auth.log` (see `assets/run_in_splunk.txt` for quick method).
3. Run the provided SPL queries in `splunk/splunk_searches.md`.
4. Use `investigation/ANALYST_TICKET.md` as your step-by-step triage and fill in evidence.
5. Publish the `CLOSEOUT.md` as the final case report.

---

## Hiring note
When you review my work, look for the "Investigation → Evidence → Action" flow:
- Detection (alert) → Investigation (logs & searches) → Determination (true/false positive) → Containment → Closeout.

If you'd like, I can adapt these searches to Splunk ES correlation searches or to other SIEMs (QRadar/Elastic/Datadog).
