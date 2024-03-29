# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/NebiyeYilmaz/Cloud-SOC/assets/156943652/6bae3b28-eabd-4f7d-9c38-fd77f192a399)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/NebiyeYilmaz/Cloud-SOC/assets/156943652/4eeef93b-fa89-486b-9ecc-59509026cabd)

## Architecture After Hardening / Security Controls
![image](https://github.com/NebiyeYilmaz/Cloud-SOC/assets/156943652/6ff2b74c-55b7-4fc8-a685-9dd8da59d9ea)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![(before) nsg-malicious-allowed-in](https://github.com/NebiyeYilmaz/Cloud-SOC/assets/156943652/4a8842cb-73d4-4982-b29a-bad5b1d9e593)
<br>
![(before) linux-ssh-auth-fail](https://github.com/NebiyeYilmaz/Cloud-SOC/assets/156943652/7f10f588-bc08-4e21-b31e-06eb3edbb7da)
<br>
![(before) windows-rdp-auth-fail](https://github.com/NebiyeYilmaz/Cloud-SOC/assets/156943652/c5f71138-b775-4564-bc49-12d2e02a0f7a)
<br>
![(before) mssql-auth-fail](https://github.com/NebiyeYilmaz/Cloud-SOC/assets/156943652/50c10c72-ed13-4657-9469-477dbd460045)
<br>
## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-01-03 14:10:48
Stop Time 2024-01-04 14:10:48

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17032
| Syslog                   | 681
| SecurityAlert            | 10
| SecurityIncident         | 203
| AzureNetworkAnalytics_CL | 86366

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

![image](https://github.com/NebiyeYilmaz/Cloud-SOC/assets/156943652/532ea3c1-1e87-4a6c-a7ae-11b83cb7eeba)

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-01-12 22:15:18
Stop Time	2024-01-13 22:15:18

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 6525
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
