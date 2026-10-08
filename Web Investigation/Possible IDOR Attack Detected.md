<!-- ===================== HEADER ===================== -->
<div align="center">

# 🔑 SOC169: Possible IDOR Attack Detected

### LetsDefend SOC Analyst Learning Path · Insecure Direct Object Reference Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=IDOR+Alert+Triage;Consecutive+POST+Request+Analysis;User+ID+Enumeration+Detection;Containment+%26+Tier+2+Escalation" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Alert](https://img.shields.io/badge/Alert-SOC169-1E90FF?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Escalated%20to%20Tier%202-F59E0B?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![AbuseIPDB](https://img.shields.io/badge/AbuseIPDB-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![Log Management](https://img.shields.io/badge/Log%20Management-334155?style=flat-square&logo=files&logoColor=white)
![EDR](https://img.shields.io/badge/Containment-1E90FF?style=flat-square&logo=crowdstrike&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/44752e85123f86c5c9fbda57914ddffb46a2e025/Web%20Investigation%20Picture/7/idor%20final.webp?raw=true" alt="IDOR illustration for the LetsDefend lab" width="85%" />

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
7. [Step 5: Containment and Escalation](#-step-5-containment-and-escalation)
8. [Step 6: Remediation](#-step-6-remediation)
9. [Step 7: Analyst Note, IoCs, and Close the Ticket](#-step-7-analyst-note-iocs-and-close-the-ticket)
10. [Decision Rationale](#-decision-rationale)
11. [Indicators of Compromise](#-indicators-of-compromise-iocs)
12. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
13. [Response Actions](#-response-actions)
14. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Possible IDOR Attack Detected |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🚨 **Alert Rule** | SOC169: Possible IDOR Attack Detected |
| ⚡ **Trigger Reason** | Consecutive requests to the same page |
| 💻 **Hostname** | WebServer1005 |
| 🎯 **Destination IP** | `172.16.17.15` |
| 🌐 **Source IP** | `134.209.118.137` (external, US, flagged malicious) |
| 📮 **HTTP Method** | POST |
| 🔗 **Requested URL** | `https://172.16.17.15/get_user_info/` |
| 🧭 **User-Agent** | `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)` |
| 🚦 **Device Action** | Allowed |
| 🔢 **Events Found** | 5 |
| 🔁 **Pattern** | Multiple requests, each with a different `user_id=x` value |
| ✅ **Attack Successful** | Yes |
| 🛑 **Response** | Server contained, escalated to Tier 2 |
| 🧰 **Tools Used** | LetsDefend SIEM, VirusTotal, AbuseIPDB, Log Management |
| ⚖️ **Final Verdict** | 🚨 **True Positive** |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🔑 **IDOR (Insecure Direct Object Reference):** a common web application security flaw. It happens when a website or app uses a direct identifier, like a user ID, account number, or file name, to access internal data without checking whether the user is allowed to see it.

In simple terms, a website or app lets you peek at or change someone else's data just by changing an ID number or name in the web address or request.

| Scenario | What Happens |
| :--- | :--- |
| ✅ **The normal way** | You log into a website to view your own profile, and the request points to profile number `101`. The server shows you your profile. |
| ❌ **The IDOR flaw** | You change the number to someone else's. If the website shows you that profile without checking whether you are allowed, that is an IDOR. |

### 🏨 The Hotel Analogy

IDOR is like staying in a hotel where your key card works on every door, because the locks only check that you have *a* card, not *which room* is yours.

| Hotel | Real Concept |
| :--- | :--- |
| 🧾 **Room 501 on your receipt** | The account number or ID (the object reference). |
| 🚪 **Swiping at Room 501** | The normal request. The system works as intended. |
| 🚨 **Swiping at Room 502** | The IDOR attack. The lock never checks whether Room 502 is assigned to you, so the door opens. |

### 🔍 How It Works

When a user makes a request, like viewing their account information, the system uses an identifier (such as a number) to decide which data to show. If the application doesn't check whether the user is allowed to access that data, someone could change the identifier (like changing the locker number) to view or steal someone else's information.

> 📮 **About POST requests:** a POST request submits data to the server to create a new resource or trigger a server-side action.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the case from the alert and take ownership. |
| **Method** | Start the playbook to begin the guided investigation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/66fbb8175781f12e99dc176cfe52e576e67e4ae7/Web%20Investigation%20Picture/7/create%20ticket.png?raw=true" alt="Creating a ticket from the SIEM alert" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/66fbb8175781f12e99dc176cfe52e576e67e4ae7/Web%20Investigation%20Picture/7/playbook.png?raw=true" alt="Starting the playbook for the case" width="85%" />
</div>

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Review the alert details. |
| **Method** | Record the key facts, then work out why the rule fired. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/44752e85123f86c5c9fbda57914ddffb46a2e025/Web%20Investigation%20Picture/7/info.png?raw=true" alt="SIEM alert details for SOC169" width="85%" />
</div>

### 🎯 Finding / Answer

| Field | Value |
| :--- | :--- |
| 💻 Hostname | `WebServer1005` |
| 🎯 Destination IP address | `172.16.17.15` |
| 🌐 Source IP address | `134.209.118.137` |
| 📮 HTTP request method | `POST` |
| 🔗 Requested URL | `https://172.16.17.15/get_user_info/` |
| 🧭 User-Agent | `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)` |
| ⚡ Alert trigger reason | Consecutive requests to the same page |
| 🚦 Device action | Allowed |

The alert fired because of consecutive requests to the same page: `hxxps://172.16.17.15/get_user_info/`.

### 🧩 Why This Points to a Possible IDOR

| Point | Explanation |
| :--- | :--- |
| ⚡ **Trigger reason** | The rule flagged rapid or repeated HTTP POST requests from the external IP `134.209.118.137` to the endpoint `/get_user_info/`. |
| 🔑 **The IDOR concept** | IDOR vulnerabilities occur when an application uses user-supplied input to access objects or records directly, without proper authorization checks. In this scenario, an attacker typically sends consecutive requests to an endpoint like `/get_user_info/` while iterating through user IDs or parameters (for example `id=1001`, `id=1002`, `id=1003`) in the request body, to enumerate and read other users' personal information. |

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK THE SOURCE IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Look up the reputation of the source IP address `134.209.118.137`. |
| **Method** | Search it in **VirusTotal** and **AbuseIPDB**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/66fbb8175781f12e99dc176cfe52e576e67e4ae7/Web%20Investigation%20Picture/7/virus.png?raw=true" alt="VirusTotal reputation result for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/ipdv.png?raw=true" alt="AbuseIPDB reputation result for the source IP, first view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/ipdb2.png?raw=true" alt="AbuseIPDB reputation result for the source IP, second view" width="85%" />
</div>

### 🎯 Finding / Answer

The address is outside our network, originates from the US, and has been flagged as **malicious**.

```text
134.209.118.137 (US, external)  →  WebServer1005 (172.16.17.15)
```

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: REVIEW THE RAW LOGS

| | |
| :--- | :--- |
| **Action** | Search Log Management for the source IP address. |
| **Method** | Open the events and compare the requests. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/logs.png?raw=true" alt="Log Management search results showing 5 events for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/1.png?raw=true" alt="Raw log event 1" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/2.png?raw=true" alt="Raw log event 2" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/3.png?raw=true" alt="Raw log event 3" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/4.png?raw=true" alt="Raw log event 4" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/5.png?raw=true" alt="Raw log event 5" width="85%" />
</div>

### 🎯 Finding / Answer

**5 events** were found for the source IP. Reviewing them shows the attacker sending multiple requests, each with a different `user_id=x` value. This is the enumeration pattern of an IDOR attack: the attacker changes the ID in the request to reach a different user's record.

The logs report a `200` status on several of these requests, which means the server accepted them and answered. If the application does not verify that the user has permission to access that identifier, an attacker can point the request to a different object, for example changing `userID=123` to `userID=456`.

| Question | Answer |
| :--- | :--- |
| Is the attack malicious? | 🔴 **Yes** |
| What type of attack is this? | 🔑 **IDOR** |
| Is there different request or traffic? | ❌ **No** |
| Is the attack successful? | 🔴 **Yes** |

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: CONTAINMENT AND ESCALATION

| | |
| :--- | :--- |
| **Action** | Contain the affected web server because the attack succeeded. |
| **Method** | Use the SIEM's containment function on `WebServer1005`. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/contain.png?raw=true" alt="Containing the affected web server" width="85%" />
</div>

### 🎯 Finding / Answer

| Question | Answer |
| :--- | :--- |
| Is containment needed? | ✅ **Yes**, the attack was successful. |
| Do we need Tier 2 escalation? | ✅ **Yes** |

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: REMEDIATION

| Area | Recommended Action |
| :--- | :--- |
| 🔒 **Secure the web application** | Properly secure web applications as a whole. |
| ✅ **Authorization checks** | Verify that the requesting user has permission to access the requested resource. |
| 🎟️ **Indirect references** | Use indirect references or opaque tokens instead of predictable IDs that users can guess or manipulate. |
| 🧱 **Server-side controls** | Implement input validation and other access control mechanisms at the server level. |

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: ANALYST NOTE, IOCS, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Write the analyst note and add the IoCs found during the investigation. |
| **Method** | Finish the playbook and close the ticket. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/analyst%20note.png?raw=true" alt="Analyst note written for the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/ioc.png?raw=true" alt="IoCs added to the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/3ae558bc6496cc46d0c1dd7ec5d078c62aa037b3/Web%20Investigation%20Picture/7/closed%20ticket.png?raw=true" alt="Closing the ticket in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE 🚨

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert is a **True Positive**: a malicious external IP sent multiple POST requests to `/get_user_info/`, each with a different `user_id` value, and the requests were accepted. The affected server was contained and the ticket escalated to Tier 2.

---

<!-- ===================== DECISION RATIONALE ===================== -->
## ⚖️ Decision Rationale

| Check | Result | Impact on Verdict |
| :--- | :--- | :--- |
| 🌐 **Source IP reputation** | External, US, flagged malicious | 🔴 Known bad source |
| 🔗 **Request pattern** | Consecutive POST requests to `/get_user_info/` with different `user_id=x` values | 🔴 ID enumeration, the IDOR pattern |
| 📋 **Raw logs** | 5 events with `200` responses | 🔴 Requests accepted by the server |
| 🚦 **Device action** | Allowed | 🟠 The network did not block the requests |
| 🛑 **Containment and escalation** | Contained, escalated to Tier 2 | 🔴 Attack successful |

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🌐 Source IP | `134.209.118.137` | SIEM alert, VirusTotal, AbuseIPDB | 🔴 Malicious (US) |
| 🔗 Targeted URL | `https://172.16.17.15/get_user_info/` | SIEM alert | 🟠 Endpoint exposed to ID enumeration |
| 🧭 User-Agent | `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)` | SIEM alert | 🟠 Very old browser string, consistent with a script or tool |
| 💻 Affected host | `WebServer1005` (`172.16.17.15`) | SIEM alert | 🟠 Compromised, contained |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Exploit Public-Facing Application | [T1190](https://attack.mitre.org/techniques/T1190/) | The attacker abused missing authorization checks on a public web endpoint to read other users' records. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Created the case, took ownership, and started the playbook.
- [x] Recorded the alert details and explained why the rule fired.
- [x] Checked the source IP reputation in VirusTotal and AbuseIPDB.
- [x] Reviewed the raw logs and identified the `user_id` enumeration pattern.
- [x] Contained `WebServer1005` and escalated the ticket to Tier 2.
- [x] Added the analyst note and IoCs, then closed the ticket as a true positive.
- [ ] Recommended follow-up: Tier 2 should determine which user records were accessed and whether personal data was exposed.
- [ ] Recommended follow-up: block `134.209.118.137` at the firewall.
- [ ] Recommended follow-up: add server-side authorization checks and opaque identifiers to `/get_user_info/`.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![IDOR Analysis](https://img.shields.io/badge/IDOR%20Analysis-1E90FF?style=for-the-badge&labelColor=0D1117)
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
