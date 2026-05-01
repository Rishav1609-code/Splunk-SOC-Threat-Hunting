# 🛡️ Splunk SOC Incident Investigation & Threat Hunting

## 🔍 Overview
This repository documents an independent Security Operations Center (SOC) investigation using **Splunk**. The project simulates a real-world enterprise environment under attack. By ingesting, parsing, and analyzing server and authentication logs, I identified multiple suspicious activities, including brute-force login attempts, abnormal file access, port scanning, and active malware infections.

This project showcases practical expertise in **SIEM log analysis, Splunk Processing Language (SPL), threat intelligence correlation, and security visualization.**

## 🎯 Project Objectives
- Ingest and normalize raw security logs into Splunk.
- Perform proactive threat hunting to uncover hidden attack vectors.
- Identify Indicators of Compromise (IoCs) related to malware, credential stuffing, and unauthorized access.
- Develop custom dashboards and visual analytics to aid rapid incident response.
- Propose actionable remediation and mitigation strategies.

## 🛠️ Tools & Technologies Used
- **SIEM:** Splunk Cloud / Splunk Enterprise
- **Query Language:** Splunk Processing Language (SPL)
- **Log Types Analyzed:** Authentication logs, Network connection attempts, File access logs, Antivirus/Malware alerts.

---

## 🕵️‍♂️ Threat Hunting Methodology & Key Findings

### 1. Baseline Ingestion & Event Overview
Before hunting for anomalies, a baseline of all ingested logs was established to understand the volume and structure of the network traffic.
* **Query:** `source="SOC_Task2_Sample_Logs.txt"`
* **Observation:** Successfully parsed authentication, network, and file system events.

![Baseline Check](Screenshots/Screenshot%202025-09-02%20021723.png)

### 2. Investigating Brute-Force & Credential Stuffing
A high volume of failed login attempts was detected, indicating a potential brute-force or credential-stuffing attack against specific user accounts.
* **Query:** `source="SOC_Task2_Sample_Logs.txt" action="login failed"`

![Failed Logins](Screenshots/Screenshot%202025-09-02%20022207.png)

To isolate the targeted accounts, I aggregated the data to identify the top 5 users experiencing the most authentication failures.
* **Query:** `source="SOC_Task2_Sample_Logs.txt" action="login failed" | stats count by user | sort - count | head 5`

![Top Failed Users](Screenshots/Screenshot%202025-09-02%20023234.png)

### 3. Network Reconnaissance & Connection Anomalies
Analyzed raw connection attempts to identify potential port scanning or aggressive network reconnaissance from external/internal IPs.
* **Query:** `source="SOC_Task2_Sample_Logs.txt" action="connection attempt" | stats count by ip | sort - count | head 5`

![Connection Attempts](Screenshots/Screenshot%202025-09-02%20022534.png)
*Visualized top offending IP addresses to assist firewall blocking rules.*

### 4. Malware Outbreak Detection
The logs revealed critical alerts indicating active malware payloads attempting to execute or spread within the environment.
* **Query:** `source="SOC_Task2_Sample_Logs.txt" threat=*`

![Malware Detection](Screenshots/Screenshot%202025-09-02%20022851.png)

I further categorized the threats to prioritize incident response efforts, discovering instances of Trojans, Rootkits, and Ransomware behavior.
* **Query:** `source="SOC_Task2_Sample_Logs.txt" threat=* | stats count by threat`

![Malware Types](Screenshots/Screenshot%202025-09-02%20024033.png)

---

## 📊 Security Dashboards & Visualizations

To provide SOC analysts with situational awareness, I constructed a **Combined Log Activity Timechart**. This area chart tracks the timeline of network connections, file access, logins, and malware triggers over time, easily highlighting the exact attack window.

* **Query:** `source="SOC_Task2_Sample_Logs.txt" | timechart count by action`

![Combined Activity Chart](Screenshots/Screenshot%202025-09-02%20024003.png)

---

## 🛡️ Remediation & Recommendations
Based on the threat intelligence gathered during this Splunk investigation, I recommend the following security controls:

1. **Enforce Multi-Factor Authentication (MFA):** Implement MFA across all critical and administrative accounts to neutralize the risk of the observed brute-force/credential-stuffing attacks.
2. **Automated Account Lockouts:** Configure directory services to temporarily lock accounts after a threshold of failed login attempts.
3. **Endpoint Detection and Response (EDR):** The presence of Rootkits and Ransomware indicates that legacy Antivirus is insufficient. Deploy modern EDR agents to actively kill malicious processes.
4. **Network Segmentation & IP Blocking:** Blacklist the high-frequency offending IP addresses identified in the connection attempt logs at the perimeter firewall.
5. **Implement UEBA:** Utilize User and Entity Behavior Analytics to monitor abnormal file access patterns from compromised internal accounts (Insider Threat mitigation).

## 📂 Repository Structure
```text
├── Data/
│   └── SOC_Task2_Sample_Logs.txt       # Raw log data used for the investigation
├── Report/
│   └── SOC_Task_02_Report.pdf          # Detailed incident response documentation
├── Screenshots/                        # Splunk query proofs and visualizations
└── README.md                           # Project documentation
```

## ⚠️ Disclaimer
All security investigations, threat hunting exercises, and log analyses documented in this repository were conducted using simulated, intentionally generated sample datasets within a controlled environment. This project is strictly for educational, portfolio, and research purposes. No actual enterprise networks, live systems, or real user data were compromised or analyzed.

## 👨‍💻 Author
**Rishav Raj** *Cyber Security Researcher | SIEM & Incident Response Analyst*

---
👉 **Star ⭐ this repository if you found this SIEM investigation methodology helpful!**
