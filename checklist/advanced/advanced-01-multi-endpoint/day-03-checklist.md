# Advanced 1 — Day 3 Checklist

## Endpoint Validation
- [x] Confirmed `soc-ubuntu` remained operational
- [x] Confirmed `soc-linux-02` remained operational
- [x] Confirmed Wazuh Agent `001` was active
- [x] Confirmed centralized Wazuh monitoring

## soc-ubuntu Authentication Test
- [x] Generated controlled failed sudo authentication
- [x] Confirmed Rule `5503`
- [x] Confirmed level `5`
- [x] Confirmed user `socadmin`
- [x] Confirmed source `soc-ubuntu`

## soc-linux-02 Authentication Test
- [x] Generated controlled failed sudo authentication
- [x] Confirmed Rule `5503`
- [x] Confirmed level `5`
- [x] Confirmed user `socuser`
- [x] Confirmed endpoint `soc-linux-02`
- [x] Confirmed Agent ID `001`
- [x] Confirmed Agent IP `192.168.56.30`

## Cross-Endpoint Validation
- [x] Generated equivalent activity on both Linux endpoints
- [x] Confirmed both events reached the Wazuh Manager
- [x] Confirmed both generated Rule `5503`
- [x] Confirmed different endpoint attribution
- [x] Confirmed different user attribution

## Dashboard Validation
- [x] Confirmed `agent.id: 001`
- [x] Confirmed `agent.name: soc-linux-02`
- [x] Confirmed `agent.ip: 192.168.56.30`
- [x] Confirmed `manager.name: soc-ubuntu`
- [x] Confirmed Rule `5503`
- [x] Confirmed Rule level `5`
- [x] Confirmed MITRE `T1110.001`
- [x] Confirmed tactic `Credential Access`
- [x] Confirmed technique `Password Guessing`