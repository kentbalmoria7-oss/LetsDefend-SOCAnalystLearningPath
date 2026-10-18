<!-- ===================== HEADER ===================== -->
<div align="center">

# 🛡️ LetsDefend SOC Analyst Learning Path Portfolio

### Email · Web · Malware · Network Investigations · Hands-On Blue Team Write-Ups

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=SIEM+Alert+Triage+%26+Playbooks;Phishing%2C+Malware+%26+Web+Attack+Investigations;Wireshark+%26+PCAP+Network+Forensics;Evidence-Based+True+and+False+Positive+Verdicts" alt="Typing SVG" />

<br/>

![Focus](https://img.shields.io/badge/Focus-Blue%20Team%20%7C%20SOC-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Platform](https://img.shields.io/badge/Platform-LetsDefend-1E90FF?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Frameworks](https://img.shields.io/badge/Frameworks-MITRE%20ATT%26CK-C8102E?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Actively%20Growing-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

</div>

---

<!-- ===================== ABOUT ===================== -->
## 👋 About This Repository

This repository documents my progress through the **LetsDefend SOC Analyst Learning Path**, a hands-on program that simulates the daily work of a Security Operations Center (SOC) analyst.

Every investigation is written up step by step with screenshots, findings, indicators of compromise (IoCs), MITRE ATT&CK mapping, remediation notes, and a clear **true positive or false positive** verdict, so anyone can follow my reasoning.

> 💡 **Reading tip:** each write-up follows the same layout: case summary, key concepts, step-by-step investigation, decision rationale, IoCs, and skills demonstrated.

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [At a Glance](#-at-a-glance)
2. [Investigation Folders](#-investigation-folders)
3. [Email Investigation](#-email-investigation)
4. [Web Investigation](#-web-investigation)
5. [Malware Analysis Investigation](#-malware-analysis-investigation)
6. [Network Logs Investigation](#-network-logs-investigation)
7. [Cyber Threat Investigation](#-cyber-threat-investigation)
8. [How I Investigate](#-how-i-investigate)
9. [Core Competencies](#-core-competencies)
10. [Tools and Frameworks](#-tools-and-frameworks)

---

<!-- ===================== AT A GLANCE ===================== -->
## 📌 At a Glance

| 📂 Investigation Areas | 🧪 Write-Ups | 🛡️ Framework | 🧰 Core Tools |
| :---: | :---: | :---: | :---: |
| **5** | **28** | **MITRE ATT&CK** | **SIEM, EDR, VirusTotal, Wireshark, CyberChef, oletools** |

---

<!-- ===================== FOLDERS ===================== -->
## 📂 Investigation Folders

| Folder | What's Inside |
| :--- | :--- |
| [📧 **Email Investigation**](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/tree/main/Email%20Investigation) | Phishing emails, malicious attachments and URLs, header analysis, and endpoint follow-up. |
| [🌐 **Web Investigation**](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/tree/main/Web%20Investigation) | SQL injection, XSS, command injection, LFI, IDOR, and brute force. |
| [🦠 **Malware Analysis Investigation**](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/tree/main/Malware%20Analysis%20Investigation) | Malicious documents, ransomware, information stealers, and static and dynamic analysis. |
| [📡 **Network Logs Investigation**](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/tree/main/Network%20Logs%20Investigation) | Wireshark and PCAP forensics: Shellshock, port scanning, data exfiltration. |

---

<!-- ===================== EMAIL ===================== -->
## 📧 Email Investigation

[**Open the folder**](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/tree/main/Email%20Investigation)

| Lab | Focus | Verdict |
| :--- | :--- | :---: |
| **Phishing Email Challenge** | Email header and Return-Path analysis, URL reputation checks | 🚨 Phishing |
| **Malicious Attachment Detected** | SIEM playbook, attachment analysis, log correlation, EDR containment | 🚨 True Positive |
| **Phishing Mail Detected: Internal to Internal** | Internal email triage, IP
