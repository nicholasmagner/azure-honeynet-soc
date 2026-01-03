# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I built a mini honeynet in Azure and ingested logs from multiple resources into a Log Analytics workspace. Microsoft Sentinel was then used to generate attack maps, trigger alerts, and create incidents from this data. Security metrics were collected over a 24-hour period in an unsecured environment, after which security controls were applied to harden the environment. Metrics were then collected for an additional 24 hours, and the results are presented below. The metrics shown include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The mini honeynet architecture in Azure is made up of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the “BEFORE” metrics, all resources were initially deployed with direct exposure to the internet. The virtual machines had both Network Security Groups and built-in firewalls fully open, and all other resources were configured with publicly accessible endpoints, meaning Private Endpoints were not used.

For the “AFTER” metrics, Network Security Groups were hardened to block all traffic except connections from my administrative workstation, and all other resources were secured using their built-in firewalls along with Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![nsg-malicious-allowed-in](https://github.com/EricMcclellan1/Cloud-Soc/assets/147299619/514381d8-b011-4b72-9179-970d2cdd33c2)<br>
![linux-ssh-auth-fail](https://github.com/EricMcclellan1/Cloud-Soc/assets/147299619/eedfc6b4-062b-41b0-9f72-42c8355eae3e)<br>
![windows-rdp-auth-fail](https://github.com/EricMcclellan1/Cloud-Soc/assets/147299619/fe9ef65d-1d41-4117-9e4c-dbbc0e4a5e75)<br>
![mssql-auth-fail](https://github.com/EricMcclellan1/Cloud-Soc/assets/147299619/df7e73d2-8740-426a-879d-574a2ecdfbc0)<br>



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2025-12-28 18:03:27
Stop Time 2025-12-28 18:03:27

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 51632
| Syslog                   | 5789
| SecurityAlert            | 41
| SecurityIncident         | 336
| AzureNetworkAnalytics_CL | 4473

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2025-12-30 04:46:53
Stop Time	2025-12-30 03:46:53

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9548
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

This project involved building a small honeynet in Microsoft Azure and connecting multiple log sources to a Log Analytics workspace. Microsoft Sentinel was used to generate alerts and incidents from the collected data. Security metrics were recorded in an unsecured state and then measured again after security controls were implemented. Following these changes, security events and incidents dropped significantly, clearly showing the impact and effectiveness of the controls.

It is important to note that if the network resources had been heavily used by normal users, a higher number of security events and alerts would likely have been generated during the 24-hour period after the security controls were implemented.
