# Day 11 Checklist

## Network activity
- [x] Verified the SOC-LAB target at `192.168.56.20`
- [x] Generated a controlled TCP SYN scan from Kali
- [x] Scanned TCP ports `20-100`
- [x] Confirmed SSH was open on TCP port `22`

## Suricata detection
- [x] Confirmed Suricata detected the Kali scan
- [x] Confirmed source IP `192.168.56.10`
- [x] Confirmed destination IP `192.168.56.20`
- [x] Confirmed Suricata SID `1000001`
- [x] Confirmed signature `SOC-LAB Possible TCP Port Scan from Kali`
- [x] Confirmed Wazuh ingested the alert as Rule `86601`

## Host activity
- [x] Generated a controlled SSH attempt from the same Kali host
- [x] Used the non-existent account `fakeuser`
- [x] Generated an intentional failed authentication event
- [x] Confirmed Wazuh Rule `5710`
- [x] Confirmed Rule `5710` identified the source as `192.168.56.10`

## Correlation
- [x] Correlated network and host telemetry using the common source IP
- [x] Confirmed both events targeted the same Ubuntu SOC host
- [x] Confirmed the SSH activity followed the scan by approximately 2.5 minutes
- [x] Viewed Rule `86601` and Rule `5710` together in Wazuh Threat Hunting
- [x] Captured Threat Hunting evidence

## Documentation
- [x] Created `day-11-network-host-correlation.md`
- [x] Documented the investigation timeline
- [x] Documented the network-to-host correlation
- [x] Saved evidence under `screenshots/day-11/`

## Result
- [x] **PASS — Suricata network reconnaissance and Wazuh host authentication telemetry were successfully correlated.**
