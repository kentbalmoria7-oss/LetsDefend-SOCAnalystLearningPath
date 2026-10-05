<!-- ===================== HEADER ===================== -->
<div align="center">

# 🔗 SOC141: Phishing URL Detected

### LetsDefend SOC Analyst Learning Path · SIEM Alert Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Phishing+URL+Alert+Triage;Endpoint+Terminal+%26+Command+Line+Analysis;Malicious+Executable+Detection;Host+Containment+%26+Remediation;Verdict%3A+True+Positive" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Alert](https://img.shields.io/badge/Alert-SOC141-1E90FF?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![AbuseIPDB](https://img.shields.io/badge/AbuseIPDB-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![Endpoint Security](https://img.shields.io/badge/Endpoint%20Security-1E90FF?style=flat-square&logo=crowdstrike&logoColor=white)
![EDR Containment](https://img.shields.io/badge/EDR%20Containment-334155?style=flat-square&logo=crowdstrike&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9067ecf5054a3e74cd6f8f99b97c88f0b6f736e9/5.1/info.png?raw=true" alt="LetsDefend SIEM alert details for SOC141 Phishing URL Detected" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Overview](#-overview)
3. [Key Concepts](#-key-concepts)
4. [Step 1: Create the Case and Start the Playbook](#-step-1-create-the-case-and-start-the-playbook)
5. [Step 2: Verify the Ticket Details](#-step-2-verify-the-ticket-details)
6. [Step 3: Collect Necessary Details](#-step-3-collect-necessary-details)
7. [Step 4: Check the Destination IP Reputation](#-step-4-check-the-destination-ip-reputation)
8. [Step 5: Investigate the Source Host](#-step-5-investigate-the-source-host)
9. [Step 6: Contain the Endpoint](#-step-6-contain-the-endpoint)
10. [Step 7: Remediation](#-step-7-remediation)
11. [Step 8: Analyst Note, IoCs, and Close the Ticket](#-step-8-analyst-note-iocs-and-close-the-ticket)
12. [Indicators of Compromise](#-indicators-of-compromise-iocs)
13. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
14. [Response Actions](#-response-actions)
15. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | SOC141: Phishing URL Detected |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🔗 **Requested URL** | `hxxp://mogagrocol.ru/wp-content/plugins/akismet/fv/index[.]php?email=ellie[@]letsdefend[.]io` |
| 🌐 **Destination IP** | `91.289.114.8` (VirusTotal and AbuseIPDB showed clean) |
| 💻 **Source Host** | `172.16.17.49` (emilycomp) |
| 🚦 **Device Action** | Allowed |
| ☣️ **Malicious File** | `KBDYAK.exe` (flagged malicious in VirusTotal) |
| 🛑 **Response** | Endpoint contained, user notified |
| 🧰 **Tools Used** | LetsDefend SIEM, VirusTotal, AbuseIPDB, Endpoint Security (terminal history, command line, EDR containment) |
| ⚖️ **Final Verdict** | 🚨 **True Positive** |

---

<!-- ===================== OVERVIEW ===================== -->
## 📖 Overview

In this alert, a host tried to request a URL pointing to the domain `mogagrocol.ru`. The goal of the investigation was to find out why the host was flagged and to verify whether the URL was malicious, using the alert playbook in the LetsDefend SIEM.

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🛡️ **Endpoint Containment:** Isolating an endpoint helps stop security incidents from spreading, protects data, preserves evidence for forensic analysis, and supports legal and regulatory compliance.

> 💻 **Host Cybersecurity:** Protecting individual devices, workstations, and servers (hosts) is essential because each endpoint is a direct gateway to sensitive data and the broader network.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND START THE PLAYBOOK

| | |
| :--- | :--- |
| **Action** | Create the case and take ownership of it. |
| **Method** | Start the playbook to begin the guided investigation. |

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: VERIFY THE TICKET DETAILS

| | |
| :--- | :--- |
| **Action** | Verify the details and information in the ticket. |
| **Method** | Review the alert story to determine whether it is a true positive and whether the URL is malicious. |

### 🎯 Finding / Answer

The host tried to request a URL on the domain `mogagrocol.ru`. The ticket was reviewed to find out why the host was flagged.

```text
hxxp://mogagrocol[.]ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend.io
```

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: COLLECT NECESSARY DETAILS

| | |
| :--- | :--- |
| **Action** | Gather the core details from the ticket. |
| **Method** | Record the source address, destination address, and user agent. |

### 🎯 Finding / Answer

| Detail | Value |
| :--- | :--- |
| 💻 Source address | `172.16.17.49` (emilycomp) |
| 🌐 Destination address | `91.289.114.8` |

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: CHECK THE DESTINATION IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Check the destination IP address `91.289.114.8`. |
| **Method** | Search the IP in **VirusTotal** and **AbuseIPDB**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/1a5c03477d4e560a8d83d94e9da99c1073b0f43e/5.1/virus.png?raw=true" alt="VirusTotal reputation result for the destination IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/1a5c03477d4e560a8d83d94e9da99c1073b0f43e/5.1/abuse%20ipdb.png?raw=true" alt="AbuseIPDB reputation result for the destination IP" width="85%" />
</div>

### 🎯 Finding / Answer

The results showed a **normal, clean IP address**.

> ⚠️ **Analyst Insight:** A clean reputation does not mean the destination is completely safe. The investigation must continue on the source host.

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: INVESTIGATE THE SOURCE HOST

| | |
| :--- | :--- |
| **Action** | Dig deeper into the source host `172[.]16[.]17[.]49` (emilycomp) in the Endpoint Security tab. |
| **Method** | Review the terminal history and command line, then check the executable file in VirusTotal. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/1a5c03477d4e560a8d83d94e9da99c1073b0f43e/5.1/terminal.png?raw=true" alt="Terminal history of the source host" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b39968bb08c0f1aa0e3033f8335eaee9d7b2db8c/5.1/command%20line.png?raw=true" alt="Suspicious command line found on the host" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/1a853eb24cf5ebb18b437ee26757e1e85bb893c5/5.1/exec%20file%20virus.png?raw=true" alt="VirusTotal results for the executable file from the command line" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5dfb561122cdfa72375ac878c860e7352b4afcf2/5.1/affected.png?raw=true" alt="Affected host confirmed in the SIEM" width="85%" />
</div>

### 🎯 Finding / Answer

```text
KBDYAK.exe - flagged malicious in VirusTotal
```

- The device action was **allowed**, so the request went through.
- The terminal history showed a **suspicious command** run on the machine that appeared to reach out and possibly download software onto the host.
- The executable seen in the command line, `KBDYAK.exe`, carries a **malicious tag** in VirusTotal.
- The source IP address `172[.]16[.]17[.]49` is **affected**.

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: CONTAIN THE ENDPOINT

| | |
| :--- | :--- |
| **Action** | Contain the affected endpoint. |
| **Method** | Isolate the host through the SIEM's containment function. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/210f6daa9b9e8eb7e136dcc917517fc5857e5c7f/5.1/contain.png?raw=true" alt="Containing the affected endpoint" width="85%" />
</div>

### 🎯 Finding / Answer

The investigation confirmed a **true positive**. Based on the IoCs found, the endpoint had to be contained to prevent further damage, and the user had to be contacted with the next steps.

> 🛑 **Why contain?** Containment isolates the security incident, protects data, preserves evidence for forensic analysis, and supports legal and regulatory compliance.

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: REMEDIATION

| Action | Purpose |
| :--- | :--- |
| 🔒 **Isolate the network** | Stop the host from communicating with other systems or the attacker. |
| 📢 **Alert the affected users** | Inform the user of the incident and the next steps. |
| 🧹 **Remove the malicious content** | Eliminate the malicious file and related artifacts from the host. |
| 👁️ **Monitor for future threats** | Watch the endpoints for any recurring or related activity. |

---

<!-- ===================== STEP 8 ===================== -->
## 📌 STEP 8: ANALYST NOTE, IOCS, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Write the analyst note and add the IoCs found during the investigation. |
| **Method** | Finish the playbook and close the ticket. |

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE 🚨

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert indicates a **True Positive**: the host reached out to a suspicious URL, a suspicious command ran on the machine, and the executable `KBDYAK.exe` was flagged as malicious. The endpoint was contained to prevent further damage.

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator (Defanged) | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🔗 URL | `hxxp://mogagrocol.ru/wp-content/plugins/akismet/fv/index.php?email=ellie@letsdefend.io` | SIEM alert | 🔴 Malicious |
| 🌐 Domain | `mogagrocol.ru` | SIEM alert | 🔴 Malicious |
| 🌐 Destination IP | `91.289.114.8` | SIEM alert, VirusTotal, AbuseIPDB | 🟡 Clean reputation, linked to the malicious URL |
| ☣️ Executable | `KBDYAK.exe` | Endpoint command line, VirusTotal | 🔴 Malicious |
| 💻 Affected Host | `172.16.17.49` (emilycomp) | Endpoint Security | 🟠 Compromised, contained |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Phishing: Spearphishing Link | [T1566.002](https://attack.mitre.org/techniques/T1566/002/) | The host requested a phishing URL containing the user's email address. |
| Command and Control | Ingress Tool Transfer | [T1105](https://attack.mitre.org/techniques/T1105/) | A suspicious command on the host appeared to download software, and `KBDYAK.exe` was flagged malicious. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Created the case, took ownership, and started the playbook.
- [x] Verified the ticket details and the requested URL.
- [x] Collected the source address, destination address, and user agent.
- [x] Checked the destination IP in VirusTotal and AbuseIPDB.
- [x] Reviewed the terminal history and command line on the source host.
- [x] Confirmed `KBDYAK.exe` as malicious in VirusTotal.
- [x] Contained the endpoint and notified the affected user.
- [x] Completed the remediation steps: isolate the network, alert the user, remove the malicious content, and monitor for future threats.
- [x] Added the analyst note and IoCs, then closed the ticket as a true positive.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Playbook Execution](https://img.shields.io/badge/Playbook%20Execution-1E90FF?style=for-the-badge&labelColor=0D1117)
![URL and IP Reputation Analysis](https://img.shields.io/badge/URL%20and%20IP%20Reputation%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Endpoint Investigation](https://img.shields.io/badge/Endpoint%20Investigation-0284C7?style=for-the-badge&labelColor=0D1117)
![Malicious File Identification](https://img.shields.io/badge/Malicious%20File%20Identification-334155?style=for-the-badge&labelColor=0D1117)
![Host Containment](https://img.shields.io/badge/Host%20Containment-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
