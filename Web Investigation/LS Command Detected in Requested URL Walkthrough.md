<!-- ===================== HEADER ===================== -->
<div align="center">

# 🔍 LS Command Detected in Requested URL

### LetsDefend SOC Analyst Learning Path · False Positive Investigation & Rule Tuning

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Command+Injection+Alert+Triage;IP+Reputation+%26+Log+Management+Review;Identifying+False+Positives;Detection+Rule+Tuning" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Command%20Injection%20Alert-1E90FF?style=for-the-badge&logo=gnubash&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-False%20Positive-22C55E?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![AbuseIPDB](https://img.shields.io/badge/AbuseIPDB-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![Log Management](https://img.shields.io/badge/Log%20Management-334155?style=flat-square&logo=files&logoColor=white)
![Detection Tuning](https://img.shields.io/badge/Detection%20Tuning-1E90FF?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b8e25066c1facd58d0394a8cabddbc2052a4094c/Web%20Investigation%20Picture/3/info.png?raw=true" alt="LetsDefend SIEM alert details for the ls command detected in a requested URL" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Overview](#-overview)
3. [Key Concepts](#-key-concepts)
4. [Step 1: Create the Case and Take Ownership](#-step-1-create-the-case-and-take-ownership)
5. [Step 2: Gather Information](#-step-2-gather-information)
6. [Step 3: Check IP Reputation](#-step-3-check-ip-reputation)
7. [Step 4: Review Log Management](#-step-4-review-log-management)
8. [Step 5: Analyze the Alerted URL](#-step-5-analyze-the-alerted-url)
9. [Step 6: Containment and Remediation](#-step-6-containment-and-remediation)
10. [Step 7: Analyst Note, IoCs, and Close the Ticket](#-step-7-analyst-note-iocs-and-close-the-ticket)
11. [Decision Rationale](#-decision-rationale)
12. [Response Actions](#-response-actions)
13. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | LS Command Detected in Requested URL |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🚨 **Detection Rule** | SOC167 |
| 🆔 **Event ID** | 117 |
| 💻 **Hostname** | EliotPRD |
| 🌐 **Source IP** | `172.16.17.46` |
| 🎯 **Destination IP** | `188.114.96.15` |
| 🔗 **Requested URL** | `https://letsdefend.io/blog/?s=skills` |
| 🧪 **Trigger Cause** | The letters `ls` appear inside the word "skills" in the search term |
| 🌐 **IP Reputation** | Good reputation |
| 🛑 **Containment** | Not needed |
| 🧰 **Tools Used** | LetsDefend SIEM, VirusTotal, AbuseIPDB, Log Management |
| ⚖️ **Final Verdict** | ✅ **False Positive (non-malicious)** |

---

<!-- ===================== OVERVIEW ===================== -->
## 📖 Overview

This alert was triggered by a possible `ls` command in a requested URL. The investigation checks whether the traffic is malicious or whether the detection rule matched harmless text.

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 💉 **Command injection:** also called OS command injection or shell injection, it occurs when an application passes unsanitized user input directly to a system shell. This lets an attacker append arbitrary operating system commands, which run with the privileges of the vulnerable application.

> 📂 **The `ls` command:** short for "list," it shows the files and folders inside the current directory. Think of it as opening a folder on your computer to see what is stored inside.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the case from the alert and take ownership. |
| **Method** | Start the playbook to begin the guided investigation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b8e25066c1facd58d0394a8cabddbc2052a4094c/Web%20Investigation%20Picture/3/create%20case.png?raw=true" alt="Creating a case from the SIEM alert" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b8e25066c1facd58d0394a8cabddbc2052a4094c/Web%20Investigation%20Picture/3/playbook.png?raw=true" alt="Starting the playbook for the case" width="85%" />
</div>

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Review the alert details. |
| **Method** | Record the key facts from the alert to answer the playbook questions. |

### 🎯 Finding / Answer

| Field | Value |
| :--- | :--- |
| 🆔 Event ID | `117` |
| 💻 Hostname | `EliotPRD` |
| 🎯 Destination IP | `188.114.96.15` |
| 🌐 Source IP | `172.16.17.46` |
| 🔗 Requested URL | `https://letsdefend.io/blog/?s=skills` |

> 🧪 **Trigger cause:** the SIEM detection rule (SOC167) flagged this HTTP request because the character sequence `ls` appears inside the URL string, in the word "skills". Rules that search for Linux command line execution, like the `ls` directory listing command, can overmatch when they use simple pattern matching on web queries.

> 👀 **First impression:** the URL looks harmless because it looks like a simple search, but it has to be verified before concluding.

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Check whether the traffic is malicious. |
| **Method** | Search the IP address in **VirusTotal** and **AbuseIPDB**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b8e25066c1facd58d0394a8cabddbc2052a4094c/Web%20Investigation%20Picture/3/virus.png?raw=true" alt="VirusTotal reputation result for the IP address" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b8e25066c1facd58d0394a8cabddbc2052a4094c/Web%20Investigation%20Picture/3/ipdb.png?raw=true" alt="AbuseIPDB reputation result for the IP address" width="85%" />
</div>

### 🎯 Finding / Answer

The IP address has a **good reputation**.

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: REVIEW LOG MANAGEMENT

| | |
| :--- | :--- |
| **Action** | Search Log Management for the source IP and destination IP. |
| **Method** | Check whether the source IP made any other requests that were harmful but not detected, such as data exfiltration or lateral movement. |

### 🤔 Why filter by these IPs?

Even when the IPs look harmless, a clean reputation does not prove the whole story.

| Reason | Explanation |
| :--- | :--- |
| 🐌 **The "low and slow" attack** | Sophisticated attackers don't always launch massive, obvious attacks. They often perform low and slow reconnaissance, sending small, innocent-looking requests over days or weeks to map the network without tripping alert thresholds. |
| 🕵️ **The compromised insider** | Even if this alert is a false positive, the source machine might be compromised. Its full history shows whether it started reaching out to unusual external servers or trying to reach restricted internal databases. |
| 💥 **The "blast radius" check** | An alert is often only the tip of the iceberg. Filtering by source and destination shows every footprint the machine left across the environment before and after the alert. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/18e87be25f7817d293089948a09ecc0aecd15e89/Web%20Investigation%20Picture/3/raw%20logs.png?raw=true" alt="Raw logs for the source and destination IP addresses" width="85%" />
</div>

### 🎯 Finding / Answer

The URL history and raw logs were reviewed, and they are all good and harmless. Some other requests were found, but none were harmful.

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: ANALYZE THE ALERTED URL

| | |
| :--- | :--- |
| **Action** | Look closely at the alerted URL, whose search term is "skills". |
| **Method** | Check for regex, obfuscation, or injection characters. |

### 🎯 Finding / Answer

There is **no regex and no obfuscation**. It is just a simple search. The alert was triggered because poorly tuned rules can produce false positives.

Overall, the SIEM is generating false positives because the rule is out of tune. The "search skills" activity matches the pattern for possible injection attacks, such as the `ls` command.

| Question | Answer |
| :--- | :--- |
| Is there a different request or traffic? | ✅ **Yes.** Other requests appeared in log management, but none were harmful. |
| Is the traffic malicious? | ❌ **Non-malicious** |

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: CONTAINMENT AND REMEDIATION

### 🛑 Containment

No further investigation or containment is needed.

### 🛠️ Remediation: Refine the Detection Logic (Recommended)

Instead of disabling the rule, make it smarter.

| Fix | What It Does |
| :--- | :--- |
| 🧩 **Add contextual check constraints** | Trigger only when `ls` appears with common injection characters, such as `; ls`, `&& ls`, `\| ls`, or a backtick-wrapped `ls`. A standalone "ls" in a search bar should not raise a high-severity alert. |
| ⚓ **Implement regex anchors** | Make the detection signature look for command boundaries, not simple string matches. |

> ❓ **Why tune the rule instead of leaving it as is?** If the rule stays this broad, it will keep producing many false positives for ordinary words that contain "ls".

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: ANALYST NOTE, IOCS, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Write the analyst note and add IoCs if there are any. |
| **Method** | Finish the playbook and close the ticket. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/c9ac40d9f3375d748f1748bc49d30e9e6a939b94/Web%20Investigation%20Picture/3/analyst%20note.png?raw=true" alt="Analyst note written for the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/c9ac40d9f3375d748f1748bc49d30e9e6a939b94/Web%20Investigation%20Picture/3/close%20alert.png?raw=true" alt="Closing the alert in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## ✅ VERDICT: FALSE POSITIVE ✅

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert is a **False Positive**: the requested URL is a simple search with no obfuscation, the IP has a good reputation, and the log history shows no harmful activity. The alert fired because the SIEM rule matched `ls` inside the word "skills".

---

<!-- ===================== DECISION RATIONALE ===================== -->
## ⚖️ Decision Rationale

| Check | Result | Impact on Verdict |
| :--- | :--- | :--- |
| 🔗 **Requested URL** | Simple search for "skills", no regex or obfuscation | 🟢 No injection attempt |
| 🌐 **IP reputation (VirusTotal, AbuseIPDB)** | Good reputation | 🟢 No known malicious source |
| 📋 **Log Management history** | Other requests found, none harmful | 🟢 No signs of exfiltration or lateral movement |
| 🛑 **Containment** | Not needed | 🟢 No action on the host |
| 🧪 **Root cause** | Out-of-tune detection rule matching `ls` as plain text | 🟡 Rule needs refinement |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Created the case, took ownership, and started the playbook.
- [x] Recorded the event details (event ID, hostname, IPs, URL).
- [x] Checked the IP reputation in VirusTotal and AbuseIPDB.
- [x] Reviewed Log Management for other requests from the source and destination IPs.
- [x] Confirmed the URL was a plain search with no regex or obfuscation.
- [x] Determined that no containment was needed.
- [x] Added the analyst note and closed the ticket as a false positive.
- [ ] Recommended follow-up: refine rule SOC167 with contextual checks and regex anchors, as described above.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![False Positive Identification](https://img.shields.io/badge/False%20Positive%20Identification-1E90FF?style=for-the-badge&labelColor=0D1117)
![IP Reputation Analysis](https://img.shields.io/badge/IP%20Reputation%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Log Correlation](https://img.shields.io/badge/Log%20Correlation-0284C7?style=for-the-badge&labelColor=0D1117)
![Detection Rule Tuning](https://img.shields.io/badge/Detection%20Rule%20Tuning-334155?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-475569?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
