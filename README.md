# 🐝 Azure Honeypot & Sentinel SIEM Project

This project demonstrates how to deploy a Windows honeypot in Azure, collect and enrich security logs using Microsoft Sentinel, and visualize attacker data using a geolocation map. It's ideal for cybersecurity learning and practice in threat detection, log analysis, and SIEM operations. Below I have explained my steps in detail, in 5 parts on how I created and operated this project.

## Part 1: Create the Honey Pot (Azure VM)

1. Go to [Azure Portal](https://portal.azure.com) and search for **Virtual Machines**.
2. Create a new **Windows 10** virtual machine.
   - Choose a size appropriate for your subscription (Cyber Range has limits).
   - Take note of the **username and password**.
   - Be aware of the monthly cost if left on 24/7 — shut down when not in use or use Cyber Range.
3. Go to the **Network Security Group** of the VM.
   - Create a rule to **allow all inbound traffic**.
4. Log into the VM and **turn off Windows Firewall**:


## Part 2: Logging into the VM and Inspecting Logs

1. Attempt **3 failed logins** as a fake user (e.g., `employee`).
2. Log in as your admin user.
3. Open **Event Viewer**:
- Navigate to: `Windows Logs > Security`
- Look for **Event ID 4625** (failed login attempts).


## Part 3: Log Forwarding and KQL

1. Create a **Log Analytics Workspace (LAW)**.
2. Create a **Microsoft Sentinel** instance and connect it to your LAW.
3. Configure the **“Windows Security Events via AMA”** connector.
4. Create the **Data Collection Rule (DCR)** within Sentinel.
5. Query your logs with **KQL** (Kusto Query Language):
```kql
SecurityEvent
| where EventId == 4625
