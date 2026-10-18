<!-- ===================== HEADER ===================== -->
<div align="center">

# 📧 Disclose The Agent: SMTP Data Exfiltration Investigation

### LetsDefend · Network Logs · Insider Threat PCAP Analysis

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=SMTP+Traffic+Analysis+in+Wireshark;Base64+Credential+Decoding;Attachment+Extraction+with+CyberChef;Insider+Threat+Data+Exfiltration" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Network%20Forensics-1E90FF?style=for-the-badge&logo=wireshark&logoColor=white&labelColor=0D1117)
![Scenario](https://img.shields.io/badge/Scenario-Insider%20Threat-DC2626?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)
![CyberChef](https://img.shields.io/badge/CyberChef-1E90FF?style=flat-square&logo=codechef&logoColor=white)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![SMTP](https://img.shields.io/badge/SMTP-0F172A?style=flat-square&logo=maildotru&logoColor=00D4FF)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/54db37d24de71dc2a33cf9a885ebe25aea82b5ad/Network%20Logs%20Picture/3/disclose.jpg?raw=true" alt="Disclose The Agent challenge cover image" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Purpose and Scenario](#-purpose-and-scenario)
3. [Challenge Questions](#-challenge-questions)
4. [Findings Summary](#-findings-summary)
5. [Exfiltration Timeline](#-exfiltration-timeline)
6. [Key Takeaways](#-key-takeaways)
7. [Indicators and Evidence](#-indicators-and-evidence)
8. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
9. [Response Actions](#-response-actions)
10. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Disclose The Agent |
| 🏫 **Platform** | LetsDefend (Network Logs) |
| 🗂️ **Evidence** | `smtpchallenge.pcap`, an SMTP network capture |
| 👤 **Insider** | Ann (fictional corporate insider suspected of leaking confidential information) |
| 📤 **Sender Account** | `sneakyg33k@aol.com` |
| 📥 **External Recipient** | `mistersecretx@aol.com` |
| 🔑 **Recovered Password** | `558r00lz` (decoded from Base64) |
| 📎 **Leaked Attachment** | `secretrendezvous.docx` |
| 🌍 **Location in the Document** | Mexico |
| 🧬 **Attachment MD5** | `9e423e11db88f01bbff81172839e1923` |
| 🧰 **Tools Used** | Wireshark (SMTP filter, Follow TCP Stream), CyberChef, VirusTotal |
| ⚖️ **Conclusion** | 🚨 **Data exfiltration over unencrypted SMTP** |

---

<!-- ===================== PURPOSE ===================== -->
## 📖 Purpose and Scenario

The purpose of "The Agent" challenge is to test and develop **network traffic analysis (PCAP)** and **digital forensics** skills.

The challenge simulates an **insider threat** scenario: a fictional corporate insider, Ann, is suspected of leaking highly confidential information outside the company. As a blue team analyst, the goal is to dissect the provided SMTP capture (`smtpchallenge.pcap`) and uncover the full scope of the data exfiltration.

| # | Task |
| :-: | :--- |
| 1️⃣ | **Identify the recipient:** find the external email address of the unauthorized party receiving the data. |
| 2️⃣ | **Recover credentials:** find and decode Ann's email account password, which means spotting and decoding Base64-encoded authentication strings in the traffic. |
| 3️⃣ | **Trace the stolen data:** find the exact name of the secret document that was attached and sent. |
| 4️⃣ | **Determine the context:** extract and decode the attachment to find the meeting location mentioned in it. |
| 5️⃣ | **Verify file integrity:** extract or calculate the MD5 hash of the leaked file for threat intelligence. |

---

<!-- ===================== QUESTIONS ===================== -->
## 🎯 Challenge Questions

---

### 1️⃣ Q1: What is the email address of Ann's secret boyfriend?

I typed `smtp` in the Wireshark filter box to look at the mail traffic. Packets **129** and **132** reveal the recipient's email address.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/49e1cae5edb40294252fdd9c985d0f00528c6110/Network%20Logs%20Picture/3/smtp.png?raw=true" alt="SMTP traffic filtered in Wireshark" width="85%" />
</div>

I opened packet 129 and read its contents with **Follow TCP Stream**. In the `From` field is Ann's email address, and in the `To` field is the external recipient's email address.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/49e1cae5edb40294252fdd9c985d0f00528c6110/Network%20Logs%20Picture/3/follow%20tcp.png?raw=true" alt="Following the TCP stream of packet 129" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/49e1cae5edb40294252fdd9c985d0f00528c6110/Network%20Logs%20Picture/3/pass%201.png?raw=true" alt="The From and To fields in the SMTP stream" width="85%" />
</div>

### 🎯 Finding / Answer

```text
mistersecretx@aol.com
```

> 🔁 The address is also clearly visible in **packet 132**.

---

### 2️⃣ Q2: What is Ann's email password?

> 🔓 **When can Wireshark show passwords?** Only when they cross the network **unencrypted**.

| Passwords Can Be Seen | Passwords Cannot Be Seen |
| :--- | :--- |
| **Unencrypted protocols:** standard HTTP, FTP, Telnet, or unencrypted POP3, IMAP, and SMTP send data without protection, so Wireshark shows login credentials in plain text. | **Encrypted traffic (HTTPS, SSL/TLS, SSH):** modern websites, secure email services, and banking apps use encrypted protocols, so Wireshark only shows scrambled ciphertext. |
| **Basic authentication:** even when HTTP uses Base64 encoding (such as HTTP Basic Auth), Wireshark can decode it to show the username and password. | **Special decryption keys:** encrypted traffic can't be read unless Wireshark is configured in advance with the server's private SSL/TLS keys or session keys. |

In packet **120**, I saw `AUTH LOGIN`, which means the login credentials follow. Reading the content, I found **two Base64-encoded strings**. In packet **126**, the password appears next to `PASS`, but it also has to be decoded from Base64.

```text
c25lYWt5ZzMza0Bhb2wuY29t
NTU4cjAwbHo=
```

Decoding them in **CyberChef** (From Base64) gives the account email and the password.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/49e1cae5edb40294252fdd9c985d0f00528c6110/Network%20Logs%20Picture/3/baker%20pass.png?raw=true" alt="CyberChef decoding the Base64 login strings" width="85%" />
</div>

| Field | Value |
| :--- | :--- |
| 📧 Email | `sneakyg33k@aol.com` |
| 🔑 Password | `558r00lz` |

### 🎯 Finding / Answer

```text
558r00lz
```

---

### 3️⃣ Q3: What is the name of the file that Ann sent?

In packet **129**, I found the file name and the attached **Base64-encoded `.docx` file**. The answer is in the `filename` field.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/49e1cae5edb40294252fdd9c985d0f00528c6110/Network%20Logs%20Picture/3/filename.png?raw=true" alt="The attachment file name in the SMTP stream" width="85%" />
</div>

### 🎯 Finding / Answer

```text
secretrendezvous.docx
```

---

### 4️⃣ Q4: In what country will Ann meet with her secret boyfriend?

I took the Base64-encoded attachment content and decoded it with **CyberChef's From Base64** operation. The output is the Word document. I downloaded it from the output panel and opened it.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/42f6cedd29b040e3ccb2bb2ef1f484c5666e0404/Network%20Logs%20Picture/3/c1.png?raw=true" alt="Decoding the attachment with From Base64 in CyberChef" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/42f6cedd29b040e3ccb2bb2ef1f484c5666e0404/Network%20Logs%20Picture/3/c2.png?raw=true" alt="Saving the decoded output from CyberChef" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/42f6cedd29b040e3ccb2bb2ef1f484c5666e0404/Network%20Logs%20Picture/3/d1.png?raw=true" alt="The downloaded document showing the meeting location" width="85%" />
</div>

The document names the location where the meeting takes place.

### 🎯 Finding / Answer

```text
Mexico
```

---

### 5️⃣ Q5: What is the MD5 value of the attachment Ann sent?

After saving the decoded file, I checked it in VirusTotal.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f463016d22e0ddb0011a486f8ec4339f4036b4bf/Network%20Logs%20Picture/3/Screenshot%202026-10-18%20130056.png?raw=true" alt="VirusTotal showing the MD5 hash of the attachment" width="85%" />
</div>

### 🎯 Finding / Answer

```text
9e423e11db88f01bbff81172839e1923
```

---

<!-- ===================== FINDINGS SUMMARY ===================== -->
## 📊 Findings Summary

| # | Question | Answer |
| :-: | :--- | :--- |
| 1️⃣ | Email address of the external recipient | `mistersecretx@aol.com` (found in the SMTP `To` field, packets 129 and 132) |
| 2️⃣ | Ann's email password | `558r00lz` (decoded from the Base64 string `NTU4cjAwbHo=`, packets 120 and 126) |
| 3️⃣ | Name of the attachment | `secretrendezvous.docx` (from packet 129) |
| 4️⃣ | Country in the document | `Mexico` (found by decoding and reading the attached `.docx` in CyberChef) |
| 5️⃣ | MD5 of the attachment | `9e423e11db88f01bbff81172839e1923` |

<br/>

<div align="center">

## 🚨 VERDICT: DATA EXFILTRATION OVER SMTP 🚨

</div>

### 🎯 Overall Conclusion

The capture shows an insider sending a confidential document to an external recipient over **unencrypted SMTP**. The login credentials and the attachment were both Base64-encoded, not encrypted, so anyone capturing the traffic could read the account, the password, the recipient, and the file contents.

---

<!-- ===================== EXFILTRATION TIMELINE ===================== -->
## ⛓️ Exfiltration Timeline

| Step | Packets | What Happened |
| :-: | :-: | :--- |
| 1️⃣ | 120 | The mail client starts `AUTH LOGIN`, so credentials follow. |
| 2️⃣ | 120 to 126 | The account and password are sent as Base64 strings. |
| 3️⃣ | 129 | The email is sent from Ann's account to `mistersecretx@aol.com`, with `secretrendezvous.docx` attached as Base64. |
| 4️⃣ | 132 | The recipient address appears again in the SMTP exchange. |

```text
 Ann's account ──AUTH LOGIN (Base64)──▶ SMTP server ──▶ email + secretrendezvous.docx ──▶ mistersecretx@aol.com
```

---

<!-- ===================== KEY TAKEAWAYS ===================== -->
## 🧠 Key Takeaways

| # | Lesson | Explanation |
| :-: | :--- | :--- |
| 1️⃣ | **Unencrypted protocols expose sensitive data** | Plain or weakly encoded traffic, like unencrypted SMTP and Base64, makes credentials, file names, and recipient addresses easy to read for anyone capturing the traffic. |
| 2️⃣ | **Base64 is encoding, not encryption** | Base64 is often used to send binary data (such as attachments or logins) over mail protocols, but it gives no security, because anyone can decode it instantly. |
| 3️⃣ | **A repeatable Wireshark workflow** | Filter the protocol (for example `smtp`), inspect streams with **Follow TCP Stream**, then locate the authentication prompts (`AUTH LOGIN`) and the attachment payloads. |
| 4️⃣ | **Tool synergy** | Wireshark finds the data, CyberChef decodes and extracts it, and VirusTotal checks the file hash, which makes a fast and efficient workflow. |

---

<!-- ===================== INDICATORS ===================== -->
## 🧬 Indicators and Evidence

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 📧 Sender account | `sneakyg33k@aol.com` | Decoded `AUTH LOGIN` string | 🟠 Insider's account |
| 🔑 Credential exposed | `558r00lz` | Decoded `AUTH LOGIN` string | 🔴 Sent in a decodable form |
| 📥 External recipient | `mistersecretx@aol.com` | SMTP `To` field | 🔴 Unauthorized recipient |
| 📎 Attachment | `secretrendezvous.docx` | Packet 129 | 🔴 Leaked document |
| 🧬 MD5 | `9e423e11db88f01bbff81172839e1923` | VirusTotal | 🟠 Fingerprint of the leaked file |
| 🌍 Content detail | Meeting location: Mexico | Decoded `.docx` | 🟡 Context from the document |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

> ⚠️ **Source of this mapping:** these techniques are my mapping from the SMTP exfiltration and the exposed credentials. They are not copied from a report.

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Exfiltration | Exfiltration Over Alternative Protocol: Unencrypted Non-C2 Protocol | [T1048.003](https://attack.mitre.org/techniques/T1048/003/) | The document was sent out through unencrypted SMTP. |
| Credential Access | Network Sniffing | [T1040](https://attack.mitre.org/techniques/T1040/) | The credentials were readable in a network capture. |
| Collection | Data from Local System | [T1005](https://attack.mitre.org/techniques/T1005/) | The insider took a confidential document and attached it to an email. |

---

<!-- ===================== RESPONSE ===================== -->
## ✅ Response Actions

- [x] Filtered the capture for SMTP traffic and found the sender and recipient in packets 129 and 132.
- [x] Found `AUTH LOGIN` in packet 120 and decoded the Base64 credentials in CyberChef.
- [x] Identified the attachment name and decoded the attachment to read its contents.
- [x] Checked the attachment's MD5 hash in VirusTotal.
- [ ] Recommended follow-up: reset the exposed account's password and require mail over encrypted connections (SMTP over TLS).
- [ ] Recommended follow-up: add data loss prevention (DLP) rules that flag confidential documents sent to external addresses.
- [ ] Recommended follow-up: hand the case to HR and legal teams for the insider threat review, and keep the pcap and file hash as evidence.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![Wireshark PCAP Analysis](https://img.shields.io/badge/Wireshark%20PCAP%20Analysis-0EA5E9?style=for-the-badge&labelColor=0D1117)
![SMTP Traffic Analysis](https://img.shields.io/badge/SMTP%20Traffic%20Analysis-1E90FF?style=for-the-badge&labelColor=0D1117)
![Base64 Decoding](https://img.shields.io/badge/Base64%20Decoding-38BDF8?style=for-the-badge&labelColor=0D1117)
![Attachment Extraction](https://img.shields.io/badge/Attachment%20Extraction-0284C7?style=for-the-badge&labelColor=0D1117)
![Insider Threat Investigation](https://img.shields.io/badge/Insider%20Threat%20Investigation-334155?style=for-the-badge&labelColor=0D1117)
![Network Forensics](https://img.shields.io/badge/Network%20Forensics-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
