<!-- ===================== HEADER ===================== -->
<div align="center">

# 🕵️ SOC168: Whoami Command Detected in Request Body

### LetsDefend SOC Analyst Learning Path · Command Injection (RCE) Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Command+Injection+Alert+Triage;Post-Exploitation+Command+Analysis;Web+Server+Containment+%26+Escalation;Verdict%3A+True+Positive" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Alert](https://img.shields.io/badge/Alert-SOC168-1E90FF?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Escalated%20to%20Tier%202-F59E0B?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![AbuseIPDB](https://img.shields.io/badge/AbuseIPDB-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![Cisco Talos](https://img.shields.io/badge/Cisco%20Talos-049FD9?style=flat-square&logo=cisco&logoColor=white)
![EDR](https://img.shields.io/badge/EDR%20Containment-334155?style=flat-square&logo=crowdstrike&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3e7a618e28047988a514af3b3c1d3b0b4baeb887/Web%20Investigation%20Picture/4/info.png?raw=true" alt="LetsDefend SIEM alert details for SOC168 Whoami Command Detected in Request Body" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Step 1: Create the Case and Take Ownership](#-step-1-create-the-case-and-take-ownership)
4. [Step 2: Gather Information](#-step-2-gather-information)
5. [Step 3: Check the Source IP Reputation](#-step-3-check-the-source-ip-reputation)
6. [Step 4: Review the Raw Logs](#-step-4-review-the-raw-logs)
7. [Step 5: Confirm Success with Terminal History](#-step-5-confirm-success-with-terminal-history)
8. [Step 6: Containment](#-step-6-containment)
9. [Step 7: Remediation and Escalation](#-step-7-remediation-and-escalation)
10. [Step 8: Analyst Note, IoCs, and Close the Ticket](#-step-8-analyst-note-iocs-and-close-the-ticket)
11. [Attack Chain Summary](#-attack-chain-summary)
12. [Indicators of Compromise](#-indicators-of-compromise-iocs)
13. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
14. [Response Actions](#-response-actions)
15. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Whoami Command Detected in Request Body |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🚨 **Alert Rule** | SOC168: Whoami Command Detected in Request Body |
| ⚡ **Trigger Reason** | The request body contains the string `whoami` |
| 💻 **Hostname** | WebServer1004 |
| 🎯 **Destination IP** | `172.16.17.16` |
| 🌐 **Source IP** | `61.177.172.87` (external, China) |
| 📮 **HTTP Method** | POST |
| 🔗 **Requested URL** | `https://172.16.17.16/video/` |
| 🔢 **Events Seen** | 5, all from the same external source IP |
| ✅ **Attack Successful** | Yes |
| 🛑 **Response** | Server contained, ticket escalated to Tier 2 |
| 🧰 **Tools Used** | LetsDefend SIEM, VirusTotal, AbuseIPDB, Cisco Talos, Log Management, Endpoint Security |
| ⚖️ **Final Verdict** | 🚨 **True Positive** |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🪪 **What is `whoami`?** `whoami` is a command-line utility that displays the username of the current user logged into the system. Think of it as checking your own digital ID card: if a computer has multiple accounts (like Admin, Guest, or John), it tells you which one you are operating under.

### 🛡️ Why `whoami` Matters in Cybersecurity

| # | Reason | Explanation |
| :-: | :--- | :--- |
| 1️⃣ | **Privilege verification (post-exploitation)** | After exploiting a vulnerability and gaining access to a remote system (often a "reverse shell"), an attacker doesn't know their access level. Running `whoami` tells them immediately. If the output is `root` (Linux) or `SYSTEM` (Windows), they have absolute control. If it is a standard user like `www-data`, they know they need privilege escalation. |
| 2️⃣ | **Indicator of Compromise (IoC)** | It is one of the first commands an attacker runs after breaking in, so EDR and SIEM tools watch for it closely. An unexpected or automated `whoami` run by web servers or database processes is a major red flag. |
| 3️⃣ | **Automation and scripting** | Malware and automated hacking scripts often use `whoami` to check conditions before delivering a payload, for example checking whether they run as root before installing a rootkit. |

> 🏢 **Simple analogy:** running `whoami` is like looking down at the security badge clipped to your jacket in a secure headquarters. It doesn't tell your life story. It just says who you are right now and which department you belong to.

> 🎭 **Why it's needed:** in computing you can "change disguises," such as switching from a standard worker to the janitor with the master keys. If you forget which disguise you're wearing, `whoami` checks the badge.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the case from the alert and take ownership. |
| **Method** | Start the playbook to begin the guided investigation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3e7a618e28047988a514af3b3c1d3b0b4baeb887/Web%20Investigation%20Picture/4/create%20ticket.png?raw=true" alt="Creating a ticket from the SIEM alert" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3e7a618e28047988a514af3b3c1d3b0b4baeb887/Web%20Investigation%20Picture/4/playbook.png?raw=true" alt="Starting the playbook for the case" width="85%" />
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
| 🚨 Alert rule | SOC168: Whoami Command Detected in Request Body |
| ⚡ Trigger reason | Request body contains the string `whoami` |
| 💻 Hostname | `WebServer1004` |
| 🎯 Destination IP address | `172.16.17.16` |
| 🌐 Source IP address | `61.177.172.87` |
| 📮 HTTP request method | `POST` |
| 🔗 Requested URL | `https://172.16.17.16/video/` |

> 🧪 **Why the rule fired:** `whoami` is a standard command execution string that attackers use to check their privileges during Command Injection or Remote Code Execution (RCE) attempts. To decide whether this is a true or false positive, the raw logs in Log Management must be checked for the exact content of the POST request body.

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK THE SOURCE IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Check the reputation of the source IP address. |
| **Method** | Search it in **VirusTotal**, **AbuseIPDB**, and **Cisco Talos Intelligence**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3e7a618e28047988a514af3b3c1d3b0b4baeb887/Web%20Investigation%20Picture/4/virus.png?raw=true" alt="VirusTotal reputation result for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3e7a618e28047988a514af3b3c1d3b0b4baeb887/Web%20Investigation%20Picture/4/abuse%20ipdb.png?raw=true" alt="AbuseIPDB reputation result for the source IP, first view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3e7a618e28047988a514af3b3c1d3b0b4baeb887/Web%20Investigation%20Picture/4/abuse%20ipdb2.png?raw=true" alt="AbuseIPDB reputation result for the source IP, second view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3e7a618e28047988a514af3b3c1d3b0b4baeb887/Web%20Investigation%20Picture/4/talos.png?raw=true" alt="Cisco Talos Intelligence result for the source IP" width="85%" />
</div>

### 🎯 Finding / Answer

| Detail | Value |
| :--- | :--- |
| 🏢 ISP | ChinaNet Jiangsu Province Network |
| 🗄️ Usage | Data center |
| 🌐 Domain name | `chinatelecom.com.cn` |
| 🌏 Country | China |
| 📍 City | Lianyungang, Jiangsu |

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: REVIEW THE RAW LOGS

| | |
| :--- | :--- |
| **Action** | Search Log Management for the source IP address, since this is a web server. |
| **Method** | Inspect the raw logs and the HTTP response codes. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/0d3839f857c3cc29a34e7a58b2c4a1777bfb8a15/Web%20Investigation%20Picture/4/logs.png?raw=true" alt="Log Management results for the source IP address" width="85%" />
</div>

```text
61.177.172.87 (China)  →  172.16.17.16 (our web server)
```

There are **5 events**, all with the source IP `61.177.172.87`, an outside internet address. All 5 show the unknown external IP communicating with the web server in question.

### 📊 HTTTP Status Codes and HTTP Request Methods

Before exploring the logs, the response codes were reviewed to understand what each status means.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/response%20code.png?raw=true" alt="Reference of HTTP response codes" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/cc955ed41f484d58f4e36dc19e546f8addf0a20c/Web%20Investigation%20Picture/4/http%20status%20code.png?raw=true" alt="Reference of HTTP response codes" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/cc955ed41f484d58f4e36dc19e546f8addf0a20c/Web%20Investigation%20Picture/4/http%20methods.png?raw=true" alt="Reference of HTTP response codes" width="85%" />
</div>

### 🧾 Raw Logs

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/rawl%20log%201.png?raw=true" alt="Raw log 1 showing the injected command in the POST body" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/raw%20log%202.png?raw=true" alt="Raw log 2 showing the injected command in the POST body" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/raw%20log%203.png?raw=true" alt="Raw log 3 showing the injected command in the POST body" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/raw%20log%204.png?raw=true" alt="Raw log 4 showing the injected command in the POST body" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/raw%20log%205.png?raw=true" alt="Raw log 5 showing the injected command in the POST body" width="85%" />
</div>

### 🔍 What Is Happening in This Scenario?

The attacker exploited a **command injection** vulnerability, typically triggered through unsanitized POST parameters in a web application. Once they find an entry point, they don't act at random. They follow a structured post-exploitation workflow:

```text
[ Phase 1: Reconnaissance ]  ──▶  [ Phase 2: System Profiling ]  ──▶  [ Phase 3: Enumeration & Credential Access ]
        whoami, ls                          uname                       cat /etc/passwd, cat /etc/shadow
```

### 🧩 Breakdown of the Commands Used

| Phase | Command | What It Does | Attacker's Intent |
| :--- | :--- | :--- | :--- |
| 1️⃣ Recon | `whoami` | Displays the effective username of the user running the shell process. | Learn their initial access level: a low-privileged service account (such as `www-data`, `apache`, or `nobody`) or an elevated user like `root`. |
| 1️⃣ Recon | `ls` | Lists directory contents and files. | Map the directory layout, locate application source code, find configuration files with hardcoded database credentials, and find writable folders for uploading payloads or web shells. |
| 2️⃣ Profiling | `uname` | Prints kernel and hardware architecture information (often `uname -a`). | Look for known local privilege escalation (LPE) exploits, such as Linux kernel vulnerabilities, specific to that OS build. |
| 3️⃣ Enumeration | `cat /etc/passwd` | Outputs the readable contents of the system user account file. | List registered users, system daemons, and administrative profiles, and see login shells (for example `/bin/bash` vs. `/sbin/nologin`). |
| 3️⃣ Credential access | `cat /etc/shadow` | Attempts to read the file that stores hashed user passwords. | Take password hashes for offline cracking with tools like John the Ripper or Hashcat. |

> 🔐 **Why `/etc/shadow` matters:** by default it is readable only by the root account. If this command succeeds and returns data, it proves the web application was configured to run with root privileges or the attacker has already achieved full system compromise.

> 🌏 **Origin:** the logs show the malicious IP is located in China, so this activity did not come from inside the network.

| Question | Answer |
| :--- | :--- |
| Is the traffic malicious? | 🔴 **Yes.** The IP is flagged as malicious and comes from China. |
| What type of attack? | 💉 **Command injection** using `whoami` |

> 💉 **Why `whoami` is used in command injection:** when an attacker finds an input field or parameter that passes user data into a system shell without checks, they perform OS command injection.

| Purpose | Explanation |
| :--- | :--- |
| 🧪 **Proof of concept (PoC)** | It returns a clear text answer (such as `www-data`, `root`, or `Administrator`) that proves the attacker can run arbitrary commands on the server. |
| 🔑 **Privilege level check** | It shows which account the web application runs as. If it returns `root` or `Administrator`, the attacker has high-level control. |
| 🙈 **Blind injection testing** | When the server doesn't show command output, attackers may redirect the output of `whoami` to a web-accessible folder, or trigger an out-of-band request such as a DNS lookup, to prove the command ran. |

---
---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: CONFIRM SUCCESS WITH TERMINAL HISTORY

| | |
| :--- | :--- |
| **Action** | Open the Endpoint Security tab and look at the command line history through **Terminal History**. |
| **Method** | Compare the commands in the history with the commands in the POST parameters. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/terminal%20history.png?raw=true" alt="Terminal history on the web server showing the injected commands" width="85%" />
</div>

### 🎯 Finding / Answer

The same commands used in the POST parameters appear in the command line and process history of the web server.

| Question | Answer |
| :--- | :--- |
| Was the attack successful? | 🔴 **Yes, absolutely.** The injected commands were executed on the server, as shown by the terminal history. |

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: CONTAINMENT

| | |
| :--- | :--- |
| **Action** | Contain the web server so the attacker can't cause further damage. |
| **Method** | Search `WebServer1004` in Endpoint Management and contain the endpoint. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/webserver.png?raw=true" alt="Containing WebServer1004 in Endpoint Management" width="85%" />
</div>

### 🎯 Finding / Answer

The server was contained to prevent further damage.

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: REMEDIATION AND ESCALATION

| Question | Answer |
| :--- | :--- |
| Do we need Tier 2 escalation? | ✅ **Yes.** Because the attack was successful, the playbook says to move the ticket up to a Tier 2 analyst. |

| Remediation Action | Purpose |
| :--- | :--- |
| 📤 **Escalate the ticket to the next tier** | Hand the successful attack to a Tier 2 analyst for deeper investigation. |
| 🔑 **Reset compromised user credentials** | Remove the attacker's ability to use any credentials that may have been exposed. |
| 💪 **Implement strong passwords** | Reduce the risk of future credential cracking. |

---

<!-- ===================== STEP 8 ===================== -->
## 📌 STEP 8: ANALYST NOTE, IOCS, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Write the analyst note and add the IoCs found during the investigation. |
| **Method** | Finish the playbook and close the ticket. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/analyst%20note.png?raw=true" alt="Analyst note written for the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/ioc.png?raw=true" alt="IoCs added to the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9a1efa6d0d4bb13869c28414cb9a70579d0ed571/Web%20Investigation%20Picture/4/closed%20ticket.png?raw=true" alt="Closing the ticket in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE 🚨

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert is a **True Positive**: an external, malicious IP exploited a command injection vulnerability on `WebServer1004` and ran reconnaissance, system profiling, and credential access commands. The web server was contained and the ticket escalated to Tier 2.

---

<!-- ===================== ATTACK CHAIN ===================== -->
## ⛓️ Attack Chain Summary

| Stage | What Happened | Evidence |
| :-: | :--- | :--- |
| 1️⃣ | **External request** | POST request from `61.177.172.87` to `https://172.16.17.16/video/` |
| 2️⃣ | **Command injection** | Commands placed in the POST request body |
| 3️⃣ | **Reconnaissance** | `whoami` and `ls` |
| 4️⃣ | **System profiling** | `uname` |
| 5️⃣ | **User enumeration and credential access** | `cat /etc/passwd` and `cat /etc/shadow` |
| 6️⃣ | **Execution confirmed** | Same commands found in the server's terminal and process history |
| 7️⃣ | **Response** | Server contained, ticket escalated to Tier 2 |

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🌐 Source IP | `61.177.172.87` | SIEM alert, Log Management | 🔴 Malicious (China, ChinaNet Jiangsu Province Network) |
| 🔗 Targeted URL | `https://172.16.17.16/video/` | SIEM alert | 🟠 Injection point on our web server |
| 💉 Injected commands | `whoami`, `ls`, `uname`, `cat /etc/passwd`, `cat /etc/shadow` | Raw logs, terminal history | 🔴 Malicious |
| 💻 Affected host | `WebServer1004` (`172.16.17.16`) | Endpoint Security | 🟠 Compromised, contained |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Exploit Public-Facing Application | [T1190](https://attack.mitre.org/techniques/T1190/) | Command injection through a POST request to the web application. |
| Execution | Command and Scripting Interpreter: Unix Shell | [T1059.004](https://attack.mitre.org/techniques/T1059/004/) | Shell commands were executed on the Linux web server. |
| Discovery | System Owner/User Discovery | [T1033](https://attack.mitre.org/techniques/T1033/) | `whoami` revealed the account running the web service. |
| Discovery | File and Directory Discovery | [T1083](https://attack.mitre.org/techniques/T1083/) | `ls` mapped the directory contents. |
| Discovery | System Information Discovery | [T1082](https://attack.mitre.org/techniques/T1082/) | `uname` gathered kernel and architecture details. |
| Credential Access | OS Credential Dumping: /etc/passwd and /etc/shadow | [T1003.008](https://attack.mitre.org/techniques/T1003/008/) | `cat /etc/passwd` and `cat /etc/shadow` targeted account and password hash files. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Created the case, took ownership, and started the playbook.
- [x] Recorded the alert details (hostname, IPs, method, URL).
- [x] Checked the source IP reputation in VirusTotal, AbuseIPDB, and Cisco Talos.
- [x] Reviewed the raw logs and identified the injected commands.
- [x] Confirmed the attack succeeded through the web server's terminal history.
- [x] Contained `WebServer1004` through Endpoint Management.
- [x] Escalated the ticket to Tier 2 as the playbook directs.
- [x] Added the analyst note and IoCs, then closed the ticket as a true positive.
- [ ] Recommended follow-up: reset compromised credentials and enforce strong passwords.
- [ ] Recommended follow-up: block the source IP at the firewall and fix the input validation flaw in the web application.
- [ ] Recommended follow-up: Tier 2 should check the server for web shells, new accounts, and other changes made after the compromise.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Command Injection Analysis](https://img.shields.io/badge/Command%20Injection%20Analysis-1E90FF?style=for-the-badge&labelColor=0D1117)
![IP Reputation Analysis](https://img.shields.io/badge/IP%20Reputation%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Raw Log Analysis](https://img.shields.io/badge/Raw%20Log%20Analysis-0284C7?style=for-the-badge&labelColor=0D1117)
![Host Containment](https://img.shields.io/badge/Host%20Containment-334155?style=for-the-badge&labelColor=0D1117)
![Escalation Procedures](https://img.shields.io/badge/Escalation%20Procedures-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
