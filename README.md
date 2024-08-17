# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC]![image](https://github.com/user-attachments/assets/61342e3e-1c30-4a20-ab65-f4d879e4b3e3)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)
## Architecture Before Hardening / Security Controls
![image](https://github.com/user-attachments/assets/968adad5-3d86-4980-8b65-050ba417aaf2)

The architecture of the mini honeynet in Azure consists of the following tools and components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Azure Key Vault
- Azure Storage Account
- Microsoft SQL Server
- SQL Server Management Studio (SSMS)
- Azure Active Directory

Additionally, the SOC utilized the following tools, components and regulations:

- Microsoft Sentinel (SIEM)
- Microsoft Defender for Cloud (MDC)
  - NIST SP 800-53 Revision 4
- Log Analytics Workspace
- Windows Event Viewer
- Kusto Query Language (KQL)
- PowerShell

To collect the metrics for the insecure environment, all resources were originally deployed, exposed to the public internet. The Virtual Machines had their Network Security Groups open (allowing all traffic) and built-in firewalls disabled. All other resources were deployed with endpoints visible to the public Internet.


## Architecture After Hardening / Security Controls
![image](https://github.com/user-attachments/assets/a59df3f6-f7c2-4b48-b8ea-ca75ecdae6d7)

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
[NSG Allowed Inbound Malicious Flows]![image](https://github.com/user-attachments/assets/91046859-8270-4278-b694-f3cbc266893d)<br>
[Linux Syslog Auth Failures]![image](https://github.com/user-attachments/assets/b7d3e2e2-e548-4dc2-965d-166d210ac63a)<br>
[Windows RDP/SMB Auth Failures]![image](https://github.com/user-attachments/assets/a25dfa9d-2fac-48b6-9ab1-fd3765413114)<br>
[mmsql-auth-fail]![image](https://github.com/user-attachments/assets/de69b298-133e-4c76-97f1-ecaffe11851a)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 8/12/2024 19:56:05

Stop Time 8/13/2024 19:56:05

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 56319
| Syslog                   | 6688
| SecurityAlert            | 10
| SecurityIncident         | 318
| AzureNetworkAnalytics_CL | 516

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 8/15/2024, 8:56:14 PM
Stop Time	8/16/2024, 8:56:14 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 14054
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

<b> Impact of Security Controls <b/>

| Metric                     |                  Change post-hardening 
|--------------------------- | -------------------------------
|Security Events (Windows VMs)                  |	-75.05%
|Syslog (Linux VMs)	                            | -99.93%
|SecurityAlert (Microsoft Defender for Cloud)	  | -100%
|Security Incident (Sentinel Incidents)	        | -100%
|NSG Inbound Malicious Flows Allowed	          | -100%
## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
