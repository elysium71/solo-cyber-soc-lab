# Privileged Command Execution Detection

## Rule
- Wazuh Rule ID: `100500`
- Severity: `11`
- Parent Rule: `80792`
- Data source: Auditd
- MITRE ATT&CK: `T1548` — Abuse Elevation Control Mechanism

## Purpose
Detect a lab event where the authenticated user has Audit UID `1000` but the resulting process executes with effective UID `0`.

## Detection logic
```xml
<field name="audit.auid">^1000$</field>
<field name="audit.euid">^0$</field>
```

Anchors prevent `0` from also matching values such as `1000`.

## Positive test
```bash
sudo id
```

Expected: Rule `100500`, Level `11`.

## Negative control
```bash
id
```

Expected: generic Rule `80792`; Rule `100500` should not fire.

## Scope
This is a lab-specific rule using Audit UID `1000`. A production rule should account for the environment's full identity model.
