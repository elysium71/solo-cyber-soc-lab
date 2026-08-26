# Mini SOC Lab: Attack Simulation and Detection

A hands-on home Security Operations Centre (SOC) lab for safely simulating, detecting, investigating, and documenting security events with Kali Linux, Ubuntu Server, Windows 11, Wazuh, Suricata, Sysmon, Auditd, and additional monitored endpoints.

> All testing is performed only against virtual machines owned and controlled by the project author on an isolated lab network.

## Project Overview

This project demonstrates a practical blue-team workflow: build an isolated SOC lab, collect endpoint and network telemetry, generate controlled security activity, investigate alerts, identify detection gaps, create and tune custom rules, correlate events, map detections to MITRE ATT&CK, and document analyst findings.

The **12-day core lab is complete**. The project is now progressing through an advanced SOC engineering phase covering multi-endpoint monitoring, Windows/Sysmon telemetry, incident investigation, Active Response, detection tuning, threat hunting, ATT&CK coverage, detection-as-code, automated validation, dashboards, incident response, and a final purple-team capstone.

## Current Architecture

| System | Role | SOC-LAB IP |
| --- | --- | --- |
| Kali Linux (`soc-kali`) | Attack simulation | `192.168.56.10` |
| Ubuntu Server (`soc-ubuntu`) | Wazuh Manager, monitored target, Suricata sensor | `192.168.56.20` |
| Ubuntu Server (`soc-linux-02`) | Additional Linux endpoint / Wazuh Agent `001` | `192.168.56.30` |
| Windows 11 (`soc-windows-01`) | Windows endpoint / Sysmon / Wazuh Agent `002` | `192.168.56.40` |

```text
                         SOC-LAB
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
    soc-kali            soc-ubuntu         soc-linux-02
 192.168.56.10        192.168.56.20       192.168.56.30
 Attack system        Wazuh/Suricata       Linux endpoint
                            │
                            │
                     soc-windows-01
                      192.168.56.40
                    Windows + Sysmon
```

## Project Progress

### Core SOC Lab

**Status: Complete — 12 / 12 days ✅**

The core lab covers:

- Wazuh SIEM/XDR deployment
- Suricata IDS integration
- SSH authentication monitoring
- Linux account and privilege monitoring
- File Integrity Monitoring
- Auditd integration
- Credential-file access detection
- Cron persistence detection
- Privileged command detection
- Custom Wazuh and Suricata rules
- MITRE ATT&CK mapping
- Network + host correlation

See **[Core Lab Progress](progress/core-lab-progress.md)** for the full day-by-day record.

### Advanced SOC Lab

**Status: In Progress**

Current progress:

```text
Advanced 1 — Multi-Endpoint Monitoring       3 / 3 complete
Advanced 2 — Windows Detection Engineering   5 / 5 complete
Advanced 3 — Incident Investigation          1 / 3 complete
```

See **[Advanced SOC Roadmap](progress/advanced-roadmap.md)** for the complete Advanced 1–13 plan.

## Detection Highlights

### Network reconnaissance

A controlled Nmap scan is detected using a custom Suricata rule and forwarded into Wazuh.

- Suricata SID: `1000001`
- Wazuh Rule: `86601`
- MITRE ATT&CK: **T1046 — Network Service Discovery**

### Privileged group modification

A custom Wazuh rule detects controlled modification of sudo-group membership.

- Wazuh Rule: `100100`
- MITRE ATT&CK: **T1098 — Account Manipulation**

### Root SSH attempts

A custom rule detects high-value SSH attempts targeting the root account.

- Wazuh Rule: `100200`
- MITRE ATT&CK: **T1110 — Brute Force**

### Credential-file access

Auditd and Wazuh detect controlled access to `/etc/shadow`.

- Wazuh Rule: `100300`
- Alert level: `12`
- MITRE ATT&CK: **T1003 — OS Credential Dumping**

### Cron persistence

Real-time monitoring of `/etc/cron.d` detects creation of a controlled persistence artifact.

- Wazuh Rule: `100400`
- MITRE ATT&CK: **T1053.003 — Cron**

### Privileged command execution

Auditd telemetry identifies commands executed as root while retaining the original non-root login identity.

- Wazuh Rule: `100500`

### Windows PowerShell ExecutionPolicy Bypass

Sysmon process telemetry and a custom Wazuh rule detect PowerShell launched with `-ExecutionPolicy Bypass`.

- Wazuh Rule: `100600`
- Alert level: `10`
- MITRE ATT&CK: **T1059.001 — PowerShell**

### Windows account discovery

Custom and built-in Wazuh detections identify account and privileged-group discovery using `net.exe` / `net1.exe`.

- Custom Wazuh Rule: `100601`
- Built-in discovery rules used during investigation: `92033`, `92031`
- MITRE ATT&CK: **T1087 — Account Discovery**
- MITRE ATT&CK: **T1069.001 — Permission Groups Discovery: Local Groups**

### Incident investigation + timeline reconstruction

Advanced 3 Day 1 correlated:

```text
powershell.exe
├── net.exe user
│   └── net1.exe user
└── net.exe localgroup administrators
    └── net1.exe localgroup administrators
```

The incident was classified as:

```text
TRUE POSITIVE — CONTROLLED LAB SIMULATION
```

See **[Incident Reports](incident-reports/)** for analyst-style investigation reports.

## Repository Structure

```text
solo-cyber-soc-lab/
├── README.md
├── progress/
│   ├── core-lab-progress.md
│   └── advanced-roadmap.md
├── docs/
│   ├── advanced-01/
│   ├── advanced-02/
│   └── advanced-03/
├── checklist/
│   └── advanced/
├── architecture/
├── attack-simulations/
├── detection-rules/
│   └── wazuh/
├── incident-reports/
├── screenshots/
├── scripts/
└── setup-guides/
```

## Tools and Technologies

- Kali Linux
- Ubuntu Server
- Windows 11
- Wazuh SIEM/XDR
- Suricata IDS
- Microsoft Sysmon
- Auditd
- Linux File Integrity Monitoring
- Nmap
- OpenSSH
- VMware
- Git and GitHub
- MITRE ATT&CK

## Skills Demonstrated

- SOC lab architecture and network isolation
- SIEM deployment and multi-endpoint monitoring
- Linux log and Auditd analysis
- Windows event and Sysmon analysis
- Network intrusion detection
- File Integrity Monitoring
- Detection-gap analysis
- Custom Wazuh and Suricata rule development
- Rule testing and alert validation
- Parent/child process correlation
- Incident triage and timeline reconstruction
- Network and endpoint correlation
- MITRE ATT&CK mapping
- Evidence collection and technical documentation
- Analyst-style incident reporting

## Advanced Direction

The advanced phase expands the project into a small detection-engineering and SOC investigation environment with:

- Multi-host Wazuh monitoring
- Linux and Windows telemetry
- Sysmon and Auditd analysis
- Incident investigation and evidence enrichment
- Wazuh Active Response
- Detection tuning and false-positive reduction
- Threat hunting
- MITRE ATT&CK coverage analysis
- Multi-stage attack correlation
- Detection-as-code
- Automated detection regression testing
- SOC metrics and dashboards
- Incident response workflows
- Controlled purple-team validation

The final goal is to demonstrate not only that security alerts can be generated, but that telemetry can be collected, correlated, investigated, tuned, documented, and repeatedly validated using a structured SOC workflow.

## Ethical Use

This repository is intended solely for defensive cybersecurity education. Security testing must be limited to systems owned by the tester or systems for which explicit authorization has been granted.
