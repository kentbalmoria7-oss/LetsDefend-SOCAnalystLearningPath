<!-- ===================== HEADER ===================== -->
<div align="center">

# 🐚 Shellshock Attack: PCAP Investigation

### LetsDefend · Network Logs · Wireshark Analysis of a Bash Exploit Attempt

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Shellshock+Exploit+Detection+in+a+PCAP;HTTP+Stream+%26+User-Agent+Payload+Analysis;CGI+Script+as+the+Attack+Vector;Server+Fingerprinting+from+HTTP+Headers" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Network%20Log%20Analysis-1E90FF?style=for-the-badge&logo=wireshark&logoColor=white&labelColor=0D1117)
![Attack](https://img.shields.io/badge/Attack-Shellshock%20(Bash)-DC2626?style=for-the-badge&logo=gnubash&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)
![HTTP](https://img.shields.io/badge/HTTP%20Stream%20Analysis-0F172A?style=flat-square&logo=files&logoColor=00D4FF)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white)
![Apache](https://img.shields.io/badge/Apache-D22128?style=flat-square&logo=apache&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/630516b532968b25a4c876cd6747a206a963ced4/Network%20Logs%20Picture/1/shellshock-methodology.png?raw=true" alt="Shellshock attack methodology" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Start the Investigation](#-start-the-investigation)
4. [Follow the HTTP Stream](#-follow-the-http-stream)
5. [Challenge Questions](#-challenge-questions)
6. [Findings Summary](#-findings-summary)
7. [Attack Chain](#-attack-chain)
8. [Indicators of Compromise](#-indicators-of-compromise-iocs)
9. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
10. [Response Actions](#-response-actions)
11. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Shellshock Attack |
| 🏫 **Platform** | LetsDefend (Network Logs) |
| 🗂️ **Evidence** | A `.pcap` file opened in Wireshark |
| 🕵️ **Attacker (Client)** | `10.246.50.6` |
| 🎯 **Target (Server)** | `10.246.50.2` on port `80` (HTTP) |
| 🔗 **Attack Vector** | `GET /exploitable.cgi HTTP/1.1` (a CGI script) |
| 💉 **Payload Location** | The HTTP `User-Agent` header |
| 💥 **Exploit String** | `() { :;}; /bin/ping -c1 10.246.50.2` |
| 📉 **Server Response** | `500 Internal Server Error` |
| 🖥️ **Server OS** | Ubuntu |
| 🌐 **Web Server** | Apache/2.2.22 |
| 🧰 **Tools Used** | Wireshark (packet list, Follow HTTP/TCP stream) |
| ⚖️ **Conclusion** | ☣️ **Shellshock exploitation attempt** |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🐚 **Shellshock:** a major security flaw in the GNU Bash command shell that lets attackers run unauthorized commands on a computer from afar.

| Topic | Explanation |
| :--- | :--- |
| 📅 **The bug** | Discovered in September 2014, it affects Bash versions 1.03 through 4.3. |
| ⚙️ **How it works** | Bash allows functions to be stored in environment variables. The bug happens when Bash runs extra code that comes **after the end** of a stored function definition. |
| 🚪 **Common entry points** | Attackers used it against Common Gateway Interface (CGI) web scripts, OpenSSH servers, DHCP clients, and mail servers. |
| 💥 **Impact** | Once inside, attackers could steal data, install malware, or use the machine in a botnet to launch DDoS attacks. |

---

<!-- ===================== START INVESTIGATION ===================== -->
## 🔎 Start the Investigation

Open the `.pcap` file in Wireshark.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/7c3af91032e98a1fb325f4906a81fe16951fef17/Network%20Logs%20Picture/1/pcap.png?raw=true" alt="The PCAP file for the Shellshock challenge" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/7c3af91032e98a1fb325f4906a81fe16951fef17/Network%20Logs%20Picture/1/info.png?raw=true" alt="The packet list in Wireshark showing the IP addresses and protocols" width="85%" />
</div>

Once the capture is open, the IP addresses and protocols are visible in the packet list.

| Observation | Details |
| :--- | :--- |
| 🖥️ **The server** | The target is at `10.246.50.2`, listening on port `80` for HTTP traffic. |
| 🕵️ **The client (attacker)** | The system talking to the web server is at `10.246.50.6`. |
| 🎯 **The attack attempt** | In **packet 11**, the client sends `GET /exploitable.cgi HTTP/1.1`. CGI scripts were the main vector for the Shellshock vulnerability. |
| 📩 **The server response** | In **packet 15**, the server (`10.246.50.2`) answers with `HTTP/1.1 500 Internal Server Error (text/html)`. |
| 📡 **Suspicious ICMP activity** | Right after the GET request, there is an ICMP Echo (ping) request in **packet 13**, followed by an Echo reply in **packet 14**. In a Shellshock attack, attackers often inject a `ping` command into the HTTP headers to check whether the server is vulnerable and running arbitrary commands. |

### 🧭 How I Told the Attacker from the Server

I checked the **direction of the HTTP traffic** in the packet list:

| Role | How I Identified It |
| :--- | :--- |
| 🕵️ **Attacker (client)** | The system that starts the web request. In packet 11, the source IP `10.246.50.6` sends the malicious request. |
| 🖥️ **Server** | The system that receives the request and replies. In packet 11 the destination is `10.246.50.2`, and in packet 15 the same address is the source of the `500` error sent back to the attacker. |

---

<!-- ===================== FOLLOW THE HTTP STREAM ===================== -->
## 🔬 Follow the HTTP Stream

To learn more, I followed the HTTP/TCP stream to read the basic server information.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/7c3af91032e98a1fb325f4906a81fe16951fef17/Network%20Logs%20Picture/1/follow.png?raw=true" alt="Following the HTTP stream in Wireshark" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/7c3af91032e98a1fb325f4906a81fe16951fef17/Network%20Logs%20Picture/1/tcp.png?raw=true" alt="The TCP stream showing the request and response" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/7c3af91032e98a1fb325f4906a81fe16951fef17/Network%20Logs%20Picture/1/info%20last.png?raw=true" alt="HTTP stream showing the exploit payload and server headers" width="85%" />
</div>

The HTTP stream shows the exact mechanics of the Shellshock attempt.

| Item | Details |
| :--- | :--- |
| 🚪 **The vulnerable vector** | The attacker targets a CGI script with `GET /exploitable.cgi`. |
| 💉 **The payload location** | The malicious payload is placed directly in the HTTP `User-Agent` header. |
| 💥 **The exploit string** | `() { :;}; /bin/ping -c1 10.246.50.2` |
| 🖥️ **Server information** | The `500` response includes a `Server` header showing the target runs **Apache/2.2.22 (Ubuntu)**. |

### 🧩 Reading the Exploit String

```text
() { :;}; /bin/ping -c1 10.246.50.2
```

| Part | Meaning |
| :--- | :--- |
| `() { :;};` | The classic **Shellshock trigger**. It looks like an empty function definition, and Bash wrongly keeps running whatever comes after it. Web servers often map HTTP headers into environment variables, which is how Bash ends up parsing it. |
| `/bin/ping -c1 10.246.50.2` | The **arbitrary command** the attacker wants the server to run: send one ping (`-c1`) to the address `10.246.50.2`. |

> 📡 This explains the ICMP ping traffic seen in the packet capture.

---

<!-- ===================== QUESTIONS ===================== -->
## 🎯 Challenge Questions

### 1️⃣ Q1: What is the server operating system?

### 🎯 Finding / Answer

```text
Ubuntu
```

---

### 2️⃣ Q2: What is the application server and version running on the target system?

### 🎯 Finding / Answer

```text
Apache/2.2.22
```

---

### 3️⃣ Q3: What is the exact command the attacker wants to run on the target server?

### 🎯 Finding / Answer

```text
/bin/ping -c1 10.246.50.2
```

---

<!-- ===================== FINDINGS SUMMARY ===================== -->
## 📊 Findings Summary

| # | Question | Answer |
| :-: | :--- | :--- |
| 1️⃣ | Server operating system | `Ubuntu` |
| 2️⃣ | Application server and version | `Apache/2.2.22` |
| 3️⃣ | Command the attacker wants to run | `/bin/ping -c1 10.246.50.2` |

<br/>

<div align="center">

## ☣️ VERDICT: SHELLSHOCK EXPLOITATION ATTEMPT ☣️

</div>

### 🎯 Overall Conclusion

This pcap captured a moment where the client `10.246.50.6` used the **Shellshock** vulnerability against an **Apache/2.2.22 web server on Ubuntu** (`10.246.50.2`). The payload was hidden in the `User-Agent` header of a request to the CGI script `/exploitable.cgi`, and it tried to make the server run a `ping` command.

---

<!-- ===================== ATTACK CHAIN ===================== -->
## ⛓️ Attack Chain

| Stage | What Happens | Evidence |
| :-: | :--- | :--- |
| 1️⃣ | **Target selection:** the attacker picks a CGI script on the web server. | `GET /exploitable.cgi HTTP/1.1` (packet 11) |
| 2️⃣ | **Payload delivery:** the exploit string is placed in the `User-Agent` header. | `() { :;}; /bin/ping -c1 10.246.50.2` |
| 3️⃣ | **Command check:** a ping appears in the capture. | ICMP echo request and reply (packets 13 and 14) |
| 4️⃣ | **Server response:** the server answers with an error. | `500 Internal Server Error` (packet 15) |

```text
 Client 10.246.50.6 ──GET /exploitable.cgi (Shellshock in User-Agent)──▶ Server 10.246.50.2 ──▶ ping seen ──▶ 500 error
```

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🌐 Attacker IP | `10.246.50.6` | Packet 11 | 🔴 Sent the Shellshock request |
| 🎯 Target IP | `10.246.50.2` | Packets 11 and 15 | 🟠 Targeted web server |
| 🔗 Targeted URL | `/exploitable.cgi` | HTTP stream | 🟠 Vulnerable CGI script |
| 💥 Payload | `() { :;}; /bin/ping -c1 10.246.50.2` | HTTP `User-Agent` header | 🔴 Shellshock exploit string |
| 📡 ICMP traffic | Echo request and reply | Packets 13 and 14 | 🟠 Seen right after the payload |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

> ⚠️ **Source of this mapping:** these techniques are my mapping from the exploit and payload in the capture. They are not copied from a report.

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Exploit Public-Facing Application | [T1190](https://attack.mitre.org/techniques/T1190/) | A public web server's CGI script was targeted with the Shellshock bug. |
| Execution | Command and Scripting Interpreter: Unix Shell | [T1059.004](https://attack.mitre.org/techniques/T1059/004/) | The payload tries to run a command through Bash. |

---

<!-- ===================== RESPONSE ===================== -->
## ✅ Response Actions

- [x] Opened the pcap in Wireshark and identified the client and server from the traffic direction.
- [x] Found the CGI request in packet 11 and the `500` response in packet 15.
- [x] Followed the HTTP stream and read the exploit string in the `User-Agent` header.
- [x] Read the server's operating system and web server version from the `Server` header.
- [ ] Recommended follow-up: update Bash to a patched version, since Shellshock affects Bash 1.03 through 4.3.
- [ ] Recommended follow-up: remove or secure the CGI script, and update Apache 2.2.22, which is very old.
- [ ] Recommended follow-up: add WAF or IDS rules that flag `() {` in HTTP headers, and review the server for signs the command ran.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![Wireshark PCAP Analysis](https://img.shields.io/badge/Wireshark%20PCAP%20Analysis-0EA5E9?style=for-the-badge&labelColor=0D1117)
![HTTP Stream Analysis](https://img.shields.io/badge/HTTP%20Stream%20Analysis-1E90FF?style=for-the-badge&labelColor=0D1117)
![Exploit Payload Analysis](https://img.shields.io/badge/Exploit%20Payload%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Server Fingerprinting](https://img.shields.io/badge/Server%20Fingerprinting-0284C7?style=for-the-badge&labelColor=0D1117)
![Traffic Direction Analysis](https://img.shields.io/badge/Traffic%20Direction%20Analysis-334155?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-475569?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
