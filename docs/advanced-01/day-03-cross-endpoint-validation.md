# Advanced 1 — Day 3: Cross-Endpoint Detection Validation

## Objective
Validate that the Wazuh manager can monitor and distinguish security activity from multiple endpoints.

## Environment
| Endpoint | Role | Wazuh ID |
| --- | --- | --- |
| `soc-ubuntu` | Wazuh server/local endpoint | `000` |
| `soc-linux-02` | Linux monitored endpoint | `001` |

## Controlled Activity
Authentication activity was generated using sudo on both systems.

Example:
```bash
sudo -k
sudo id
```

A failed authentication attempt followed by successful privileged execution provided useful telemetry.

## Detection Validation
On `soc-ubuntu`:
```bash
sudo tail -80 /var/ossec/logs/alerts/alerts.log
```

Observed detections included:
- Rule `5503` — PAM user login failed
- Authentication failures from `soc-linux-02`
- Local privileged activity on `soc-ubuntu`
- Custom privilege-escalation telemetry where configured

## Agent Validation
```bash
sudo /var/ossec/bin/agent_control -l
```

Validated:
```text
ID: 000, Name: soc-ubuntu, Active/Local
ID: 001, Name: soc-linux-02, Active
```

## Result
**PASS — Multi-endpoint monitoring and centralized detection were validated.**

## Advanced 1 Status
```text
✅ A1-Day 1 — Build + network second Linux endpoint
✅ A1-Day 2 — Wazuh agent deployment
✅ A1-Day 3 — Cross-endpoint detection validation
```
