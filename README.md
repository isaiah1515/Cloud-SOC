# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/isaiah1515/Cloud-SOC/assets/142617629/4e0b1f43-e741-4181-a582-0b3e17c344fe)


## Introduction

In this lab, I build a honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I gathered metrics for the insecure environment for 24 hours and after that I hardened the environment, applied security controls, and gathered metrics for another 24 hours. Here are some of the types so metrics that were gathered:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/isaiah1515/Cloud-SOC/assets/142617629/32658e6c-93e7-4615-894c-6ea4dca95b44)


## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/isaiah1515/Cloud-SOC/assets/142617629/eae97192-4b5d-48fb-b4a3-e0510b42a95c)

The architecture of the honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

The "BEFORE" metrics:
All resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open to any and all traffic. All other resources were deployed with public endpoints visible to the Internet.

The "AFTER" metrics:
Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this lab, a honeynet was built in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel then ingested the logs from a central repository which provided alerts, triggers and created incidents based off the data. After hardening the environment it’s clear that the number of security events has reduced significantly. This project was a perfect hands-on learning experience to help better understand a real-world SOC environment.

*Please note that it is more than likely that more security events, alerts, and incidents would have been generated in a larger, enterprise environment. 
