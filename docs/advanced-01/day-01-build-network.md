# Advanced 1 — Day 1: Build + Network Second Linux Endpoint

## Objective
Add a second Linux endpoint to the SOC-LAB network and validate connectivity before Wazuh enrollment.

## Endpoint
- Hostname: `soc-linux-02`
- SOC-LAB IP: `192.168.56.30/24`
- Wazuh Manager: `soc-ubuntu` — `192.168.56.20`

## Validation
```bash
hostname
ip -br addr
ping -c 3 192.168.56.20
ping -c 3 packages.wazuh.com
```

Validated:
- `soc-linux-02` hostname
- `192.168.56.30/24` on the SOC-LAB interface
- Connectivity to `soc-ubuntu`
- Internet/package repository connectivity

## Result
**PASS — The second Linux endpoint was built and connected to the SOC-LAB network.**

## Next
**A1-Day 2 — Deploy and enroll the Wazuh agent.**
