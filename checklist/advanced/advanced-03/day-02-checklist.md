# Advanced 3 — Day 2 Checklist: Incident Triage + Evidence Enrichment

## Lab Health
- [x] Confirm Wazuh Manager is active
- [x] Confirm all three Wazuh agents are active

## Alert Recovery
- [x] Check current Wazuh alert files
- [x] Identify archived alert storage
- [x] Recover original Rule `100600` alert from `ossec-alerts-26.json.gz`

## Primary Alert Triage
- [x] Confirm Rule `100600`, Level `10`
- [x] Identify endpoint, Agent ID, and IP
- [x] Confirm Sysmon Event ID `1`
- [x] Extract process, command line, user, and integrity level
- [x] Extract parent-process information
- [x] Confirm `T1059.001` metadata

## Related Alert Correlation
- [x] Retrieve Rules `92033` and `92031`
- [x] Identify `net user`
- [x] Identify `net localgroup administrators`
- [x] Correlate by endpoint, user, and time

## Process Enrichment
- [x] Extract process IDs and parent process IDs
- [x] Extract process and parent GUIDs
- [x] Confirm High integrity context
- [x] Reconstruct `powershell.exe → net.exe → net1.exe` relationships
- [x] Distinguish Rule `100600` PID from the parent PowerShell session

## MITRE ATT&CK Enrichment
- [x] Validate Rule `100600` metadata
- [x] Validate Rules `92033` and `92031` metadata
- [x] Confirm `T1059.001` — PowerShell
- [x] Confirm `T1087` — Account Discovery
- [x] Avoid assigning unsupported additional techniques

## Incident Assessment
- [x] Create `A3-DAY2-CASE-001`
- [x] Document primary and related alerts
- [x] Document endpoint, user, and process correlation
- [x] Classify as True Positive — Controlled Lab Simulation
- [x] Close as Controlled Test

## Evidence
- [x] `01-primary-alert-triage.png`
- [x] `02-related-alert-correlation.png`
- [x] `03-process-user-enrichment.png`
- [x] `04-mitre-attack-enrichment.png`
- [x] `05-incident-case-summary.png`
- [x] Remove duplicate incident-case screenshot before commit

## Documentation
- [x] Create Day 2 investigation write-up
- [x] Create Day 2 checklist
- [ ] Update Advanced roadmap
- [ ] Review Git changes
- [ ] Commit Day 2 evidence and documentation
- [ ] Push to GitHub

## Result
- [x] **PASS — Advanced 3 Day 2 technical investigation COMPLETE**

## Advanced 3 Status
- [x] Day 1 — Windows incident investigation + timeline reconstruction
- [x] Day 2 — Incident triage + evidence enrichment
- [ ] Day 3 — Investigation report + lessons learned

## Next
- [ ] **Advanced 3 — Day 3: Investigation Report + Lessons Learned**
