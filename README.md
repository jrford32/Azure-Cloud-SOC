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

When our resources were created, they were purposely exposed to the internet by leaving the network security groups and built-in firewalls for the virtual machines wide open. The remaining resources were deployed with public endpoints visible to the Internet.

To harden the resources we created, Network Security Groups were updated by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

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

## Attack Maps After Hardening / Security Controls

Map queries returned no results of malicious activity for the 24 hour period after hardening.

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

To summarize this project, a mini honeynet was set up in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents from the logs. We measured key metrics in the unsecured environment before applying security controls, then compared them after implementation. The results showed a significant drop in security events and incidents, highlighting the effectiveness of the security measures.

It's important to note that if the network resources had been heavily used by regular users, more security events and alerts might have been triggered in the 24-hour window following the implementation of the security controls.
