# Mini SOC Lab: Attack Simulation and Detection

A hands-on home Security Operations Centre (SOC) lab for safely simulating, detecting, investigating, and documenting security events with Kali Linux, Ubuntu Server, Wazuh, and Suricata.

> All testing is performed only against virtual machines owned and controlled by the project author on an isolated lab network.

## Project Overview

This project demonstrates a practical blue-team workflow: establish a secure lab, generate controlled security activity, inspect host and network telemetry, identify detection gaps, create custom rules, validate alerts, and map the activity to MITRE ATT&CK.

The environment uses Kali Linux as the attack-simulation system and Ubuntu Server as both the monitored target and the Wazuh server. Suricata adds network-level visibility, while Wazuh collects and analyses the resulting host and IDS events.

## Objectives

- Build an isolated and repeatable cybersecurity lab.
- Collect Linux authentication, system, and network IDS logs.
- Simulate authorized reconnaissance and account-related activity.
- Detect and investigate suspicious behaviour in Wazuh.
- Identify detection gaps and develop custom rules.
- Map observed activity to MITRE ATT&CK.
- Preserve technical evidence and documentation in GitHub.

## Lab Architecture

| System | Role | Internal IP |
| --- | --- | --- |
| Kali Linux (`soc-kali`) | Attack simulation | `192.168.56.10` |
| Ubuntu Server (`soc-ubuntu`) | Monitored target, Wazuh, and Suricata | `192.168.56.20` |

Each VM has two network adapters:

- **NAT:** Internet access for updates and software installation.
- **SOC-LAB:** Isolated internal network for controlled security testing.

## Detection Pipeline

```text
Kali Linux → Controlled activity → Ubuntu Server
                                      │
                         ┌────────────┴────────────┐
                         │                         │
                   Host logs                 Suricata IDS
                         │                         │
                         └────────────┬────────────┘
                                      ↓
                                    Wazuh
                                      ↓
                          Alert investigation and
                           MITRE ATT&CK mapping
```

## Current Progress

## Lab Progress Roadmap

| Status | Day | Focus | Key outcome / goal |
| --- | --- | --- | --- |
| ✅ | **Day 1** | Lab and network setup | Built Kali and Ubuntu VMs, configured the isolated `SOC-LAB` network, and verified connectivity. |
| ✅ | **Day 2** | Wazuh installation and validation | Installed Wazuh and validated failed SSH authentication telemetry with Rules `5710` and `5503`. |
| ✅ | **Day 3** | Suricata and Nmap detection | Integrated Suricata with Wazuh and detected Nmap reconnaissance using Suricata SID `1000001` and Wazuh Rule `86601`. |
| ✅ | **Day 4** | Account and privilege monitoring | Detected account activity and created Rule `100100` for sudo-group modification. |
| ✅ | **Day 5** | File Integrity Monitoring | Configured and validated Wazuh FIM for controlled file changes. |
| ✅ | **Day 6** | SSH authentication detection | Investigated repeated SSH authentication activity and validated Wazuh authentication telemetry. |
| ✅ | **Day 7** | Custom SSH detection | Created Rule `100200` for high-value SSH attempts against the root account and mapped it to MITRE ATT&CK `T1110`. |
| ✅ | **Day 8** | Auditd credential-access detection | Integrated Auditd and created Rule `100300` for `/etc/shadow` access, mapped to `T1003`. |
| ✅ | **Day 9** | Cron persistence detection | Added real-time monitoring of `/etc/cron.d` and Rule `100400`, mapped to `T1053.003`. |
| ✅ | **Day 10** | Privileged command execution | Used Auditd telemetry and Rule `100500` to detect commands executed as root by a non-root login identity. |
| ✅ | **Day 11** | Network + host correlation | Correlated a Suricata port-scan alert (`86601`) with subsequent SSH invalid-user activity (`5710`) from the same source IP. |
| ✅ | **Day 12** | Final SOC lab review  | Perform an end-to-end validation, clean repository documentation, update screenshots/links, summarize skills demonstrated, and prepare the project as a finished portfolio piece. |



## Repository Structure

