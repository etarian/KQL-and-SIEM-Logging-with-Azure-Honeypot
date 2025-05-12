# 🐝 Azure Honeypot & Sentinel SIEM Project

This project demonstrates how to deploy a Windows honeypot in Azure, collect and enrich security logs using Microsoft Sentinel, and visualize attacker data using a geolocation map. It's ideal for cybersecurity learning and practice in threat detection, log analysis, and SIEM operations. Below I have explained my steps in detail, in 5 parts on how I created and operated this project.
![Screenshot 2025-05-12 171012](https://github.com/user-attachments/assets/31689010-5c4d-4662-bbfd-c8e1ef28f80c)

## Part 1: Create the Honey Pot (Azure VM)

1. Go to [Azure Portal](https://portal.azure.com) and search for **Virtual Machines**.
2. Create a new **Windows 10** virtual machine.
3. Go to the **Network Security Group** of the VM.
   - Create a rule to **allow all inbound traffic**.
4. Log into the VM and **turn off Windows Firewall**:
![Screenshot 2025-05-11 230554](https://github.com/user-attachments/assets/7b12933d-ee16-47ec-a062-180f80e2aa95)


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

