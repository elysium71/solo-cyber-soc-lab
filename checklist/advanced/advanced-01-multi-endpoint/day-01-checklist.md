# Advanced 1 — Day 1 Checklist

## VM build
- [x] Created second Ubuntu Server endpoint
- [x] Set hostname to `soc-linux-02`
- [x] Created user `socuser`
- [x] Installed OpenSSH Server
- [x] Configured two VMware network adapters

## Network configuration
- [x] Configured NAT interface for Internet access
- [x] Confirmed NAT interface `ens33`
- [x] Confirmed DHCP address `192.168.229.137/24`
- [x] Configured SOC-LAB interface `ens37`
- [x] Assigned static SOC-LAB address `192.168.56.30/24`
- [x] Left the SOC-LAB interface without a default gateway
- [x] Corrected the Netplan interface name from `eth0` to `ens37`
- [x] Validated Netplan with `netplan generate`
- [x] Applied configuration with `netplan apply`

## Connectivity validation
- [x] Confirmed `soc-linux-02` can reach `soc-ubuntu`
- [x] Confirmed `192.168.56.30 → 192.168.56.20`
- [x] Confirmed 0% ICMP packet loss
- [x] Confirmed `soc-ubuntu` can reach `soc-linux-02`
- [x] Confirmed `192.168.56.20 → 192.168.56.30`
- [x] Confirmed TCP port `22` is reachable on `soc-linux-02`
- [x] Confirmed SSH access to the new endpoint

## Final topology

```text
soc-kali
192.168.56.10
      │
      │
    SOC-LAB
      │
      ├──────── soc-ubuntu
      │         192.168.56.20
      │         Wazuh Manager
      │
      └──────── soc-linux-02
                192.168.56.30
                New Linux Endpoint