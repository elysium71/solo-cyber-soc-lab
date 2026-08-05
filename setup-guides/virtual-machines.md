# Virtual Machine Setup

## Overview

This project uses two virtual machines to create an isolated security
monitoring lab.

## Virtualization Platform

- Platform: Oracle VirtualBox
- Host operating system: Windows
- Lab network name: SOC-LAB

## Kali Linux

- Purpose: Attack simulation machine
- Hostname: soc-kali
- Operating system: Kali Linux
- CPU: 2 virtual CPUs
- Memory: 4 GB
- Storage: 80 GB
- NAT adapter: Enabled
- Internal adapter: SOC-LAB
- Internal IP address: 192.168.56.10/24

## Ubuntu Server

- Purpose: Target server and Wazuh server
- Hostname: soc-ubuntu
- Operating system: Ubuntu Server 24.04 LTS
- CPU: 4 virtual CPUs
- Memory: 8 GB
- Storage: 90 GB
- NAT adapter: Enabled
- Internal adapter: SOC-LAB
- Internal IP address: 192.168.56.20/24


## Security Boundaries

All attack simulations will be performed only against virtual machines
owned and controlled by the project author. The private SOC-LAB network
will be used to prevent accidental testing against external systems.