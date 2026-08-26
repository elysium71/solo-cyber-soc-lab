# Incident Report — A3-Day 1

## Incident Summary

| Field | Value |
| --- | --- |
| Incident ID | `A3-DAY1` |
| Environment | Mini SOC Lab |
| Classification | **TRUE POSITIVE — CONTROLLED LAB SIMULATION** |
| Endpoint | `soc-windows-01` |
| Agent ID | `002` |
| User | `SOC-WINDOWS-01\soc-windows-01` |
| Primary detection | Wazuh Rule `100600` |
| Severity | Level `10` |
| Primary ATT&CK technique | `T1059.001` — PowerShell |
| Incident end marker | `2026-08-27 02:22:54` local Windows time |

## Executive Summary

A controlled Windows incident was generated on `soc-windows-01` to practice SOC alert investigation and timeline reconstruction. The activity began with PowerShell executed using `-ExecutionPolicy Bypass`. Sysmon captured the process creation and Wazuh generated custom Rule `100600` at level 10.

Follow-on account and privileged-group discovery was observed through `net.exe` and `net1.exe`. Wazuh Rules `92033` and `92031` detected discovery activity. Parent/child process information was used to correlate these events into a single investigation sequence rather than treating them as unrelated alerts.

The incident was classified as a true positive because the detected behavior matched the deliberately generated lab activity. No real compromise occurred.

## Detection Details

### Primary Detection

```text
Rule: 100600
Level: 10
Description: Custom Windows detection: PowerShell executed with ExecutionPolicy Bypass.
MITRE ATT&CK: T1059.001 — PowerShell
```

The triggering command contained:

```text
powershell.exe -ExecutionPolicy Bypass -NoProfile -Command ...
```

### Follow-On Detections

```text
Rule 92033 (level 3) — Discovery activity spawned via powershell execution
Rule 92031 (level 3) — Discovery activity executed
```

Observed discovery commands:

```text
net.exe user
net1.exe user
net.exe localgroup administrators
net1.exe localgroup administrators
```

## Process Chain

```text
powershell.exe
├── net.exe user
│   └── net1.exe user
└── net.exe localgroup administrators
    └── net1.exe localgroup administrators
```

The parent-process evidence linked the discovery activity to the PowerShell session.

## Timeline

| Approx. UTC time | Event | Analyst interpretation |
| --- | --- | --- |
| 18:07 | Rule `100600`, Level 10 | Suspicious PowerShell ExecutionPolicy Bypass detected |
| 18:09 | `net.exe user` / Rule `92033` | Account discovery from PowerShell |
| 18:09 | `net1.exe user` / Rule `92031` | Child discovery process |
| 18:09 | `net.exe localgroup administrators` / Rule `92033` | Privileged-group discovery |
| 18:09 | `net1.exe localgroup administrators` / Rule `92031` | Child discovery process |
| Local 02:22:54 | Incident end marker | Controlled simulation closed |

> Windows local timestamps and Wazuh/Sysmon UTC timestamps were treated as different representations of the same investigation period.

## Scope and Impact

The observed activity was limited to the isolated lab endpoint `soc-windows-01`. The affected account shown in the correlated events was `SOC-WINDOWS-01\soc-windows-01`.

There was no production impact, data loss, or unauthorized compromise because all activity was deliberately generated for defensive validation.

## Detection Gaps

Additional controlled system/network discovery activity and creation of `incident-note.txt` did not produce the specific alert evidence expected in `alerts.log`.

This was recorded as a visibility/detection gap. An activity may:

1. occur on the endpoint;
2. generate telemetry;
3. reach the SIEM;
4. still fail to generate an alert if no matching detection rule fires.

These gaps should be revisited during later detection-tuning and detection-as-code modules.

## MITRE ATT&CK Mapping

| Activity | Mapping |
| --- | --- |
| PowerShell ExecutionPolicy Bypass | `T1059.001` — PowerShell |
| Account/group discovery | Discovery behavior observed through Windows `net.exe`/`net1.exe`; additional ATT&CK mappings should be formally assigned when the associated detection rules are reviewed. |

## Analyst Verdict

**TRUE POSITIVE — CONTROLLED LAB SIMULATION**

The PowerShell alert and follow-on discovery detections accurately represented activity intentionally generated during the exercise. Process ancestry and command-line telemetry provided sufficient evidence to correlate the events.

## Recommendations

- Preserve Rule `100600` as a documented custom detection.
- Review detection coverage for the controlled activities that produced no alert.
- Continue using parent/child process correlation during Windows investigations.
- Preserve both local and UTC timestamps in future incident reports.
- Use A3-Day 2 to enrich this case with structured alert, host, user, process, and ATT&CK context.

## Evidence

```text
a3-day1-01-lab-health.png
a3-day1-02-powershell-bypass-detection.png
a3-day1-03-privileged-group-discovery.png
a3-day1-04-investigation-evidence.png
a3-day1-05-incident-end-marker.png
a3-day1-06-incident-timeline.png
a3-day1-07-detection-rule-100600.png
```

## Conclusion

The exercise demonstrated an end-to-end SOC workflow: controlled activity generation, centralized detection, alert validation, process correlation, timeline reconstruction, detection-gap identification, and analyst classification.
