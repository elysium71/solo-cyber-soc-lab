# Advanced 2 — Day 1 Checklist

## Windows VM Build
- [x] Created Windows 11 VM
- [x] Configured 8 GB RAM
- [x] Configured 2 processors
- [x] Configured 64 GB virtual disk
- [x] Configured NAT adapter
- [x] Added `SOC-LAB` LAN Segment adapter
- [x] Installed/updated VMware Tools

## Windows Configuration
- [x] Renamed endpoint to `soc-windows-01`
- [x] Verified hostname
- [x] Identified `Ethernet0` as NAT
- [x] Identified `Ethernet1` as SOC-LAB

## SOC-LAB Network
- [x] Assigned `192.168.56.40`
- [x] Configured `/24`
- [x] Left SOC-LAB gateway unset
- [x] Preserved NAT DHCP/default gateway
- [x] Verified address state as `Preferred`

## Connectivity
- [x] Windows → `192.168.56.20`
- [x] Windows → `192.168.56.30`
- [x] Windows → Internet
- [x] Added scoped inbound ICMPv4 firewall rule
- [x] `soc-linux-02` → Windows
- [x] `soc-ubuntu` → Windows
- [x] Confirmed `0%` packet loss from `soc-ubuntu`

## Result
- [x] **PASS — A2-Day 1 COMPLETE**

## Next
- [ ] **A2-Day 2 — Wazuh Windows agent**
