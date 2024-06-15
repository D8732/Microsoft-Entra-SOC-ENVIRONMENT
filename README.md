# AZURE-SOC-ENVIRONMENT# Building a SOC + Honeynet in Azure (Live Traffic)


## Introduction

In this project, I built a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Before](https://github.com/D8732/AZURE-SOC-ENVIRONMENT/assets/130248485/0b2b70f1-0117-40f4-a1aa-57a433cf36b9)


## Architecture After Hardening / Security Controls
![After](https://github.com/D8732/AZURE-SOC-ENVIRONMENT/assets/130248485/79e0f501-b457-4611-b708-eb0f4f7bb5f2)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows]![NSG-malicious-allowed-in](https://github.com/D8732/AZURE-SOC-ENVIRONMENT/assets/130248485/b2be2d49-6a97-4497-995b-46a242b5aa0e)
<br>
![Linux Syslog Auth Failures]![Linux-ssh-auth-fail](https://github.com/D8732/AZURE-SOC-ENVIRONMENT/assets/130248485/0efcf582-a6af-4d14-9eab-9f0b67717478)
<br>
!![windows-rdp-auth-fail](https://github.com/D8732/AZURE-SOC-ENVIRONMENT/assets/130248485/a5ba556d-56ae-48be-b969-b04ac1036637)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics were I measured in our insecure environment for 24 hours:
Start Time 6/10/2024, 7:11:44 PM
Stop Time 6/11/2024, 7:11:44 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent Windows VM's | 62200
| Syslog        Linux VM's  | 4312
| SecurityAlert  Microsoft defender for cloud          | 1
| SecurityIncident    Microsoft Sentinel     | 216
| AzureNetworkAnalytics_CL | 1370

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in my environment for another 24 hours, but after  applied security controls:
Start Time 6/13/2024, 11:53:13 PM
Stop Time	6/14/2024, 11:53:13 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent Windows VM's | 13758
| Syslog  Linux VM's       | 5
| SecurityAlert  Microsoft Defender for Cloud          | 0
| SecurityIncident  Microsoft Sentinel       | 0
| AzureNetworkAnalytics_CL | 16


In Microsoft Sentinel Incidents section showing process of open ticket For Possible Brute Force attempt which was deemed a true Positive, and possible steps for Remediation and Documenation
![Steps to remediate True Positive Incident](https://github.com/D8732/AZURE-SOC-ENVIRONMENT/assets/130248485/75b08d1f-564a-4b11-a519-e4953b07e32a)



## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
