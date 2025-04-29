# Azure SOC Home Lab - Honeypot + Sentinel SIEM

This project documents the creation of my first Home Lab using Microsoft Azure, focusing on cybersecurity learning through hands-on practice.

---

## Objective

To build a controlled vulnerable environment that simulates real-world attacks and integrate it with a SIEM for log collection and analysis.

---

## Lab Architecture

### 1. Azure Account Setup
- Created a Free Azure Account using the official link:  
  👉 [https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account](https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account)

### 2. Resource Group
- **Name**: `EW-SOC-Lab`
- **Region**: `East US`

### 3. Virtual Network
- **Name**: `Vnet-soc-lab`
- **Address Space**: `10.0.0.0/16`
- **Subnet**: `10.0.0.0/24`

### 4. Virtual Machine (Honeypot)
- **Name**: `CORP-NET-EAST-1`
- **Image**: `Windows 10 Pro`
- **Size**: `B1s`
- **Public IP**: Dynamic
- **Internal Firewall**: Disabled
- **NSG Rules**: Open to any traffic (Any → Any)


### 5. VM Network Configuration
- Associated the VM with the `Vnet-soc-lab` and `default` subnet.
- Created a new Public IP (`CORP-NET-EAST-1-ip`).
- Configured the Network Security Group (NSG) to allow all inbound traffic for simulation purposes.

### 6. Firewall Configuration
- Connected to the VM via RDP using the public IP.
- Disabled the Windows Defender Firewall on all profiles (Domain, Private, Public) via `wf.msc`.

---

## Validation Tests

- **Ping Test**: Successfully pinged the VM's public IP.
- **Failed Login Attempts**: Attempted 4 incorrect logins on purpose.
- **Event Viewer Logs**:  
  - Navigated to `Windows Logs > Security`.
  - Searched for Event ID `4625` (failed logon events).
  - Confirmed that the login failures were correctly logged.

---

## Log Collection Setup

- Created a **Log Analytics Workspace (LAW)**:
  - **Name**: `LAW-soc-lab-0001`

- Enabled **Microsoft Sentinel** on this workspace.
- Installed **Windows Security Events** from Content Hub.
- Configured an **Azure Monitor Agent (AMA)** Data Collection Rule:
  - **Rule Name**: `WSE-windows`
  - Collects Windows security logs.

---

## Log Analysis

- Accessed **Logs** in **KQL Mode**.
- Filtered and queried the `SecurityEvent` table.
- Example KQL query:

```kusto
SecurityEvent
| project TimeGenerated, Account, Computer, EventID, Activity, IpAddress
| top 10 by TimeGenerated desc
