# Advanced 1 — Day 2 Checklist

## Wazuh repository
- [x] Confirmed network connectivity from `soc-linux-02`
- [x] Confirmed access to `packages.wazuh.com`
- [x] Added the Wazuh signing key
- [x] Added the Wazuh 4.x APT repository
- [x] Successfully ran `apt update`

## Wazuh Agent
- [x] Confirmed Manager version `v4.13.1`
- [x] Installed Wazuh Agent `4.13.1-1`
- [x] Set agent name to `soc-linux-02`
- [x] Set Manager address to `192.168.56.20`
- [x] Confirmed TCP port `1514`
- [x] Confirmed agent type
- [x] Enabled `wazuh-agent`
- [x] Started `wazuh-agent`
- [x] Confirmed service `active (running)`

## Enrollment
- [x] Confirmed Agent ID `001`
- [x] Confirmed agent name `soc-linux-02`
- [x] Confirmed status `Active`
- [x] Confirmed `soc-ubuntu` remains `Active/Local`

## Telemetry validation
- [x] Generated a controlled failed sudo authentication
- [x] Confirmed Rule `5557`
- [x] Confirmed Rule `5503`
- [x] Confirmed user `socuser`
- [x] Confirmed alert source `(soc-linux-02)`
- [x] Confirmed endpoint telemetry reached the Wazuh Manager

## Evidence
- [x] Captured Agent ID `001` as Active
- [x] Captured Wazuh authentication alert from `soc-linux-02`

## Result
- [x] **PASS — Wazuh Agent deployment and second-endpoint telemetry validation completed successfully.**

## Next
- [ ] **A1-Day 3 — Cross-endpoint detection validation**
