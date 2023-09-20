# Azure Honeynet and SOC with Real-World Cyber Attacks


## Objective

In this project, a honeynet was constructed using Microsoft Azure. The objective was to capture and analyze logs from all resources, and consolidate them into a Log Analytics workspace. Microsoft Sentinel was utilized as a SIEM and leverage these logs to develop attack maps, create alerts, and generate security incidents. Azure Sentinel measured insecure environment over a 24-hour period. After that, incidents were dealt with using NIST 800-61 as a guide.  Security controls were implemented to strengthen our virtual environment, using NIST 800-53 as a general guide. Another group of 24-hour metrics were gathered and the results obtained from the tables presented below. The metrics analyzed were:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)


## Technologies, Azure Components, and Regulations Employed
- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (1 Windows VM, 1 Linux VM)
- Log Analytics Workspace with KQL Queries
- Azure Key Vault for Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- PowerShell for Automation and Configuration Management
- NIST SP 800-53 Revision 5 for Security Controls
- NIST SP 800-61 Revision 2 for Incident Handling Guidance


## Architecture BEFORE Hardening and Implementing Security Controls
![Architecture Diagram](assets/diagrams/before_securing.png)
During the "BEFORE" stage of the project, and exposed to the public for malicious actors to discover. The intention for this stage was to attract bad actors and observe their attack patterns. To achieve this, A Windows virtual machine hosting a SQL database was deployed and the Windows and Linux servers both had their network security groups (NSGs) had high priority inbound rules added, allowing all traffic from all ports in any protocols. This is an incredibly dangerous thing to do, but it made for same enticing targets and some great metrics. To entice attackers even more, a storage account and key vault were deployed with public endpoints which were visible on the open internet. In this stage, the unsecured environment was monitored by Microsoft Sentinel using logs aggregated by the Log Analytics workspace.


## Architecture AFTER Hardening and Implementing Security Controls
![Architecture Diagram]()
During the "AFTER" stage of the project, the environment was hardened and security controls were implemented in order to satisfy NIST SP 800-53 Rev4 SC-7(3) Access Points. These hardening tactics included:
- <b>Network Security Groups (NSGs)</b>: NSGs were hardened by blocking all inbound and outbound traffic with the exception of designated public IP addresses that required access to the virtual machines. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

- <b>Built-in Firewalls</b>: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM which mitigated the attack surface bad actors had access to.

- <b>Private Endpoints</b>: To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that access to these sensitive resources was limited to the virtual network and not the public internet.


## Attack Maps Before Hardening / Security Controls
<b>This attack map shows the traffic allowed by a Network Security Group with all traffic allowed inbound</b>
![NSG Allowed Inbound Malicious Flows]()<br>

<b>This attack map shows all the attempts malicious actors attempting to access the Linux virtual machine</b>
![Linux Syslog Auth Failures]()<br>

 <b>This attack map shows all the attempts malicious actors attempting to access the Windows virtual machine</b>
![Windows RDP/SMB Auth Failures]()<br>


## Metrics Before Hardening / Security Controls
The following table shows the metrics measured within the unsecured environment for 24 hours:
Start Time 2023-06-16 23:48:28
Stop Time 2023-03-17 23:48:28

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15953
| Syslog                   | 3068
| SecurityAlert            | 52
| SecurityIncident         | 268
| AzureNetworkAnalytics_CL | 528


## Metrics After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

The following table shows the metrics measured in the environment for another 24 hours after applying security controls:
Start Time 2023-06-18 17:52:43
Stop Time	2023-08-19 17:52:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 600
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini, but effective, honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the unsecured environment before security controls were applied and after implementing security measures. After the implementation of more robust security controls, there was a 96% reduction in Windows Security Events, a 99% reduction in Linux Events, and a 100% reduction in security alerts, incidents, and malicious inbound network traffic.
