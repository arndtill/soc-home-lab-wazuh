# Lab Architecture

## Overview

This lab consists of a Wazuh server running on an Ubuntu virtual machine and a Windows endpoint running on the physical host.

## Infrastructure

| Component | Specification |
|---|---|
| Host machine | Lenovo ThinkStation P320 |
| Host operating system | Windows 10 Pro |
| Processor | Intel Core i7-7700K |
| Host memory | 64 GB |
| Hypervisor | Oracle VirtualBox |
| Guest operating system | Ubuntu Server 24.04 LTS |
| Virtual CPUs | 4 |
| Allocatedmemory | 4 GB |
| Virtual disk | 80 GB |

## Architecture

1. Windows generates security events and file system changes
2. The Wazuh Agent collects these events and sends them to the Manager through TCP port 1514
3. The Wazuh Manager analyzes the events and generates alerts
4. Filebeat forwards the alerts to the Wazuh Indexer
5. The Wazuh Indexer stores and indexes the alerts
6. The Wazuh Dashboard retrieves the indexed alerts for visualization and investigation

## Wazuh Components

| Component | Function |
|---|---|
| Wazuh Agent | Collects endpoint security events and monitors file integrity |
| Wazuh Manage | Processes events, applies detection rules and generates alerts |
| Filebeat | Forwards alerts to the Wazuh Indexer |
| Wazuh Indexer | Stores and indexes security alerts |
| Wazuh Dashboard | Provides a graphical interface for monitoring and investigating alerts |

## Network Configuration

The virtual machine uses two network adapters:

- NAT: Provides internet access for package installation and updates
- Host-only: Provides communication between the Windows host and the Wazuh server without directly exposing the virtual machine to the physical network.

## Monitored Endpoint

| Field | Value |
|---|---|
| Agent name | Windows10Pro |
| Agent ID | 001 |
| Operating system | Windows 10 Pro |
| Status | Active |

[Agent Status Evidence](../evidence/infrastructure/wazuh-agent.png)
## Security Considerations

- Wazuh Indexer listens on the local interface
- Endpoint communication takes place through the host-only network
- Credentials and personal identifiers are excluded from the public repository
