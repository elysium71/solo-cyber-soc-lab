# solo-cyber-soc-lab
A home SOC lab using Kali Linux, Ubuntu Server and Wazuh to simulate, detect and investigate security events.

# Mini SOC Lab: Attack Simulation and Detection

## Project Overview

This project is a small Security Operations Centre lab built to practise
attack simulation, security monitoring, alert investigation and incident
documentation.

The lab uses Kali Linux as an attack-simulation machine and Ubuntu Server
as the monitored target. Wazuh will be installed as the SIEM platform to
collect and analyse security logs.

All security tests are performed only against virtual machines owned and
controlled by the project author.

## Project Objectives

- Build an isolated virtual security lab.
- Collect Linux authentication and system logs.
- Simulate safe reconnaissance and authentication attacks.
- Detect suspicious activity using Wazuh.
- Investigate generated alerts.
- Map activity to MITRE ATT&CK.
- Document the project using Git and GitHub.

## Lab Architecture

| Machine | Purpose | Internal IP |
|---|---|---|
| Kali Linux | Attack simulation | 192.168.56.10 |
| Ubuntu Server | Target and Wazuh server | 192.168.56.20 |

Each machine has:

- One NAT adapter for internet access.
- One internal adapter connected to the isolated `SOC-LAB` network.

## Version 1 Scope

The first version will include:

- Two virtual machines
- One Nmap scan simulation
- One failed SSH login simulation
- Two Wazuh alerts
- One incident report
- Setup and investigation documentation

## Current Progress

- [x] Created GitHub repository
- [x] Created Kali Linux VM
- [x] Created Ubuntu Server VM
- [x] Configured isolated lab network
- [x] Confirmed network connectivity
- [x] Installed Wazuh
- [x] Verified Wazuh services
- [x] Accessed Wazuh Dashboard
- [x] Verified SSH authentication event detection
- [ ] Nmap reconnaissance simulation
- [ ] Incident reports


## Repository Structure

```text
solo-cyber-soc-lab/
├── README.md
├── architecture/
├── setup-guides/
├── attack-simulations/
├── detection-rules/
├── incident-reports/
├── scripts/
└── screenshots/
```

# Ethical Use

This project is intended only for defensive cybersecurity education.
Attack simulations must only be performed against systems owned by the
project author or systems for which explicit permission has been granted.