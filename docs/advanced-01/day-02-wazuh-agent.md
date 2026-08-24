# Advanced 1 — Day 2: Wazuh Agent Deployment

## Objective
Install Wazuh Agent 4.13.1 on `soc-linux-02`, connect it to `soc-ubuntu`, and verify centralized telemetry.

## Installation
```bash
sudo apt update
sudo WAZUH_MANAGER="192.168.56.20" \
WAZUH_AGENT_NAME="soc-linux-02" \
apt install wazuh-agent=4.13.1-1
```

## Configuration Validation
```bash
sudo grep -A 8 "<client>" /var/ossec/etc/ossec.conf
sudo /var/ossec/bin/wazuh-control info
```

Expected manager:
```text
192.168.56.20
```

Expected type:
```text
WAZUH_TYPE="agent"
```

## Start Agent
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now wazuh-agent
sudo systemctl status wazuh-agent --no-pager
```

## Manager Validation
On `soc-ubuntu`:
```bash
sudo /var/ossec/bin/agent_control -l
```

Validated:
```text
ID: 001, Name: soc-linux-02, IP: any, Active
```

## Telemetry Validation
Wazuh received Linux authentication activity including PAM/sudo events from `soc-linux-02`.

## Result
**PASS — `soc-linux-02` was enrolled as active Wazuh Agent `001`.**

## Next
**A1-Day 3 — Cross-endpoint detection validation.**
