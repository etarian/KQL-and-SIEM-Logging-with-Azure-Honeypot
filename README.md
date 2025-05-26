# 🐝 Azure Honeypot & Sentinel SIEM Project

> Inspired by Josh Madakor’s YouTube tutorial on deploying a honeypot in Azure and integrating with Microsoft Sentinel for security analytics.

This project demonstrates how to:

- Deploy a Windows honeypot VM in Azure  
- Ingest and analyze logs using Microsoft Sentinel  
- Enrich logs with IP geolocation data  
- Visualize attacker data on an interactive map  

It’s ideal for hands-on cybersecurity practice in threat detection, log analysis, and SIEM workflows.

![Screenshot 2025-05-12 171012](https://github.com/user-attachments/assets/61766446-980a-47c3-9c2f-5a1da24ca304)


## 🧭 Project Overview

The project is divided into five parts:

---

### ⚙️ Part 1: Create the Honeypot (Azure VM)

1. Create a new **Windows 10 virtual machine** in Azure (choose an appropriate size).  
2. In the **Network Security Group**, add an **inbound rule to allow all traffic**.  
3. Disable the Windows firewall inside the VM:  
   - Open Run → `wf.msc` → Right-click "Windows Defender Firewall with Advanced Security" → Properties → Set all profiles to "Off".

![Screenshot 2025-05-11 230554](https://github.com/user-attachments/assets/a3b219c9-d0ac-49be-b1c9-93f3b564714a)


### 🔍 Part 2: Trigger and Inspect Logs

1. Attempt 3 failed logins using a fake user (e.g., `employee`).  
2. Successfully log into the VM.  
3. Open **Event Viewer** and inspect **Security Logs**.  
4. Look for Event ID `4625` – Failed logon attempts.

![Screenshot 2025-05-18 012830](https://github.com/user-attachments/assets/e1763dff-ae79-4c67-9cd6-298da3fe16d2)



### 📈 Part 3: Log Forwarding & KQL Queries

1. Create a **Log Analytics Workspace (LAW)**.  
2. Create a **Microsoft Sentinel** instance and connect it to the LAW.  
3. Configure the **"Windows Security Events via AMA"** data connector.  
4. Create a **Data Collection Rule (DCR)** and validate successful setup.  
5. Use **Kusto Query Language (KQL)** to query failed logins:

```kql
SecurityEvent
| where EventId == 4625
```
![Screenshot 2025-05-18 012743](https://github.com/user-attachments/assets/dcfb4f81-5e84-4d70-8442-4826802f04e9)

---

### 🌐 Part 4: Log Enrichment (Geolocation)

1. Observe that logs contain IP addresses but no geolocation.  
2. Import a **geolocation watchlist** (`geoip-summarized.csv`) to Sentinel.  
   - 📥 [Download here](https://raw.githubusercontent.com/joshmadakor1/lognpacific-public/refs/heads/main/misc/geoip-summarized.csv)  
3. Configure the watchlist:
   - **Name/Alias**: `geoip`  
   - **Source type**: Local File  
   - **Search Key**: `network`  
4. Enrich events with location using KQL:

```kql
let GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent
    | where IpAddress == "<attacker IP address>"
    | where EventID == 4625
    | order by TimeGenerated desc
    | evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network);
WindowsEvents
```

---

### 🗺️ Part 5: Create Attack Map

1. In Sentinel, create a new **Workbook**.  
2. Delete default elements and add a new **Query** element.  
3. Switch to the **Advanced Editor** and paste the JSON from `map.json`.  
4. Customize and observe the attack map with enriched IP data.

![Screenshot 2025-05-12 001731](https://github.com/user-attachments/assets/f41299c9-9c16-4bb2-93f2-88eeaabb3d5f)


## 📂 Repository Structure

```
azure-honeypot-sentinel/
├── assets/
│   └── map.json                  # JSON for attack map visualization in Sentinel Workbook
├── watchlists/
│   └── geoip-summarized.csv     # IP geolocation watchlist
├── README.md                    # Main documentation
└── LICENSE                      # (Optional) Add a license if open-sourcing
```

---

## 🙏 Acknowledgements

Special thanks to **Josh Madakor** for his YouTube tutorial and open resources that inspired and guided this project.

---

## 🛡 Disclaimer

This project is intended for educational purposes. Do not expose unsecured VMs or collect unauthorized data in production environments.

