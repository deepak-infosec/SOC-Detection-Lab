
# Project 02 - Port Scan Detection using Splunk

## Overview

This project demonstrates the detection of network reconnaissance activity by capturing traffic generated during an Nmap TCP SYN port scan. Network packets were captured using tcpdump, converted into a text-based log, imported into Splunk Enterprise, and analyzed using SPL queries. A dashboard and scheduled alert were created to visualize and monitor scan activity.

---

## Objectives

- Perform an Nmap TCP SYN port scan
- Capture network traffic using tcpdump
- Convert PCAP into a Splunk-compatible log
- Import network logs into Splunk Enterprise
- Analyze reconnaissance traffic using SPL
- Create a dashboard for visualization
- Configure an alert for monitoring

---

## Lab Environment

| Component | Technology |
|-----------|------------|
| Attacker | Kali Linux |
| Victim | Ubuntu Linux |
| SIEM | Splunk Enterprise 10.4.1 |
| Network Capture | tcpdump |
| Scanner | Nmap |
| Virtualization | VirtualBox |

---

# Attack Scenario

This project simulates a reconnaissance attack where an attacker performs an Nmap TCP SYN scan against a Linux target. The generated network traffic is captured using tcpdump, converted into a text log, imported into Splunk Enterprise, and analyzed using SPL queries to identify scan activity.

---

# Implementation Steps

## Step 1 - Perform Nmap Port Scan

A TCP SYN scan was launched from the attacker machine.

```bash
nmap -sS <Target-IP>
```

### Screenshot

![Nmap Scan](screenshots/01-nmap-scan.png)

---

## Step 2 - Capture Network Traffic

Traffic generated during the scan was captured using tcpdump.

```bash
sudo tcpdump -i any -nn -w ~/portscan.pcap
```

Stop packet capture.

```text
Ctrl + C
```

### Screenshot

![Packet Capture](screenshots/02-tcpdump-capture.png)

---

## Step 3 - Convert PCAP into Text Log

Since Splunk Enterprise does not natively support importing PCAP files, the packet capture was converted into a readable text log.

```bash
sudo tcpdump -nn -r ~/portscan.pcap | tee ~/portscan.log > /dev/null
```

---

## Step 4 - Import Log into Splunk

The generated log file was uploaded into Splunk Enterprise using the **Add Data** feature.

### Screenshot

![Splunk Search](screenshots/03-splunk-log-search.png)

---

## Step 5 - Create Dashboard

A dashboard was created to visualize imported network traffic.

### Dashboard Query

```spl
source="portscan.log"
| stats count AS "Captured Packets"
```

Visualization

```
Single Value
```

### Screenshot

![Dashboard](screenshots/04-dashboard.png)

---

## Step 6 - Configure Alert

A scheduled alert was configured to monitor imported network traffic.

### Alert Query

```spl
source="portscan.log"
| stats count
```

### Alert Settings

```
Name:
Port Scan Detection

Type:
Scheduled

Frequency:
Every Hour

Trigger:
Number of Results > 0

Action:
Add to Triggered Alerts
```

### Screenshot

![Alert](screenshots/05-alert.png)

---

# Splunk Queries

## Display Imported Log

```spl
source="portscan.log"
```

---

## Count Captured Packets

```spl
source="portscan.log"
| stats count
```

---

## Search SYN Packets

```spl
source="portscan.log" "Flags [S]"
```

---

## Search Destination Port

```spl
source="portscan.log" "22"
```

---

## Search Target IP

```spl
source="portscan.log" "10.0.2.15"
```

---

# Detection Workflow

```
Attacker
      │
      ▼
Nmap TCP SYN Scan
      │
      ▼
tcpdump Packet Capture
      │
      ▼
Convert PCAP to Text Log
      │
      ▼
Import into Splunk
      │
      ▼
SPL Analysis
      │
      ▼
Dashboard
      │
      ▼
Alert
```

---

# Skills Demonstrated

- Network Reconnaissance Detection
- Nmap Port Scanning
- Packet Capture using tcpdump
- Network Log Analysis
- Splunk Log Ingestion
- SPL Query Development
- Dashboard Creation
- Alert Configuration
- SIEM Investigation

---

# Lessons Learned

- Captured live network traffic using tcpdump.
- Converted packet captures into Splunk-compatible logs.
- Imported network logs into Splunk Enterprise.
- Developed SPL queries to identify reconnaissance activity.
- Built dashboards for network monitoring.
- Configured scheduled alerts for basic detection.

---

# Disclaimer

This project was conducted in a controlled virtual laboratory for educational and defensive cybersecurity purposes only.
