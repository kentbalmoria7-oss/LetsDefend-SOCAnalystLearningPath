<!-- ===================== HEADER ===================== -->
<div align="center">

# 📡 Port Scan Activity: PCAP Investigation

### LetsDefend · Network Logs · Wireshark Analysis of a Ping Sweep

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=ICMP+Ping+Sweep+Detection;Identifying+the+Scanner+and+Live+Hosts;MAC+Address+%26+Vendor+Lookup;OS+Fingerprinting+with+NetBIOS" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Network%20Log%20Analysis-1E90FF?style=for-the-badge&logo=wireshark&logoColor=white&labelColor=0D1117)
![Activity](https://img.shields.io/badge/Activity-ICMP%20Ping%20Sweep-DC2626?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=flat-square&logo=wireshark&logoColor=white)
![ICMP](https://img.shields.io/badge/ICMP-0F172A?style=flat-square&logo=cisco&logoColor=00D4FF)
![NetBIOS](https://img.shields.io/badge/NetBIOS-0078D4?style=flat-square&logo=windows&logoColor=white)
![MAC Lookup](https://img.shields.io/badge/MAC%20Vendor%20Lookup-334155?style=flat-square&logo=apple&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/port%20scanning.jpg?raw=true" alt="Port scanning illustration" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Start the Investigation](#-start-the-investigation)
4. [Challenge Questions](#-challenge-questions)
5. [Network Map](#-network-map)
6. [Findings Summary](#-findings-summary)
7. [Purpose of the Challenge](#-purpose-of-the-challenge)
8. [Indicators and Observations](#-indicators-and-observations)
9. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
10. [Response Actions](#-response-actions)
11. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Port Scan Activity |
| 🏫 **Platform** | LetsDefend (Network Logs) |
| 🗂️ **Evidence** | A `.pcap` file opened in Wireshark |
| 🕵️ **Scanning IP** | `10.42.42.253` |
| 🎯 **IP Found by the Scan** | `10.42.42.50` (answered a ping) |
| 🍎 **Apple System** | `10.42.42.25`, MAC address `00:16:cb:92:6e:dc` |
| 🪟 **Windows System** | `10.42.42.50` |
| 📣 **Broadcast Address** | `10.255.255.255` |
| ❓ **Unidentified Host** | `10.42.42.56` |
| 🧰 **Tools Used** | Wireshark (ICMP filter, Conversations, Endpoints, Protocol Hierarchy, NBSS filter) |
| ⚖️ **Conclusion** | 🚨 **Network scanning (ICMP ping sweep) detected** |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 📡 **Port scan activity:** sending requests to a series of network ports (numbered 0 through 65,535) on a computer or server to see which ones are open, closed, or filtered by a firewall.

> 🚪 **Analogy:** it is like walking down a hallway and testing every door to see which are unlocked, which are locked, and which have a security guard in front.

### ❓ Why Port Scanning Happens

| Purpose | Explanation |
| :--- | :--- |
| 🛠️ **Network administration** | IT administrators and security teams scan their own networks to map them, verify server configurations, and find accidental vulnerabilities. |
| 🕵️ **Reconnaissance (cyberattacks)** | Attackers and automated scripts scan public IP addresses to find unpatched services, weak entry points, or exposed databases to target. |

> 🏓 **Ping sweep (ICMP sweep):** a basic scanning technique used to find out which of a range of IP addresses belong to live hosts (computers). It is the technique this capture shows.

---

<!-- ===================== START INVESTIGATION ===================== -->
## 🔎 Start the Investigation

Open the `.pcap` file provided by the challenge in Wireshark.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/info.png?raw=true" alt="The PCAP file opened in Wireshark" width="85%" />
</div>

---

<!-- ===================== QUESTIONS ===================== -->
## 🎯 Challenge Questions

---

### 1️⃣ Q1: What is the IP address scanning the environment?

A ping sweep is a basic way to find live hosts, so the first step is to filter the capture for **ICMP packets**.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/icmp.png?raw=true" alt="ICMP packets in Wireshark showing the ping requests" width="85%" />
</div>

| Observation | Details |
| :--- | :--- |
| 🚀 **The initiator** | In the Source column, `10.42.42.253` repeatedly sends **Echo (ping) request** packets. |
| 🎯 **The targets** | It sends them to several different addresses in the Destination column: `10.42.42.56`, `10.42.42.25`, and `10.42.42.50`. |
| 📩 **The responses** | The other IP addresses are only seen sending **Echo (ping) reply** or **Destination unreachable** messages back to `10.42.42.253`. |

### 🎯 Finding / Answer

```text
10.42.42.253
```

---

### 2️⃣ Q2: What is the IP address found as a result of the scan?

The same ICMP view shows whether the scanner found anything. A ping reply comes from another IP, `10.42.42.50`, back to the scanner's machine.

### 🎯 Finding / Answer

```text
10.42.42.50
```

---

### 3️⃣ Q3: What is the MAC address of the Apple system it finds?

There is an easy way to find MAC addresses in a pcap:

| Step | Action |
| :-: | :--- |
| 1️⃣ | Go to **Statistics**. |
| 2️⃣ | Open **Conversations**. |
| 3️⃣ | Choose the **Ethernet** tab to see all MAC addresses in the capture. |
| 4️⃣ | Turn on **Name resolution** to see the vendor name behind each MAC address. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/stat1.png?raw=true" alt="Wireshark Statistics menu" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/conversion.png?raw=true" alt="Ethernet conversations listing the MAC addresses" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/apple.png?raw=true" alt="Name resolution showing the Apple MAC address" width="85%" />
</div>

### 🎯 Finding / Answer

```text
00:16:cb:92:6e:dc
```

---

### 4️⃣ Q4: What is the IP address of the detected Windows system?

First, list all the IP addresses in the capture:

| Step | Action |
| :-: | :--- |
| 1️⃣ | Go to **Statistics**. |
| 2️⃣ | Open **Endpoints**. |
| 3️⃣ | Choose the **IPv4** tab. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/endpoimt.png?raw=true" alt="Wireshark Endpoints window" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/ipv4.png?raw=true" alt="IPv4 endpoints in the capture" width="85%" />
</div>

The capture holds **5 IP addresses**:

```text
10.42.42.25
10.42.42.50
10.42.42.56
10.42.42.253
10.255.255.255
```

### 🧩 Sorting Out the Hosts

| IP Address | Role |
| :--- | :--- |
| `10.42.42.253` | 🕵️ **Attacker (scanner)** |
| `10.255.255.255` | 📣 **Broadcast address** |
| `10.42.42.25` | 🍎 **Apple system** (matched to the Apple MAC address) |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/ip%20address.png?raw=true" alt="The Apple MAC address matched to its IP address" width="85%" />
</div>

That leaves `10.42.42.56` and `10.42.42.50`.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/192ff479c6f51f59eaff753908362d6d37c795fc/Network%20Logs%20Picture/2/Screenshot%202026-10-17%20210952.png?raw=true" alt="The two remaining IP addresses to identify" width="85%" />
</div>

### 🪟 Finding the Windows System with Protocols

To find the Windows system, I checked all the protocols in the capture with **Statistics, Protocol Hierarchy**. It shows a tree of every protocol in the capture, where each row holds the statistics for one protocol, and the Percent Packets and Percent Bytes columns double as bar graphs.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f9b99bb50fd7742b26e9167fd9c79f4fc111d8af/Network%20Logs%20Picture/2/protocol%20hierachy.png?raw=true" alt="Protocol Hierarchy of the capture" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/192ff479c6f51f59eaff753908362d6d37c795fc/Network%20Logs%20Picture/2/netbiox.png?raw=true" alt="NetBIOS packets in the protocol hierarchy" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/192ff479c6f51f59eaff753908362d6d37c795fc/Network%20Logs%20Picture/2/nbss.png?raw=true" alt="Filtering for NBSS (NetBIOS Session Service) packets" width="85%" />
</div>

There are some **NetBIOS** packets, and Windows uses NetBIOS. Filtering for **NBSS** (NetBIOS Session Service) shows which host uses it. Since `10.42.42.25` is the Apple system, the remaining candidate, `10.42.42.50`, is the Windows system.

### 🎯 Finding / Answer

```text
10.42.42.50
```

---

<!-- ===================== NETWORK MAP ===================== -->
## 🗺️ Network Map

| IP Address | Role | How I Identified It |
| :--- | :--- | :--- |
| `10.42.42.253` | 🕵️ Scanner (attacker) | Sent repeated ICMP echo requests to other hosts |
| `10.42.42.25` | 🍎 Apple system | MAC address `00:16:cb:92:6e:dc` resolved to Apple |
| `10.42.42.50` | 🪟 Windows system | Answered the ping, and uses NetBIOS (NBSS) |
| `10.42.42.56` | ❓ Unidentified | Pinged by the scanner, but not identified in the lab |
| `10.255.255.255` | 📣 Broadcast | Broadcast address |

```text
                    ┌──▶ 10.42.42.56   (not identified)
 10.42.42.253 ──────┼──▶ 10.42.42.25   (Apple system)
 (scanner)          └──▶ 10.42.42.50   (Windows system, replied to the ping)
```

---

<!-- ===================== FINDINGS SUMMARY ===================== -->
## 📊 Findings Summary

| # | Question | Answer |
| :-: | :--- | :--- |
| 1️⃣ | IP address scanning the environment | `10.42.42.253` |
| 2️⃣ | IP address found as a result of the scan | `10.42.42.50` |
| 3️⃣ | MAC address of the Apple system | `00:16:cb:92:6e:dc` |
| 4️⃣ | IP address of the detected Windows system | `10.42.42.50` |

<br/>

<div align="center">

## 🚨 VERDICT: NETWORK SCANNING DETECTED 🚨

</div>

### 🎯 Overall Conclusion

The capture shows **scanning activity** from `10.42.42.253`, which sent repeated ICMP echo requests to several hosts to find which ones are alive. The scan found a Windows system (`10.42.42.50`) and an Apple system (`10.42.42.25`), and the protocols and MAC addresses in the capture reveal the operating systems of those hosts.

---

<!-- ===================== PURPOSE ===================== -->
## 🎓 Purpose of the Challenge

The specific purpose of this challenge is to determine whether port scan activity occurred within a network. More broadly, it gives aspiring blue team (defensive security) professionals hands-on **digital forensics and incident response (DFIR)** experience. By analyzing a provided network capture in Wireshark, you learn how to:

| # | Skill |
| :-: | :--- |
| 1️⃣ | Identify an attacker's IP address by spotting ICMP (ping) sweeps. |
| 2️⃣ | Identify active machines (victims) that answered the scan. |
| 3️⃣ | Extract hardware MAC addresses and identify device manufacturers (like Apple). |
| 4️⃣ | Identify operating systems (like Windows) by analyzing specific network protocols (like NetBIOS). |

---

<!-- ===================== INDICATORS ===================== -->
## 🧬 Indicators and Observations

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🌐 Scanner IP | `10.42.42.253` | ICMP filter | 🔴 Sent the ping sweep |
| 🎯 Responding host | `10.42.42.50` | ICMP filter | 🟠 Replied to the scanner (Windows) |
| 🍎 Apple system | `10.42.42.25` / `00:16:cb:92:6e:dc` | Ethernet conversations | 🟠 Targeted by the scan |
| ❓ Unidentified host | `10.42.42.56` | Endpoints | 🟡 Targeted, not identified |
| 📣 Broadcast | `10.255.255.255` | Endpoints | 🟡 Normal broadcast address |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

> ⚠️ **Source of this mapping:** these techniques are my mapping from the ping sweep and the host identification in the capture. They are not copied from a report.

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Discovery | Remote System Discovery | [T1018](https://attack.mitre.org/techniques/T1018/) | The scanner pinged several IP addresses to find live hosts on the network. |
| Discovery | System Network Configuration Discovery | [T1016](https://attack.mitre.org/techniques/T1016/) | Network details such as MAC addresses and protocols reveal the hosts' types. |

---

<!-- ===================== RESPONSE ===================== -->
## ✅ Response Actions

- [x] Opened the pcap in Wireshark and filtered for ICMP traffic.
- [x] Identified the scanning IP and the host that replied.
- [x] Found the Apple system's MAC address through Statistics, Conversations.
- [x] Listed the IP addresses and identified the Windows system through Protocol Hierarchy and the NBSS filter.
- [ ] Recommended follow-up: find out who owns `10.42.42.253` and whether the scan was authorized (for example an approved network scan).
- [ ] Recommended follow-up: if the scan was not authorized, isolate `10.42.42.253` and check it for compromise.
- [ ] Recommended follow-up: add IDS rules or alerts for ping sweeps and other rapid scanning, and limit ICMP between network segments where it isn't needed.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![Wireshark PCAP Analysis](https://img.shields.io/badge/Wireshark%20PCAP%20Analysis-0EA5E9?style=for-the-badge&labelColor=0D1117)
![Scan Detection](https://img.shields.io/badge/Scan%20Detection-1E90FF?style=for-the-badge&labelColor=0D1117)
![MAC Address Analysis](https://img.shields.io/badge/MAC%20Address%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![OS Fingerprinting](https://img.shields.io/badge/OS%20Fingerprinting-0284C7?style=for-the-badge&labelColor=0D1117)
![Network Statistics](https://img.shields.io/badge/Network%20Statistics-334155?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-475569?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
