# SOC Home Lab | Wazuh

## Project Overview

This project documents a home lab built to gain hands-on experience with Wazuh as a SIEM and endpoint security platform. The lab focuses on collecting Windows security events, monitoring file integrity and investigating alerts through the Wazuh Dashboard.

## Lab Environment

| Component | Details |
|---|---|
| SIEM | Wazuh 4.14.10 |
| Server | Ubuntu Server 24.04 LTS |
| Endpoint | Windows 10 Pro |
| Virtualization | Oracle VirtualBox |
| Network | NAT and Host-only |

## Objectives

- Deploy and configure a Wazuh server
- Connect and monitor a Windows endpoint
- Collect and analyze Windows security points
- Monitor file creation, modificaton and deletion
- Investigate alerts using the Wazuh Dashboard
- Document findings and supporting evidence

## Investigations

### Windows Authentication Failure

Generated a controlled failed login attempt and investigated the resulting alert

- Windows Event: 4625
- Wazuh Rule ID: 60122
- Alert Level: 5

Investigation report

## File Integrity Monitoring

Configured a real-time monitoring of a dedicated directory and verified the detection of file creation, modification and deletion.

| Activity | Rule ID | Alert Level |
|---|---|---|
| File creation | 554 | 5 |
| File modification | 550 | 7 |
| File deletion | 553 | 7 |

[Investigation report (Authentication)](reports/failed-logon-investigation.md)
[Investigation report (FIM)](reports/fim-investigation.md)

## Documentation

- [Lab Architecture](documentation/lab-architecture.md)
- [Deployment and Configuration](documentation/deployment.md)

## Skills demonstrated

- SIEM deployment and configuration
- Endpoint monitoring
- Windows security log analysis
- Alert triage
- File integrity monitoring
- Security event investigation
- Technical documentation

## Limitations

This project was conducted in a controlled home lab environment. The generated events represent simulated security scenarios rather than actual malicous activity.
