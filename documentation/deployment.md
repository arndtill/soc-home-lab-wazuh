# Deployment and Configuration

## 1. Virtual Machine Setup

An Ubuntu Server 24.04 LTS virtual machine was created using Oracle VirtualBox, configured with:

- 4 virtual CPUs
- 8 GB of RAM
- 80 GB of virtual disk space
- NAT and Host-only network adapters

OpenSSH was enabled to allow remote administration from the Windows Host.

## 2. Wazuh Installation

Wazuh 4.14.10 was deployed using the official installation assistant with the all-in-one installation option.
Installation commands:
```
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```
The deployment includes the Wazuh Manager, Indexer, Dashboard and Filebeat.

## 3. Service Verification

The status of the main Wazuh Services was verified using:

sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard

All three services were running successfully. Access to the Wazuh Dashboard was established through the Host-only network.

## 4. Windows Agent Deployment

The Windows agent was installed on the physical host using the MSI installer provided by Wazuh. The agent was configured with:

- Manager address: Wazuh server Host-only IP
- Agent name: Windows10Pro
- Agent group: default

The agent service was started using PowerShell:

NET START WazuhSvc

Agent connectivity was verified from the Wazuh server:

sudo /var/ossec/bin/agent_control -l

The Windows endpoint was successfully registered and displayed as active

## 5. File Integrity Monitoring Configuration

A dedicated directory was created on the Windows endpoint:

New-Item -ItemType Directory -Path C:\Wazuh-Lab -Force

The Wazuh agent configuration file was modified to enable real-time monitoring of this directory. Configuration file:

C:\Program Files (x86)\ossec-agent\ossec.conf

The following entry was added inside the syscheck section:

```xml
<directories realtime="yes">C:\Wazuh-Lab</directories>
```

The agent service was restarted to apply the configuration:

Restart-Service WazuhSvc

## 6. Windows Security Event Collection

The agent configuration was inspected to verify that the Windows Security event log was being monitored. Event collection was validated by generating a controlled authentication failure and locating the resulting alert in Wazuh.

## 7. Deployment Validation

| Validation | Result |
|---|---|
| Wazuh Manager running | Passed |
| Wazuh Indexer running | Passed |
| Wazuh Dashboard accessible | Passed |
| Windows agent connected | Passed |
| Windows security event collection | Passed |
| Real-time file integrity monitoring | Passed |
| Alert visualization in Dashboard | Passed |

## References

- [Wazuh Documentation](https://documentation.wazuh.com/current/)
- [Wazuh Quickstart](https://documentation.wazuh.com/current/quickstart.html)
