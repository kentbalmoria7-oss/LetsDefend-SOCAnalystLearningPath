<!-- ===================== HEADER ===================== -->
<div align="center">

# 💉 SOC165: Possible SQL Injection Payload Detected

### LetsDefend SOC Analyst Learning Path · SQL Injection Alert Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=SQL+Injection+Alert+Triage;URL+Decoding+%26+Payload+Analysis;HTTP+Status+Code+Interpretation;Attack+Attempt%3A+Unsuccessful" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Alert](https://img.shields.io/badge/Alert-SOC165%20(High)-1E90FF?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Outcome](https://img.shields.io/badge/Attack-Unsuccessful-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![AbuseIPDB](https://img.shields.io/badge/AbuseIPDB-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![Log Management](https://img.shields.io/badge/Log%20Management-334155?style=flat-square&logo=files&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/e8ead4d05732f4a0cb333c722b312139507a16fa/Web%20Investigation%20Picture/5/SQL%20Injection.png?raw=true" alt="SQL injection overview image for the LetsDefend lab" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Step 1: Create the Case and Take Ownership](#-step-1-create-the-case-and-take-ownership)
4. [Step 2: Gather Information](#-step-2-gather-information)
5. [Step 3: Check the Source IP Reputation](#-step-3-check-the-source-ip-reputation)
6. [Step 4: Decode the Requested URL](#-step-4-decode-the-requested-url)
7. [Step 5: Review the Raw Logs](#-step-5-review-the-raw-logs)
8. [Step 6: Containment](#-step-6-containment)
9. [Step 7: Remediation](#-step-7-remediation)
10. [Step 8: Analyst Note, IoCs, and Close the Ticket](#-step-8-analyst-note-iocs-and-close-the-ticket)
11. [Decision Rationale](#-decision-rationale)
12. [Indicators of Compromise](#-indicators-of-compromise-iocs)
13. [MITRE ATT&CK Mapping](#-mitre-attck-mapping)
14. [Response Actions](#-response-actions)
15. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Possible SQL Injection Payload Detected |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🚨 **Alert Rule** | SOC165 (High severity) |
| ⚡ **Trigger Reason** | Requested URL contains `OR 1=1` |
| 💻 **Hostname** | WebServer1001 |
| 🎯 **Destination IP** | `172.16.17.18` |
| 🌐 **Source IP** | `167.99.169.17` (external, US, flagged malicious) |
| 📮 **HTTP Method** | GET |
| 🔗 **Requested URL** | `https://172.16.17.18/search/?q=%22%20OR%201%20%3D%201%20--%20-` |
| 🧭 **User-Agent** | `Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.1` |
| 🚦 **Device Action** | Allowed |
| 🔢 **Events Found** | 6 events between 11:30 and 11:34 AM |
| 📉 **Server Response to the Payload** | `500` Internal Server Error, 948 bytes |
| ✅ **Attack Successful** | No data was returned |
| 🛑 **Containment** | Not needed |
| 🧰 **Tools Used** | LetsDefend SIEM, VirusTotal, AbuseIPDB, urldecoder.org, Log Management |
| ⚖️ **Final Verdict** | 🚨 **True Positive** (malicious attempt, unsuccessful) |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🗄️ **SQL (Structured Query Language):** a standard programming language used to talk to, manage, and query relational databases, such as saving, reading, updating, or deleting user data.

| SQL Basics | Details |
| :--- | :--- |
| 🎯 **Purpose** | Lets applications store and pull data from tables (rows and columns). |
| ⌨️ **Common commands** | `SELECT`, `INSERT`, `UPDATE`, and `DELETE`. |
| 🧱 **Common databases** | MySQL, PostgreSQL, Microsoft SQL Server, and SQLite. |

> 💉 **SQL Injection (SQLi):** a security flaw where an attacker puts malicious code into a database input field, tricking the database into running commands the creator did not intend.

| SQL Injection | Details |
| :--- | :--- |
| ⚙️ **How it works** | An app takes user input (such as a username or search box) and glues it directly into a database query without checking it first. |
| ⚠️ **The danger** | An attacker can type special SQL commands, like `' OR 1=1 --`, to bypass login pages, steal private data, or delete tables. |
| 🛡️ **How to prevent it** | Use parameterized queries (also called prepared statements) and strict input validation, so the database treats user input only as data and never as executable code. |

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the case from the alert and take ownership. |
| **Method** | Start the playbook to begin the guided investigation. |

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Review the alert details. |
| **Method** | Record the key facts from the alert to answer the playbook questions. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f1747d45653e5ced472813d0757f900024321baf/Web%20Investigation%20Picture/5/info.png?raw=true" alt="SIEM alert details for the possible SQL injection payload" width="85%" />
</div>

### 🎯 Finding / Answer

| Field | Value |
| :--- | :--- |
| 💻 Hostname | `WebServer1001` |
| 🎯 Destination IP address | `172.16.17.18` |
| 🌐 Source IP address | `167.99.169.17` |
| 📮 HTTP request method | `GET` |
| 🔗 Requested URL | `https://172.16.17.18/search/?q=%22%20OR%201%20%3D%201%20--%20-` |
| 🧭 User-Agent | `Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.1` |
| ⚡ Alert trigger reason | Requested URL contains `OR 1=1` |
| 🚦 Device action | Allowed |

> 🧪 **What this is:** detection rule SOC165 saw a web request that looks like a SQL injection attempt against an internal web server and raised a high-severity ticket for an analyst to triage.

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK THE SOURCE IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Check the reputation of the source IP address `167.99.169.17`. |
| **Method** | Search it in **VirusTotal** and **AbuseIPDB**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/46d04d2cae59778158c8fb2d13e5dda10d403fce/Web%20Investigation%20Picture/5/virus.png?raw=true" alt="VirusTotal reputation result for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/46d04d2cae59778158c8fb2d13e5dda10d403fce/Web%20Investigation%20Picture/5/abuseipdb.png?raw=true" alt="AbuseIPDB reputation result for the source IP, first view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/46d04d2cae59778158c8fb2d13e5dda10d403fce/Web%20Investigation%20Picture/5/abuseipdb2.png?raw=true" alt="AbuseIPDB reputation result for the source IP, second view" width="85%" />
</div>

### 🎯 Finding / Answer

The address is outside our network, originates from the US, and has been flagged as **malicious**.

```text
167.99.169.17 (US, external)  →  WebServer1001 (172.16.17.18)
```

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: DECODE THE REQUESTED URL

| | |
| :--- | :--- |
| **Action** | Decode the web request to read the payload. |
| **Method** | Use a URL decoder such as [urldecoder.org](https://www.urldecoder.org/). |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/a85573e01950ad64e5c0622729f17b172bdd36da/Web%20Investigation%20Picture/5/decoder.png?raw=true" alt="URL decoder showing the decoded SQL injection payload" width="85%" />
</div>

**Encoded request:**

```text
https://172.16.17.18/search/?q=%22%20OR%201%20%3D%201%20--%20-
```

**Decoded `q` parameter:**

```text
" OR 1 = 1 -- -
```

### 🧩 What Each Part Does

| Part | Purpose |
| :--- | :--- |
| `"` | Closes the quote around the search term in the application's SQL query. |
| `OR 1 = 1` | Always true, so a vulnerable query would match every row instead of only the intended search results. This is called a **tautology** attack. |
| `-- -` | Starts an SQL comment, so the rest of the original query is ignored. |

A vulnerable backend could turn this query:

```sql
SELECT * FROM products WHERE name = "<input>"
```

into one that returns the whole table. It's a classic probe for whether input is sanitized, and it is often the first step before an attacker tries data extraction.

### 🎯 Finding / Answer

The URL contains SQL commands, as the SOC alert flagged.

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: REVIEW THE RAW LOGS

| | |
| :--- | :--- |
| **Action** | Search Log Management for the source IP address. |
| **Method** | Open the events and compare the requests and responses. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/01e22fb46bfebd2d0c1c15bec2dbadeec6a70e47/Web%20Investigation%20Picture/5/raw.png?raw=true" alt="Log Management search results showing 6 events for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/01e22fb46bfebd2d0c1c15bec2dbadeec6a70e47/Web%20Investigation%20Picture/5/1.png?raw=true" alt="First log event for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/01e22fb46bfebd2d0c1c15bec2dbadeec6a70e47/Web%20Investigation%20Picture/5/2.png?raw=true" alt="Second log event for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/01e22fb46bfebd2d0c1c15bec2dbadeec6a70e47/Web%20Investigation%20Picture/5/3.png?raw=true" alt="Third log event for the source IP" width="85%" />
</div>

### 🎯 Finding / Answer

Searching the logs for the attacker's IP (`167.99.169.17`) returned **6 firewall events between 11:30 and 11:34 AM**. I compared the first and the last.

| Log | Time | Request | Response | What It Means |
| :-: | :--- | :--- | :--- | :--- |
| 1️⃣ | 11:30 AM | `https://172.16.17.18/`, the site's home page | `200`, 3547 bytes | A normal request. A `200` means the page loaded. |
| 2️⃣ | 11:34 AM | `/search/?q=%22%20OR%201%20%3D%201%20--%20-`, the SQL injection payload from the alert | `500`, 948 bytes | The server tried to handle the request and failed with an Internal Server Error. |

**Reading log 1:** anyone who loads the home page gets a `200`, so it does not prove an attacker "gained access." It only shows the attacker could reach the public website, which is expected. It is best read as reconnaissance: the attacker looked at the site before attacking it.

**Reading log 2:** the `500` response is only 948 bytes, compared with 3547 bytes for the normal page. That is a short error page, not a dump of database data.

### 🤔 Did the Attack Work?

**No.** No data came back, so the attacker did not get anything out. The other requests from the same IP also failed.

> ⚠️ **But a `500` is not a clean "blocked" result.** It does not prove the application is safe, and it does not mean the attacker can't try other payloads.
> - The payload reached the application and likely broke the SQL query, probably through a syntax error caused by the quote.
> - The firewall did not stop it (device action: Allowed).
> - If the application used parameterized queries, the payload would normally be treated as plain search text, and the response would be a normal `200` with no results. Breaking the query suggests the input may not be sanitized, so a more careful attacker could keep adjusting the payload.
>
> That is why the recommendation is to fix the code and block the IP, not just close the ticket.

| Question | Answer |
| :--- | :--- |
| Is the traffic malicious? | 🔴 **Yes** |
| What type of attack? | 💉 **SQL injection** |
| Is there different traffic? | ❌ **No** |

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: CONTAINMENT

| | |
| :--- | :--- |
| **Action** | Decide whether to contain `WebServer1001`. |
| **Method** | Review the logs and the server history for signs of compromise. |

### 🎯 Finding / Answer

Containment (isolating the server from the network) is for when a system is compromised. Here the attack appears to have failed, so isolating `WebServer1001` is not justified.

| Why Containment Isn't Needed | Explanation |
| :--- | :--- |
| 📉 **No data returned** | The injection returned a `500` with a small error page. |
| ❌ **Other requests failed too** | The other requests from the same IP did not succeed either. |
| 🔍 **No signs of compromise** | Nothing in the logs shows a successful login, file upload, command execution, or data leaving the server. |
| 💼 **Business cost** | Taking a production web server offline has a real cost, which should only be accepted when there is evidence of compromise. |

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: REMEDIATION

Instead of containment, the focus is on blocking the source and fixing the weakness.

| Area | Recommended Action |
| :--- | :--- |
| 🌐 **Network** | Block `167.99.169.17` and enable WAF SQL injection rules. |
| 💻 **Application** | Use parameterized queries, input validation, and generic error pages. |
| 🗄️ **Database** | Use a least-privilege service account. |
| 📈 **Monitoring** | Alert on repeated `500` responses and SQL injection patterns, and apply rate limiting. |
| 📝 **Process** | Retest, scan other endpoints, and document the case. |

---

<!-- ===================== STEP 8 ===================== -->
## 📌 STEP 8: ANALYST NOTE, IOCS, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Write the analyst note and add the IoCs found during the investigation. |
| **Method** | Finish the playbook and close the ticket. |

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE (ATTACK UNSUCCESSFUL) 🚨

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert is a **True Positive**: an external, malicious IP sent a SQL injection payload to `WebServer1001`. The attempt did not succeed, since the server returned only a small `500` error page, so the server was not contained. The source IP should be blocked and the application fixed.

---

<!-- ===================== DECISION RATIONALE ===================== -->
## ⚖️ Decision Rationale

| Check | Result | Impact on Verdict |
| :--- | :--- | :--- |
| 🔗 **Requested URL** | Decodes to `" OR 1 = 1 -- -`, a classic SQL injection payload | 🔴 Malicious attempt |
| 🌐 **Source IP reputation** | External, US, flagged malicious | 🔴 Known bad source |
| 📋 **Raw logs** | 6 events: a normal home page visit, then the injection | 🟠 Reconnaissance followed by attack |
| 📉 **Server response** | `500` with a small 948 byte error page | 🟢 No data returned |
| 🚦 **Device action** | Allowed | 🟠 The firewall did not stop the payload |
| 🛑 **Containment** | Not needed | 🟢 No evidence of compromise |

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🌐 Source IP | `167.99.169.17` | SIEM alert, VirusTotal, AbuseIPDB | 🔴 Malicious (US) |
| 💉 Payload | `" OR 1 = 1 -- -` (encoded as `%22%20OR%201%20%3D%201%20--%20-`) | SIEM alert, URL decoder | 🔴 Malicious SQL injection |
| 🔗 Targeted URL | `https://172.16.17.18/search/?q=` | SIEM alert | 🟠 Injectable search parameter |
| 💻 Targeted host | `WebServer1001` (`172.16.17.18`) | SIEM alert | 🟡 Targeted, attack unsuccessful |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Exploit Public-Facing Application | [T1190](https://attack.mitre.org/techniques/T1190/) | A SQL injection payload was sent to the web application's search parameter. The attempt did not succeed. |

---

<!-- ===================== RESPONSE ===================== -->
## 🛡️ Response Actions

- [x] Created the case, took ownership, and started the playbook.
- [x] Recorded the alert details (hostname, IPs, method, URL, user-agent).
- [x] Checked the source IP reputation in VirusTotal and AbuseIPDB.
- [x] Decoded the requested URL and identified the SQL injection payload.
- [x] Reviewed the raw logs and interpreted the `200` and `500` responses.
- [x] Concluded the attack was unsuccessful and that containment was not needed.
- [x] Added the analyst note and IoCs, then closed the ticket.
- [ ] Recommended follow-up: block `167.99.169.17` and enable WAF SQL injection rules.
- [ ] Recommended follow-up: use parameterized queries, input validation, and generic error pages in the application.
- [ ] Recommended follow-up: alert on repeated `500` responses and SQL injection patterns, and scan other endpoints for the same flaw.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![SQL Injection Analysis](https://img.shields.io/badge/SQL%20Injection%20Analysis-1E90FF?style=for-the-badge&labelColor=0D1117)
![URL Decoding](https://img.shields.io/badge/URL%20Decoding-38BDF8?style=for-the-badge&labelColor=0D1117)
![HTTP Status Code Analysis](https://img.shields.io/badge/HTTP%20Status%20Code%20Analysis-0284C7?style=for-the-badge&labelColor=0D1117)
![IP Reputation Analysis](https://img.shields.io/badge/IP%20Reputation%20Analysis-334155?style=for-the-badge&labelColor=0D1117)
![Remediation Planning](https://img.shields.io/badge/Remediation%20Planning-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Do document everything.</i></sub>

</div>
