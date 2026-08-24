# Advanced 2 — Day 1: Build + Network Windows Endpoint

## Objective
Build a Windows 11 endpoint for the SOC lab, connect it to NAT and the isolated `SOC-LAB` network, assign a stable internal address, and validate bidirectional connectivity.

## Environment

| System | Role | Internal IP |
| --- | --- | --- |
| `soc-kali` | Attack simulation | `192.168.56.10` |
| `soc-ubuntu` | Wazuh Manager, monitored target, Suricata | `192.168.56.20` |
| `soc-linux-02` | Linux monitored endpoint / Agent `001` | `192.168.56.30` |
| `soc-windows-01` | Windows endpoint | `192.168.56.40` |

## Windows VM Configuration

```text
Hostname:     soc-windows-01
Guest OS:     Windows 11
Memory:       8 GB
Processors:   2
Disk:         64 GB
Adapter 1:    NAT
Adapter 2:    LAN Segment — SOC-LAB
```

VMware Tools was installed/updated for improved guest integration and performance.

## Network Configuration
`Ethernet0` was retained as the VMware NAT interface. `Ethernet1` was configured as the isolated SOC-LAB interface:

```text
Interface:       Ethernet1
IPv4:            192.168.56.40
Prefix:          /24
Default gateway: None
DNS:             None
```

Validation:

```powershell
Get-NetIPAddress -InterfaceAlias "Ethernet1" -AddressFamily IPv4
```

Confirmed:

```text
IPAddress      : 192.168.56.40
InterfaceAlias : Ethernet1
PrefixLength   : 24
PrefixOrigin   : Manual
AddressState   : Preferred
```

## Connectivity Validation

From Windows:

```powershell
ping 192.168.56.20
ping 192.168.56.30
ping 8.8.8.8
```

Results:

```text
Windows → soc-ubuntu       PASS — 0% packet loss
Windows → soc-linux-02     PASS — 0% packet loss
Windows → Internet         PASS
```

## Windows Firewall
Inbound ICMP was initially blocked. Instead of disabling Windows Firewall, a scoped rule was created:

```powershell
New-NetFirewallRule `
-DisplayName "SOC-LAB Allow ICMPv4 Echo" `
-Protocol ICMPv4 `
-IcmpType 8 `
-Direction Inbound `
-Action Allow `
-RemoteAddress 192.168.56.0/24
```

## Reverse Connectivity

From `soc-linux-02`:

```bash
ping -c 4 192.168.56.40
```

Result: **PASS**

From `soc-ubuntu`:

```bash
ping -c 4 192.168.56.40
```

Confirmed:

```text
4 packets transmitted, 4 received, 0% packet loss
```

## Final SOC-LAB Addressing

```text
soc-kali        192.168.56.10
soc-ubuntu      192.168.56.20
soc-linux-02    192.168.56.30
soc-windows-01  192.168.56.40
```


## Result
**PASS — The Windows 11 endpoint was built, assigned `192.168.56.40/24`, and validated for bidirectional SOC-LAB connectivity.**

## Advanced 2 Status

```text
✅ A2-Day 1 — Build + network Windows endpoint
⬜ A2-Day 2 — Wazuh Windows agent
⬜ A2-Day 3 — Sysmon deployment
⬜ A2-Day 4 — Windows detection validation
```

## Next
**Advanced 2 — Day 2: Install and enroll the Wazuh Windows agent.**
