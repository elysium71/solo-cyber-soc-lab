**Date:** 2026-08-05

## Environment Preparation

- [x] Updated Ubuntu package list
- [x] Upgraded Ubuntu packages
- [x] Verified internet connectivity
- [x] Confirmed SSH access from Kali to Ubuntu

## Wazuh Installation

- [x] Downloaded the Wazuh installer (`wazuh-install.sh`)
- [x] Made the installer executable
- [x] Installed Wazuh using the official all-in-one installer
- [x] Successfully completed installation

## Service Verification

- [x] Verified Wazuh Manager is running
- [x] Verified Wazuh Indexer is running
- [x] Verified Wazuh Dashboard is running
- [x] Verified HTTPS service is listening on port 443

## Dashboard Verification

- [x] Accessed the Wazuh Dashboard
- [x] Logged in successfully
- [x] Confirmed `soc-ubuntu` is reporting events
- [x] Opened the Threat Hunting page
- [x] Verified security events are visible

## Log & Event Verification

- [x] Tested log generation using `logger`
- [x] Learned that generic syslog messages do not automatically generate Wazuh alerts
- [x] Generated a failed SSH login from Kali
- [x] Verified Wazuh detected the failed SSH login
- [x] Observed Wazuh Rule ID **5710** (`sshd: Attempt to login using a non-existent user`)
- [x] Observed Wazuh Rule ID **5503** (`PAM: User login failed`)

## Documentation

- [x] Took screenshots of the Wazuh installation
- [x] Took screenshots of the Wazuh Dashboard
- [x] Took screenshots of the SSH detection event
- [x] Finish `setup-guides/wazuh-installation.md`
- [x] Update `README.md` progress checklist

## Git

- [x] Commit Day 2 changes
- [x] Push changes to GitHub

## Virtual Machine Backup

- [x] Create VirtualBox snapshot
  - Ubuntu: `Day 2 - Wazuh Installed and Verified`
  - Kali: `Day 2 - Ready for Attack Simulation`

---

## Skills Learned Today

- Installed and configured Wazuh SIEM
- Verified Wazuh Manager, Indexer, and Dashboard services
- Accessed and navigated the Wazuh Dashboard
- Performed basic security event validation
- Investigated Wazuh rules and authentication events
- Understood the difference between log collection and alert generation
- Verified SSH authentication failure detection