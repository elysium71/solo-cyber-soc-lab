# Network Setup

## Network Design

Each virtual machine uses two network adapters.

### Adapter 1: NAT

The NAT adapter provides internet access for operating-system updates and
software installation.

### Adapter 2: Internal Network

The internal adapter connects the two lab machines through an isolated
VirtualBox network named `SOC-LAB`.

## IP Address Plan

| Device | Role | Internal IP |
|---|---|---|
| SOC-Kali | Attack simulation machine | 192.168.56.10/24 |
| SOC-Ubuntu | Monitored target and Wazuh server | 192.168.56.20/24 |

The internal adapters do not use a default gateway. Internet traffic uses
the NAT adapters.

## Connectivity Tests

The following tests were completed:

```bash
# Kali to Ubuntu
ping -c 4 192.168.56.20

# Ubuntu to Kali
ping -c 4 192.168.56.10

# SSH from Kali to Ubuntu
ssh socadmin@192.168.56.20
```

## Results
Kali successfully reached Ubuntu.
Ubuntu successfully reached Kali.
SSH access to Ubuntu was successful.
Both machines retained internet access through NAT.