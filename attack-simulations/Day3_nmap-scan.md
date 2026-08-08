# Nmap Port Scan Detection

## Objective

Simulate a controlled Nmap service scan from the Kali Linux attacker VM
against the Ubuntu SOC server and determine whether the activity can be
detected by the monitoring environment.

All testing was performed only against virtual machines owned and
controlled within the isolated SOC lab.

## Lab Systems

  Role          System          IP Address
  ------------- --------------- ----------------
  Attacker      Kali Linux      192.168.56.10
  Target        Ubuntu Server   192.168.56.20
  SIEM          Wazuh           Ubuntu Server
  Network IDS   Suricata        Ubuntu `ens34`

## Attack Simulation

The following command was executed from Kali Linux:

``` bash
nmap -sV 192.168.56.20
```

The scan performed service/version detection against the Ubuntu target.

The scan identified exposed services including:

-   TCP/22 --- SSH
-   TCP/443 --- HTTPS

## Initial Detection Result

During the initial test, Wazuh was successfully receiving host-based
security events such as authentication, PAM, and sudo activity.

However, the Nmap scan did not generate a useful Wazuh alert clearly
identifying the network scan.

This exposed a detection gap in the lab: host telemetry alone did not
provide sufficient network visibility for this particular activity.

## Detection Improvement

Suricata IDS was added to provide network-level monitoring.

Suricata was configured to monitor the isolated SOC-LAB interface:

``` text
ens34
```

The Emerging Threats Open ruleset was installed using:

``` bash
sudo suricata-update
```

Suricata events were written to:

``` text
/var/log/suricata/eve.json
```

Wazuh was configured to ingest this JSON log.

## Custom Detection Rule

The default ruleset did not generate the desired alert for the
controlled `nmap -sV` test, so a custom Suricata rule was created:

``` text
alert tcp 192.168.56.10 any -> 192.168.56.20 any (msg:"SOC-LAB Possible TCP Port Scan from Kali"; flags:S; threshold:type both, track by_src, count 10, seconds 10; sid:1000001; rev:1;)
```

### Rule Information

``` text
Suricata SID: 1000001
Message: SOC-LAB Possible TCP Port Scan from Kali
Source: 192.168.56.10
Destination: 192.168.56.20
```

The rule looks for repeated TCP SYN activity from the Kali attacker VM
toward the Ubuntu target.

## Validation

After loading the custom rule, the same scan was repeated:

``` bash
nmap -sV 192.168.56.20
```

Suricata successfully generated the following alert:

``` text
[1:1000001:1] SOC-LAB Possible TCP Port Scan from Kali
```

This confirmed that Suricata detected the simulated network scanning
activity.

## Wazuh Alert

Suricata's `eve.json` output was ingested by Wazuh.

The event appeared in Wazuh Threat Hunting as:

``` text
Rule ID: 86601
Rule Level: 3
Description: Suricata: Alert - SOC-LAB Possible TCP Port Scan from Kali
```

The Suricata and Wazuh rule identifiers represent different stages of
the detection pipeline:

-   **Suricata SID 1000001** --- custom IDS signature that detected the
    network activity.
-   **Wazuh Rule ID 86601** --- Wazuh rule that processed the Suricata
    alert.

## Investigation

### Source

``` text
192.168.56.10
```

Kali Linux attacker/simulation VM.

### Destination

``` text
192.168.56.20
```

Ubuntu SOC target.

### Activity

A service/version scan was performed using Nmap.

### Assessment

The activity was expected and authorized because it was generated as
part of the isolated SOC lab.

In a production environment, similar scanning activity should be
investigated to determine:

-   Whether the source system is authorized to perform vulnerability or
    network scanning.
-   Which systems and ports were targeted.
-   Whether the scanning was followed by authentication attempts or
    exploitation.
-   Whether the source host shows other indicators of compromise.

## MITRE ATT&CK Mapping

**Technique:** T1046 --- Network Service Discovery

Network service scanning can be used to identify reachable systems,
exposed ports, and available services before further activity.

## Detection Pipeline

``` text
Kali Linux
192.168.56.10
      |
      | nmap -sV
      v
Ubuntu Server
192.168.56.20
      |
      v
Suricata IDS
ens34
      |
      | SID 1000001
      v
eve.json
      |
      v
Wazuh
      |
      | Rule 86601
      v
SOC Alert
```

## Outcome

The simulation successfully demonstrated both a detection gap and its
remediation.

Initially, the Nmap scan did not produce a useful scan alert with the
host-focused telemetry available to Wazuh.

Adding Suricata provided network visibility. A custom detection rule was
then created and validated, and the resulting Suricata alert was
successfully ingested by Wazuh.

The final detection chain was:

**Kali → Nmap → Ubuntu → Suricata → eve.json → Wazuh → Alert**

## Evidence

Relevant evidence for this simulation should include:

1.  Kali terminal showing the `nmap -sV 192.168.56.20` scan.
2.  Suricata `fast.log` showing SID `1000001`.
3.  Wazuh Threat Hunting showing
    `Suricata: Alert - SOC-LAB Possible TCP Port Scan from Kali`.
