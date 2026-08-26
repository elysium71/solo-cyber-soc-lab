# Advanced SOC Roadmap

The advanced phase expands the completed core Mini SOC Lab into a multi-endpoint detection-engineering environment.

Each module is divided into focused days so implementation, validation, evidence, and documentation are completed before moving forward.

## Advanced 1 — Multi-Endpoint Monitoring

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ✅ | **A1-Day 1** | Build + network second Linux endpoint | Built `soc-linux-02`, configured NAT and `SOC-LAB`, assigned `192.168.56.30/24`, verified bidirectional ICMP connectivity, and confirmed SSH TCP/22 access. |
| ✅ | **A1-Day 2** | Wazuh agent deployment | Installed Wazuh Agent `4.13.1`, enrolled `soc-linux-02` as Agent ID `001`, confirmed Active status, and validated Rules `5557` and `5503`. |
| ✅ | **A1-Day 3** | Cross-endpoint detection validation | Generated controlled events on both Linux endpoints, compared host attribution in Wazuh, validated multi-endpoint visibility, and documented the completed module. |

## Advanced 2 — Windows Detection Engineering

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ✅ | **A2-Day 1** | Build + network Windows endpoint | Created a Windows VM, configured NAT and `SOC-LAB`, assigned the lab IP, and validated connectivity. |
| ✅ | **A2-Day 2** | Wazuh Windows agent | Installed and enrolled the Wazuh Windows agent and validated Windows event collection. |
| ✅ | **A2-Day 3** | Sysmon deployment | Installed Sysmon and validated process, network, and security telemetry. |
| ✅ | **A2-Day 4** | Windows detection validation | Generated controlled PowerShell, process, and account activity and created/tuned detections. |
| ✅ | **A2-Day 5** | Custom detection engineering | Created custom Wazuh rules, validated detections, and documented MITRE ATT&CK mappings. |

## Advanced 3 — Incident Investigation + Timeline Reconstruction

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ✅ | **A3-Day 1** | Windows incident investigation | Generated a controlled Windows incident, validated Rule `100600` and discovery detections, correlated the PowerShell → `net.exe` → `net1.exe` process chain, reconstructed the timeline, documented detection gaps, and produced an analyst verdict. |
| ⬜ | **A3-Day 2** | Incident triage + evidence enrichment | Enrich alerts with endpoint, user, process, telemetry, and ATT&CK context and build a structured incident case. |
| ⬜ | **A3-Day 3** | Investigation report + lessons learned | Finalize the incident report, document detection/visibility gaps, recommendations, and reusable investigation procedures. |

## Advanced 4 — Wazuh Active Response

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A4-Day 1** | Active Response design | Select a high-confidence detection and configure a carefully scoped lab-only response. |
| ⬜ | **A4-Day 2** | Response validation | Trigger the detection and verify response, logging, timeout, and recovery. |
| ⬜ | **A4-Day 3** | Safety + documentation | Test edge cases and document false-positive and operational risks. |

## Advanced 5 — Detection Tuning

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A5-Day 1** | Baseline testing | Generate normal administrative activity and identify noisy detections. |
| ⬜ | **A5-Day 2** | Rule tuning | Refine fields, thresholds, exclusions, severity, and conditions. |
| ⬜ | **A5-Day 3** | Regression validation | Re-run benign and suspicious tests and document before/after results. |

## Advanced 6 — Threat Hunting

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A6-Day 1** | Authentication hunting | Build repeatable hunts for failed logins, invalid users, unusual sources, and privileged authentication. |
| ⬜ | **A6-Day 2** | Host + persistence hunting | Hunt Auditd/FIM/process data for privilege escalation, credential access, persistence, and suspicious execution. |
| ⬜ | **A6-Day 3** | Cross-host hunting | Combine Suricata and endpoint data and document reusable investigation queries. |

## Advanced 7 — MITRE ATT&CK Coverage Matrix

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A7-Day 1** | Coverage inventory | Map detections to telemetry source, tactic, technique, severity, and evidence. |
| ⬜ | **A7-Day 2** | Gap analysis | Build the ATT&CK coverage matrix and prioritize missing detection coverage. |

## Advanced 8 — Attack-Chain Correlation

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A8-Day 1** | Scenario design | Design a controlled reconnaissance → authentication → privilege → persistence sequence. |
| ⬜ | **A8-Day 2** | Execute + collect | Run the simulation and collect network and host telemetry. |
| ⬜ | **A8-Day 3** | Correlation investigation | Correlate events by source IP, endpoint, user, time, and ATT&CK stage. |
| ⬜ | **A8-Day 4** | Detection improvement | Identify gaps, improve detections, retest, and document results. |

## Advanced 9 — Detection-as-Code

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A9-Day 1** | Repository standard | Standardize rule naming, metadata, ATT&CK mappings, tests, and expected results. |
| ⬜ | **A9-Day 2** | Versioned workflow | Add Git-based validation and change tracking for detections. |
| ⬜ | **A9-Day 3** | Detection documentation | Document rule purpose, source, severity, testing, output, and limitations. |

## Advanced 10 — Automated Detection Validation

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A10-Day 1** | Test framework | Define safe reproducible validation tests. |
| ⬜ | **A10-Day 2** | Linux validation | Automate controlled Linux detection tests. |
| ⬜ | **A10-Day 3** | Network + Windows validation | Extend validation to Suricata and Windows detections. |
| ⬜ | **A10-Day 4** | Regression suite | Run all validation tests following detection changes. |

## Advanced 11 — SOC Metrics and Dashboards

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A11-Day 1** | Metrics design | Define severity, endpoint, ATT&CK, authentication, and trend metrics. |
| ⬜ | **A11-Day 2** | Dashboard implementation | Build Wazuh operational and investigation dashboards. |
| ⬜ | **A11-Day 3** | Dashboard documentation | Capture evidence and explain analyst use cases. |

## Advanced 12 — Incident Response Workflow

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A12-Day 1** | Triage workflow | Create reusable triage, severity, evidence, and escalation templates. |
| ⬜ | **A12-Day 2** | Investigation + containment | Define investigation, containment, recovery, and evidence procedures. |
| ⬜ | **A12-Day 3** | Tabletop validation | Apply and refine the workflow against a simulated incident. |

## Advanced 13 — Purple-Team Capstone

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ⬜ | **A13-Day 1** | Planning | Define authorized scope, ATT&CK stages, expected telemetry, and success criteria. |
| ⬜ | **A13-Day 2** | Initial simulation | Execute controlled attack stages and preserve evidence. |
| ⬜ | **A13-Day 3** | Investigation | Investigate endpoint, network, authentication, Auditd, FIM, Sysmon, and Wazuh telemetry. |
| ⬜ | **A13-Day 4** | Gap remediation | Identify and improve detection gaps. |
| ⬜ | **A13-Day 5** | Retest | Repeat relevant stages and demonstrate improved coverage. |
| ⬜ | **A13-Day 6** | Final portfolio release | Publish the final report, ATT&CK timeline, detection summary, screenshots, and lessons learned. |

## Progress Summary

```text
Advanced 1      3 / 3 complete
Advanced 2      5 / 5 complete
Advanced 3      1 / 3 complete
Advanced 4      0 / 3
Advanced 5      0 / 3
Advanced 6      0 / 3
Advanced 7      0 / 2
Advanced 8      0 / 4
Advanced 9      0 / 3
Advanced 10     0 / 4
Advanced 11     0 / 3
Advanced 12     0 / 3
Advanced 13     0 / 6
```

## Current Position

**Current:** A3-Day 1 complete.

**Next:** A3-Day 2 — Incident triage + evidence enrichment.
