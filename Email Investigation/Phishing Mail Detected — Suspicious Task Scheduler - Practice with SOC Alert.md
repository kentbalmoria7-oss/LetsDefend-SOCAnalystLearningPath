<!-- ===================== HEADER ===================== -->
<div align="center">

# 🕒 Phishing Mail Detected: Suspicious Task Scheduler

### LetsDefend SOC Analyst Learning Path · SIEM Alert Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Phishing+Alert+Triage+%26+Email+Analysis;MD5+Hash+Reputation+%26+Malware+Detection;Log+Correlation+%26+Endpoint+Review;Verdict%3A+True+Positive" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Phishing%20Alert-1E90FF?style=for-the-badge&logo=maildotru&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![Email Security](https://img.shields.io/badge/Email%20Security-334155?style=flat-square&logo=maildotru&logoColor=white)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![Hybrid Analysis](https://img.shields.io/badge/Hybrid%20Analysis-0F172A?style=flat-square&logo=crowdstrike&logoColor=00D4FF)
![Endpoint Security](https://img.shields.io/badge/Endpoint%20Security-1E90FF?style=flat-square&logo=crowdstrike&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/d6e14de087dccf42acc6979c1884e19ec6ab0a10/4/info.png?raw=true" alt="LetsDefend SIEM alert details for the suspicious task scheduler phishing alert" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Overview](#-overview)
3. [Key Concepts](#-key-concepts)
4. [Common Symptoms of Phishing](#-common-symptoms-of-phishing)
5. [Step 1: Create the Case and Take Ownership](#-step-1-create-the-case-and-take-ownership)
6. [Step 2: Gather Information](#-step-2-gather-information)
7. [Step 3: Check Email Security](#-step-3-check-email-security)
8. [Step 4: Review the Message Content](#-step-4-review-the-message-content)
9. [Step 5: Check the MD5 Hash Reputation](#-step-5-check-the-md5-hash-reputation)
10. [Step 6: Review the Log Records](#-step-6-review-the-log-records)
11. [Step 7: Check Endpoint Security](#-step-7-check-endpoint-security)
12. [Step 8: Answer the Playbook Questions](#-step-8-answer-the-playbook-questions)
13. [Step 9: Analyst Note, IoC Note, and Close the Ticket](#-step-9-analyst-note-ioc-note-and-close-the-ticket)
14. [Indicators of Compromise](#-indicators-of-compromise-iocs)
15. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
16. [Response Actions](#-response-actions)
17. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Phishing Mail Detected: Suspicious Task Scheduler |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 📅 **Email Sent** | March 21, 2021 |
| 🎣 **Lure Theme** | COVID-19 vaccine / breaking news |
| 📧 **Sender** | `aaronluo@cmail.carleton.ca` |
| 📥 **Recipient** | `marked@letsdefend.io` |
| 🌐 **SMTP Address** | `189.162.189.159` |
| 🖥️ **Destination** | `172.16.20.3` (Exchange Server) |
| 📎 **Attachment** | Yes, flagged as malicious |
| 🚦 **Delivery Status** | Blocked, not delivered to the user |
| 🧰 **Tools Used** | LetsDefend SIEM, Email Security, Log Management, Endpoint Security, VirusTotal, Hybrid Analysis |
| ⚖️ **Final Verdict** | 🚨 **True Positive** |

---

<!-- ===================== OVERVIEW ===================== -->
## 📖 Overview

An alert was assigned to me in the LetsDefend SIEM for a phishing attempt on March 21, 2021. The email used a COVID-19 vaccine theme and carried an attachment. This write-up walks through the investigation step by step using the alert playbook.

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🎣 **Phishing:** A social engineering cyberattack where criminals impersonate trusted brands, government agencies, or individuals through deceptive emails, text messages, phone calls, or websites to trick you into giving up passwords, financial data, or sensitive information.

> 🕒 **Suspicious Task Scheduler Entry:** An automated program or script in an operating system (most commonly Windows) that has been set up by malware, adware, or an unauthorized user to run hidden commands, launch malicious software, or maintain access to a computer.

> 🐴 **Trojan Horse:** A type of malicious software that misleads users by disguising itself as legitimate, safe software.

> 📬 **Exchange Server:** A mail server and calendaring server developed by Microsoft that runs exclusively on Windows Server operating systems.

---

<!-- ===================== SYMPTOMS ===================== -->
## 🚩 Common Symptoms of Phishing

| # | Symptom | Description |
| :-: | :--- | :--- |
| 1️⃣ | **Suspicious Sender Addresses** | Subtle misspellings, swapped letters, unusual domains (e.g., `.net` instead of `.com`), or fake lookalike brand names. |
| 2️⃣ | **Urgent or Threatening Language** | Messages create panic or false pressure, claiming an account will be suspended, locked, or closed unless you act immediately. |
| 3️⃣ | **Generic Greetings** | Vague openings like "Dear Valued Customer" or "Account Holder" instead of your real name. |
| 4️⃣ | **Requests for Sensitive Data** | Legitimate organizations never ask for full passwords, PINs, or Social Security numbers via email, text, or pop-up links. |
| 5️⃣ | **Unexpected Attachments** | Unsolicited files (invoices, shipping notices, security updates) that prompt a download or enabling macros can install malware or ransomware. |
| 6️⃣ | **Spelling and Grammar Errors** | Clumsy phrasing, odd formatting, or broken sentences, though modern AI tools make this less common. |

> 🔍 **In this case:** the email showed **urgent language** ("Open it now!") and an **unexpected attachment**, and the sender used **different subjects across multiple emails** to lure the victim.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the case from the alert. |
| **Method** | Take ownership of the case and start the playbook. |

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Review the alert details. |
| **Method** | Collect the key facts from the alert and ticket to answer the playbook questions. |

### 🎯 Finding / Answer

| Question | Answer |
| :--- | :--- |
| 🕒 When was it sent? | `March 21, 2021` |
| 🎣 What was the theme? | COVID-19 vaccine |
| 📧 What is the sender address? | `aaronluo@cmail.carleton[.]ca` |
| 🌐 What is the SMTP address? | `189.162.189.159` |
| 📥 What is the recipient address? | `marked@letsdefend.io` |

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK EMAIL SECURITY

| | |
| :--- | :--- |
| **Action** | Go to the Email Security tab and search for the sender address `aaronluo[@]cmail[.]carleton[.]ca`. |
| **Method** | Review every email the sender has sent. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/a944bd7bce4969188dcac4bd6f1d1bc95c2573cf/4/emails.png?raw=true" alt="Email Security search results showing two emails from the same sender" width="85%" />
</div>

### 🎯 Finding / Answer

The **same sender sent two different emails**. The varied subjects are designed to make the recipient click the file or link, a red flag consistent with phishing and a technique to lure the victim.

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: REVIEW THE MESSAGE CONTENT

| | |
| :--- | :--- |
| **Action** | Open the message to read its content and see the attachment. |
| **Method** | Look for phishing characteristics in the wording and the attached file. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/0e4f10cd1b871421903aa7b170ad1624a4cb7d1a/4/content.png?raw=true" alt="Email message content and attachment" width="85%" />
</div>

### 🎯 Finding / Answer

The message asks the recipient to read breaking news about COVID-19 and open the attachment immediately.

> 🚩 **Red flag: urgency.** The sender pushes the recipient to open the attached file right away. Phishing emails often follow current events and trends to raise the likelihood that the recipient will interact with the email.

> ⚠️ **Be careful:** If an email looks suspicious, do not click it first. Research it, or consult your IT or cybersecurity team to avoid risks, especially in a company.

> 🧪 **Safety Warning:** Suspicious files should be investigated in an isolated environment such as a virtual machine. Downloading any malicious or suspicious file directly to your own OS environment could harm your computer.

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: CHECK THE MD5 HASH REPUTATION

| | |
| :--- | :--- |
| **Action** | Take the MD5 hash of the attached file. |
| **Method** | Search the hash in **VirusTotal** and **Hybrid Analysis** to check its reputation and history. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/74a5043cabc3f3b25a36b73b0895f7b027a8f8fe/4/virus1.png?raw=true" alt="VirusTotal results for the attachment MD5 hash, first view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/74a5043cabc3f3b25a36b73b0895f7b027a8f8fe/4/virus%202.png?raw=true" alt="VirusTotal results for the attachment MD5 hash, second view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/74a5043cabc3f3b25a36b73b0895f7b027a8f8fe/4/hybrid1.png?raw=true" alt="Hybrid Analysis results for the attachment MD5 hash, first view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/74a5043cabc3f3b25a36b73b0895f7b027a8f8fe/4/hybrid2.png?raw=true" alt="Hybrid Analysis results for the attachment MD5 hash, second view" width="85%" />
</div>

### 🎯 Finding / Answer

Multiple security vendors flagged the file as **malicious**, for example as a **trojan horse**.

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: REVIEW THE LOG RECORDS

| | |
| :--- | :--- |
| **Action** | Find the log records showing the email's interaction with the network. |
| **Method** | Search Log Management for the SMTP address `189.162.189.159` with destination `172.16.20.3` on port `25`. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/4ca99d525f186c876966b0a4419e5c12dd541592/4/ips.png?raw=true" alt="Log Management search for the SMTP address and destination on port 25" width="85%" />
</div>

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: CHECK ENDPOINT SECURITY

| | |
| :--- | :--- |
| **Action** | Open Endpoint Security to see the phishing attempt on the target host. |
| **Method** | Look up the destination IP, then review its process history and terminal history. |

<div align="center">
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/72d7fe1154a2f0ce54463b4d45b0a511f56ce11f/4/process.png?raw=true" alt="Process history of the Exchange Server" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/72d7fe1154a2f0ce54463b4d45b0a511f56ce11f/4/terminal.png?raw=true" alt="Terminal history of the Exchange Server" width="85%" />
</div>

### 🎯 Finding / Answer

The destination is an **Exchange Server**, the mail server that received the phishing attempt.

---

<!-- ===================== STEP 8 ===================== -->
## 📌 STEP 8: ANSWER THE PLAYBOOK QUESTIONS

### 1️⃣ Are there attachments or URLs in the email?

✅ **Yes.** A suspicious file was attached to the email.

### 2️⃣ Analyze the URL / attachment

🔴 The attachment was **malicious**.

### 3️⃣ Check if the mail was delivered to the user

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/e0e20fab951681ab4f538da31e1a4d261c453f60/4/info2.png?raw=true" alt="Ticket details showing the email was blocked" width="85%" />
</div>

🟢 After re-checking the ticket in the SIEM, the email was **blocked**. It was **not delivered** to the user and did not penetrate the server.

---

<!-- ===================== STEP 9 ===================== -->
## 📌 STEP 9: ANALYST NOTE, IOC NOTE, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Write the analyst note and the IoC note. |
| **Method** | Finish the playbook and close the case. |

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE 🚨

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert indicates a **True Positive**: a phishing email with a malicious attachment was sent to the recipient, but it was blocked and never delivered.

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator (Defanged) | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 📧 Sender Address | `aaronluo@cmail.carleton.ca` | Alert / Email Security | 🔴 Malicious |
| 🌐 SMTP Address | `189.162.189.159` | Alert / Log Management | 🔴 Malicious |
| 📎 Email Attachment | MD5 hash of the attached file | VirusTotal, Hybrid Analysis | 🔴 Malicious (flagged as trojan by multiple vendors) |
| 🖥️ Targeted Host | `172.16.20.3` (Exchange Server) | Log Management, Endpoint Security | 🟢 Targeted, email blocked |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Phishing: Spearphishing Attachment | [T1566.001](https://attack.mitre.org/techniques/T1566/001/) | Phishing email sent with a malicious attachment using a COVID-19 lure. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Created the case and took ownership.
- [x] Gathered the alert details (date, sender, SMTP address, recipient).
- [x] Reviewed the sender's emails in Email Security and identified the phishing red flags.
- [x] Checked the attachment's MD5 hash in VirusTotal and Hybrid Analysis.
- [x] Reviewed the log records for the SMTP address and destination on port 25.
- [x] Checked Endpoint Security, including process history and terminal history.
- [x] Confirmed the email was blocked and not delivered to the user.
- [x] Wrote the analyst note and IoC note, then closed the ticket as a true positive.
- [ ] Recommended follow-up: keep the sender and SMTP address blocked at the email gateway.
- [ ] Recommended follow-up: remind users not to open unexpected attachments and to report suspicious emails to the IT or security team.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Playbook Execution](https://img.shields.io/badge/Playbook%20Execution-1E90FF?style=for-the-badge&labelColor=0D1117)
![Phishing Identification](https://img.shields.io/badge/Phishing%20Identification-38BDF8?style=for-the-badge&labelColor=0D1117)
![Hash Reputation Analysis](https://img.shields.io/badge/Hash%20Reputation%20Analysis-0284C7?style=for-the-badge&labelColor=0D1117)
![Log Correlation](https://img.shields.io/badge/Log%20Correlation-334155?style=for-the-badge&labelColor=0D1117)
![Endpoint Review](https://img.shields.io/badge/Endpoint%20Review-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
