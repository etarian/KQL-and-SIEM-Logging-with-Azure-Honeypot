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

Part 4. Log Enrichment and Finding Location Data

Observe the SecurityEvent logs in the Log Analytics Workspace; there is no location data, only IP address, which we can use to derive the location data.

We are going to import a spreadsheet (as a “Sentinel Watchlist”) which contains geographic information for each block of IP addresses.

Download: geoip-summarized.csv

Within Sentinel, create the watchlist:

Name/Alias: geoip
Source type: Local File
Number of lines before row: 0
Search Key: network

Allow the watchlist to fully import, there should be a total of roughly 54,000 rows.

In real life, this location data would come from a live source or it would be updated automatically on the back end by your service provider.

(observe architecture)

Observe the logs now have geographic information, so you can see where the attacks are coming from

let GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent
    | where IpAddress == <attacker IP address>
    | where EventID == 4625
    | order by TimeGenerated desc
    | evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network);
WindowsEvents

Part 5. Attack Map Creation

Within Sentine, create a new Workbook

Delete the prepopulated elements and add a “Query” element

Go to the advanced editor tab, and paste the JSON

Workbook (Attack map):
map.json

Observe the query
Observe the map settings
Observe the map
![Screenshot 2025-05-12 001731](https://github.com/user-attachments/assets/0056ac56-b164-4577-839d-fd86763d5b38)




