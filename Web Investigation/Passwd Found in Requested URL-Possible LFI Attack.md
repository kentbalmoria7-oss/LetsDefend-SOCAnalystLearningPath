<!-- ===================== HEADER ===================== -->
<div align="center">

# 📂 Passwd Found in Requested URL: Possible LFI Attack

### LetsDefend SOC Analyst Learning Path · Local File Inclusion (Path Traversal) Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=LFI+Alert+Triage;Path+Traversal+Payload+Analysis;HTTP+500+Response+Interpretation;Attack+Attempt%3A+Unsuccessful" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Category](https://img.shields.io/badge/Category-Local%20File%20Inclusion-1E90FF?style=for-the-badge&logo=linux&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Outcome](https://img.shields.io/badge/Attack-Unsuccessful-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![Cisco Talos](https://img.shields.io/badge/Cisco%20Talos-049FD9?style=flat-square&logo=cisco&logoColor=white)
![Log Management](https://img.shields.io/badge/Log%20Management-334155?style=flat-square&logo=files&logoColor=white)
![Endpoint Security](https://img.shields.io/badge/Endpoint%20Security-1E90FF?style=flat-square&logo=crowdstrike&logoColor=white)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/740cb9a054689d65ee465960429aa8267df274cc/Web%20Investigation%20Picture/8/idor.png?raw=true" alt="Illustration for the LetsDefend LFI lab" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Step 1: Create the Case and Take Ownership](#-step-1-create-the-case-and-take-ownership)
4. [Step 2: Gather Information](#-step-2-gather-information)
5. [Step 3: Check the Source IP Reputation](#-step-3-check-the-source-ip-reputation)
6. [Step 4: Review the Logs in Log Management](#-step-4-review-the-logs-in-log-management)
7. [Step 5: Check Endpoint Security](#-step-5-check-endpoint-security)
8. [Step 6: Containment and Escalation](#-step-6-containment-and-escalation)
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
| 🧪 **Lab** | Passwd Found in Requested URL: Possible LFI Attack |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| ⚡ **Trigger Reason** | URL contains `passwd` |
| 💻 **Hostname** | WebServer1006 |
| 🎯 **Destination IP** | `172.16.17.13` |
| 🌐 **Source IP** | `106.55.45.162` (external, China) |
| 📮 **HTTP Method** | GET |
| 🔗 **Requested URL** | `https://172.16.17.13/?file=../../../../etc/passwd` |
| 🧭 **User-Agent** | `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)` |
| 🚦 **Device Action** | Allowed |
| 📉 **Server Response** | `500` error with no data returned (0 bytes) |
| 🔎 **Endpoint Check** | No unusual processes |
| ✅ **Attack Successful** | No |
| 🛑 **Containment / Escalation** | Not needed |
| 🧰 **Tools Used** | LetsDefend SIEM, VirusTotal, Cisco Talos, Log Management, Endpoint Security |
| ⚖️ **Final Verdict** | 🚨 **True Positive** (malicious attempt, unsuccessful) |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 📂 **Local File Inclusion (LFI):** a web security vulnerability where an attacker tricks a web application into exposing or running local files on the server by using unsafe, unsanitized user input.

### ⚙️ How LFI Attacks Work

| Technique | Explanation |
| :--- | :--- |
| 🪜 **Path traversal** | Attackers put special characters like `../` into URL parameters or input fields to move up directory levels and reach restricted system files. A common target on Unix systems is `/etc/passwd`. |
| 🔗 **Dynamic includes** | Vulnerable applications pass user-controlled variables straight into server file-handling functions (such as PHP's `include` or `require`) without validating them. |

### 🤖 A Simple Analogy

Imagine a website with a page loader built like a helper robot. The robot is told: "Go fetch whatever file name the user types into the browser address bar."

| Scenario | What Happens |
| :--- | :--- |
| ✅ **Normal use** | A user types `about.png`, and the robot fetches and shows `about.png`. |
| ❌ **The attack** | An attacker types a trick path like `../../etc/passwd` (a file holding user account data on Linux servers). Because the website doesn't check or clean the input, the robot obeys, walks backward out of the allowed folder, and hands over private server files. |

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the case from the alert and take ownership. |
| **Method** | Start the playbook to begin the guided investigation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/e2efeef80cb19b977b8715f367ac58adf0865c13/Web%20Investigation%20Picture/8/create%20ticket.png?raw=true" alt="Creating a ticket from the SIEM alert" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/e2efeef80cb19b977b8715f367ac58adf0865c13/Web%20Investigation%20Picture/8/playbook.png?raw=true" alt="Starting the playbook for the case" width="85%" />
</div>

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Review the alert details. |
| **Method** | Record the key facts, then work out what the request is trying to do. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/e2efeef80cb19b977b8715f367ac58adf0865c13/Web%20Investigation%20Picture/8/info.png?raw=true" alt="SIEM alert details for the possible LFI attack" width="85%" />
</div>

### 🎯 Finding / Answer

| Field | Value |
| :--- | :--- |
| 💻 Hostname | `WebServer1006` |
| 🎯 Destination IP address | `172.16.17.13` |
| 🌐 Source IP address | `106.55.45.162` |
| 📮 HTTP request method | `GET` |
| 🔗 Requested URL | `https://172.16.17.13/?file=../../../../etc/passwd` |
| 🧭 User-Agent | `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)` |
| ⚡ Alert trigger reason | URL contains `passwd` |
| 🚦 Device action | Allowed |

### ⚡ What Triggered It

The rule fired because the requested URL contains the word `passwd`. The alert reason, "URL Contains passwd," is simple pattern matching: any request with `passwd` in the URL sets it off, and the rule name adds that it could be an LFI attack.

### 🎯 What the Request Is Trying to Do

This looks like a **Local File Inclusion (LFI)** attempt using a technique called **directory traversal** (or path traversal).

```text
https://172.16.17.13/?file=../../../../etc/passwd
```

| Part | Purpose |
| :--- | :--- |
| `?file=` | A parameter the page uses to decide which file to load. This is the input the attacker is testing. |
| `../` repeated four times | Each one means "go up one folder." Repeating it climbs out of the web directory toward the server's root folder (`/`). The attacker doesn't know how deep the web folder is, so they stack several. |
| `/etc/passwd` | A Linux file listing the system's user accounts. It exists on essentially every Linux server and any user can read it, so it is the classic test file for checking whether LFI works. |

A vulnerable page takes the `file` value and opens that file without checking it. A normal request might be `?file=about.html`. If the page doesn't validate the input, it follows the `../` steps and returns the contents of `/etc/passwd`, which is a list of account names.

> 🔐 **What it would and wouldn't give the attacker:** it doesn't directly reveal passwords, since those hashes live in `/etc/shadow`. But it confirms the flaw exists and shows which accounts are on the server. Attackers often use it as step one before reading more sensitive files such as configuration files and logs.

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK THE SOURCE IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Look up the reputation of the source IP address `106.55.45.162`. |
| **Method** | Search it in **VirusTotal** and **Cisco Talos**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/bdb254b08b7b107d08c5c74de34a72805784ce9d/Web%20Investigation%20Picture/8/virustotal.png?raw=true" alt="VirusTotal reputation result for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/bdb254b08b7b107d08c5c74de34a72805784ce9d/Web%20Investigation%20Picture/8/cisco%20talos.png?raw=true" alt="Cisco Talos reputation result for the source IP" width="85%" />
</div>

### 🎯 Finding / Answer

The reputation lookup doesn't necessarily confirm malicious intent on its own, but the request is **suspicious** and unrelated to any legitimate or internal testing.

> 🌏 **Direction of traffic:** the request originated from an **external** source (China) and targeted internal infrastructure.

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: REVIEW THE LOGS IN LOG MANAGEMENT

| | |
| :--- | :--- |
| **Action** | Search Log Management for the source IP address. |
| **Method** | Open the event and compare the response status and size. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/338eb0c1a8465339e37b801a2fec7d7b4df8a3ee/Web%20Investigation%20Picture/8/logs.png?raw=true" alt="Log Management search results for the source IP" width="85%" />
</div>

### 🎯 Finding / Answer

The server returned a **`500` error with a response size of `0` bytes**, so no data was returned to the attacker.

| Evidence | Why It Points to Failure |
| :--- | :--- |
| 📉 **Status `500`** | For LFI to succeed, the server has to send the file's contents back in the response. A successful read of `/etc/passwd` would normally be a `200` with a body containing lines of account names. |
| 📏 **Response size `0`** | Nothing was returned to the attacker. That is even less than a typical error page. |

### ⚠️ What It Doesn't Prove

- A `500` is **not** the same as "blocked." The request got past the network and into the application, and something broke while processing it. That suggests the `file` parameter may not be properly validated, and the attacker could try other payloads.
- This review covers one event. Other events from `106.55.45.162` in Log Management could show different paths, other file names, or `/etc/shadow` attempts, and some of them might have returned a `200`.

| Question | Answer |
| :--- | :--- |
| Is the traffic malicious? | 🔴 **Yes.** It is a classic path traversal LFI attempt targeting `/etc/passwd`. |
| What is the attack type? | 📂 **LFI (Local File Inclusion).** Similar techniques are used in RFI (Remote File Inclusion), but this is clearly an LFI attempt. |
| What is the direction of traffic? | 🌐 External (China) to internal infrastructure |
| Was the attack successful? | ✅ **No.** The server responded with a `500` error and no data was returned. |

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: CHECK ENDPOINT SECURITY

| | |
| :--- | :--- |
| **Action** | Check the web server in the Endpoint Security tab. |
| **Method** | Look for unusual processes that would suggest a compromise. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/2d7bb86ca82ca65893e0e1029252d0dd15622ee6/Web%20Investigation%20Picture/8/endpoint.png?raw=true" alt="Endpoint Security view of the web server showing no unusual processes" width="85%" />
</div>

### 🎯 Finding / Answer

There are **no unusual processes**, which supports the conclusion that the attack did not penetrate the system.

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: CONTAINMENT AND ESCALATION

| Question | Answer |
| :--- | :--- |
| Is containment needed? | ❌ **No.** There is no sign of a returned file or a compromise, so isolating the server isn't justified on this evidence. |
| Do you need Tier 2 escalation? | ❌ **No.** The attack was not successful and came from the internet, so escalation is not necessary. |

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: REMEDIATION

> 🛠️ **Priority:** because the `500` shows the request got into the application and broke something, the **code fix** matters more than the IP block. Blocking one address does nothing against a different attacker or a different IP.

| Area | Recommended Action |
| :--- | :--- |
| 💻 **Input validation** | Harden input validation on web applications, especially file parameters. |
| 🌐 **Network** | Block `106.55.45.162`, and block the IP or geolocation if the traffic is unnecessary for business use. |
| 🛡️ **WAF** | Deploy or fine-tune a Web Application Firewall to detect and block LFI attempts, including path traversal patterns (`../`, encoded variants like `%2e%2e%2f`, and `/etc/passwd`). |
| 📋 **Allowlist** | Don't pass user input directly into file paths. Use an allowlist of permitted files or page names (for example `?page=about` mapped to a fixed file), and reject anything containing `../`, `..\`, or absolute paths. |
| 🧹 **Path handling** | Resolve the final path and confirm it stays inside the intended web directory before opening it. |
| ⚠️ **Error handling** | Return a generic error page instead of letting the app crash with a `500`. The crash suggests the input reached the file-handling code. |
| 🔐 **Server permissions** | Run the web server with a least-privilege account so that even a successful traversal can only read what that account can access. |
| 📈 **Monitoring** | Alert on traversal patterns and on repeated access attempts or repeated `500` responses from the same IP. |
| 📝 **Process** | Retest the `file` parameter after the fix, scan other endpoints for the same flaw, and document the case. |

---

<!-- ===================== STEP 8 ===================== -->
## 📌 STEP 8: ANALYST NOTE, IOCS, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Write the analyst note and add the IoCs found during the investigation. |
| **Method** | Finish the playbook and close the ticket as a true positive. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/05f97a9bdf62175bf417c7f0921a434f19c11d81/Web%20Investigation%20Picture/8/ioc.png?raw=true" alt="IoCs added to the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/05f97a9bdf62175bf417c7f0921a434f19c11d81/Web%20Investigation%20Picture/8/closed.png?raw=true" alt="Closing the ticket in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE (ATTACK UNSUCCESSFUL) 🚨

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert is a **True Positive**: an external IP sent a path traversal request targeting `/etc/passwd` through the `file` parameter. The attempt did not succeed, since the server returned a `500` error with no data and the endpoint showed no unusual processes. The source IP should be blocked and the application fixed to validate the `file` parameter.

---

<!-- ===================== DECISION RATIONALE ===================== -->
## ⚖️ Decision Rationale

| Check | Result | Impact on Verdict |
| :--- | :--- | :--- |
| 🔗 **Requested URL** | `?file=../../../../etc/passwd`, a classic traversal payload | 🔴 Malicious LFI attempt |
| 🌐 **Source IP** | External (China), suspicious and unrelated to any legitimate testing | 🟠 Suspicious source |
| 📉 **Server response** | `500` with `0` bytes | 🟢 No file contents returned |
| 🔎 **Endpoint Security** | No unusual processes | 🟢 No signs of compromise |
| 🚦 **Device action** | Allowed | 🟠 The network did not block it |
| 🛑 **Containment and escalation** | Not needed | 🟢 No evidence of compromise |

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🌐 Source IP | `106.55.45.162` | SIEM alert, VirusTotal, Cisco Talos | 🔴 Sent the traversal request (China) |
| 💉 Payload | `?file=../../../../etc/passwd` | SIEM alert | 🔴 Malicious LFI attempt |
| 🔗 Targeted URL | `https://172.16.17.13/?file=` | SIEM alert | 🟠 `file` parameter probed for path traversal |
| 🧭 User-Agent | `Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; .NET CLR 1.1.4322)` | SIEM alert | 🟠 Very old browser string, consistent with a script or tool |
| 💻 Targeted host | `WebServer1006` (`172.16.17.13`) | SIEM alert | 🟡 Targeted, attack unsuccessful |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Exploit Public-Facing Application | [T1190](https://attack.mitre.org/techniques/T1190/) | A path traversal payload was sent to a public web application's `file` parameter. The attempt did not succeed. |

---

<!-- ===================== RESPONSE ===================== -->
## ✅ Response Actions

- [x] Created the case, took ownership, and started the playbook.
- [x] Recorded the alert details and explained what triggered the rule.
- [x] Checked the source IP reputation in VirusTotal and Cisco Talos.
- [x] Reviewed the log and interpreted the `500` status and `0` byte response.
- [x] Checked Endpoint Security and found no unusual processes.
- [x] Decided that containment and Tier 2 escalation were not needed.
- [x] Added the IoCs and closed the ticket as a true positive.
- [ ] Recommended follow-up: block `106.55.45.162` and enable WAF rules for path traversal.
- [ ] Recommended follow-up: validate the `file` parameter with an allowlist and fix the crash that produced the `500`.
- [ ] Recommended follow-up: review the other events from this IP for different paths or `/etc/shadow` attempts.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![LFI and Path Traversal Analysis](https://img.shields.io/badge/LFI%20and%20Path%20Traversal%20Analysis-1E90FF?style=for-the-badge&labelColor=0D1117)
![IP Reputation Analysis](https://img.shields.io/badge/IP%20Reputation%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Raw Log Analysis](https://img.shields.io/badge/Raw%20Log%20Analysis-0284C7?style=for-the-badge&labelColor=0D1117)
![Endpoint Verification](https://img.shields.io/badge/Endpoint%20Verification-334155?style=for-the-badge&labelColor=0D1117)
![Remediation Planning](https://img.shields.io/badge/Remediation%20Planning-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
