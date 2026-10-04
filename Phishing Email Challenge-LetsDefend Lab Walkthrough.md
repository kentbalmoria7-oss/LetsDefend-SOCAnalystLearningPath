<!-- ===================== HEADER ===================== -->
<div align="center">

# 🎣 Phishing Email Challenge

### LetsDefend SOC Analyst Learning Path · Lab Walkthrough

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=700&height=50&lines=Email+Header+Analysis;IoC+Extraction+%26+Threat+Intelligence;Verdict%3A+Confirmed+Phishing" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Email%20Analysis-1E90FF?style=for-the-badge&logo=maildotru&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-Phishing-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![Hybrid Analysis](https://img.shields.io/badge/Hybrid%20Analysis-0F172A?style=flat-square&logo=crowdstrike&logoColor=00D4FF)
![URLhaus](https://img.shields.io/badge/URLhaus-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![WHOIS](https://img.shields.io/badge/WHOIS-334155?style=flat-square&logo=internetarchive&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ef90cd22941fc47db9e830a4e1627073e9af46a9/Screenshot%202026-10-02%20050644.png?raw=true" alt="LetsDefend Phishing Email Challenge lab overview" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Overview](#-overview)
3. [Lab Objectives](#-lab-objectives)
4. [Scenario](#-scenario)
5. [Step 1: Check the Return-Path](#-step-1-check-the-return-path)
6. [Step 2: Extract and Verify the Destination URL](#-step-2-extract-and-verify-the-destination-url)
7. [Indicators of Compromise](#-indicators-of-compromise-iocs)
8. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
9. [Recommended Response Actions](#-recommended-response-actions)
10. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Phishing Email Challenge |
| 🏫 **Platform** | LetsDefend (SOC Analyst Learning Path) |
| 📨 **Lure Theme** | Urgent PayPal account matter |
| 🎯 **Target** | Email address exposed in a data breach |
| 🔎 **Key Artifact** | Return-Path (envelope sender) and embedded hyperlink |
| 🧰 **Tools Used** | Raw email source, VirusTotal, Hybrid Analysis |
| ⚖️ **Final Verdict** | 🚨 **Phishing (True Positive)** |

---

<!-- ===================== OVERVIEW ===================== -->
## 📖 Overview

Phishing email analysis involves safely isolating and inspecting suspicious emails, headers, and links to identify malicious activity without risking endpoint compromise.

---

<!-- ===================== OBJECTIVES ===================== -->
## 🎯 Lab Objectives

| # | Objective | Description |
| :-: | :--- | :--- |
| 1️⃣ | **Extract Core IoCs** | Inspect raw email headers, Return-Path data, and embedded hyperlinks to isolate key technical artifacts. |
| 2️⃣ | **Evaluate Threat Intelligence** | Run extracted domains, URLs, and file hashes through tools like VirusTotal, URLhaus, and WHOIS to assess reputation. |
| 3️⃣ | **Inspect Payloads Safely** | Preview linked landing pages using sandboxed or visual tools (e.g., URL2PNG, Hybrid Analysis) inside an isolated virtual machine. |
| 4️⃣ | **Determine Attack Verdict** | Synthesize findings to confirm whether the message is a legitimate notification or a malicious phishing campaign. |

---

<!-- ===================== SCENARIO ===================== -->
## 🕵️ Scenario

You received an email sent to an address exposed in a data breach, claiming to concern an urgent PayPal matter. The goal is to investigate and analyze the message to determine whether it is malicious.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dc15b4f01887be2c74eaa80ea217a76bd61cf215/1/paypal.png?raw=true" alt="Suspicious PayPal-themed email received" width="85%" />
</div>

> ⚠️ **Safety Note:** As a cybersecurity best practice, any suspicious emails, links, or file attachments should be inspected and detonated within an isolated sandbox environment to safely determine if they are malicious.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CHECK THE RETURN-PATH

| | |
| :--- | :--- |
| **Action** | View the raw source code of the email header. |
| **Method** | Use the search feature (`CTRL + F`) to search for `Return-Path`. |

> 💡 **What is the Return-Path?** The return path (also known as the envelope sender, `MAIL FROM` address, or reverse path) is a hidden email header that specifies where mail servers should send delivery failure notifications or bounced emails.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3d1e14828fe3a802cf3329c0c9be2d07af02b7ca/1/viewsource.png?raw=true" alt="Viewing the raw email source" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/427c0d7c0978b907497c564ef7a778c70be893cc/1/return%20path.png?raw=true" alt="Return-Path header located in the raw source" width="85%" />
</div>

### 🎯 Finding / Answer

```text
bounce@rjttznyzjjzydnillquh.designclub.uk.com
```

> 🔍 **Analyst Insight:** A legitimate PayPal message would not use an unrelated bounce address on a third-party domain. The mismatch between the claimed sender and the envelope sender is a strong red flag.

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: EXTRACT AND VERIFY THE DESTINATION URL

| | |
| :--- | :--- |
| **Action** | Copy the destination URL from the link provided in the email. |
| **Method** | Verify the domain name to ensure it routes to the correct destination, then check its reputation using **VirusTotal** and **Hybrid Analysis**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3f52bf4419c051e891242c7f49562c082e80ec19/1/Link%20location.png?raw=true" alt="Destination URL revealed by hovering over the link" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3f52bf4419c051e891242c7f49562c082e80ec19/1/Virustotal.png?raw=true" alt="VirusTotal reputation results for the URL" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3f52bf4419c051e891242c7f49562c082e80ec19/1/hyrbid.png?raw=true" alt="Hybrid Analysis report for the URL" width="85%" />
</div>

<br/>

<div align="center">

## 🚨 VERDICT: PHISHING 🚨

</div>

### 🎯 Finding / Answer

Based on the analysis, this email is confirmed to be a phishing attack containing a hyperlink that redirects to a malicious URL.

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator (Defanged) | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 📧 Envelope Sender | `bounce[@]rjttznyzjjzydnillquh[.]designclub[.]uk[.]com` | Email header (`Return-Path`) | 🔴 Suspicious / Malicious |
| 🌐 Sender Domain | `designclub[.]uk[.]com` | Email header (`Return-Path`) | 🔴 Unrelated to PayPal |
| 🔗 Embedded Link | Destination URL from the email body | Link inspection, VirusTotal, Hybrid Analysis | 🔴 Malicious |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Phishing: Spearphishing Link | [T1566.002](https://attack.mitre.org/techniques/T1566/002/) | Email contains a hyperlink leading to a malicious page. |
| Initial Access | Phishing | [T1566](https://attack.mitre.org/techniques/T1566/) | Message impersonates PayPal to create urgency. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Recommended Response Actions

- [x] Classify the alert as a **true positive** (phishing).
- [x] Document all IoCs (sender, domain, URL) for the case record.
- [ ] Block the sender domain and malicious URL at the email gateway and web proxy.
- [ ] Search mail logs for other recipients of the same campaign and remove the message from their mailboxes.
- [ ] Check proxy and DNS logs for any user who clicked the link, and reset credentials if so.
- [ ] Report the URL to threat intelligence and takedown services.
- [ ] Remind users to verify urgent account emails by going directly to the official website.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![Email Header Analysis](https://img.shields.io/badge/Email%20Header%20Analysis-0EA5E9?style=for-the-badge&labelColor=0D1117)
![IoC Extraction](https://img.shields.io/badge/IoC%20Extraction-1E90FF?style=for-the-badge&labelColor=0D1117)
![Threat Intelligence](https://img.shields.io/badge/Threat%20Intelligence-38BDF8?style=for-the-badge&labelColor=0D1117)
![URL Reputation Analysis](https://img.shields.io/badge/URL%20Reputation%20Analysis-0284C7?style=for-the-badge&labelColor=0D1117)
![Safe Sandbox Handling](https://img.shields.io/badge/Safe%20Sandbox%20Handling-334155?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-475569?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
