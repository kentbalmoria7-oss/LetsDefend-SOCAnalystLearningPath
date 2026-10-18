<!-- ===================== HEADER ===================== -->
<div align="center">

# 🔨 Brute Force Attacks Challenge: Web, RDP, and SSH

### LetsDefend · Network Logs · Wireshark and Auth Log Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Brute+Force+Detection+in+a+PCAP;RDP+%26+Web+Login+Attack+Analysis;SSH+Auth+Log+Investigation;MITRE+ATT%26CK+T1110" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Network%20Forensics-1E90FF?style=for-the-badge&logo=wireshark&logoColor=white&labelColor=0D1117)
![Attack](https://img.shields.io/badge/Attack-Brute%20Force%20(T1110)-DC2626?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)
![Linux CLI](https://img.shields.io/badge/Linux%20CLI%20(grep)-FCC624?style=flat-square&logo=linux&logoColor=black)
![RDP](https://img.shields.io/badge/RDP-0078D4?style=flat-square&logo=windows&logoColor=white)
![SSH](https://img.shields.io/badge/SSH-0F172A?style=flat-square&logo=gnubash&logoColor=00D4FF)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/9ece6e514cd2f2e74d597a8cbaec2c6112e9a2d5/Web%20Investigation%20Picture/9/brute.png?raw=true" alt="Brute force attacks illustration" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Scenario and Setup](#-scenario-and-setup)
4. [Challenge Questions](#-challenge-questions)
5. [Findings Summary](#-findings-summary)
6. [Evidence by Service](#-evidence-by-service)
7. [Wireshark Filters and Commands Used](#-wireshark-filters-and-commands-used)
8. [Indicators of Compromise](#-indicators-of-compromise-iocs)
9. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
10. [Response Actions](#-response-actions)
11. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Brute Force Attacks |
| 🏫 **Platform** | LetsDefend |
| 🗂️ **Evidence** | `BruteForce.7z` (`/root/Desktop/ChallengeFile/BruteForce.7z`), which holds a **pcap file** and an **authentication log** |
| 🎯 **Targeted Server** | `51.116.96.181` |
| 🔗 **Targeted Page** | `index.php` (web login page) |
| 🔑 **Valid Web Credentials** | `web-hacker:admin12345` |
| 🖥️ **RDP Accounts Brute-Forced** | 7 |
| 💻 **Attacker's Client Name** | `t3m0-virtual-ma` |
| 🔐 **Last Successful SSH Login** | `mmox` at `11:43:54` |
| ❌ **Failed SSH Attempts** | 7480 |
| 🗺️ **Technique** | `T1110` (Brute Force) |
| 🧰 **Tools Used** | 7z, Wireshark, Linux terminal (`grep`, `wc`) |
| ⚖️ **Conclusion** | 🚨 **Brute force attacks confirmed against web, RDP, and SSH** |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🔨 **Brute force attack:** a trial-and-error method where automated software or scripts systematically test millions of character, password, or encryption key combinations until the correct one is found.

| Type | Explanation |
| :--- | :--- |
| 🔤 **Simple brute force** | Automated tools run endless random character combinations against a login or PIN. |
| 📖 **Dictionary attack** | Software tests a pre-made list of common words, phrases, dates, and leaked passwords instead of random strings. |
| 🔁 **Credential stuffing** | Attackers take username and password pairs stolen in one data breach and automatically test them on unrelated websites. |
| 🌧️ **Password spraying (reverse brute force)** | An attacker tries one known password across hundreds or thousands of accounts, to avoid triggering lockout limits. |
| 🧬 **Hybrid attack** | Combines dictionary words with predictable number or symbol variations (for example `Password2026!`). |

> 📦 **What is a `.7z` file?** A compressed archive format that bundles multiple files and folders into one container to save space or make sharing easier, similar to a standard `.zip` or `.rar` file.

---

<!-- ===================== SCENARIO ===================== -->
## 📖 Scenario and Setup

The web server has been compromised, and the task is to investigate the breach: dive into the system, analyze logs, dissect network traffic, and uncover clues to identify the attacker and the extent of the damage.

> 🗂️ **File location:** `/root/Desktop/ChallengeFile/BruteForce.7z`

### 🛠️ Extracting the Files

Unzip the archive to get the two artifacts used in the investigation.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/476f7e4c892709a37178237f74a29ebc7c115371/Web%20Investigation%20Picture/9/unzip%20file.png?raw=true" alt="Unzipping the BruteForce archive" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/476f7e4c892709a37178237f74a29ebc7c115371/Web%20Investigation%20Picture/9/after%20unzip.png?raw=true" alt="The extracted contents of the archive" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/476f7e4c892709a37178237f74a29ebc7c115371/Web%20Investigation%20Picture/9/the%20file.png?raw=true" alt="The pcap file and the authentication log" width="85%" />
</div>

| Artifact | Used For |
| :--- | :--- |
| 📡 **PCAP file** | Web and RDP brute force analysis in Wireshark |
| 📜 **Authentication log (`auth.log`)** | SSH login analysis with `grep` |

---

<!-- ===================== QUESTIONS ===================== -->
## 🎯 Challenge Questions

---

### 1️⃣ Q1: What is the IP address of the server targeted by the attacker's brute-force attack?

I opened the pcap in Wireshark.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/476f7e4c892709a37178237f74a29ebc7c115371/Web%20Investigation%20Picture/9/the%20wireshark.png?raw=true" alt="The pcap file opened in Wireshark" width="85%" />
</div>

| Observation | Details |
| :--- | :--- |
| 🖥️ **RDP connections** | The first thing I noticed was multiple RDP connections from one IP address to a private IP address, which should be the web server this capture came from. |
| 🌐 **The external IP** | When I filtered for the external IP address, it turned out to be a web server that was also brute-forced through its login page. |

### 🎯 Finding / Answer

```text
51.116.96.181
```

---

### 2️⃣ Q2: Which directory was targeted by the attacker's brute-force attempt?

I filtered the capture for the IP address of the targeted server.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/99a1d50e516eda3705b3a9bf9eb6f20a9b2336b5/Web%20Investigation%20Picture/9/ip%20traegted.png?raw=true" alt="Packets filtered by the targeted server's IP address" width="85%" />
</div>

### 🎯 Finding / Answer

```text
index.php
```

> 📄 **About `index.php`:** the main entry point or default landing page of a website or web application on a PHP server.
>
> | Role | Explanation |
> | :--- | :--- |
> | 🏠 **Default homepage** | When a user visits a domain or folder, the server automatically looks for and runs an `index.php` (or `index.html`) file if there is one. |
> | 🚦 **Front controller** | In many modern frameworks and content management systems (CMS) like WordPress or Joomla, `index.php` acts as a central router. It intercepts incoming requests, sets up the environment, loads the core configuration, and sends the user to the right page. |

---

### 3️⃣ Q3: Identify the correct username and password combination used for login.

I first filtered for HTTP response code `302` (redirect), since most websites redirect to a dashboard after a successful login, but there was **no `302`** in this capture. So I looked for what separates a successful login from a failed one.

I found one HTTP response that was **1 byte smaller** than the rest. When I inspected it, that attempt turned out to be the successful login.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/99a1d50e516eda3705b3a9bf9eb6f20a9b2336b5/Web%20Investigation%20Picture/9/internet%20protocol.png?raw=true" alt="The HTTP response with a different size" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/99a1d50e516eda3705b3a9bf9eb6f20a9b2336b5/Web%20Investigation%20Picture/9/follow%20tcp%20stream.png?raw=true" alt="Following the TCP stream of the successful attempt" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/99a1d50e516eda3705b3a9bf9eb6f20a9b2336b5/Web%20Investigation%20Picture/9/username%20and%20password.png?raw=true" alt="The valid credentials accepted by the web server" width="85%" />
</div>

### 🎯 Finding / Answer

```text
web-hacker:admin12345
```

---

### 4️⃣ Q4: How many user accounts did the attacker attempt to compromise via RDP brute force?

Filtering for `tpkt` (the framing layer that wraps RDP traffic), the **Negotiation Request** packets show a user account name. Using that, I could list every account requested from this IP address.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b7b667fc69a5cac8d0ccedf1a3c93b7c15f10c1a/Web%20Investigation%20Picture/9/tpkt.png?raw=true" alt="RDP traffic filtered with tpkt" width="85%" />
</div>

With the filter `rdp.neg_type == 0x01`, I found **10 unique users** that sent negotiation requests:

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/99a1d50e516eda3705b3a9bf9eb6f20a9b2336b5/Web%20Investigation%20Picture/9/rdp.png?raw=true" alt="The unique users in RDP negotiation requests" width="85%" />
</div>

```text
t3m0
Mosalah
Messi
web-hacker
Kareem
Mostafa
mmox
Mohamed
Ali
Mohsen
```

Of these, only **7** were actually brute-forced.

### 🎯 Finding / Answer

```text
7
```

---

### 5️⃣ Q5: What is the `clientName` of the attacker's machine?

The `rdp.client.name` filter shows the client name of the attacker's machine.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b7b667fc69a5cac8d0ccedf1a3c93b7c15f10c1a/Web%20Investigation%20Picture/9/client%20name.png?raw=true" alt="The RDP client name of the attacker's machine" width="85%" />
</div>

### 🎯 Finding / Answer

```text
t3m0-virtual-ma
```

---

### 6️⃣ Q8: When did the user last log in successfully via SSH, and who was it?

In the authentication log, I searched for successful logins.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b7b667fc69a5cac8d0ccedf1a3c93b7c15f10c1a/Web%20Investigation%20Picture/9/failed%20pass%20find.png?raw=true" alt="The authentication log" width="85%" />
</div>

In Bash, I filtered for all successful authentications and took the **last record** in the results:

```bash
grep -i "accepted" auth.log
```

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b7b667fc69a5cac8d0ccedf1a3c93b7c15f10c1a/Web%20Investigation%20Picture/9/accepted%20bash.png?raw=true" alt="Running grep for accepted logins" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b7b667fc69a5cac8d0ccedf1a3c93b7c15f10c1a/Web%20Investigation%20Picture/9/accepted%20result%20bash.png?raw=true" alt="The last accepted login in the results" width="85%" />
</div>

### 🎯 Finding / Answer

Answer format: `username:time (HH:MM:SS)`

```text
mmox:11:43:54
```

---

### 7️⃣ Q9: How many unsuccessful SSH connection attempts were made by the attacker?

At first I thought I had to identify the attacker's IP address and then filter for failed attempts, but it is simpler than that: counting every failed password line in the authentication log answers the question.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b7b667fc69a5cac8d0ccedf1a3c93b7c15f10c1a/Web%20Investigation%20Picture/9/failed%20pass%20find.png?raw=true" alt="Failed password entries in the authentication log" width="85%" />
</div>

```bash
grep -i "failed password" auth.log | wc -l
```

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b7b667fc69a5cac8d0ccedf1a3c93b7c15f10c1a/Web%20Investigation%20Picture/9/failed%20pass%20basj.png?raw=true" alt="Counting the failed password lines" width="85%" />
</div>

### 🎯 Finding / Answer

```text
7480
```

---

### 8️⃣ Q10: What technique is used to gain access?

Since this is a brute force attack, the matching MITRE ATT&CK technique ID can be found on the official MITRE ATT&CK website, `https://attack.mitre.org/`.

### 🎯 Finding / Answer

```text
T1110
```

---

<!-- ===================== FINDINGS SUMMARY ===================== -->
## 📊 Findings Summary

| # | Question | Answer |
| :-: | :--- | :--- |
| 1️⃣ | IP address of the server targeted by the brute force | `51.116.96.181` |
| 2️⃣ | Directory targeted by the brute force | `index.php` |
| 3️⃣ | Correct username and password | `web-hacker:admin12345` |
| 4️⃣ | User accounts attacked through RDP brute force | `7` |
| 5️⃣ | Client name of the attacker's machine | `t3m0-virtual-ma` |
| 8️⃣ | Last successful SSH login (user and time) | `mmox:11:43:54` |
| 9️⃣ | Unsuccessful SSH attempts | `7480` |
| 🔟 | MITRE ATT&CK technique used to gain access | `T1110` |

<br/>

<div align="center">

## 🚨 VERDICT: BRUTE FORCE ATTACKS CONFIRMED 🚨

</div>

### 🎯 Overall Conclusion

In this challenge, I investigated network traffic with Wireshark to identify the attacker's activity and detect brute force attacks on the web server and on RDP. I also investigated the authentication log to find the last successful SSH login and how many failed login attempts happened before the attacker gained a foothold on the web server.

---
> 🖥️ **What is RDP?** RDP stands for **Remote Desktop Protocol**, a proprietary network communication technology developed by Microsoft.

### ⚙️ What RDP Does

| Function | Explanation |
| :--- | :--- |
| 🎮 **Remote control** | Lets a user view and control a physical computer or virtual machine over a network or the internet as if they were sitting directly in front of it. |
| 🖼️ **Graphical interface** | Sends the screen output from the host server to the client, and sends mouse and keyboard input back. |
| 🔌 **Port and protocol** | By default, it runs over TCP and UDP port `3389`. |

### 🔨 Why RDP Matters in a Brute Force Investigation

| Point | Explanation |
| :--- | :--- |
| 🎯 **A common target** | An RDP login gives full control of a machine, so attackers try many username and password guesses against it. |
| 👀 **How it shows up in Wireshark** | The `tpkt` filter shows the framing layer that wraps RDP traffic, and `rdp.neg_type == 0x01` shows the connection requests that name the account being tried. |
| 🪪 **What it reveals** | The negotiation requests expose the usernames being attacked (10 in this capture), and `rdp.client.name` exposes the attacker's machine name. |

---

<!-- ===================== EVIDENCE BY SERVICE ===================== -->
## 🧩 Evidence by Service

| Service | Where I Looked | What I Found |
| :--- | :--- | :--- |
| 🌐 **Web login** | PCAP: HTTP POST requests and response sizes | Repeated login attempts against `index.php`. One response was 1 byte smaller than the rest, and it carried the valid credentials `web-hacker:admin12345`. |
| 🖥️ **RDP** | PCAP: `tpkt` and `rdp.neg_type == 0x01` | 10 usernames appear in the negotiation requests, and 7 accounts were brute-forced. The attacker's client name is `t3m0-virtual-ma`. |
| 🔐 **SSH** | `auth.log` | 7480 failed password entries, and the last successful login was `mmox` at `11:43:54`. |

---
<!-- ===================== FILTERS AND COMMANDS ===================== -->
## 🛠️ Wireshark Filters and Commands Used

### 🦈 Wireshark Display Filters

| Filter | What It Does | Why It's Used |
| :--- | :--- | :--- |
| `http.post` | Shows only HTTP POST requests. | POST is how browsers send data to a server, such as login credentials, uploads, and forms. Analysts use it to inspect what clients send to web servers. |
| `http.response.code` | Shows packets with an HTTP response status code (200, 404, 403, 500, and so on). | Quickly checks the status of server replies. It can be narrowed, for example `http.response.code == 404`. |
| `tpkt` | Shows TPKT packets (ISO transport service over TCP). | TPKT provides reliable transport and framing for upper-layer protocols, and is most often seen wrapping RDP traffic. |
| `rdp.neg_type == 0x01` | Shows RDP connection request (negotiation request) packets. | During an RDP handshake, the client sends a negotiation request to set up security and features, so this filter marks when a connection attempt begins. |
| `rdp.client.name` | Shows the RDP client name field. | Reveals the name of the connecting machine. |

### 🐧 Linux Commands

| Command | What It Does |
| :--- | :--- |
| `7z x BruteForce.7z` | Extracts (`x`) all files from the compressed archive. |
| `grep -i "accepted" auth.log` | Searches the authentication log for the word "accepted", ignoring case (`-i`). Used to find successful SSH or system logins. |
| `grep -i "failed password" auth.log \| wc -l` | Finds every "failed password" line, ignoring case, and counts the lines (`wc -l`). A quick check of whether a system was hit by a brute force attack. |

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🌐 Targeted server | `51.116.96.181` | PCAP | 🟠 Web server brute-forced through its login page |
| 🔗 Targeted page | `index.php` | PCAP | 🟠 Login endpoint attacked |
| 🔑 Valid credentials | `web-hacker:admin12345` | PCAP HTTP stream | 🔴 Accepted by the web server |
| 💻 Attacker's RDP client name | `t3m0-virtual-ma` | PCAP RDP filter | 🔴 Attacker's machine |
| 👤 SSH account | `mmox` (last login `11:43:54`) | `auth.log` | 🟠 Last successful SSH login |
| ❌ Failed SSH attempts | 7480 | `auth.log` | 🔴 Brute force volume |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Credential Access | Brute Force | [T1110](https://attack.mitre.org/techniques/T1110/) | The lab's answer: repeated login attempts against web, RDP, and SSH. |
| Credential Access | Brute Force: Password Guessing | [T1110.001](https://attack.mitre.org/techniques/T1110/001/) | The attacker guessed passwords until one worked, such as `admin12345`. |
| Lateral Movement | Remote Services: Remote Desktop Protocol | [T1021.001](https://attack.mitre.org/techniques/T1021/001/) | RDP connections to accounts on the private server. |
| Initial Access | Valid Accounts | [T1078](https://attack.mitre.org/techniques/T1078/) | A brute-forced account (`web-hacker`) was accepted by the web server. |

> ⚠️ Only **T1110** comes from the lab. The other three are my mapping from the evidence.

---

<!-- ===================== RESPONSE ===================== -->
## ✅ Response Actions

- [x] Extracted the archive and identified the pcap and the authentication log.
- [x] Identified the targeted server and the targeted page in the pcap.
- [x] Found the successful web login by comparing response sizes.
- [x] Counted the RDP accounts that were brute-forced and found the attacker's client name.
- [x] Found the last successful SSH login and the failed SSH attempt count in `auth.log`.
- [x] Mapped the activity to MITRE ATT&CK T1110.
- [ ] Recommended follow-up: reset the exposed passwords (`web-hacker`, `mmox`, and the brute-forced RDP accounts) and enforce strong passwords and MFA.
- [ ] Recommended follow-up: add account lockout and rate limiting on the web login, RDP, and SSH, and don't expose RDP directly to the internet.
- [ ] Recommended follow-up: block the attacker's address and alert on repeated failed logins in the SIEM.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![Wireshark PCAP Analysis](https://img.shields.io/badge/Wireshark%20PCAP%20Analysis-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Brute Force Detection](https://img.shields.io/badge/Brute%20Force%20Detection-1E90FF?style=for-the-badge&labelColor=0D1117)
![RDP Traffic Analysis](https://img.shields.io/badge/RDP%20Traffic%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Auth Log Analysis](https://img.shields.io/badge/Auth%20Log%20Analysis-0284C7?style=for-the-badge&labelColor=0D1117)
![Linux Command Line](https://img.shields.io/badge/Linux%20Command%20Line-334155?style=for-the-badge&labelColor=0D1117)
![MITRE ATT&CK Mapping](https://img.shields.io/badge/MITRE%20ATT%26CK%20Mapping-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
