# Day 11 — Network + Host Alert Correlation

## Objective
Correlate network reconnaissance detected by Suricata with subsequent host authentication activity detected by Wazuh.

## Detection flow
```text
Kali Linux (192.168.56.10)
        ↓
TCP SYN port scan
        ↓
Suricata SID 1000001
        ↓
Wazuh Rule 86601
        ↓
Same source IP
        ↓
SSH invalid-user login attempt
        ↓
Ubuntu sshd / journald
        ↓
Wazuh Rule 5710
```

## Controlled test
Network reconnaissance was generated from Kali:

```bash
sudo nmap -sS -p 20-100 192.168.56.20
```

The scan identified SSH:

```text
PORT   STATE SERVICE
22/tcp open  ssh
```

A controlled SSH authentication attempt was then made from the same Kali system:

```bash
ssh fakeuser@192.168.56.20
```

An intentionally incorrect password generated the authentication failure.

## Network detection
Suricata detected:

```text
src_ip: 192.168.56.10
dest_ip: 192.168.56.20
signature_id: 1000001
signature: SOC-LAB Possible TCP Port Scan from Kali
```

Wazuh generated:

```text
Rule: 86601 (level 3) -> 'Suricata: Alert - SOC-LAB Possible TCP Port Scan from Kali'
```

Approximate event time:

```text
2026-08-20 10:16:00 UTC
```

## Host detection
The subsequent SSH attempt generated:

```text
Rule: 5710 (level 5) -> 'sshd: Attempt to login using a non-existent user'
Src IP: 192.168.56.10
```

The SSH log included:

```text
Failed password for invalid user fakeuser from 192.168.56.10
```

Approximate event time:

```text
2026-08-20 10:18:29–10:18:35 UTC
```

## Correlation
The detections were correlated using the common source IP and close timestamps.

```text
10:16:00 UTC
192.168.56.10 → 192.168.56.20
Network reconnaissance
Suricata SID 1000001
Wazuh Rule 86601

        ↓ approximately 2.5 minutes

10:18:29–10:18:35 UTC
192.168.56.10 → soc-ubuntu
Invalid-user SSH authentication attempt
Wazuh Rule 5710
```

The same Kali source first performed network reconnaissance and shortly afterwards attempted SSH authentication against the same Ubuntu target.

## Threat Hunting validation
Wazuh Threat Hunting was searched for:

```text
192.168.56.10
```

The results showed both:

```text
Rule 86601 — Suricata: Alert - SOC-LAB Possible TCP Port Scan from Kali
Rule 5710  — sshd: Attempt to login using a non-existent user
```

## Evidence
Screenshot:

```text
screenshots/day-11/network-host-correlation.png
```

## Result
**PASS — Network reconnaissance and subsequent host authentication activity were successfully detected and correlated using Suricata and Wazuh telemetry.**
