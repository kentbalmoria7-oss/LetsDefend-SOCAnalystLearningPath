<!-- ===================== HEADER ===================== -->
<div align="center">

# 🔗 SOC105: Requested T.I. URL Address

### LetsDefend SOC Analyst Learning Path · Threat Intelligence URL Alert Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=Threat+Intelligence+URL+Alert+Triage;Shortened+URL+%26+Redirect+Analysis;Safe+Browsing+with+Browserling;Verdict%3A+False+Positive" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Alert](https://img.shields.io/badge/Alert-SOC105%20(High)-1E90FF?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-False%20Positive-22C55E?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Status](https://img.shields.io/badge/Status-Completed-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![AbuseIPDB](https://img.shields.io/badge/AbuseIPDB-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![Browserling](https://img.shields.io/badge/Browserling-0EA5E9?style=flat-square&logo=googlechrome&logoColor=white)
![Threat Intelligence](https://img.shields.io/badge/Threat%20Intelligence-334155?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/30bb11848216f41845823a660348afda1c674046/Cyber%20Threat%20Investigation%20Picture/cti.png?raw=true" alt="Cyber threat intelligence illustration" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Step 1: Create the Case and Take Ownership](#-step-1-create-the-case-and-take-ownership)
4. [Step 2: Gather Information](#-step-2-gather-information)
5. [Step 3: Check the IP Reputation](#-step-3-check-the-ip-reputation)
6. [Step 4: Investigate the Shortened URL](#-step-4-investigate-the-shortened-url)
7. [Step 5: Check the Logs](#-step-5-check-the-logs)
8. [Step 6: Containment and Remediation](#-step-6-containment-and-remediation)
9. [Step 7: Artifacts, Analyst Note, and Close the Case](#-step-7-artifacts-analyst-note-and-close-the-case)
10. [Decision Rationale](#-decision-rationale)
11. [Artifacts](#-artifacts)
12. [Response Actions](#-response-actions)
13. [Skills Demonstrated](#-skills-demonstrated)

---

<!-- ===================== CASE SUMMARY ===================== -->
## 📋 Case Summary

| Field | Details |
| :--- | :--- |
| 🧪 **Lab** | Requested T.I. URL Address |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🆔 **Event ID** | 75 |
| 🕒 **Event Time** | Mar 07, 2021, 05:47 PM |
| 🚨 **Rule** | SOC105: Requested T.I. URL Address (High severity) |
| 💻 **Source Hostname** | MarksPhone |
| 🌐 **Source Address** | `10.15.15.12` |
| 👤 **Username** | Mark |
| 🎯 **Destination Address** | `67.199.248.10` (`bit.ly`) |
| 🔗 **Request URL** | `https://bit.ly/TAPSCAN` |
| 🧭 **User Agent** | `Mozilla/5.0 (Windows NT 5.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.90 Safari/537.36` |
| 🚦 **Device Action** | Allowed |
| 🔀 **Redirects To** | `play.google.com` (the TapScanner app page) |
| 🛑 **Containment** | Not needed |
| 🧰 **Tools Used** | LetsDefend SIEM, AbuseIPDB, VirusTotal, Browserling, Log Management, Endpoint Security |
| ⚖️ **Final Verdict** | ✅ **False Positive** |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🛰️ **Threat intelligence (TI or CTI):** the collection, analysis, and sharing of data about existing or potential cyberattacks against an organization. Instead of only reacting to a breach after it happens, TI helps security teams predict and stop attacks before they cause damage.

### 📚 What Threat Intelligence Contains

| Element | Explanation |
| :--- | :--- |
| 🧬 **Indicators of Compromise (IoCs)** | Technical artifacts like malicious IP addresses, domain names, file hashes, or phishing URLs. |
| 🧰 **Tactics, Techniques, and Procedures (TTPs)** | The specific methods, tools, and routines that attackers use to break into systems. |
| 🕵️ **Threat actors** | The individuals or groups behind attacks, including their motives and target industries. |

### 🧭 Three Main Types of Threat Intelligence

| Type | Explanation |
| :--- | :--- |
| 🏢 **Strategic TI** | High-level reports for business leaders and board members, focused on broad trends and financial risks. |
| 🤖 **Tactical TI** | Automated data feeds (like IoCs) built into security tools to block known bad traffic or files. |
| 🔬 **Operational TI** | Detailed analysis of active attacks, attacker behavior, and incident responses, used by security operations centers (SOCs). |

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE CASE AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the case from the alert and take ownership. |
| **Method** | Start the playbook to begin the guided investigation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/402d46142b3303c3e9510baaa4886cbe0bcc3785/Cyber%20Threat%20Investigation%20Picture/create%20case.png?raw=true" alt="Creating a case from the SIEM alert" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/402d46142b3303c3e9510baaa4886cbe0bcc3785/Cyber%20Threat%20Investigation%20Picture/start%20playbook.png?raw=true" alt="Starting the playbook for the case" width="85%" />
</div>

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Review the alert details. |
| **Method** | Record the key facts, then work out why the alert fired. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/402d46142b3303c3e9510baaa4886cbe0bcc3785/Cyber%20Threat%20Investigation%20Picture/info.png?raw=true" alt="SIEM alert details for SOC105" width="85%" />
</div>

### 🎯 Finding / Answer

| Field | Value |
| :--- | :--- |
| 🆔 Event ID | `75` |
| 🕒 Event time | Mar 07, 2021, 05:47 PM |
| 🚨 Rule | SOC105: Requested T.I. URL Address |
| 🌐 Source address | `10.15.15.12` |
| 💻 Source hostname | `MarksPhone` |
| 🎯 Destination address | `67.199.248.10` |
| 🏷️ Destination hostname | `bit.ly` |
| 👤 Username | `Mark` |
| 🔗 Request URL | `https://bit.ly/TAPSCAN` |
| 🚦 Device action | Allowed |

### ⚡ Why the Alert Fired

The **SOC105: Requested T.I. URL Address** alert is triggered when a device on the network requests a web address that matches a known threat indicator in the system's threat intelligence rules.

| Point | Details |
| :--- | :--- |
| 📱 **The request** | A device named `MarksPhone` (`10.15.15.12`), used by the user Mark, tried to open `https://bit.ly/TAPSCAN`. |
| 🎯 **The destination** | The request went to the hostname `bit.ly` at `67.199.248.10`. |
| 🚨 **The severity** | The system flagged the request as **High** severity. |
| 🚦 **The outcome** | Despite the high severity, the device action is **Allowed**, so the security appliance let the connection through instead of blocking it. |

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK THE IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Check the reputation of the destination IP address `67.199.248.10`. |
| **Method** | Search it in **AbuseIPDB** and **VirusTotal**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/402d46142b3303c3e9510baaa4886cbe0bcc3785/Cyber%20Threat%20Investigation%20Picture/abuseipdb.png?raw=true" alt="AbuseIPDB result for the destination IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/402d46142b3303c3e9510baaa4886cbe0bcc3785/Cyber%20Threat%20Investigation%20Picture/virustotal.png?raw=true" alt="VirusTotal result for the destination IP" width="85%" />
</div>

### 🎯 Finding / Answer

VirusTotal categorizes the destination as **non-malicious**.

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: INVESTIGATE THE SHORTENED URL

| | |
| :--- | :--- |
| **Action** | Look at what the shortened URL actually leads to. |
| **Method** | Use **Browserling** to open the site in a safe, remote browser, then check the specific URL in VirusTotal. |

### 🌐 What Is Bitly?

Out of curiosity about the service, I used Browserling to open the website securely, even though VirusTotal categorizes it as non-malicious.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/402d46142b3303c3e9510baaa4886cbe0bcc3785/Cyber%20Threat%20Investigation%20Picture/browserling%201.png?raw=true" alt="Opening bit.ly in Browserling, first view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/402d46142b3303c3e9510baaa4886cbe0bcc3785/Cyber%20Threat%20Investigation%20Picture/browserling%202.png?raw=true" alt="Opening bit.ly in Browserling, second view" width="85%" />
</div>

The website is clean. **Bitly** is a link management and URL shortening platform that turns long web addresses into short, clean, trackable links. On the other hand, attackers can use shorteners to **obfuscate** banned domain names and mask the real address of a malicious website, which makes it harder for users to spot a dangerous link.

### 🔎 Checking the Specific URL

Next, the specific URL `https://bit.ly/TAPSCAN` was opened in Browserling and checked in VirusTotal.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/402d46142b3303c3e9510baaa4886cbe0bcc3785/Cyber%20Threat%20Investigation%20Picture/browserling.png?raw=true" alt="Opening the specific shortened URL in Browserling" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5c4d6d0fc11944e0eb2cc3263cb814b334822ebb/Cyber%20Threat%20Investigation%20Picture/Screenshot%202026-10-18%20202026.png?raw=true" alt="VirusTotal result for the specific shortened URL" width="85%" />
</div>

### 🎯 Finding / Answer

| Observation | Details |
| :--- | :--- |
| 🚩 **VirusTotal flags it** | The shortened URL is flagged as **malicious**, but that could be a false positive, so a deeper look was needed. |
| 🔀 **Where it really goes** | The URL redirects to `play.google.com`, which VirusTotal does not flag as malicious. |
| 📱 **The app** | The destination is the **TapScanner** app, which has 50M+ downloads and a 4.7 star rating. |

### ❓ Why Do Bitly Links Get Flagged So Often?

`bit.ly` is a legitimate and widely used link shortener, but VirusTotal, antivirus vendors, and threat intelligence feeds often flag generic Bitly links, and sometimes even the main domain, as phishing or suspicious. This comes from how shorteners work and how automated scanners judge risk.

| Reason | Explanation |
| :--- | :--- |
| 🚨 **High rate of abuse** | Bitly lets anyone shorten almost any URL for free, so attackers use it heavily in phishing emails and SMS messages to mask credential-harvesting pages or direct malware downloads. Millions of bad links pass through alongside legitimate ones, so scanners treat the domain as higher risk. |
| 🎭 **URL obfuscation** | Security tools judge the full destination URL (reputation, domain age, risk). A shortened link hides the real host, and scanners see shorteners as a way to bypass email gateways and filters. If a scanner can't verify the destination, or the link redirects several times, it may flag it as suspicious in advance. |
| 🤝 **Shared domain reputation** | Every Bitly short link shares the base domain `bit.ly`. If thousands of malicious short links are reported to a vendor in a short time, some vendors end up penalizing the whole domain until their database updates. |
| 🕶️ **Dynamic redirection and cloaking** | Attackers can set a link to show a harmless page to a security scanner and a malicious phishing page to a real victim. Because vendors know about this, they flag Bitly links aggressively. |

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: CHECK THE LOGS

| | |
| :--- | :--- |
| **Action** | Search Log Management and Endpoint Security for the source host. |
| **Method** | Confirm which device made the request, and check for suspicious activity. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5c4d6d0fc11944e0eb2cc3263cb814b334822ebb/Cyber%20Threat%20Investigation%20Picture/logs.png?raw=true" alt="Log Management search results for the source host" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5c4d6d0fc11944e0eb2cc3263cb814b334822ebb/Cyber%20Threat%20Investigation%20Picture/1%20log.png?raw=true" alt="First raw log for the request" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5c4d6d0fc11944e0eb2cc3263cb814b334822ebb/Cyber%20Threat%20Investigation%20Picture/2%20log.png?raw=true" alt="Second raw log for the request" width="85%" />
</div>

### 🎯 Finding / Answer

| Check | Result |
| :--- | :--- |
| 📱 **Who made the request** | `MarksPhone` |
| 🌐 **Source IP address** | `10.15.15.12` |
| 🔎 **Suspicious process or network action** | None found |
| 🛑 **Containment required** | No |

From all of this information, the alert is a **false positive**.

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: CONTAINMENT AND REMEDIATION

### 🛑 Containment

| Question | Answer |
| :--- | :--- |
| Is containment needed? | ❌ **No** |

### 🛠️ Remediation

| Area | Recommended Action |
| :--- | :--- |
| 🔒 **No asset containment required** | Don't isolate or quarantine `MarksPhone` (`10.15.15.12`) from the network, since no threat was executed. |
| 🧹 **No host remediation needed** | Malicious file scanning, credential resets, and endpoint reimaging are unnecessary for this alert. |
| 🔗 **Add URL unrolling at the gateway** | Configure Secure Email Gateways (SEG), web proxies, and next-gen firewalls (NGFW) to automatically inspect the **final destination** of shortened links (such as `bit.ly`, `tinyurl.com`, and `t.co`) before scoring reputation. |
| 🧠 **Refine the SIEM and threat intel alert logic** | Change the alert rule so it needs both a high-risk URL shortener match **and** a suspicious follow-on event on the endpoint, such as a process or payload download. |
| ✅ **Update the local allowlist** | If business tasks need specific shortened links that point to trusted places (like official app stores), document them and allowlist the verified destination patterns. |

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: ARTIFACTS, ANALYST NOTE, AND CLOSE THE CASE

| | |
| :--- | :--- |
| **Action** | Add the artifacts and write the analyst note. |
| **Method** | Finish the playbook and close the case as a false positive. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5c4d6d0fc11944e0eb2cc3263cb814b334822ebb/Cyber%20Threat%20Investigation%20Picture/ioc.png?raw=true" alt="Artifacts added to the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5c4d6d0fc11944e0eb2cc3263cb814b334822ebb/Cyber%20Threat%20Investigation%20Picture/analyst%20note.png?raw=true" alt="Analyst note written for the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/5c4d6d0fc11944e0eb2cc3263cb814b334822ebb/Cyber%20Threat%20Investigation%20Picture/case%20closed.png?raw=true" alt="Closing the case in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## ✅ VERDICT: FALSE POSITIVE ✅

</div>

### 🎯 Overall Conclusion

Based on the analysis done, this alert is a **False Positive**: the shortened URL redirects to the Google Play page of a popular, well-rated app (TapScanner), the IP is not flagged as malicious, and Log Management and Endpoint Security show no suspicious process or network activity. The alert fired because Bitly links are often flagged by threat intelligence feeds, even when their destination is harmless.

---

<!-- ===================== DECISION RATIONALE ===================== -->
## ⚖️ Decision Rationale

| Check | Result | Impact on Verdict |
| :--- | :--- | :--- |
| 🌐 **Destination IP reputation** | VirusTotal: non-malicious | 🟢 No known malicious infrastructure |
| 🔗 **Shortened URL** | Flagged malicious in VirusTotal | 🟡 Likely a false positive from the shortener |
| 🔀 **Real destination** | `play.google.com`, the TapScanner app page (50M+ downloads, 4.7 stars) | 🟢 Legitimate destination |
| 🌐 **Browserling check** | The site loaded as clean | 🟢 No malicious behavior seen |
| 📋 **Logs and endpoint** | No suspicious process or network action | 🟢 No signs of compromise |
| 🚦 **Device action** | Allowed | 🟢 No harm resulted |
| 🛑 **Containment** | Not needed | 🟢 No endpoint action required |

---

<!-- ===================== ARTIFACTS ===================== -->
## 🧬 Artifacts

No malicious indicators were found. These are the artifacts recorded for the case:

| Type | Artifact | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🔗 URL | `https://bit.ly/TAPSCAN` | SIEM alert | 🟡 Flagged by VirusTotal, but redirects to a legitimate app page |
| 🎯 Destination IP | `67.199.248.10` (`bit.ly`) | SIEM alert, VirusTotal | 🟢 Benign |
| 💻 Source host | `MarksPhone` (`10.15.15.12`), user `Mark` | SIEM alert | 🟢 No compromise found |

---

<!-- ===================== RESPONSE ===================== -->
## ✅ Response Actions

- [x] Created the case, took ownership, and started the playbook.
- [x] Recorded the alert details and explained why the rule fired.
- [x] Checked the destination IP in AbuseIPDB and VirusTotal.
- [x] Opened the shortened URL safely in Browserling and traced where it redirects.
- [x] Reviewed Log Management and Endpoint Security and found nothing suspicious.
- [x] Decided that containment was not needed.
- [x] Added the artifacts and the analyst note, then closed the case as a false positive.
- [ ] Recommended follow-up: add URL unrolling at the web gateway, so shortened links are scored by their final destination.
- [ ] Recommended follow-up: tune rule SOC105 to need a follow-on suspicious event, and allowlist verified destinations such as official app stores.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![Threat Intelligence Alert Triage](https://img.shields.io/badge/Threat%20Intelligence%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![False Positive Identification](https://img.shields.io/badge/False%20Positive%20Identification-1E90FF?style=for-the-badge&labelColor=0D1117)
![URL Redirect Analysis](https://img.shields.io/badge/URL%20Redirect%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![Safe Browsing Practices](https://img.shields.io/badge/Safe%20Browsing%20Practices-0284C7?style=for-the-badge&labelColor=0D1117)
![Log Review](https://img.shields.io/badge/Log%20Review-334155?style=for-the-badge&labelColor=0D1117)
![Detection Rule Tuning](https://img.shields.io/badge/Detection%20Rule%20Tuning-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
