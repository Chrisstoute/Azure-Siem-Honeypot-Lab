<div align="center">

# 🚨 Azure SIEM Honeypot Project

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f2027,50:203a43,100:2c5364&height=180&section=header&text=Azure%20SIEM%20Honeypot%20Lab&fontSize=40&fontColor=ffffff&animation=fadeIn&fontAlignY=35" />

<div align="center">

<table>
<tr>
<td align="center">

<h3>🛡️ Cyber Range Scenario Credit 🛡️</h3>

<strong>This attack scenario was provided by Josh Madakor, CEO of The Cyber Range.</strong>

<br><br>

<a href="https://www.skool.com/cyber-range">
  <img src="https://img.shields.io/badge/JOIN%20THE%20CYBER%20RANGE-CLICK%20HERE-red?style=for-the-badge&labelColor=000000&color=ff0000" alt="Join The Cyber Range">
</a>

</td>
</tr>
</table>

</div>

### Microsoft Azure • Microsoft Sentinel • Log Analytics • KQL • Threat Detection

</div>

---

# 📖 Project Overview

This project demonstrates the deployment of a cloud-based honeypot environment in Microsoft Azure to capture, analyze, and visualize real-world attack traffic.

A Windows virtual machine was intentionally exposed to the internet to attract unauthorized login attempts. Security events were collected using Azure Log Analytics Workspace and analyzed through Microsoft Sentinel using KQL queries.

The project simulates core SOC analyst workflows, including:

- Cloud infrastructure deployment
- Honeypot configuration
- Security log ingestion
- Failed login analysis
- KQL-based investigation
- GeoIP enrichment
- Attack map visualization

---

# 🧰 Technologies Used

- Microsoft Azure
- Microsoft Sentinel SIEM
- Log Analytics Workspace
- Windows Virtual Machine
- Azure Network Security Groups (NSGs)
- Windows Security Event Logs
- KQL / Kusto Query Language
- GeoIP Enrichment
- Attack Map Visualization

---

# 🏗️ Lab Architecture

```text
Internet
   |
   v
Azure Network Security Group
   |
   v
Windows Honeypot VM
   |
   v
Azure Monitor Agent
   |
   v
Log Analytics Workspace
   |
   v
Microsoft Sentinel
   |
   v
KQL Queries + Attack Map Dashboard
```

---

# 🎯 Lab Objectives

- Deploy a Windows VM in Microsoft Azure
- Configure the VM as an exposed honeypot
- Allow inbound traffic through NSG rules
- Collect Windows Security Event Logs
- Detect failed login attempts using Event ID 4625
- Analyze attacker IP addresses with KQL
- Enrich logs with geographic location data
- Visualize attack activity using a custom map dashboard

---

# 🔹 Phase 1: Azure Environment Setup

A resource group, virtual network, and Windows virtual machine were created in Microsoft Azure to serve as the foundation for the honeypot lab.

![Resource Group](screenshots/initial-setup/1-creating-resource-group.jpg)

![Virtual Network](screenshots/initial-setup/2-creating-virtual-network.jpg)

![VNet in Resource Group](screenshots/initial-setup/3-virtual-network-inside-RG.jpg)

![Virtual Machine Creation](screenshots/initial-setup/4-creating-virtual-machine.jpg)

![Verification](screenshots/initial-setup/5-verification.jpg)

---

# 🔹 Phase 2: Network Security Group Configuration

The Azure Network Security Group was configured to allow inbound traffic to the virtual machine. This intentionally increased exposure so the honeypot could attract real-world unauthorized access attempts.

> Note: This configuration was used strictly for lab and monitoring purposes. In production environments, inbound access should be restricted using least privilege principles.

![Selecting NSG](screenshots/creating-network-security-group/1.%20Selecting%20NSG.jpg)

![Opening NSG](screenshots/creating-network-security-group/2.%20Opening%20up%20NSG%20to%20the%20Public.jpg)

![Inbound Rule](screenshots/creating-network-security-group/3.%20Creating%20new%20Inbound%20Security%20Rule.jpg)

![Rule Parameters](screenshots/creating-network-security-group/4.%20Setting%20Rule%20Parameters.jpg)

![New Rule](screenshots/creating-network-security-group/5.%20New%20Security%20Rule.jpg)

---

# 🔹 Phase 3: Windows Firewall Configuration

Windows Firewall was disabled inside the virtual machine to ensure inbound traffic could reach the honeypot. This increased visibility into unsolicited connection attempts and failed authentication activity.

![Retrieve IP](screenshots/disabling-windows-fw-on-vm/1.%20Retrieving%20IP%20Address.jpg)

