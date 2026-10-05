<!-- ===================== HEADER ===================== -->
<div align="center">

# 🕸️ LetsDefend: Investigate Web Attack Walkthrough

### Web Server Log Analysis · Attack Chain Reconstruction

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Web+Access+Log+Analysis;Recon+%E2%86%92+Brute+Force+%E2%86%92+Code+Injection;Persistence+Payload+Decoding+with+CyberChef;Reconstructing+the+Attack+Chain" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Web%20Attack%20Investigation-1E90FF?style=for-the-badge&logo=nginx&logoColor=white&labelColor=0D1117)
![Attacks](https://img.shields.io/badge/Attacks%20Found-4-DC2626?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![Log Analysis](https://img.shields.io/badge/Log%20Analysis-0F172A?style=flat-square&logo=files&logoColor=00D4FF)
![Nikto](https://img.shields.io/badge/Nikto-334155?style=flat-square&logo=hackthebox&logoColor=white)
![CyberChef](https://img.shields.io/badge/CyberChef-1E90FF?style=flat-square&logo=codechef&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/images.jpg?raw=true" alt="LetsDefend Investigate Web Attack challenge overview" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Overview](#-overview)
3. [Log Basics](#-log-basics)
4. [Challenge Questions](#-challenge-questions)
5. [Attack Chain Summary](#-attack-chain-summary)
6. [Indicators of Compromise](#-indicators-of-compromise-iocs)
7. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
8. [Log Analysis Learning Guide](#-log-analysis-learning-guide)
9. [Response Actions](#-response-actions)
10. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Investigate Web Attack |
| 🏫 **Platform** | LetsDefend |
| 🗂️ **Evidence** | Web server access logs |
| 🔎 **Recon Tool** | Nikto |
| 🧭 **Discovery Technique** | Directory brute force |
| 🔑 **Third Attack** | Brute force against a login form (successful) |
| 💉 **Fourth Attack** | Code injection |
| 🪪 **First Payload** | `whoami` |
| 🕳️ **Persistence Clue** | Payload that adds a local user named `hacker` |
| 🧰 **Tools** | Access logs, CyberChef |

---

<!-- ===================== OVERVIEW ===================== -->
## 📖 Overview

This walkthrough hunts through web server logs for suspicious activity that may point to an attack. The goal is to read the logs, work out each stage of the attack in order, and answer the challenge questions.

---

<!-- ===================== LOG BASICS ===================== -->
## 🧾 Log Basics

> 📘 **What is a log?** Logs are automated digital files that record events, user actions, and system errors happening across a network, device, or application.

Logs matter in cybersecurity because they act as a detailed digital trail, like a "receipt," of every action, event, and connection across computers, networks, and applications. Think of a log as a digital security guard's notebook: every time a user logs in, a firewall blocks a port, or an application throws an error, the system writes down exactly what happened.

### 🧬 Core Anatomy of a Log Entry

| Field | What It Tells You |
| :--- | :--- |
| 🕒 **Timestamp** | The exact date and time the event happened (crucial for time zone alignment). |
| 🌐 **Source IP / Host** | Where the activity or request originated from. |
| 🎯 **Destination IP / Port** | Where the traffic or action was headed. |
| ⚙️ **Event Type / Action** | What took place (for example `login_failed`, `connection_allow`, `file_modify`). |
| 👤 **User / Account Identity** | The specific account or service name tied to the action. |
| 🚦 **Status / Severity Code** | Whether the action succeeded, failed, or triggered a warning (HTTP 200, 404, 500, or severity levels like Info, Warning, Error). |

### 🗂️ Primary Categories of Security Logs

| Category | What It Records |
| :--- | :--- |
| 🔥 **Network & Firewall Logs** | Incoming and outgoing connections, allowed or dropped packets, and DNS queries. |
| 🔑 **Authentication & Access Logs** | User logins, logouts, failed password attempts, and privilege escalations. |
| 💻 **Endpoint & System Logs** | OS activity, process creations, file modifications, and driver loads (such as Windows Event Logs). |
| ☁️ **Application & Cloud Logs** | Web server requests (like Nginx or Apache access logs), API calls, and cloud resource provisioning. |

---

<!-- ===================== QUESTIONS ===================== -->
## 🎯 Challenge Questions

With the basics covered, the challenge questions can be answered from the access logs.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/access%20logs.png?raw=true" alt="Web server access logs opened for analysis" width="85%" />
</div>

---

### 1️⃣ Question 1: Which automated scan tool did the attacker use for web reconnaissance?

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/nikto.png?raw=true" alt="Access log entries showing Nikto scanner activity" width="85%" />
</div>

### 🎯 Finding / Answer

```text
Nikto
```

> 🔍 **About Nikto:** Nikto is a free, open-source command-line tool that scans web servers for dangerous files, outdated software, and security misconfigurations.

---

### 2️⃣ Question 2: After web reconnaissance, which technique did the attacker use for directory listing discovery?

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/directory.png?raw=true" alt="Access log entries showing many 404 responses from directory guessing" width="85%" />
</div>

### 🎯 Finding / Answer

```text
Directory brute force
```

> 🔍 **About it:** Directory brute-forcing is a reconnaissance technique used to discover hidden files, folders, and administrative endpoints on a web server.

**Why you see 404 in the logs:** when an attacker uses an automated directory brute-forcing tool (like Gobuster, Dirbuster, or Dirb), the tool guesses hundreds or thousands of common directory names.

| Log Behavior | Explanation |
| :--- | :--- |
| ❌ **The 404 response** | The attacker is guessing blindly, so most of these directories do not exist. The server correctly returns `404 Not Found` for every failed guess. |
| 👤 **Client-side errors** | 4xx codes are classed broadly as client-side errors, meaning the issue came from the sender's request, not the server's stability. |

---

### 3️⃣ Question 3: What is the third attack type after directory listing discovery?

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/brute%20forces.png?raw=true" alt="Access log entries showing repeated POST requests to a login page" width="85%" />
</div>

### 🎯 Finding / Answer

```text
Brute force
```

> 🔍 **About it:** brute force means using massive raw effort, such as repeated guesses, instead of skill or a planned approach.

**Why the logs point to a login brute force:**

| Indicator | Explanation |
| :--- | :--- |
| 📮 **POST requests** | Login forms almost always use the POST method to send credentials (username and password) in the request body, rather than exposing them in the URL. |
| ✅ **200 OK status code** | Many modern web applications return `200 OK` even when a login fails. They re-render the login page with a small error message such as "Invalid username or password". |
| 📏 **Similar response sizes** | Because the same login page is re-rendered over and over, the response size stays almost identical across all requests. |

> 💡 If a login had succeeded, you would usually expect a `302 Redirect` (sending the user to a dashboard) or a noticeably different response size caused by a new landing page or a session cookie.

---

### 4️⃣ Question 4: Is the third attack successful?

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/302.png?raw=true" alt="Access log entries where the response size and status code change to 302" width="85%" />
</div>

### 🎯 Finding / Answer

```text
Yes
```

The attacker consistently received a `200` status code with a similar response size of `4,086` bytes, which means the login attempts failed. However, around line `12,546` the response sizes change, reaching `23,369` and `12,759` with a `302` status code. This suggests the attacker gained access, so the brute force was **successful**.

---

### 5️⃣ Question 5: What is the name of the fourth attack?

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/whoami.png?raw=true" alt="Access log entries showing a command in the request" width="85%" />
</div>

### 🎯 Finding / Answer

```text
Code injection
```

> 🔍 **About it:** the attacker tried to slip malicious commands, scripts, or database queries into input fields, URLs, or HTTP headers, which then appear in the server's request records.

---

### 6️⃣ Question 6: What is the first payload for the fourth attack?

### 🎯 Finding / Answer

```text
whoami
```

> 🔍 **About payloads:** a payload in cybersecurity is the malicious code or component of an attack that carries out the harmful action after access is gained.

> 🔍 **About `whoami`:** it comes from the standard command-line tool `whoami` ("Who am I?"). In a log, it answers a basic security question: which user or system account is executing commands. Attackers often run it first to learn what level of access they have.

---

### 7️⃣ Question 7: Is there any persistence clue for the victim machine in the log file? If yes, what is the related payload?

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/cyberchef.png?raw=true" alt="Encoded payload found in the access logs" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dac19efaf9c4647adc2c00206c5b66447d115cb2/Web%20Investigation%20Picture/cyberchef%202.png?raw=true" alt="CyberChef decoding the payload" width="85%" />
</div>

### 🎯 Finding / Answer

Yes. The logs show a payload that creates a new local account:

```text
%27net%20user%20hacker%20asd123!!%20/add%27
```

Decoded in CyberChef, it reads:

```text
'net user hacker asd123!! /add'
```

> 🕳️ **Why this is persistence:** the command adds a new local user named `hacker` to the victim machine. With its own account, the attacker can log back in later even if the original entry point is closed.

---

<!-- ===================== ATTACK CHAIN ===================== -->
## ⛓️ Attack Chain Summary

| Stage | Attack | Evidence in the Logs | Result |
| :-: | :--- | :--- | :--- |
| 1️⃣ | **Web reconnaissance** | Nikto scanner activity | Server probed for weaknesses |
| 2️⃣ | **Directory listing discovery** | Many `404 Not Found` responses | Hidden paths searched by guessing |
| 3️⃣ | **Brute force** | Repeated `POST` requests with `200` responses of size `4,086`, then a change to `302` | ✅ Successful, access gained |
| 4️⃣ | **Code injection** | `whoami` payload in requests | Commands executed on the target |
| 5️⃣ | **Persistence** | Payload `net user hacker asd123!! /add` | New local user `hacker` created |

```text
 Recon (Nikto) ──▶ Directory brute force ──▶ Login brute force ──▶ Code injection ──▶ Persistence
```

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🔎 Scanner | Nikto activity | Access logs | 🔴 Malicious reconnaissance |
| 🔑 Login brute force | Repeated `POST` requests followed by a `302` response | Access logs | 🔴 Malicious, successful |
| 💉 Injected command | `whoami` | Access logs | 🔴 Malicious |
| 👤 Persistence payload | `net user hacker asd123!! /add` | Access logs, CyberChef | 🔴 Malicious |
| 🪪 Created account | `hacker` | Decoded payload | 🔴 Attacker-controlled local account |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Reconnaissance | Active Scanning: Vulnerability Scanning | [T1595.002](https://attack.mitre.org/techniques/T1595/002/) | Nikto scanned the web server for weaknesses. |
| Reconnaissance | Active Scanning: Wordlist Scanning | [T1595.003](https://attack.mitre.org/techniques/T1595/003/) | Directory brute forcing guessed common paths. |
| Credential Access | Brute Force | [T1110](https://attack.mitre.org/techniques/T1110/) | Repeated login attempts eventually succeeded. |
| Initial Access | Exploit Public-Facing Application | [T1190](https://attack.mitre.org/techniques/T1190/) | Code injection through the web application. |
| Execution | Command and Scripting Interpreter | [T1059](https://attack.mitre.org/techniques/T1059/) | Operating system commands were run through the injection. |
| Discovery | System Owner/User Discovery | [T1033](https://attack.mitre.org/techniques/T1033/) | `whoami` revealed the executing account. |
| Persistence | Create Account: Local Account | [T1136.001](https://attack.mitre.org/techniques/T1136/001/) | `net user hacker ... /add` created a new local account. |

---

<!-- ===================== LEARNING GUIDE ===================== -->
## 📚 Log Analysis Learning Guide

Analyzing logs is one of the most critical skills in cybersecurity. It is the "digital detective work" that uncovers breaches, tracks insider threats, and proves compliance.

### Phase 1: Build the Foundation

| Step | What to Do |
| :--- | :--- |
| 📖 **Learn core log formats** | Windows Event Logs (Security, System, Application), Linux Syslog (`/var/log/syslog`, `/var/log/auth.log`), web server logs (Apache, Nginx, IIS), and network logs (firewall, DNS, NetFlow). |
| ⚖️ **Understand normal behavior** | You cannot spot an anomaly without a baseline, so study logs from a healthy, uncompromised system. |
| 🐧 **Master Linux command-line tools** | Practice `grep` (filtering), `awk` (parsing fields), `sed` (editing text streams), `cut`, `sort`, and `uniq`. |

### Phase 2: Develop a Methodical Process

| # | Step | Details |
| :-: | :--- | :--- |
| 1️⃣ | **Define the objective** | Know exactly what you are looking for: a specific IP, an altered account, or an incident timeline. |
| 2️⃣ | **Filter out the noise** | Remove known, benign traffic such as automated backups or health checks. |
| 3️⃣ | **Identify anomalies** | Look for volume spikes, unusual timestamps (like admin logins at 3:00 AM), or uncommon process executions. |
| 4️⃣ | **Correlate across sources** | Never rely on one log source. Combine firewall alerts with host logs and Active Directory logs to build the full story. |

### Phase 3: Sharpen Your Analysis Skills

| Step | What to Do |
| :--- | :--- |
| 🔎 **Learn a SIEM query language** | Get comfortable with KQL (Microsoft Sentinel), Splunk SPL, or Lucene (Elasticsearch). |
| 🗺️ **Study MITRE ATT&CK** | Map log events to adversary tactics and techniques, such as Credential Access versus Lateral Movement. |
| 🧪 **Practice with real-world data** | Use platforms like Boss of the SOC (BOTS) by Splunk, CyberDefenders, or LetsDefend. |

### 💡 Pro-Tips for Efficient Log Analysis

| Tip | Why It Helps |
| :--- | :--- |
| 🔟 **Focus on the "Top 5" fields** | Scan for Timestamp, Source IP/User, Destination IP/Resource, Action/Event ID, and Status (Success/Failure). |
| 🕒 **Beware of time zones** | Check whether logs use UTC or local time. A one-hour mismatch can break an incident timeline. |
| 🧰 **Look for "Living off the Land"** | Attackers often use built-in tools. Watch for unusual use of `PowerShell.exe`, `wmic.exe`, `vssadmin.exe`, or bash execution by non-admin users. |
| 📝 **Document your queries** | Save useful regex and SIEM queries in a personal cheat sheet so you can reuse them. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Reviewed the web server access logs and identified the attack stages in order.
- [x] Identified Nikto as the reconnaissance tool and directory brute force as the discovery technique.
- [x] Confirmed the login brute force was successful from the status code and response size change.
- [x] Identified code injection and its first payload, `whoami`.
- [x] Decoded the persistence payload in CyberChef and identified the new `hacker` account.
- [ ] Recommended follow-up: disable and remove the `hacker` account, and reset the compromised credentials.
- [ ] Recommended follow-up: add login rate limiting, account lockout, and MFA to stop brute force.
- [ ] Recommended follow-up: validate and sanitize input in the web application to stop code injection.
- [ ] Recommended follow-up: review the server for other changes made after the successful login.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![Web Log Analysis](https://img.shields.io/badge/Web%20Log%20Analysis-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Attack Chain Reconstruction](https://img.shields.io/badge/Attack%20Chain%20Reconstruction-1E90FF?style=for-the-badge&labelColor=0D1117)
![HTTP Status Code Analysis](https://img.shields.io/badge/HTTP%20Status%20Code%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Payload Decoding](https://img.shields.io/badge/Payload%20Decoding-0284C7?style=for-the-badge&labelColor=0D1117)
![Brute Force Detection](https://img.shields.io/badge/Brute%20Force%20Detection-334155?style=for-the-badge&labelColor=0D1117)
![MITRE ATT&CK Mapping](https://img.shields.io/badge/MITRE%20ATT%26CK%20Mapping-475569?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
