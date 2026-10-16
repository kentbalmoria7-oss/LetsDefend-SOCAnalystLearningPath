<!-- ===================== HEADER ===================== -->
<div align="center">

# 📊 SOC138: Detected Suspicious Xls File

### LetsDefend SOC Analyst Learning Path · Macro-Enabled Excel Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Malicious+Excel+Macro+Investigation;Hash+Reputation+%26+Sandbox+Analysis;C2+Communication+Detected;Host+Containment+%26+Remediation" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Alert](https://img.shields.io/badge/Alert-SOC138-1E90FF?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![C2](https://img.shields.io/badge/C2-Accessed-DC2626?style=for-the-badge&logo=cloudflare&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![Hybrid Analysis](https://img.shields.io/badge/Hybrid%20Analysis-0F172A?style=flat-square&logo=crowdstrike&logoColor=00D4FF)
![Microsoft Excel](https://img.shields.io/badge/Microsoft%20Excel-217346?style=flat-square&logo=microsoftexcel&logoColor=white)
![EDR](https://img.shields.io/badge/EDR%20Containment-334155?style=flat-square&logo=crowdstrike&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/1c05f57380f7b4e12b03d5d85775b842eff77c01/Malware%20Analysis%20Investigation%20Picture/6/xls.jpeg?raw=true" alt="Excel file illustration for the LetsDefend lab" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Step 1: Create the Case and Take Ownership](#-step-1-create-the-case-and-take-ownership)
4. [Step 2: Gather Information](#-step-2-gather-information)
5. [Step 3: Analyze the File Hash](#-step-3-analyze-the-file-hash)
6. [Step 4: Check Log Management and the C2](#-step-4-check-log-management-and-the-c2)
7. [Step 5: Containment](#-step-5-containment)
8. [Step 6: Remediation](#-step-6-remediation)
9. [Step 7: IoCs, Analyst Note, and Close the Ticket](#-step-7-iocs-analyst-note-and-close-the-ticket)
10. [Decision Rationale](#-decision-rationale)
11. [Indicators of Compromise](#-indicators-of-compromise-iocs)
12. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
13. [Response Actions](#-response-actions)
14. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Detected Suspicious Xls File |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🆔 **Event ID** | 77 |
| 🕒 **Event Time** | Mar 13, 2021, 08:20 PM |
| 🚨 **Rule** | SOC138: Detected Suspicious Xls File |
| 💻 **Source Hostname** | Sofia |
| 🌐 **Source Address** | `172.16.17.56` |
| 📄 **File Name** | `ORDER SHEET & SPEC.xlsm` |
| 🧬 **File Hash (MD5)** | `7ccf88c0bbe3b29bf19d877c4596a8d4` |
| 📏 **File Size** | 2.66 MB |
| 🚦 **Device Action** | Allowed (the file was not blocked) |
| 🌐 **C2 IP** | `177.53.143.89` |
| 🔴 **C2 Accessed?** | Yes, Sofia's machine accessed the malicious IP |
| 🛑 **Response** | Host contained (disconnected from the network) |
| 🧰 **Tools Used** | LetsDefend SIEM, VirusTotal, Hybrid Analysis, Log Management, Endpoint Security |
| ⚖️ **Final Verdict** | 🚨 **True Positive** |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 📊 **Suspicious XLS and XLSM files:** these are a common vector for cyberattacks. They are often used to deliver banking trojans, download second-stage payloads (like `.exe` files through PowerShell), or connect to command and control (C2) servers.

### 📄 XLS vs. XLSM

| Format | Details |
| :--- | :--- |
| 🗂️ **`.xls`** | The older legacy format used by Excel versions from 1997 to 2003. It stores data in a binary format. |
| 🧩 **`.xlsm`** | A modern, XML-based format introduced in Excel 2007. The "M" stands for **macros**. |

### ⚠️ Why They Are Prone to Cybersecurity Issues

The main reason these files are flagged is their ability to run macros written in **Visual Basic for Applications (VBA)**.

| # | Reason | Explanation |
| :-: | :--- | :--- |
| 1️⃣ | **Embedded malicious code (macros)** | Macros automate repetitive tasks by running scripts, but attackers can write malicious VBA inside a spreadsheet. When a user opens the file and enables macros, the code runs automatically and can silently download malware, ransomware, or spyware. |
| 2️⃣ | **Administrative and system access** | VBA is powerful. A macro isn't limited to editing cells. It can run hidden commands through PowerShell or Command Prompt, modify the Windows Registry, and access, delete, or encrypt local files. |
| 3️⃣ | **Delivery method for phishing** | Criminals often send `.xls` and `.xlsm` files as email attachments disguised as urgent invoices, shipping documents, or financial statements, to trick victims into opening them and clicking **"Enable Content."** |
| 4️⃣ | **Legacy vulnerabilities in `.xls`** | The outdated `.xls` format lacks the modern security controls of newer file types, and security software has a harder time scanning the old binary format, so attackers can hide exploits in the file structure itself. |

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the case from the alert and take ownership. |
| **Method** | Start the playbook to begin the guided investigation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/279e46c0417e122e1ffc847c068cb02e4c90a7ff/Malware%20Analysis%20Investigation%20Picture/6/create%20case.png?raw=true" alt="Creating a case from the SIEM alert" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/279e46c0417e122e1ffc847c068cb02e4c90a7ff/Malware%20Analysis%20Investigation%20Picture/6/playbook.png?raw=true" alt="Starting the playbook for the case" width="85%" />
</div>

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Review the alert details. |
| **Method** | Record the key facts, then work out why the rule fired. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/8701d63b67494627b139adff9a56dc787e02cf7f/Malware%20Analysis%20Investigation%20Picture/6/gather%20info.png?raw=true" alt="SIEM alert details for SOC138" width="85%" />
</div>

### 🎯 Finding / Answer

| Field | Value |
| :--- | :--- |
| 🆔 Event ID | `77` |
| 🕒 Event time | Mar 13, 2021, 08:20 PM |
| 🚨 Rule | SOC138: Detected Suspicious Xls File |
| 🌐 Source address | `172.16.17.56` |
| 💻 Source hostname | `Sofia` |
| 📄 File name | `ORDER SHEET & SPEC.xlsm` |
| 🧬 File hash | `7ccf88c0bbe3b29bf19d877c4596a8d4` |
| 📏 File size | 2.66 MB |
| 🚦 Device action | Allowed |

### ⚡ Why the Alert Fired

The alert was triggered by the rule **SOC138: Detected Suspicious Xls File**, and the file that caused it is `ORDER SHEET & SPEC.xlsm`.

The `.xlsm` extension marks a Microsoft Excel **macro-enabled workbook**. Security systems commonly flag these because macros use VBA code, which attackers often abuse to run malicious scripts, download malware (such as ransomware or trojans), or set up a backdoor. The system detected this high-risk file type and logged its hash (`7ccf88c0bbe3b29bf19d877c4596a8d4`) for further investigation.

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: ANALYZE THE FILE HASH

| | |
| :--- | :--- |
| **Action** | Analyze the file through the MD5 hash provided in the alert. |
| **Method** | Search the hash in **VirusTotal** and **Hybrid Analysis**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/51b7b9e0b03f20593aa5a1a7ca541cdc3d91b146/Malware%20Analysis%20Investigation%20Picture/6/virus.png?raw=true" alt="VirusTotal results for the file hash" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/51b7b9e0b03f20593aa5a1a7ca541cdc3d91b146/Malware%20Analysis%20Investigation%20Picture/6/hybrid.png?raw=true" alt="Hybrid Analysis results for the file hash" width="85%" />
</div>

### 🎯 Finding / Answer

Both tools gave the same result: the file is indeed **malicious**.

| Question | Answer |
| :--- | :--- |
| Is the file malicious? | 🔴 **Yes, malicious** |

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: CHECK LOG MANAGEMENT AND THE C2

| | |
| :--- | :--- |
| **Action** | Check Log Management for connections to the file's command and control server. |
| **Method** | Search for the malicious IP and see whether the host reached it. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ac11031e3e88e4c6536a261f7b947fbfe102730d/Malware%20Analysis%20Investigation%20Picture/6/logs.png?raw=true" alt="Log Management showing the host connecting to the malicious IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ac11031e3e88e4c6536a261f7b947fbfe102730d/Malware%20Analysis%20Investigation%20Picture/6/c2.png?raw=true" alt="Playbook answer: the C2 was accessed" width="85%" />
</div>

### 🎯 Finding / Answer

Sofia's machine **accessed the malicious IP `177.53.143.89`**, so the playbook answer for "Check if someone requested the C2" is **Accessed**.

```text
Sofia (172.16.17.56)  →  177.53.143.89 (C2)
```

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: CONTAINMENT

| | |
| :--- | :--- |
| **Action** | Contain the compromised device. |
| **Method** | Disconnect it from the network through Endpoint Security (EDR). |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ac11031e3e88e4c6536a261f7b947fbfe102730d/Malware%20Analysis%20Investigation%20Picture/6/contain.png?raw=true" alt="Containing Sofia's machine through the EDR" width="85%" />
</div>

### 🎯 Finding / Answer

Because Sofia's machine was compromised, it is **mandatory to contain it** by disconnecting it from the network, to prevent further spread.

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: REMEDIATION

| Phase | Action | Details |
| :--- | :--- | :--- |
| 1️⃣ **Containment** | Isolate the endpoint | Immediately isolate Sofia's machine (`172.16.17.56`) from the network, to stop the malware from spreading to other devices or talking further with the C2 server (`177.53.143.89`). |
| 2️⃣ **Eradication and recovery** | Clean the host | Remove the malicious file (`ORDER SHEET & SPEC.xlsm`) and any dropped payloads or persistence mechanisms. Often the safest approach is to reimage the machine or revert it to a known clean backup from before the infection. |
| 3️⃣ **Credential reset** | Change passwords | Change the passwords for the user (Sofia) and any other accounts that were active on the compromised endpoint, since the malware may have stolen credentials. |
| 4️⃣ **Further investigation** | Check for data loss | Analyze the network logs to see whether any sensitive data was sent to the C2 address before the device was contained. |
| 5️⃣ **Preventive measures** | Harden defenses | Add robust email filtering to block suspicious attachments (like `.xlsm` files from untrusted sources), and make sure endpoint antivirus and EDR tools are fully up to date. |

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: IOCS, ANALYST NOTE, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Add the IoCs found during the investigation and write the analyst note. |
| **Method** | Finish the playbook and close the ticket as a true positive. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ac11031e3e88e4c6536a261f7b947fbfe102730d/Malware%20Analysis%20Investigation%20Picture/6/ioc.png?raw=true" alt="IoCs added to the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ac11031e3e88e4c6536a261f7b947fbfe102730d/Malware%20Analysis%20Investigation%20Picture/6/analyst.png?raw=true" alt="Analyst note written for the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ac11031e3e88e4c6536a261f7b947fbfe102730d/Malware%20Analysis%20Investigation%20Picture/6/closed.png?raw=true" alt="Closing the ticket in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE 🚨

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert is a **True Positive**: the macro-enabled Excel file `ORDER SHEET & SPEC.xlsm` is malicious, it was allowed to reach Sofia's machine, and that machine accessed the C2 address `177.53.143.89`. The host was contained and the remediation steps were defined.

---

<!-- ===================== DECISION RATIONALE ===================== -->
## ⚖️ Decision Rationale

| Check | Result | Impact on Verdict |
| :--- | :--- | :--- |
| 📄 **File type and name** | Macro-enabled `.xlsm` with an order and spec lure | 🔴 High-risk phishing format |
| 🧬 **Hash reputation** | VirusTotal and Hybrid Analysis both show it as malicious | 🔴 Confirmed malicious |
| 🚦 **Device action** | Allowed | 🔴 The file was not blocked |
| 🌐 **C2 communication** | The host accessed `177.53.143.89` | 🔴 Compromise confirmed |
| 🛑 **Containment** | Host disconnected from the network | 🟠 Required to stop spread |

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 📄 File name | `ORDER SHEET & SPEC.xlsm` | SIEM alert | 🔴 Malicious macro workbook |
| 🧬 MD5 | `7ccf88c0bbe3b29bf19d877c4596a8d4` | SIEM alert, VirusTotal, Hybrid Analysis | 🔴 Malicious |
| 🌐 C2 IP | `177.53.143.89` | Log Management | 🔴 Accessed by the infected host |
| 💻 Affected host | `Sofia` (`172.16.17.56`) | SIEM alert | 🟠 Compromised, contained |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

> ⚠️ **Source of this mapping:** these techniques are my mapping from the file type and the C2 connection in the logs. They are not copied from a sandbox report in your notes.

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Phishing: Spearphishing Attachment | [T1566.001](https://attack.mitre.org/techniques/T1566/001/) | A macro-enabled Excel file with an order and spec lure. |
| Execution | User Execution: Malicious File | [T1204.002](https://attack.mitre.org/techniques/T1204/002/) | The macro needs the user to open the file and enable content. |
| Execution | Command and Scripting Interpreter: Visual Basic | [T1059.005](https://attack.mitre.org/techniques/T1059/005/) | The workbook carries VBA macro code. |
| Command and Control | Application Layer Protocol | [T1071](https://attack.mitre.org/techniques/T1071/) | The host connected to the C2 address `177.53.143.89`. |

---

<!-- ===================== RESPONSE ===================== -->
## ✅ Response Actions

- [x] Created the case, took ownership, and started the playbook.
- [x] Recorded the alert details and explained why the rule fired.
- [x] Checked the file hash in VirusTotal and Hybrid Analysis.
- [x] Reviewed Log Management and confirmed the host accessed the C2 IP.
- [x] Contained Sofia's machine by disconnecting it from the network.
- [x] Added the IoCs and the analyst note, then closed the ticket as a true positive.
- [ ] Recommended follow-up: reimage the host and reset the credentials of the affected user.
- [ ] Recommended follow-up: block `177.53.143.89` and the file hash, and search SIEM and EDR logs for other hosts that contacted the C2.
- [ ] Recommended follow-up: check network logs for data sent to the C2 before containment.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Malware Triage](https://img.shields.io/badge/Malware%20Triage-1E90FF?style=for-the-badge&labelColor=0D1117)
![Hash Reputation Analysis](https://img.shields.io/badge/Hash%20Reputation%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![C2 Detection](https://img.shields.io/badge/C2%20Detection-0284C7?style=for-the-badge&labelColor=0D1117)
![Host Containment](https://img.shields.io/badge/Host%20Containment-334155?style=for-the-badge&labelColor=0D1117)
![Incident Remediation](https://img.shields.io/badge/Incident%20Remediation-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