```text
solo-cyber-soc-lab/
├── architecture/          # Architecture material
├── attack-simulations/    # Controlled attack procedures and findings
├── checklist/             # Daily implementation checklists
├── detection-rules/       # Suricata and Wazuh custom detections
├── incident-reports/      # Investigation reports
├── screenshots/           # Evidence organised by lab day
├── scripts/               # Supporting automation
├── setup-guides/          # VM, network, and Wazuh setup notes
└── README.md
```

## Tools and Technologies

- Kali Linux
- Ubuntu Server
- Wazuh SIEM/XDR
- Suricata IDS
- Nmap
- Linux authentication and system logs
- Git and GitHub
- MITRE ATT&CK

## Skills Demonstrated

- SOC lab design and network isolation
- SIEM deployment and alert investigation
- Linux log analysis
- Network intrusion detection
- Detection-gap analysis
- Custom Suricata and Wazuh rule development
- Rule testing and alert validation
- MITRE ATT&CK mapping
- Evidence collection and technical documentation

## Future Planned Work

After the core 12-day lab is complete, the project can continue into a more advanced SOC engineering and detection-engineering phase. These extensions are optional and are intended to make the lab closer to a realistic SOC environment.

| Phase | Focus | Planned work |
| --- | --- | --- |
| **Advanced 1** | Multi-endpoint monitoring | Add another Linux endpoint and a Windows endpoint to Wazuh and compare telemetry across multiple hosts. |
| **Advanced 2** | Windows detection engineering | Deploy Sysmon, collect Windows event telemetry, and create detections for suspicious PowerShell, process creation, and account activity. |
| **Advanced 3** | Active Response | Configure carefully scoped Wazuh Active Response actions and validate automatic responses to selected high-confidence lab detections. |
| **Advanced 4** | Detection tuning | Generate benign and suspicious activity, identify false positives, and tune thresholds and rule conditions without losing useful coverage. |
| **Advanced 5** | Threat hunting | Create repeatable hunting queries for authentication, privilege escalation, persistence, credential access, and network reconnaissance. |
| **Advanced 6** | ATT&CK coverage matrix | Build a MITRE ATT&CK coverage table showing which techniques are detected, which telemetry provides coverage, and where gaps remain. |
| **Advanced 7** | Attack-chain correlation | Simulate a controlled multi-stage sequence such as reconnaissance → authentication attempt → privilege activity → persistence and correlate the events into one investigation timeline. |
| **Advanced 8** | Detection-as-code | Store Wazuh and Suricata rules in version-controlled directories with documentation, test cases, rule IDs, severity, ATT&CK mappings, and expected results. |
| **Advanced 9** | Automated validation | Create safe scripts that reproduce lab events and verify that expected Wazuh/Suricata detections are generated after rule changes. |
| **Advanced 10** | SOC metrics and dashboards | Build useful dashboards for alert severity, detection category, source hosts, ATT&CK techniques, authentication activity, and alert trends. |
| **Advanced 11** | Incident response workflow | Create reusable triage, investigation, containment, evidence, and closure templates and use them during simulated incidents. |
| **Advanced 12** | Purple-team capstone | Run a final controlled attack simulation across multiple ATT&CK stages, investigate it entirely from collected telemetry, document gaps, improve detections, and publish a final incident report. |

### Long-term target

The advanced phase will evolve the project from a basic home SOC into a small detection-engineering environment demonstrating:

- Multi-host SIEM monitoring
- Linux and Windows telemetry
- Wazuh and Suricata detection engineering
- Sysmon and Auditd analysis
- Network and endpoint correlation
- MITRE ATT&CK coverage analysis
- Threat hunting
- Detection tuning and false-positive reduction
- Automated detection testing
- Incident response documentation
- SOC dashboards and metrics
- Controlled purple-team exercises
- Detection-as-code and Git-based change tracking

The final goal is to demonstrate not only that security alerts can be generated, but that telemetry can be collected, correlated, investigated, tuned, documented, and repeatedly validated using a structured SOC workflow.


## Ethical Use

This repository is intended solely for defensive cybersecurity education. Security testing must be limited to systems owned by the tester or systems for which explicit authorization has been granted.
