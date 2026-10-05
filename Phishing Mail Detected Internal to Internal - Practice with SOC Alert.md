<!-- ===================== HEADER ===================== -->
<div align="center">

# 📨 Phishing Mail Detected: Internal to Internal

### LetsDefend SOC Analyst Learning Path · SIEM Alert Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Internal+Phishing+Alert+Triage;Email+Security+%26+IP+Reputation+Analysis;Endpoint+Verification+%26+Containment+Decision;Verdict%3A+False+Positive" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Internal%20Phishing%20Alert-1E90FF?style=for-the-badge&logo=maildotru&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-False%20Positive-22C55E?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![Email Security](https://img.shields.io/badge/Email%20Security-334155?style=flat-square&logo=maildotru&logoColor=white)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![AbuseIPDB](https://img.shields.io/badge/AbuseIPDB-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![Endpoint Security](https://img.shields.io/badge/Endpoint%20Security-1E90FF?style=flat-square&logo=crowdstrike&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9d3c737714903c05f9118399e707380c2ed3125b/3/info.png?raw=true" alt="LetsDefend SIEM alert details for the internal phishing mail alert" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Overview](#-overview)
3. [Key Concepts](#-key-concepts)
4. [Step 1: Create the Ticket and Take Ownership](#-step-1-create-the-ticket-and-take-ownership)
5. [Step 2: Gather Information](#-step-2-gather-information)
6. [Step 3: Check Email Security](#-step-3-check-email-security)
7. [Step 4: Check IP Reputation](#-step-4-check-ip-reputation)
8. [Step 5: Check the Endpoint Security Tab](#-step-5-check-the-endpoint-security-tab)
9. [Step 6: Decide on Containment](#-step-6-decide-on-containment)
10. [Step 7: Add Artifacts and Analyst Note](#-step-7-add-artifacts-and-analyst-note)
11. [Step 8: Close the Ticket](#-step-8-close-the-ticket)
12. [Decision Rationale](#-decision-rationale)
13. [Response Actions](#-response-actions)
14. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Phishing Mail Detected: Internal to Internal |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🔀 **Direction** | Internal to internal (John to Susie) |
| 📧 **Sender** | `john@letsdefend.io` |
| 🚦 **Email Action** | Allowed |
| 💻 **Source Endpoint** | `172.16.20.3` (john@letsdefend.io) |
| 📎 **Attachments / URLs** | None found |
| 🧰 **Tools Used** | LetsDefend SIEM, Email Security, VirusTotal, AbuseIPDB, Endpoint Security |
| ⚖️ **Final Verdict** | ✅ **False Positive** |

---

<!-- ===================== OVERVIEW ===================== -->
## 📖 Overview

A ticket was created and assigned to me from the LetsDefend SIEM for a phishing mail detected between two internal users. This write-up walks through the investigation step by step using the alert playbook, noting that the email action was **allowed**.

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🏢 **Internal Cyber Threat:** Security risks that originate from inside an organization, involving current or former employees, contractors, partners, or compromised internal systems.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE TICKET AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the ticket from the alert, then take ownership of it. |
| **Method** | Start the playbook to begin the guided investigation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/844b7902f6ba7eb5bf19c0c0212000b946dc5817/3/playbook%20start.png?raw=true" alt="Taking ownership of the ticket and starting the playbook" width="85%" />
</div>

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Parse the email details from the alert. |
| **Method** | Check the SMTP address, source address, and destination address, then answer the playbook questions. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/fbc75a386bf92746fce07496ece86337d7e6ee63/3/parse%20email.png?raw=true" alt="Parsing the email details from the alert" width="85%" />
</div>

### 🎯 Finding / Answer

The email was sent **from John to Susie**, both internal users.

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK EMAIL SECURITY

| | |
| :--- | :--- |
| **Action** | Go to Email Security and search for messages from the source/sender address. |
| **Method** | Review the email content and check for attachments or URLs. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/08955355d62b33d851bc7b69e31a6a12753a66a2/3/addresss.png?raw=true" alt="Email Security search for the sender address" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/fa3d46184b1ac80735e7b0c6e44c98f9b2c79ab6/3/content.png?raw=true" alt="Email content reviewed in Email Security" width="85%" />
</div>

### 🎯 Finding / Answer

| Question | Answer |
| :--- | :--- |
| Are there attachments or URLs in the email? | ❌ **No attachments found** |

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: CHECK IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Check the source IP address reputation. |
| **Method** | Search the IP in **VirusTotal** and **AbuseIPDB**, then match it to the host in Endpoint Security. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/0a5dc535b4c1c5b6a18069d3794722d709a591f4/3/virus.png?raw=true" alt="VirusTotal reputation result for the IP address" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/0a5dc535b4c1c5b6a18069d3794722d709a591f4/3/abuseidp.png?raw=true" alt="AbuseIPDB reputation result for the IP address" width="85%" />
</div>

### 🎯 Finding / Answer

```text
172.16.20.3 - john@letsdefend.io
```

The IP address is **clean**. No bad reputation was found.

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: CHECK THE ENDPOINT SECURITY TAB

| | |
| :--- | :--- |
| **Action** | Open the Endpoint Security tab for the affected host. |
| **Method** | Check whether any malicious script was added or is running. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/30e2147aa32aa0ba574a3c715d9dc56423e16a44/3/endpoint.png?raw=true" alt="Endpoint Security tab showing no malicious activity" width="85%" />
</div>

### 🎯 Finding / Answer

Nothing malicious was found, so I can **confirm no other malicious activity is running** on the host.

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: DECIDE ON CONTAINMENT

| | |
| :--- | :--- |
| **Action** | Decide whether containment is needed. |
| **Method** | Evaluate the findings from the email, IP reputation, and endpoint checks. |

### 🎯 Finding / Answer

Containment is **not necessary** for the Exchange server, since no malicious indicators were found.

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: ADD ARTIFACTS AND ANALYST NOTE

| | |
| :--- | :--- |
| **Action** | Add artifacts and IoCs if there are any, and write the analyst note. |
| **Method** | Complete the remaining playbook questions. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/41499c81b7a08cf1e7597556f824de841d09d155/3/add%20artifacts.png?raw=true" alt="Adding artifacts to the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/41499c81b7a08cf1e7597556f824de841d09d155/3/analyst%20note.png?raw=true" alt="Analyst note written for the case" width="85%" />
</div>

---

<!-- ===================== STEP 8 ===================== -->
## 📌 STEP 8: CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Close the ticket after the analysis is complete. |
| **Method** | Close the alert with the final classification. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/41499c81b7a08cf1e7597556f824de841d09d155/3/closed%20ticket.png?raw=true" alt="Closing the ticket in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## ✅ VERDICT: FALSE POSITIVE ✅

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert indicates a **False Positive**: the email had no attachments or URLs, the source IP has a clean reputation, and no malicious activity was found on the endpoint.

---

<!-- ===================== DECISION RATIONALE ===================== -->
## ⚖️ Decision Rationale

| Check | Result | Impact on Verdict |
| :--- | :--- | :--- |
| 📎 Attachments or URLs in the email | None found | 🟢 No malicious payload or link |
| 🌐 IP reputation (VirusTotal, AbuseIPDB) | Clean, no bad reputation | 🟢 No known malicious source |
| 💻 Endpoint Security review | No malicious script added or running | 🟢 No signs of compromise |
| 🛑 Containment | Not necessary | 🟢 No action required on the Exchange server |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Created the ticket and took ownership.
- [x] Started the playbook and parsed the email details.
- [x] Reviewed the sender's messages in Email Security.
- [x] Confirmed there were no attachments or URLs.
- [x] Checked the IP reputation in VirusTotal and AbuseIPDB.
- [x] Reviewed the Endpoint Security tab for malicious scripts.
- [x] Determined that containment was not necessary.
- [x] Added artifacts and the analyst note, then closed the ticket.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Playbook Execution](https://img.shields.io/badge/Playbook%20Execution-1E90FF?style=for-the-badge&labelColor=0D1117)
![Email Security Analysis](https://img.shields.io/badge/Email%20Security%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![IP Reputation Checks](https://img.shields.io/badge/IP%20Reputation%20Checks-0284C7?style=for-the-badge&labelColor=0D1117)
![Endpoint Verification](https://img.shields.io/badge/Endpoint%20Verification-334155?style=for-the-badge&labelColor=0D1117)
![False Positive Identification](https://img.shields.io/badge/False%20Positive%20Identification-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
