<!-- ===================== HEADER ===================== -->
<div align="center">

# 📎 Malicious Attachment Detected: Phishing Alert

### LetsDefend SOC Analyst Learning Path · SIEM Alert Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=SIEM+Alert+Triage+%26+Playbook+Investigation;Email+Security+%26+Attachment+Analysis;Endpoint+Containment+via+EDR;Verdict%3A+True+Positive" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Phishing%20Alert-1E90FF?style=for-the-badge&logo=maildotru&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![EDR](https://img.shields.io/badge/EDR-334155?style=flat-square&logo=crowdstrike&logoColor=white)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![Hybrid Analysis](https://img.shields.io/badge/Hybrid%20Analysis-0F172A?style=flat-square&logo=crowdstrike&logoColor=00D4FF)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/59afd04d33f74a56cf7c66c7af048a788d945a44/2/info.png?raw=true" alt="LetsDefend SIEM alert details for the malicious attachment phishing alert" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Overview](#-overview)
3. [Key Concepts](#-key-concepts)
4. [Step 1: Create the Case and Start the Playbook](#-step-1-create-the-case-and-start-the-playbook)
5. [Step 2: Gather Ticket Information](#-step-2-gather-ticket-information)
6. [Step 3: Check Email Security](#-step-3-check-email-security)
7. [Step 4: Analyze the Attachment](#-step-4-analyze-the-attachment)
8. [Step 5: Delete the Delivered Email](#-step-5-delete-the-delivered-email)
9. [Step 6: Confirm the Attachment Was Opened](#-step-6-confirm-the-attachment-was-opened)
10. [Step 7: Investigate the IP and Contain the Host](#-step-7-investigate-the-ip-and-contain-the-host)
11. [Step 8: Add IoCs, Analyst Note, and Finish the Playbook](#-step-8-add-iocs-analyst-note-and-finish-the-playbook)
12. [Step 9: Close the Alert](#-step-9-close-the-alert)
13. [Indicators of Compromise](#-indicators-of-compromise-iocs)
14. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
15. [Response Actions](#-response-actions)
16. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Malicious Attachment Detected: Phishing Alert |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 📅 **Email Sent** | Jan 31, 2021, 03:48 PM |
| 📧 **Sender** | `accounting@cmail.carleton.ca` |
| 📥 **Recipient** | `richard[@]letsdefend[.]io` |
| 🌐 **SMTP Address** | `49.234.43.39` |
| 💻 **Affected Endpoint** | Richardprd (`5[.]135[.]143[.]133`) |
| 🧰 **Tools Used** | LetsDefend SIEM, Email Security, Log Management, Endpoint Security (EDR), VirusTotal, Hybrid Analysis |
| ⚖️ **Final Verdict** | 🚨 **True Positive** |

---

<!-- ===================== OVERVIEW ===================== -->
## 📖 Overview

The LetsDefend SIEM detected a phishing email and raised an alert. This write-up walks through the step-by-step investigation using a playbook inside the LetsDefend SIEM, a simulated environment for getting comfortable with real SIEM workflows.

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🧠 **SIEM (Security Information and Event Management):** A cybersecurity tool that collects and analyzes log data from across an organization's entire digital network to detect threats in real time.

> 📘 **Playbook:** A predefined, structured workflow that tells security analysts, responders, or automated systems what action to take when a specific security alert or threat is confirmed.

> 🛑 **Containment:** A critical defensive step because it stops attackers from gaining a foothold inside your digital environment.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND START THE PLAYBOOK

| | |
| :--- | :--- |
| **Action** | Review the alert that was received, then create a case. |
| **Method** | Start the playbook and answer its questions using the information in the alert and ticket. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/18104a122d925442b7cd731c754ad19c5d4ac296/2/create%20case.png?raw=true" alt="Creating a case from the SIEM alert" width="85%" />
</div>

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER TICKET INFORMATION

The first playbook questions are answered directly from the ticket details.

| Question | Answer |
| :--- | :--- |
| 🕒 When was it sent? | `Jan 31, 2021, 03:48 PM` |
| 🌐 What is the email's SMTP address? | `49.234.43.39` |
| 📧 What is the sender address? | `accounting@cmail.carleton.ca` |
| 📥 What is the recipient address? | `richard@letsdefend.io` |

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK EMAIL SECURITY

| | |
| :--- | :--- |
| **Action** | Search Email Security for the recipient email address to review the email content. |
| **Method** | Open the email and check whether the content is suspicious. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/40ac7ebd80f1cd9e079bd3cfb86fe33ae6c679a4/2/email%20sec.png?raw=true" alt="Email Security search results for the recipient address" width="85%" />
</div>

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: ANALYZE THE ATTACHMENT

| | |
| :--- | :--- |
| **Action** | Open the email and copy the attached file's details. |
| **Method** | Search the attachment in **VirusTotal** and **Hybrid Analysis** to check its reputation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/91c23f208e22f4fc201e3eef57d11ab2a47f3267/2/virus.png?raw=true" alt="VirusTotal results for the email attachment" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/386bff4fb9624b50602d324f6742f0362255f758/2/hybrid.png?raw=true" alt="Hybrid Analysis results for the email attachment" width="85%" />
</div>

### 🎯 Finding / Answer

| Question | Answer |
| :--- | :--- |
| Is the mail content suspicious? | ✅ **Yes.** Based on the results, the email content is suspicious. |
| Are there any attachments? | ✅ **Yes** |

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: DELETE THE DELIVERED EMAIL

| | |
| :--- | :--- |
| **Action** | Check whether the email was delivered to the user. |
| **Method** | The ticket confirms delivery, so the email and its attachment are deleted from the mailbox. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f0dd782f1cd74952d54c0241714bf612d288aa69/2/delete.png?raw=true" alt="Deleting the malicious email from the user's mailbox" width="85%" />
</div>

### 🎯 Finding / Answer

The email was **delivered to the user** (confirmed by the ticket), so it was deleted.

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: CONFIRM THE ATTACHMENT WAS OPENED

| | |
| :--- | :--- |
| **Action** | Check VirusTotal for the addresses the attachment contacts. |
| **Method** | Correlate the VirusTotal contacted addresses with the SIEM's Log Management dashboard. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b379409120965dd829531ffbea94f3a5869f4f0e/2/virus2.png?raw=true" alt="VirusTotal contacted addresses for the attachment" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/c21fcf0733e51d49569d1cfe937eab3170558b91/2/log%20management.png?raw=true" alt="Log Management entries matching the contacted domain" width="85%" />
</div>

### 🎯 Finding / Answer

The contacted address in VirusTotal and the domain in the Log Management records **match**. Based on these findings, **someone opened the email attachment**.

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: INVESTIGATE THE IP AND CONTAIN THE HOST

| | |
| :--- | :--- |
| **Action** | Identify the IP address used by the recipient when the attachment was accessed. |
| **Method** | Review the Log Management panel, then look up the IP in Endpoint Security, and contain the host through the SIEM's EDR. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/da42c4608b7cb8dd7b09eca7ed5a418c6a9199dd/2/ip.png?raw=true" alt="Log Management panel showing the IP address of the recipient access" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b5664019960bee8de88a26e609dec9d3f46159d8/2/ip.png?raw=true" alt="Endpoint Security showing the host that owns the IP address" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b5664019960bee8de88a26e609dec9d3f46159d8/2/ip%20-%20containment.png?raw=true" alt="Containing the affected host through the EDR" width="85%" />
</div>

### 🎯 Finding / Answer

```text
5.135.143.133 - Richardprd
```

> 🛑 **Why containment matters:** Containment stops attackers from gaining a foothold inside your digital environment, so the affected host was isolated through the EDR.

---

<!-- ===================== STEP 8 ===================== -->
## 📌 STEP 8: ADD IOCS, ANALYST NOTE, AND FINISH THE PLAYBOOK

| | |
| :--- | :--- |
| **Action** | Record the IoCs found during the analysis and write the analyst note. |
| **Method** | Complete the remaining playbook questions and finish the playbook. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5cdda47a935ef9de1932339e4b8e0ffae96d3b3f/2/analyst%20note.png?raw=true" alt="Analyst note written for the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5cdda47a935ef9de1932339e4b8e0ffae96d3b3f/2/ioc.png?raw=true" alt="IoCs added to the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5cdda47a935ef9de1932339e4b8e0ffae96d3b3f/2/finish.png?raw=true" alt="Playbook completed" width="85%" />
</div>

---

<!-- ===================== STEP 9 ===================== -->
## 📌 STEP 9: CLOSE THE ALERT

| | |
| :--- | :--- |
| **Action** | Once the analysis is complete, close the ticket. |
| **Method** | Close the alert with the final classification. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5cdda47a935ef9de1932339e4b8e0ffae96d3b3f/2/close%20alert.png?raw=true" alt="Closing the alert in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE 🚨

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert indicates a **True Positive**: a phishing email with a malicious attachment was delivered, the attachment was opened, and the affected host was contained.

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator (Defanged) | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 📧 Sender Address | `accounting@cmail.carleton.ca` | Alert / ticket | 🔴 Malicious |
| 🌐 SMTP Address | `49.234.43.39` | Alert / ticket | 🔴 Malicious |
| 📎 Email Attachment | Attached file from the phishing email | Email Security, VirusTotal, Hybrid Analysis | 🔴 Malicious |
| 🔗 Contacted Domain | Domain contacted by the attachment | VirusTotal, Log Management | 🔴 Malicious |
| 💻 Affected Host | `5.135.143.133` (Richardprd) | Log Management, Endpoint Security | 🟠 Compromised, contained |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Phishing: Spearphishing Attachment | [T1566.001](https://attack.mitre.org/techniques/T1566/001/) | Phishing email delivered with a malicious attachment. |
| Execution | User Execution: Malicious File | [T1204.002](https://attack.mitre.org/techniques/T1204/002/) | The recipient opened the attachment, which then contacted an external address. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Created the case and started the playbook.
- [x] Verified the email details (time sent, SMTP address, sender, recipient).
- [x] Confirmed the email content and attachment were suspicious.
- [x] Deleted the delivered email from the user's mailbox.
- [x] Confirmed the attachment was opened by correlating VirusTotal with Log Management.
- [x] Identified the affected host and contained it through the EDR.
- [x] Added IoCs and the analyst note, finished the playbook, and closed the alert.
- [ ] Recommended follow-up: block the sender and SMTP address at the email gateway.
- [ ] Recommended follow-up: review the contained host and the user's credentials before restoring access.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Playbook Execution](https://img.shields.io/badge/Playbook%20Execution-1E90FF?style=for-the-badge&labelColor=0D1117)
![Email Security Analysis](https://img.shields.io/badge/Email%20Security%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Malware Reputation Checks](https://img.shields.io/badge/Malware%20Reputation%20Checks-0284C7?style=for-the-badge&labelColor=0D1117)
![Log Correlation](https://img.shields.io/badge/Log%20Correlation-334155?style=for-the-badge&labelColor=0D1117)
![EDR Containment](https://img.shields.io/badge/EDR%20Containment-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
