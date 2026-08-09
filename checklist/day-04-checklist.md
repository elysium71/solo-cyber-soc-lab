# Day 4 Checklist — Account & Privilege Monitoring

## Lab Verification

[x] Ubuntu Server started successfully  
[x] Wazuh Manager is active  
[x] Suricata is active  
[x] Kali and Ubuntu SOC-LAB network is available  

## User Account Baseline

[x] Checked existing local users  
[x] Confirmed `soclab-test` did not exist before simulation  
[x] Checked existing sudo group membership  
[x] Confirmed `soclab-test` was not initially a sudo member  
[x] Saved baseline evidence  

## Suspicious Account Creation

[x] Created controlled `soclab-test` account  
[x] Verified new account with `getent passwd`  
[x] Confirmed account creation in `/var/log/auth.log`  
[x] Confirmed Wazuh detected new user creation  
[x] Observed Wazuh Rule 5902 — New user added to the system  
[x] Observed Wazuh Rule 5901 — New group added to the system  
[x] Saved Wazuh account-creation evidence  

## Privilege Modification

[x] Added `soclab-test` to the sudo group  
[x] Verified sudo group membership  
[x] Verified privilege change with `id soclab-test`  
[x] Confirmed `usermod` activity in `/var/log/auth.log`  
[x] Investigated corresponding events in Wazuh  
[x] Observed generic Wazuh Rule 5402  
[x] Identified missing specific detection for sudo-group modification  

## Custom Wazuh Detection

[x] Backed up `local_rules.xml`  
[x] Created custom Wazuh Rule 100100  
[x] Configured custom alert level 10  
[x] Created detection for additions to the sudo group  
[x] Tested custom rule using `wazuh-logtest`  
[x] Confirmed Rule 100100 matched the test event  
[x] Restarted Wazuh Manager successfully  
[x] Reproduced the sudo-group modification  
[x] Confirmed custom Rule 100100 triggered on a real event  
[x] Confirmed custom alert appeared in Wazuh Threat Hunting  

## MITRE ATT&CK

[x] Mapped privileged account modification to T1098  
[x] Documented Account Manipulation technique  
[x] Confirmed Wazuh displayed MITRE ATT&CK mapping  

## Documentation & Evidence

[x] Created `sudo-group-modification.md`  
[x] Saved custom Wazuh rule for GitHub  
[x] Saved account-creation screenshot  
[x] Saved Wazuh account-creation alert screenshot  
[x] Saved sudo-group modification evidence  
[x] Saved custom rule configuration evidence  
[x] Saved `wazuh-logtest` evidence  
[x] Saved final Rule 100100 Wazuh alert evidence  
