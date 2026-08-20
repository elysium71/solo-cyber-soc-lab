# Advanced SOC Roadmap

The advanced phase expands the completed core Mini SOC Lab into a multi-endpoint detection-engineering environment.

Each module is divided into multiple days so implementation, validation, evidence collection, tuning, and documentation can be completed properly.

## Advanced 1 — Multi-Endpoint Monitoring

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ✅ | **A1-Day 1** | Build + network second Linux endpoint | Built `soc-linux-02`, configured NAT and `SOC-LAB`, assigned `192.168.56.30/24`, validated bidirectional ICMP connectivity with `soc-ubuntu`, and confirmed SSH TCP/22 connectivity. |
| ⬜ | **A1-Day 2** | Wazuh agent deployment | Install and enroll the Wazuh agent on `soc-linux-02`, verify agent status, and confirm endpoint telemetry reaches Wazuh. |
| ⬜ | **A1-Day 3** | Cross-endpoint detection validation | Generate safe activity on the additional endpoint, compare telemetry between hosts, validate multi-endpoint visibility, capture evidence, and document the completed module. |

## Advanced 2 — Windows Detection Engineering

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A2-Day 1** | Build + network Windows endpoint | Create a Windows VM, configure NAT and `SOC-LAB`, assign a lab IP, and validate connectivity. |
| ⬜ | **A2-Day 2** | Wazuh Windows agent | Install and enroll the Windows agent and validate Windows event collection. |
| ⬜ | **A2-Day 3** | Sysmon deployment | Deploy Sysmon and validate process, network, and security-relevant telemetry. |
| ⬜ | **A2-Day 4** | Windows detection validation | Generate controlled PowerShell, process, and account activity and create or tune detections. |

## Advanced 3 — Wazuh Active Response

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A3-Day 1** | Active Response design | Select a high-confidence detection and configure a carefully scoped lab-only response. |
| ⬜ | **A3-Day 2** | Response validation | Trigger the detection and verify the automated response, logs, timeout, and recovery. |
| ⬜ | **A3-Day 3** | Safety + documentation | Test edge cases and document limitations and false-positive risks. |

## Advanced 4 — Detection Tuning

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A4-Day 1** | Baseline + false-positive testing | Generate normal administrative activity and identify noisy or overly broad detections. |
| ⬜ | **A4-Day 2** | Rule tuning | Refine thresholds, fields, exclusions, severity, and conditions while retaining useful coverage. |
| ⬜ | **A4-Day 3** | Regression validation | Re-run benign and suspicious tests and document before/after behaviour. |

## Advanced 5 — Threat Hunting

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A5-Day 1** | Authentication hunting | Develop repeatable hunts for failed logins, invalid users, unusual sources, and privileged authentication. |
| ⬜ | **A5-Day 2** | Host + persistence hunting | Hunt Auditd, FIM, and process telemetry for privilege escalation, credential access, persistence, and suspicious execution. |
| ⬜ | **A5-Day 3** | Network + cross-host hunting | Hunt Suricata and endpoint telemetry together and document reusable queries. |

## Advanced 6 — MITRE ATT&CK Coverage Matrix

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A6-Day 1** | Coverage inventory | Map existing detections to telemetry source, tactic, technique, severity, and validation evidence. |
| ⬜ | **A6-Day 2** | Gap analysis | Build the final ATT&CK coverage matrix, identify meaningful gaps, and prioritize future detection work. |

## Advanced 7 — Attack-Chain Correlation

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A7-Day 1** | Scenario design | Design a controlled multi-stage reconnaissance → authentication → privilege → persistence sequence. |
| ⬜ | **A7-Day 2** | Execute + collect | Run the authorized simulation and collect network and endpoint evidence. |
| ⬜ | **A7-Day 3** | Correlation investigation | Correlate events by host, source IP, user, timestamps, and ATT&CK stage. |
| ⬜ | **A7-Day 4** | Detection improvement | Identify visibility gaps, improve detections, retest, and document findings. |

## Advanced 8 — Detection-as-Code

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A8-Day 1** | Rule repository standard | Standardize rule directories, naming, metadata, ATT&CK mappings, test cases, and expected results. |
| ⬜ | **A8-Day 2** | Versioned detection workflow | Add validation procedures and Git-based change tracking for detection changes. |
| ⬜ | **A8-Day 3** | Detection documentation | Document purpose, data source, severity, test procedure, expected alert, and known limitations. |

## Advanced 9 — Automated Detection Validation

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A9-Day 1** | Test framework design | Define safe and reproducible validation tests for selected Wazuh and Suricata detections. |
| ⬜ | **A9-Day 2** | Linux validation scripts | Build scripts that generate controlled Linux test events and verify expected alerts. |
| ⬜ | **A9-Day 3** | Network + Windows validation | Extend safe validation to network and Windows detections where applicable. |
| ⬜ | **A9-Day 4** | Regression suite | Run the complete validation set after rule changes and record pass/fail results. |

## Advanced 10 — SOC Metrics and Dashboards

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A10-Day 1** | Metrics design | Define useful severity, endpoint, rule, ATT&CK, authentication, and alert-trend metrics. |
| ⬜ | **A10-Day 2** | Dashboard implementation | Build Wazuh dashboard views for operational monitoring and investigation. |
| ⬜ | **A10-Day 3** | Dashboard documentation | Capture screenshots and explain how each visualization supports analyst decisions. |

## Advanced 11 — Incident Response Workflow

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A11-Day 1** | Triage workflow | Create reusable alert triage, severity, evidence, and escalation templates. |
| ⬜ | **A11-Day 2** | Investigation + containment | Define investigation, containment, recovery, and evidence-preservation procedures. |
| ⬜ | **A11-Day 3** | Tabletop validation | Apply the workflow to a simulated incident and improve the templates based on findings. |

## Advanced 12 — Purple-Team Capstone

| Status | Day | Focus | Work |
| --- | --- | --- | --- |
| ⬜ | **A12-Day 1** | Capstone planning | Define authorized scope, ATT&CK stages, expected telemetry, and success criteria. |
| ⬜ | **A12-Day 2** | Initial simulation | Execute the first controlled attack stages and preserve raw evidence. |
| ⬜ | **A12-Day 3** | Full telemetry investigation | Investigate endpoint, network, authentication, Auditd, FIM, Sysmon, and Wazuh telemetry as applicable. |
| ⬜ | **A12-Day 4** | Detection-gap remediation | Identify gaps, develop or tune detections, and document the reasoning behind changes. |
| ⬜ | **A12-Day 5** | Retest | Repeat relevant simulation stages and demonstrate improved detection coverage. |
| ⬜ | **A12-Day 6** | Final report + portfolio release | Produce the final incident report, ATT&CK timeline, detection summary, lessons learned, screenshots, and polished repository documentation. |

## Progress Summary

```text
Advanced 1      1 / 3 complete
Advanced 2      0 / 4
Advanced 3      0 / 3
Advanced 4      0 / 3
Advanced 5      0 / 3
Advanced 6      0 / 2
Advanced 7      0 / 4
Advanced 8      0 / 3
Advanced 9      0 / 4
Advanced 10     0 / 3
Advanced 11     0 / 3
Advanced 12     0 / 6
```

## Current Position

**Current:** Advanced 1 — Day 1 complete.

**Next:** Advanced 1 — Day 2 — Wazuh agent deployment on `soc-linux-02`.
