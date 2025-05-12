# KQL-and-SIEM-Logging-with-Azure-Honeypot# 🐝 Azure Honeypot & Sentinel SIEM Project

This project demonstrates how to deploy a Windows honeypot in Azure, collect and enrich security logs using Microsoft Sentinel, and visualize attacker data using a geolocation map. It's ideal for cybersecurity learning and practice in threat detection, log analysis, and SIEM operations.

---

## 🖼️ Project Architecture

![Azure Honeypot SOC Architecture](./Screen%20Shot%202025-05-11%20at%208.14.20%20PM.png)

This diagram shows how a publicly exposed honeypot VM receives attacks, logs are forwarded to Log Analytics, enriched via a GeoIP watchlist, and visualized in Microsoft Sentinel.

---

## 📌 Table of Contents

- [Part 2: Create the Honey Pot (Azure VM)](#part-2-create-the-honey-pot-azure-vm)
- [Part 3: Logging into the VM and Inspecting Logs](#part-3-logging-into-the-vm-and-inspecting-logs)
- [Part 4: Log Forwarding and KQL](#part-4-log-forwarding-and-kql)
- [Part 5: Log Enrichment and Finding Location Data](#part-5-log-enrichment-and-finding-location-data)
- [Part 6: Attack Map Creation](#part-6-attack-map-creation)
- [Resources](#resources)

---

## Part 2: Create the Honey Pot (Azure VM)
...
