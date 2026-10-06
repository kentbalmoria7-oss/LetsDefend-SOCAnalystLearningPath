<!-- ===================== HEADER ===================== -->
<div align="center">

# 🦈 HTTP Basic Auth: Wireshark PCAP Investigation

### LetsDefend · Packet Capture Analysis · Clear-Text Credential Exposure

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Wireshark+PCAP+Analysis;HTTP+Display+Filters;Server+%26+Client+Fingerprinting;Decoding+Basic+Auth+Credentials" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-PCAP%20Analysis-1E90FF?style=for-the-badge&logo=wireshark&logoColor=white&labelColor=0D1117)
![Finding](https://img.shields.io/badge/Finding-Credentials%20in%20Clear%20Text-DC2626?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)
![CyberChef](https://img.shields.io/badge/CyberChef-1E90FF?style=flat-square&logo=codechef&logoColor=white)
![HTTP](https://img.shields.io/badge/HTTP-0F172A?style=flat-square&logo=files&logoColor=00D4FF)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b27cafa4bd10d3b83430245031f670cedc175b44/Web%20Investigation%20Picture/2/pcap.png?raw=true" alt="The PCAP file opened in Wireshark" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Scenario](#-scenario)
3. [Wireshark Basics](#-wireshark-basics)
4. [Challenge Questions](#-challenge-questions)
5. [Findings Summary](#-findings-summary)
6. [Indicators of Compromise](#-indicators-and-exposed-data)
7. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
8. [Response Actions](#-response-actions)
9. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | HTTP Basic Auth |
| 🏫 **Platform** | LetsDefend |
| 🗂️ **Evidence** | A `.pcap` file opened in Wireshark |
| 🔢 **HTTP GET Requests** | 5 |
| 🖥️ **Server OS** | FreeBSD |
| 🌐 **Web Server** | Apache/2.2.15 |
| 🔐 **OpenSSL Version** | OpenSSL/0.9.8n |
| 💻 **Client User-Agent** | `lynx/2.8.7rel.1 libwww-fm/2.14 ssl-mm/1.4.1 openssl/0.9.8n` |
| 🪪 **Basic Auth Username** | `webadmin` |
| 🔑 **Basic Auth Password** | `W3b4Dm1n` |
| 🧰 **Tools** | Wireshark, CyberChef |

---

<!-- ===================== SCENARIO ===================== -->
## 📖 Scenario

The logs indicate a possible attack, so a `.pcap` file is investigated in Wireshark. The goal is to answer questions about the HTTP traffic, including the server details, the client, and the credentials sent with HTTP Basic Authentication.

---

<!-- ===================== WIRESHARK BASICS ===================== -->
## 🦈 Wireshark Basics

> 📘 **What is Wireshark?** Wireshark is a free, open-source network packet analyzer that captures and interactively inspects traffic moving across a computer network. Think of it as a digital microscope for your network.

It intercepts raw data streams passing through a network interface (like Ethernet or Wi-Fi) and translates them into a readable, structured format.

### ⚙️ How Wireshark Works

| Component | What It Does |
| :--- | :--- |
| 🔌 **Packet capture driver** | Wireshark does not talk to network hardware directly. It relies on a helper driver, **Npcap** on Windows or **libpcap** on Mac and Linux, which asks the operating system's network stack for a copy of the packets. |
| 📡 **Interface selection** | You pick a network path (such as Wi-Fi or Ethernet) so Wireshark knows where to listen. |
| 🧩 **Dissection and decoding** | Wireshark breaks the raw binary data apart layer by layer, from Ethernet up to HTTP or DNS, and shows it in readable text. |

### 🧭 Shortcuts and Terminology

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/fa76b336df013388d2b1687e443aa8cbc3dd972b/Web%20Investigation%20Picture/2/parts.png?raw=true" alt="The parts of the Wireshark interface" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/fa76b336df013388d2b1687e443aa8cbc3dd972b/Web%20Investigation%20Picture/2/cheatsheet.png?raw=true" alt="Wireshark display filter cheat sheet" width="85%" />
</div>

### 🔎 Why Use Display Filters?

Display filters hide uninteresting network noise so you can focus on the specific packets you need.

| Benefit | Explanation |
| :--- | :--- |
| 🧹 **Reduce clutter** | A capture can create thousands of lines in seconds. Filters clear out the noise. |
| 🛟 **Non-destructive viewing** | Filters don't delete or alter the captured file. Clearing the filter brings all packets back instantly. |
| 🛠️ **Troubleshoot issues** | Isolate traffic for a single device, port, or protocol to find network faults. |
| 🕵️ **Security analysis** | Analysts use filters to hunt for threats, clear-text passwords, and suspicious connection flags. |

---

<!-- ===================== QUESTIONS ===================== -->
## 🎯 Challenge Questions

---

### 1️⃣ Q1: How many HTTP GET requests are in the pcap?

To count them, apply this display filter:

```text
http.request.method == GET
```

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/62f247f11335c7d75f1e0a1b7d657706d9e13326/Web%20Investigation%20Picture/2/httppacket.png?raw=true" alt="Display filter showing only HTTP GET request packets" width="85%" />
</div>

The filter shows only the matching HTTP packets. Counting them by hand takes forever in large files, so the packet count shown in Wireshark's status bar is the smarter way.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/62f247f11335c7d75f1e0a1b7d657706d9e13326/Web%20Investigation%20Picture/2/5.png?raw=true" alt="Wireshark status bar showing the count of displayed packets" width="85%" />
</div>

### 🎯 Finding / Answer

```text
5
```

---

### 2️⃣ Q2: What is the server operating system?

> 🔍 **About the Server header:** the `Server` header in the response message identifies the software running on the server that generated the response.

To read it, right-click any HTTP packet and choose **Follow > HTTP Stream**.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/bc3d17166c4d9e54e18bf08f4760e946fbb45b1d/Web%20Investigation%20Picture/2/follow.png?raw=true" alt="Following the HTTP stream in Wireshark" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/bc3d17166c4d9e54e18bf08f4760e946fbb45b1d/Web%20Investigation%20Picture/2/server.png?raw=true" alt="HTTP stream showing the Server header" width="85%" />
</div>

The software is Apache/2.2.15, and the operating system is FreeBSD.

### 🎯 Finding / Answer

```text
FreeBSD
```

---

### 3️⃣ Q3: What is the name and version of the web server software?

This works like the previous question: the software shown in the `Server` header is Apache/2.2.15.

### 🎯 Finding / Answer

```text
Apache/2.2.15
```

---

### 4️⃣ Q4: What is the version of OpenSSL running on the server?

This also works like the previous two questions, using the same `Server` header.

### 🎯 Finding / Answer

```text
OpenSSL/0.9.8n
```

---

### 5️⃣ Q5: What is the client's user-agent information?

> 🔍 **What is a user agent?** A user agent is software, such as a web browser, a media player, or a bot, that retrieves and displays web content on behalf of a user. When it connects to a website, it sends a short text line called the **User-Agent header**, which tells the server details about the device and application. Think of it like showing an ID badge when you walk into a store: it tells the server who is knocking on the door.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/065b2d9cded565dda9dce7a36b0beeb875d1eb1c/Web%20Investigation%20Picture/2/user%20agent.png?raw=true" alt="HTTP request showing the User-Agent header" width="85%" />
</div>

### 🎯 Finding / Answer

```text
lynx/2.8.7rel.1 libwww-fm/2.14 ssl-mm/1.4.1 openssl/0.9.8n
```

---

### 6️⃣ Q6: What is the username used for Basic Authentication?

> 🔍 **About authentication headers:** an authentication header is data added to a web request or network packet to prove identity and secure communication.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3943cf7f4790a0eb00874202242202a9390d4d68/Web%20Investigation%20Picture/2/auth.png?raw=true" alt="HTTP request showing the Authorization Basic header" width="85%" />
</div>

The request contains this header:

```text
Authorization: Basic d2ViYWRtaW46VzNiNERtMW4=
```

The string `d2ViYWRtaW46VzNiNERtMW4=` is **Base64 encoded**, so it can be decoded in CyberChef.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3943cf7f4790a0eb00874202242202a9390d4d68/Web%20Investigation%20Picture/2/cyberchef.png?raw=true" alt="CyberChef decoding the Base64 Authorization header" width="85%" />
</div>

We decode authentication headers to extract, verify, and use the user identity or credentials hidden inside encoded formats. The credentials are written in `user:pass` format.

### 🎯 Finding / Answer

```text
webadmin
```

---

### 7️⃣ Q7: What is the user password used for Basic Authentication?

The decoded `user:pass` string gives the password after the colon.

### 🎯 Finding / Answer

```text
W3b4Dm1n
```

> ⚠️ **Why this matters:** Basic Authentication only Base64-**encodes** credentials. It does not encrypt them, so anyone who captures the traffic can read the username and password in seconds.

---

<!-- ===================== FINDINGS SUMMARY ===================== -->
## 📊 Findings Summary

| # | Question | Answer |
| :-: | :--- | :--- |
| 1️⃣ | How many HTTP GET requests are in the pcap? | `5` |
| 2️⃣ | What is the server operating system? | `FreeBSD` |
| 3️⃣ | What is the name and version of the web server software? | `Apache/2.2.15` |
| 4️⃣ | What is the version of OpenSSL running on the server? | `OpenSSL/0.9.8n` |
| 5️⃣ | What is the client's user-agent information? | `lynx/2.8.7rel.1 libwww-fm/2.14 ssl-mm/1.4.1 openssl/0.9.8n` |
| 6️⃣ | What is the username used for Basic Authentication? | `webadmin` |
| 7️⃣ | What is the user password used for Basic Authentication? | `W3b4Dm1n` |

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators and Exposed Data

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🔓 **Exposed credentials** | `webadmin:W3b4Dm1n` | `Authorization: Basic` header, decoded in CyberChef | 🔴 Sent in a decodable form over HTTP |
| 🖥️ **Server fingerprint** | Apache/2.2.15 on FreeBSD with OpenSSL/0.9.8n | `Server` header in the HTTP stream | 🟠 Reveals software and version details |
| 💻 **Client fingerprint** | `lynx/2.8.7rel.1 libwww-fm/2.14 ssl-mm/1.4.1 openssl/0.9.8n` | `User-Agent` header | 🟡 Text-based browser used for the requests |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

> ⚠️ **Not confirmed:** the lab doesn't show who captured the traffic or how the credentials were used, so this mapping shows the techniques the exposure enables.

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Credential Access | Network Sniffing | [T1040](https://attack.mitre.org/techniques/T1040/) | Anyone who captures this traffic can read the Basic Auth credentials. |
| Credential Access | Unsecured Credentials | [T1552](https://attack.mitre.org/techniques/T1552/) | The credentials were sent in an easily decoded form instead of being protected. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Opened the pcap in Wireshark and applied display filters to focus on HTTP traffic.
- [x] Counted the HTTP GET requests and followed an HTTP stream to read the server headers.
- [x] Identified the server operating system, web server software, and OpenSSL version.
- [x] Identified the client's user-agent.
- [x] Decoded the Base64 `Authorization` header in CyberChef to recover the username and password.
- [ ] Recommended follow-up: stop using HTTP Basic Authentication over plain HTTP, and require HTTPS (TLS) for any login.
- [ ] Recommended follow-up: change the `webadmin` password and review the account for unauthorized access.
- [ ] Recommended follow-up: update the web server software and OpenSSL, since the versions seen are old, and hide version details in server headers.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![Wireshark PCAP Analysis](https://img.shields.io/badge/Wireshark%20PCAP%20Analysis-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Display Filters](https://img.shields.io/badge/Display%20Filters-1E90FF?style=for-the-badge&labelColor=0D1117)
![HTTP Stream Analysis](https://img.shields.io/badge/HTTP%20Stream%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Header Analysis](https://img.shields.io/badge/Header%20Analysis-0284C7?style=for-the-badge&labelColor=0D1117)
![Base64 Decoding](https://img.shields.io/badge/Base64%20Decoding-334155?style=for-the-badge&labelColor=0D1117)
![Credential Exposure Detection](https://img.shields.io/badge/Credential%20Exposure%20Detection-475569?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
