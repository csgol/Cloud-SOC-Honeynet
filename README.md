# Building a SOC + Honeynet in Azure (Live Traffic)
![Untitled Diagram drawio](https://github.com/user-attachments/assets/4b32c70b-7aad-4b01-b6e1-b2a9250860e3)


## Introduction

In this project, I created a mini honeynet in Azure and integrated log sources from various resources into a Log Analytics workspace. This workspace was then used by Microsoft Sentinel to generate attack maps, trigger alerts, and create incidents. I recorded specific security metrics in an unsecured environment for 24 hours, applied security controls to enhance the environment, measured the metrics again for another 24 hours, and presented the results below. The metrics include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Public internet drawio](https://github.com/user-attachments/assets/55da37de-ef17-4d92-b2d4-e71912709278)

## Architecture After Hardening / Security Controls
![After drawio](https://github.com/user-attachments/assets/163657b4-a13a-4a83-920f-6f253bf2ccba)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were initially deployed with exposure to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls fully open, and all other resources were deployed with public endpoints accessible from the internet, meaning Private Endpoints were not utilized.

For the "AFTER" metrics, Network Security Groups were tightened by blocking all traffic except from my admin workstation, and all other resources were secured using their built-in firewalls along with Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![nsg-malicious-allowed-in-Before](https://imgur.com/EELwaQQ.png)<br>
![linux-ssh-auth-fail -Before](https://imgur.com/wp8pdEA.png)<br>
![windows-rdp-auth-fail Before](https://imgur.com/ZRKrU8K.png)<br>
![mssql-auth-fail-Before](https://imgur.com/FmNc6Hm.png)


## Metrics Before Hardening / Security Controls

The table below displays the metrics we measured in our insecure environment over a 24-hour period:
Start Time 2024-07-30 17:06:49
Stop Time 2024-07-31 17:06:49

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 63728
| Syslog                   | 8589
| SecurityAlert            | 7
| SecurityIncident         | 194
| AzureNetworkAnalytics_CL | 2942

## Attack Maps Before Hardening / Security Controls

```All map queries returned no results due to the absence of malicious activity during the 24-hour period following the hardening.```

## Metrics After Hardening / Security Controls

The table below displays the metrics we measured in our environment over another 24-hour period, this time after applying security controls:
Start Time 2024-08-11 15:51:13
Stop Time	2024-08-12 15:51:13

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 16107
| Syslog                   | 11
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was set up in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Metrics were measured in the insecure environment before applying security controls and then again after implementing security measures. The results showed a significant reduction in security events and incidents after the security controls were applied, demonstrating their effectiveness.

It is important to note that if the network resources had been heavily utilized by regular users, more security events and alerts might have been generated during the 24-hour period following the implementation of the security controls.
