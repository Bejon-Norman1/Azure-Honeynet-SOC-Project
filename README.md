
# Building a SOC + Honeynet in Azure (Live Traffic)
![Screenshot 2023-05-30 021705](https://github.com/ibmancodin23/Azure-Honeynet-SOC-Project/assets/19808403/85dc5588-8bde-45a0-b7f4-230116331359)



## Introduction

For this project, I constructed a miniature honeynet using Azure and established a Log Analytics workspace to gather log data from diverse sources. This workspace is utilized by Microsoft Sentinel to construct attack maps, activate alerts, and generate incidents. Initially, I assessed security metrics within an insecure environment for a duration of 24 hours. Subsequently, I implemented several security controls to fortify the environment, and measured the metrics for an additional 24-hour period. The outcomes of this evaluation are presented below, highlighting the following metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Objective
The central aim of this project was to create intentionally vulnerable virtual machines within the Azure infrastructure. This initiative served two purposes: first, to attract and analyze cyber attacks, providing valuable insights into the tactics and techniques utilized by attackers; second, to showcase my capability to swiftly and effectively respond to any identified issues.

## Regulations, and Azure Componets Deployed

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2x windows, 1x linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account for data storage
- Microsoft Sentinel
- Windows Remote Desktop to remotely connect to virtual machines
- Microsoft Defender Cloud  
- Command Line Interface (CLI)
- PowerShell 
- Visual Studio Code
- Kusto Query Language (KQL)
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
- [NIST SP 800-61 Revision 2](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final) 


## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

In the initial stage of the project, referred to as the <b>"BEFORE"</b> stage, the resources were deployed without any hardening measures or security controls in place. The purpose of this setup was to intentionally create an insecure environment to attract potential cyber attackers and study their tactics. During this stage:

- 1.) <b>Public Exposure:</b> All resources were initially deployed with direct public exposure to the internet. This means that they were accessible from any source on the internet without any restrictions.

- 2.) <b>Network Security Groups (NSGs) and Firewalls:</b> The Virtual Machines had their Network Security Groups (NSGs) and built-in firewalls configured in a permissive manner. This allowed unrestricted access to and from the Virtual Machines, without any limitations or filtering of network traffic.

- 3.) <b>Public Endpoints:</b> Other resources, such as storage accounts and databases, were deployed with public endpoints that were visible and accessible from the internet. Private Endpoints, which provide a more secure and private connection to these resources, were not utilized during this stage.

The purpose of this insecure setup was to actively observe and analyze potential cyber attacks, their techniques, and the impact on the deployed resources. This allowed for a comprehensive understanding of the vulnerabilities and risks present in the initial environment.



## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

In the subsequent phase, known as the <b>"AFTER"</b> stage, I implemented significant enhancements to strengthen the security posture of the environment. These improvements focused on robust hardening measures and stringent security controls. Here are the key highlights of the improvements:

- 1.) <b>Reinforced Network Security Groups (NSGs):</b> I bolstered the NSGs by implementing stringent access rules. This ensured that only trusted sources, such as specific authorized IP addresses, were allowed to connect to the virtual machines. By limiting access to known entities, I mitigated the risk of unauthorized entry and potential attacks.

- 2.) <b>Enhanced Built-in Firewalls:</b> I fine-tuned the built-in firewalls on the virtual machines to establish a resilient defense mechanism. Through meticulous configuration, I reduced the attack surface and fortified the overall security posture. These firewalls acted as effective barriers, preventing unauthorized access and protecting our resources from potential threats.

- 3.) <b>Secure Private Endpoints:</b> To elevate the security of critical Azure resources, I transitioned from public endpoints to secure Private Endpoints. This strategic shift ensured that access to sensitive resources, such as storage accounts and databases, was limited to our internal network. By isolating these resources from the public internet, I significantly reduced the exposure to potential attacks, enhancing the overall privacy and protection.

By conducting a thorough comparison of the security metrics before and after the implementation of these robust hardening measures and stringent security controls, I demonstrated the effectiveness of these enhancements in significantly fortifying the security posture of the Azure environment. These measures not only mitigate risks but also instill confidence in safeguarding valuable data and resources.




## Attack Maps Before Hardening / Security Controls
![(Before) nsg-malicious-allowed ](https://github.com/ibmancodin23/Azure-Honeynet-SOC-Project/assets/19808403/ec8ae96d-030f-4208-8eb8-ca9623d22b8c)


- The showcased attack map offers a visual depiction of the direct consequences that occur when the <b>Network Security Group (NSG)</b> is left unsecured, permitting unrestricted entry of malicious traffic. This compelling visualization strongly highlights the critical significance of implementing stringent security measures, including the enforcement of restrictions on NSG rules. Such measures play a pivotal role in proactively preventing unauthorized access and effectively minimizing potential threats to the system.


![(Before) syslog-ssh-auth-fail](https://github.com/ibmancodin23/Azure-Honeynet-SOC-Project/assets/19808403/706314f4-38ca-45c1-ad0c-c787538231a8)

- The highlighted attack map draws attention to a notable occurrence of <b>syslog authentication failures</b> observed on the deployed Linux server, which indicates unauthorized access attempts originating from external sources. This serves as a pivotal reminder of the indispensable need to prioritize the security of Linux servers by implementing robust authentication mechanisms and maintaining vigilant monitoring of system logs to promptly detect and respond to any signs of intrusion attempts.

![(Before) windows-rdp-smb-auth-fail](https://github.com/ibmancodin23/Azure-Honeynet-SOC-Project/assets/19808403/d5d6d41c-a4f9-47c3-bef4-7bf72d5e0d4e)

- This visual representation serves as a compelling reminder of the paramount importance of securing remote access and file sharing services. By implementing robust security measures in these areas, organizations can effectively mitigate the risk of unauthorized access and potential cyber threats, ensuring the protection of valuable data and resources.

By understanding the implications showcased in these attack maps, organizations can better appreciate the need for robust security measures to protect their networks, systems, and data from malicious actors.

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time   2023-05-24 08:48:43
Stop Time    2023-05-25 08:48:43

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 41042
| Syslog                   | 5439
| SecurityAlert            | 2
| SecurityIncident         | 219
| AzureNetworkAnalytics_CL | 412

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-05-25 19:22:57
Stop Time	2023-05-26 19:22:57

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 14127
| Syslog                   | 290
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
