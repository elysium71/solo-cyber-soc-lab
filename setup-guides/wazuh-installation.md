# Wazuh Installation

## Environment

- Ubuntu Server 26.04 LTS
- Wazuh 4.13.1

## Installation

The official Wazuh all-in-one installer was used:

```bash
sudo ./wazuh-install.sh -a
```

## Installed Components

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Filebeat

## Verification

| Service | Status |
|----------|--------|
| Wazuh Manager | Running |
| Wazuh Indexer | Running |
| Wazuh Dashboard | Running |

## Notes

The installation completed successfully and all required services started without errors.