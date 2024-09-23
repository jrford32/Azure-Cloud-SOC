# SOC + Honeynet in Azure Cloud
![Screenshot 2024-09-14 205840](https://github.com/user-attachments/assets/297486f1-c1ff-4e70-a26a-d8396e9aeff8)

## Introduction

In this project, I created a honeynet in Microsft Azure that shows live attack traffic from threat actors worldwide. The environment created was purposely left vulnerable to attack traffic for a period of 24 hours to monitor the numerous threats. It was then hardened the show the reduction in attacks and we will compare the before and after of the environment. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Kusto Query Language (KQL) Queries
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/user-attachments/assets/d32f46c1-7901-442a-b4da-81aa1c4d6d10)<br>
![Linux Syslog Auth Failures](https://github.com/user-attachments/assets/82b3d91c-7218-4f59-a47c-ab9fb5161e67)<br>
![Windows RDP/SMB Auth Failures](https://github.com/user-attachments/assets/cc1f729a-beaf-4c96-9525-f7033741c57d)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 9/18/2024 23:38:48
Stop Time 9/19/2024 23:38:48

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 77141
| Syslog                   | 8796
| SecurityAlert            | 2
| SecurityIncident         | 197
| AzureNetworkAnalytics_CL | 2222

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 9/21/2024 22:28:41
Stop Time	9/22/2024 22:28:41

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 21866
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