![RDP](screenshots/disabling-windows-fw-on-vm/2.%20RDP%20into%20VM.jpg)

![Disable Firewall](screenshots/disabling-windows-fw-on-vm/3.%20Disabling%20Firewall.jpg)

![Ping](screenshots/disabling-windows-fw-on-vm/4.%20Pinging%20VM.jpg)

![Checking Logs](screenshots/disabling-windows-fw-on-vm/5.%20Checking%20for%20Failed%20Login%20Attempts.jpg)

![Failed Login](screenshots/disabling-windows-fw-on-vm/6.%20Failed%20Login%20Attempt.jpg)

---

# 🔹 Phase 4: Log Collection & Microsoft Sentinel Integration

A Log Analytics Workspace was created to collect Windows Security logs from the honeypot VM. Microsoft Sentinel was then connected to the workspace to provide SIEM functionality.

![Create LAW](screenshots/creating-log-repository/1.%20Creating%20LAW.jpg)

![Create SIEM](screenshots/creating-log-repository/2.%20Creating%20SIEM.jpg)

![Attach LAW](screenshots/creating-log-repository/3.%20Attaching%20LAW%20to%20SIEM.jpg)

![Architecture](screenshots/creating-log-repository/4.%20Current%20Architecture.jpg)

![Connect VM](screenshots/creating-log-repository/5.%20Connecting%20VM%20to%20Log%20Repository%20via%20Windows%20Security%20Events%20AMA.jpg)

![AMA Extension](screenshots/creating-log-repository/6.%20AMA%20Connector%20Added%20to%20VM%20Extension.jpg)

---

# 🔹 Phase 5: KQL Investigation

KQL was used to query failed authentication attempts from Windows Security Event Logs.

The primary event analyzed was:

```text
Event ID 4625 — Failed Logon Attempt
```

Example KQL query:

```kql
SecurityEvent
| where EventID == 4625
| summarize FailureCount = count() by IpAddress
| order by FailureCount desc
```

This query identifies source IP addresses generating failed login attempts and ranks them by frequency.

![KQL Query](screenshots/kql-queries/1.%20Security%20Event%20KQL%20Query.jpg)

![1400 Failed Logins](screenshots/kql-queries/2.%201400%20Failed%20Login%20Attempts.jpg)

![Filtered Search](screenshots/kql-queries/3.%20Filtered%20Search%20Query.jpg)

![Geolocation Netherlands](screenshots/kql-queries/4.%20Geolocation%20-%20Netherlands.jpg)

![6300 Login Attempts](screenshots/kql-queries/5.%206300%20Login%20Attempts.jpg)

---

# 🔹 Phase 6: GeoIP Enrichment & Attack Map Visualization

Attacker IP addresses were enriched with geographic location data to identify the source countries and regions associated with failed login activity.

A custom attack map was created to visualize global threat activity targeting the honeypot.

![Attack Map](screenshots/kql-queries/6.%20Attack%20Map%20After%2018%20Hours.jpg)

---

# 📊 Results

During the lab, the honeypot captured real-world unauthorized access attempts from the public internet.

Key results included:

- Captured real failed login attempts
- Identified attacker source IP addresses
- Enriched attacker IP data with geographic location
- Visualized global attack activity on a custom dashboard
- Observed over 6,000 failed login attempts within approximately 18 hours

---

# 🧠 Lessons Learned

This project reinforced several core SOC analyst concepts:

- Publicly exposed systems are quickly discovered by attackers
- Failed login events are valuable indicators of brute-force activity
- Event ID 4625 is critical for Windows authentication monitoring
- SIEM tools help centralize and analyze security telemetry
- KQL enables fast investigation of large log datasets
- GeoIP enrichment improves situational awareness
- Cloud security requires careful NSG and firewall configuration

---

# 💼 Skills Demonstrated

## SIEM & Detection Engineering
- Microsoft Sentinel configuration
- Security event collection
- Log Analytics Workspace setup
- KQL querying
- Failed login detection
- Threat visualization

## Cloud Security
- Microsoft Azure VM deployment
- Network Security Group configuration
- Public exposure risk analysis
- Azure Monitor Agent configuration

## SOC Analyst Skills
- Windows Event Log analysis
- Brute-force detection
- Source IP investigation
- GeoIP enrichment
- Dashboard creation
- Security monitoring workflow

---

# 🚀 Final Takeaway

This project demonstrates how a SOC analyst can use Microsoft Azure, Microsoft Sentinel, and KQL to collect, analyze, and visualize real-world attack traffic.

The lab highlights the importance of centralized logging, cloud security monitoring, and investigative querying in modern security operations.
