<!-- ===================== HEADER ===================== -->
<div align="center">

# 🧨 SOC166: JavaScript Code Detected in Requested URL

### LetsDefend SOC Analyst Learning Path · Cross-Site Scripting (XSS) Alert Investigation

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=20&duration=3000&pause=900&color=00D4FF&center=true&vCenter=true&width=760&height=50&lines=XSS+Alert+Triage;Five+Payload+Variations+Analyzed;Raw+Log+%26+HTTP+Status+Code+Analysis;Attack+Attempt%3A+Unsuccessful" alt="Typing SVG" />

<br/>

![Platform](https://img.shields.io/badge/Platform-LetsDefend%20SIEM-0EA5E9?style=for-the-badge&logo=hackthebox&logoColor=white&labelColor=0D1117)
![Alert](https://img.shields.io/badge/Alert-SOC166%20(Medium)-1E90FF?style=for-the-badge&logo=target&logoColor=white&labelColor=0D1117)
![Verdict](https://img.shields.io/badge/Verdict-True%20Positive-DC2626?style=for-the-badge&logo=shield&logoColor=white&labelColor=0D1117)
![Outcome](https://img.shields.io/badge/Attack-Unsuccessful-22C55E?style=for-the-badge&logo=checkmarx&logoColor=white&labelColor=0D1117)

<br/>

![SIEM](https://img.shields.io/badge/SIEM-0F172A?style=flat-square&logo=splunk&logoColor=00D4FF)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=flat-square&logo=virustotal&logoColor=white)
![AbuseIPDB](https://img.shields.io/badge/AbuseIPDB-DC2626?style=flat-square&logo=abusedotch&logoColor=white)
![Log Management](https://img.shields.io/badge/Log%20Management-334155?style=flat-square&logo=files&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE%20ATT%26CK-C8102E?style=flat-square&logo=target&logoColor=white)

<br/>

<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/620ce0c20f0033e0cbdfee438e80782937383ed2/Web%20Investigation%20Picture/6/js_cover-min.png?raw=true" alt="JavaScript cover image for the LetsDefend XSS lab" width="85%" />

</div>

---

<!-- ===================== NAVIGATION ===================== -->
## 🧭 Table of Contents

1. [Case Summary](#-case-summary)
2. [Key Concepts](#-key-concepts)
3. [Step 1: Create the Ticket and Take Ownership](#-step-1-create-the-ticket-and-take-ownership)
4. [Step 2: Gather Information](#-step-2-gather-information)
5. [Step 3: Check the Source IP Reputation](#-step-3-check-the-source-ip-reputation)
6. [Step 4: Review the Raw Logs](#-step-4-review-the-raw-logs)
7. [Step 5: Did the Attack Work?](#-step-5-did-the-attack-work)
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
| 🧪 **Lab** | JavaScript Code Detected in Requested URL |
| 🏫 **Platform** | LetsDefend SIEM (simulated SOC environment) |
| 🚨 **Alert Rule** | SOC166 (Medium severity) |
| ⚡ **Trigger Reason** | JavaScript code detected in URL |
| 💻 **Hostname** | WebServer1002 |
| 🌐 **Source IP** | `112.85.42.13` (external) |
| 🎯 **Destination IP** | `172.16.17.17` on port `443` |
| 📮 **HTTP Method** | GET |
| 🔗 **Requested URL** | `https://172.16.17.17/search/?q=<$script>javascript:$alert(1)<$/script>` |
| 🧭 **User-Agent** | `Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.1` |
| 📅 **Date** | Feb 26, 2022, between 6:34 PM and 6:56 PM |
| 🔢 **Events Found** | 8 (3 normal requests and 5 XSS payload attempts) |
| 🚦 **Device Action** | Allowed |
| 📉 **Response to Every Payload** | `302 Found`, response size `0` |
| ✅ **Attack Successful** | No, no page containing the payload was returned |
| 🛑 **Containment / Escalation** | Not needed |
| 🧰 **Tools Used** | LetsDefend SIEM, VirusTotal, AbuseIPDB, Log Management |
| ⚖️ **Final Verdict** | 🚨 **True Positive** (malicious attempt, unsuccessful) |

---

<!-- ===================== KEY CONCEPTS ===================== -->
## 💡 Key Concepts

> 🌐 **JavaScript:** a high-level, dynamic programming language that makes websites interactive. It is one of the core technologies of the web, alongside HTML and CSS.

> 🏠 **The house analogy:** if a website were a house, HTML would be the wooden frame and walls (the structure), CSS would be the paint and furniture (the look), and JavaScript would be the electricity and plumbing, the part that makes things actually happen when you click them.

### 🛡️ Why JavaScript Matters in Cybersecurity

JavaScript powers the vast front-end attack surface of the modern web, so it is central to web application vulnerabilities and client-side attacks.

| Topic | Explanation |
| :--- | :--- |
| 💉 **Cross-Site Scripting (XSS)** | A vulnerability where an app runs untrusted script data inside a user's browser. It is the main injection risk unique to JavaScript. |
| 💳 **Magecart and digital skimming** | Attackers inject malicious JavaScript sniffers into e-commerce checkout pages to steal payment card data directly from users' browsers. |
| 👁️ **Client-side exposure** | Code shipped to a browser is fully visible and editable. Hiding business logic, API secrets, or security checks in client-side JavaScript is unsafe. |
| 🔍 **Web application security** | Professionals need to understand JavaScript methods, DOM manipulation, and insecure APIs like `eval()` to find and fix vulnerabilities. |

### 😈 How Attackers Use JavaScript

- Cross-Site Scripting (XSS)
- Malvertising (malicious ads)
- Denial of Service (DoS) attacks
- Keylogging

### 💉 What Is XSS?

> Cross-site scripting (XSS) is a security flaw that lets a hacker put bad code into a safe website. When you visit the site, your browser runs the hidden code because it thinks the code belongs to the website.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/c7e8209e9bf7ef024b3f8d567ae84d9ddb4030cd/Web%20Investigation%20Picture/6/anatomy%20%20xss.png?raw=true" alt="Anatomy of an XSS attack" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/3%20types%20xss.png?raw=true" alt="The three types of XSS" width="85%" />
</div>

### 🧭 Types of XSS

| Type | Analogy | How It Works | Key Takeaway |
| :--- | :--- | :--- | :--- |
| 📌 **Stored XSS** | **The bulletin board.** An attacker pins a malicious note to a public community bulletin board (the server database). | The attacker walks away. Later, any innocent person who reads the note triggers the instructions on it. | The bad code is permanently saved on the website, so every visitor to the page triggers the attack without further action from the attacker. |
| 🪞 **Reflected XSS** | **The megaphone / custom sign.** An attacker hands a specific person a tricky sign or link, often through a phishing email or message (a malicious URL). | The victim takes the sign to the front desk (web server), which reads it out loud and echoes it back (HTTP response). The bad code runs only for that one person. | The code is not saved on the server. The attacker has to trick a specific user into clicking a link or sending a custom request every time. |

> 🔎 **This case:** the payload sits in the URL's search parameter (`q`), so it is a **reflected XSS** attempt.

---

<!-- ===================== STEP 1 ===================== -->
## 📌 STEP 1: CREATE THE TICKET AND TAKE OWNERSHIP

| | |
| :--- | :--- |
| **Action** | Create the ticket from the alert and take ownership. |
| **Method** | Start the playbook to begin the guided investigation. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/50dc01ad8e9117f4853619255876507c60989864/Web%20Investigation%20Picture/6/create.png?raw=true" alt="Creating a ticket from the SIEM alert" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/50dc01ad8e9117f4853619255876507c60989864/Web%20Investigation%20Picture/6/playbook.png?raw=true" alt="Starting the playbook for the case" width="85%" />
</div>

---

<!-- ===================== STEP 2 ===================== -->
## 📌 STEP 2: GATHER INFORMATION

| | |
| :--- | :--- |
| **Action** | Review the alert details. |
| **Method** | Record the key facts, then work out what triggered the rule. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/c7e8209e9bf7ef024b3f8d567ae84d9ddb4030cd/Web%20Investigation%20Picture/6/info.png?raw=true" alt="SIEM alert details for SOC166" width="85%" />
</div>

### 🎯 Finding / Answer

| Field | Value |
| :--- | :--- |
| 💻 Hostname | `WebServer1002` |
| 🎯 Destination IP address | `172.16.17.17` |
| 🌐 Source IP address | `112.85.42.13` |
| 📮 HTTP request method | `GET` |
| 🔗 Requested URL | `https://172.16.17.17/search/?q=<$script>javascript:$alert(1)<$/script>` |
| 🧭 User-Agent | `Mozilla/5.0 (Windows NT 6.1; WOW64; rv:40.0) Gecko/20100101 Firefox/40.1` |
| ⚡ Alert trigger reason | JavaScript code detected in URL |
| 🚦 Device action | Allowed |

The SIEM raised an alert for a possible **cross-site scripting (XSS)** attempt. Rule **SOC166** fired because a web request to `WebServer1002` had JavaScript code inside the URL. It is a **Medium** severity alert.

### 🧩 What Triggered the Rule

The search parameter `q` contains script-like content:

| Part | Purpose |
| :--- | :--- |
| `<script> ... </script>` | The HTML tag that runs JavaScript in a browser. |
| `javascript:` | A URL scheme that also executes code. |
| `alert(1)` | The classic XSS test payload. It pops up a message box, so an attacker can quickly tell whether their code ran. |

The rule is **pattern matching**, so keywords like `<script>`, `javascript:`, and `alert(` in a URL are enough to set it off.

### 🎯 What the Attacker Was Trying to Do

The goal is probably **reflected XSS**. If the search page echoes the `q` value back into its HTML without escaping it, the browser would treat it as real code and run it. A real attacker would replace `alert(1)` with code that steals session cookies or redirects users, then trick a victim into clicking the crafted link. Using `alert(1)` suggests this was a test to see whether the search box is vulnerable.

> 💡 **An interesting detail:** the payload contains `$` characters (`<$script>`, `$alert(1)`). A real browser would not treat `<$script>` as a script tag, so this exact string most likely would not execute even if it were reflected back. That could mean a sloppy scanner or script, or the `$` may simply be how the platform displays the log. It should be verified against the raw log instead of assumed either way.

---

<!-- ===================== STEP 3 ===================== -->
## 📌 STEP 3: CHECK THE SOURCE IP REPUTATION

| | |
| :--- | :--- |
| **Action** | Look up the reputation of the source IP address `112.85.42.13`. |
| **Method** | Search it in **VirusTotal** and **AbuseIPDB**. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/virus.png?raw=true" alt="VirusTotal reputation result for the source IP" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/ipdb.png?raw=true" alt="AbuseIPDB reputation result for the source IP, first view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/ipdb2.png?raw=true" alt="AbuseIPDB reputation result for the source IP, second view" width="85%" />
</div>

### 🎯 Finding / Answer

The source IP is an **external address**, not part of the internal network, and its reputation was checked in both tools. Whatever the reputation shows, the raw logs below are what decide the verdict.

---

<!-- ===================== STEP 4 ===================== -->
## 📌 STEP 4: REVIEW THE RAW LOGS

| | |
| :--- | :--- |
| **Action** | Search Log Management for the source IP address `112.85.42.13`. |
| **Method** | Open each event and compare the request, response status, and response size. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/raw%20logs.png?raw=true" alt="Log Management search results showing 8 events for the source IP" width="85%" />
</div>

### 🎯 Finding / Answer

**8 events** were found. They run from `112.85.42.13` to `172.16.17.17` on port `443` between about 6:34 PM and 6:56 PM on Feb 26, 2022. All use the same Firefox user-agent and the same device action.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/1.png?raw=true" alt="Raw log event 1" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/2.png?raw=true" alt="Raw log event 2" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/f1bfbaa6aff333ba6500b37c229df8a77f1b456e/Web%20Investigation%20Picture/6/3.1.png?raw=true" alt="Raw log event 3, first view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/3.png?raw=true" alt="Raw log event 3, second view" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/4.png?raw=true" alt="Raw log event 4" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/5.png?raw=true" alt="Raw log event 5" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/6.png?raw=true" alt="Raw log event 6" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/333d19cb493f85b67f42a743946c7f9c005ef2dc/Web%20Investigation%20Picture/6/7.png?raw=true" alt="Raw log event 7" width="85%" />
</div>

### 📖 The Overall Story

One outside computer (`112.85.42.13`) visited the company web server (`172.16.17.17`) over about 22 minutes. It first browsed the site, then used the search box with a harmless word, and finally sent a series of script payloads through the same search box. The last event correlates with the alert.

### ✅ The Three Normal Requests

| Request URL | Method | Response | Size | What It Means |
| :--- | :---: | :---: | :---: | :--- |
| `https://172.16.17.17/` | GET | `200` | 1024 bytes | Visited the home page |
| `https://172.16.17.17/about-us/` | GET | `200` | 3531 bytes | Visited a normal page |
| `https://172.16.17.17/search/?q=test` | GET | `200` | 885 bytes | Tried the search box with harmless text |

> 🔍 **About the `200` responses:** a `200` only means "OK, here's the page." Those requests aren't dangerous on their own, and they are not what the alert is about. They look like someone exploring the site and finding the search feature. The `q=test` request also gives a **baseline**: a normal search answers with a `200` and real content.

### 🚨 The Five XSS Payload Attempts

Every payload goes to the search box (`/search/?q=`) and tries a different XSS technique. The idea in each case is to make the website print the attacker's code back on the page so a visitor's browser runs it.

| # | Payload in the `q` Parameter | Technique | Response | Size |
| :-: | :--- | :--- | :---: | :---: |
| 1️⃣ | `prompt(8)` | A bare JavaScript function call with no tags, a quick test of whether input is run or reflected. | `302` | 0 |
| 2️⃣ | `<$img src =q onerror=prompt(8)$>` | A deliberately broken image. When it fails to load, the `onerror` handler runs the code. Attackers use this to get around filters that only look for `<script>` tags. | `302` | 0 |
| 3️⃣ | `<$svg><$script ?>$alert(1)` | An SVG tag variation, another way of slipping past tag filters. | `302` | 0 |
| 4️⃣ | `<$script>$for((i)in(self))eval(i)(1)<$/script>` | An obfuscated payload that loops through the browser's built-in names and runs them, a known trick for calling `alert` without typing the word, so keyword filters miss it. | `302` | 0 |
| 5️⃣ | `<$script>javascript:$alert(1)` | The classic test: if a popup with "1" appears, the attacker knows scripts run on that page and can move on to something harmful. | `302` | 0 |

> 📝 **The pattern:** the attacker started simple and kept changing technique, which suggests they were probing for a filter weakness, possibly with a tool.

---

<!-- ===================== STEP 5 ===================== -->
## 📌 STEP 5: DID THE ATTACK WORK?

### 🔀 What a `302` Means

The HTTP `302 Found` status code indicates that the requested resource has been **temporarily moved** to a different URL. The client should keep using the original URL for future requests. Typically, the server provides the new location in the `Location` header of the response, telling the client where to redirect.

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/b84c921f1fc8a0a3f087faedbaf30efe82dcb016/Web%20Investigation%20Picture/6/0231-http-header.png?raw=true" alt="HTTP 302 redirect and the Location header" width="85%" />
</div>

| Key Detail | Explanation |
| :--- | :--- |
| 🔁 **Temporary redirect** | It tells the browser or search engine that the original URL will return later. |
| 📍 **Location header** | The server includes it in the response to tell the browser where to go immediately. |

### 🎯 Finding / Answer

**No, the attack was not successful.** The last request got a `302` and a response size of `0`. A `302` means the server did not show a results page and told the browser to go somewhere else instead, and a size of `0` means no page content came back. For XSS to work, the script has to be sent back inside a page, and here no page came back. So the payload was never shown to a browser, and nothing was executed.

| Evidence | Why It Points to Failure |
| :--- | :--- |
| 🔀 **Status `302` on all five payloads** | The server redirected instead of returning a results page. |
| 📏 **Response size `0`** | No page content returned, so there was nothing for a browser to execute. |
| 🔁 **Same result for five different techniques** | Identical handling across plain, image, SVG, obfuscated, and script payloads is stronger evidence of a filter or application-level protection than one failed request would be. |

### ⚠️ What the Logs Can't Tell Us

- A `302` doesn't prove the site is safe. It only shows these inputs were handled, and a different payload might get through.
- The device action says **Allowed** on every request, so the network didn't block anything. Whatever stopped the payloads happened at the application level. The redirect is good, but it was not a guarantee.
- The logs don't show where the redirects pointed.

> 🔎 **Where to find the `Location` header:** by default, standard web server logs (like the Common Log Format on Nginx, Apache, or IIS) do **not** record response headers like `Location`. The log configuration has to be updated to capture it, or the header can be read from reverse proxy logs or captured traffic.

| Question | Answer |
| :--- | :--- |
| Is the traffic malicious? | 🔴 **Yes** |
| Is there a different request or traffic? | ❌ **No** |
| What type of attack? | 💉 **Reflected cross-site scripting (XSS)** |
| Was the attack successful? | ✅ **No** |

---

<!-- ===================== STEP 6 ===================== -->
## 📌 STEP 6: CONTAINMENT AND ESCALATION

| Question | Answer |
| :--- | :--- |
| Is containment needed? | ❌ **No.** There is no sign of compromise, so the server doesn't need to be isolated. |
| Does this need to be escalated? | ❌ **No** |

---

<!-- ===================== STEP 7 ===================== -->
## 📌 STEP 7: REMEDIATION

| Area | Recommended Action |
| :--- | :--- |
| 🔒 **Secure the web application** | Block special characters like `<`, `>`, and `$`, or script tags altogether, to prevent this type of injection. |
| 🧩 **Patch and update** | Keep the web application and its components patched and up to date. |
| 🌐 **Network** | Block `112.85.42.13` and enable WAF XSS rules. |
| 🧱 **Browser protections** | Add a Content Security Policy (CSP) and set cookies as `HttpOnly` and `Secure`. |
| 📈 **Monitoring** | Alert on `<script>`, `javascript:`, `onerror=`, and similar patterns in URLs. |

> 💡 **Best practice:** besides blocking characters, encode output so user input is shown as plain text and never run as code.

---

<!-- ===================== STEP 8 ===================== -->
## 📌 STEP 8: ANALYST NOTE, IOCS, AND CLOSE THE TICKET

| | |
| :--- | :--- |
| **Action** | Write the analyst note and add the IoCs found during the investigation. |
| **Method** | Finish the playbook and close the ticket. |

<div align="center">
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/c975e6c0cc2d2d39e7dd940e70b1dc36492ff290/Web%20Investigation%20Picture/6/analyst%20note.png?raw=true" alt="Analyst note written for the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/c975e6c0cc2d2d39e7dd940e70b1dc36492ff290/Web%20Investigation%20Picture/6/ioc.png?raw=true" alt="IoCs added to the case" width="85%" />
<br/><br/>
<img src="https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/c975e6c0cc2d2d39e7dd940e70b1dc36492ff290/Web%20Investigation%20Picture/6/closed%20ticket.png?raw=true" alt="Closing the ticket in the SIEM" width="85%" />
</div>

<br/>

<div align="center">

## 🚨 VERDICT: TRUE POSITIVE (ATTACK UNSUCCESSFUL) 🚨

</div>

### 🎯 Overall Conclusion

This was a real attack attempt from an outside IP, so it is a **True Positive**, but it appears unsuccessful because every payload ended in a redirect with no content. The source IP should be blocked and the search input handling reviewed, but the server doesn't need to be isolated.

---

<!-- ===================== DECISION RATIONALE ===================== -->
## ⚖️ Decision Rationale

| Check | Result | Impact on Verdict |
| :--- | :--- | :--- |
| 🔗 **Request URLs** | Five search requests with script, image, SVG, and obfuscated payloads | 🔴 Malicious XSS attempts |
| 🧭 **Earlier requests** | Home page, `/about-us/`, then a harmless test search | 🟠 Reconnaissance before the attack |
| 📉 **Server responses** | `302` with a response size of `0` for every payload | 🟢 Payloads not reflected |
| 🚦 **Device action** | Allowed | 🟠 The network did not block the requests |
| 🛑 **Containment and escalation** | Not needed | 🟢 No evidence of compromise |

---

<!-- ===================== IOCS ===================== -->
## 🧬 Indicators of Compromise (IoCs)

| Type | Indicator | Source | Assessment |
| :--- | :--- | :--- | :--- |
| 🌐 Source IP | `112.85.42.13` | Log Management | 🔴 Sent the XSS payloads |
| 💉 Payload | `<$script>javascript:$alert(1)<$/script>` | SIEM alert | 🔴 Malicious XSS attempt |
| 💉 Payload | `<$img src =q onerror=prompt(8)$>` | Raw log | 🔴 Malicious XSS attempt |
| 💉 Payload | `<$svg><$script ?>$alert(1)` | Raw log | 🔴 Malicious XSS attempt |
| 💉 Payload | `<$script>$for((i)in(self))eval(i)(1)<$/script>` | Raw log | 🔴 Malicious XSS attempt |
| 💉 Payload | `prompt(8)` | Raw log | 🟠 Suspicious test input |
| 🔗 Targeted URL | `https://172.16.17.17/search/?q=` | SIEM alert, raw logs | 🟠 Search parameter probed for XSS |
| 💻 Targeted host | `WebServer1002` (`172.16.17.17`) | SIEM alert | 🟡 Targeted, attack unsuccessful |

---

<!-- ===================== MITRE ===================== -->
## 🗺️ MITRE ATT&CK Mapping

| Tactic | Technique | ID | Relevance |
| :--- | :--- | :--- | :--- |
| Initial Access | Exploit Public-Facing Application | [T1190](https://attack.mitre.org/techniques/T1190/) | XSS payloads were sent to the web application's search parameter. The attempts did not succeed. |

---

<!-- ===================== RESPONSE ===================== -->
## ✅ Response Actions

- [x] Created the ticket, took ownership, and started the playbook.
- [x] Recorded the alert details and explained what triggered the rule.
- [x] Checked the source IP reputation in VirusTotal and AbuseIPDB.
- [x] Reviewed all 8 events in Log Management and identified five XSS payloads.
- [x] Interpreted the `302` status and `0` byte responses to conclude the attacks failed.
- [x] Decided that containment and escalation were not needed.
- [x] Added the analyst note and IoCs, then closed the ticket as a true positive.
- [ ] Recommended follow-up: block `112.85.42.13` and enable WAF XSS rules.
- [ ] Recommended follow-up: add output encoding, input validation, and a Content Security Policy to the application.
- [ ] Recommended follow-up: record the `Location` header in logs to see where redirects point, and scan other input points for the same flaw.

---

<!-- ===================== SKILLS ===================== -->
## 🧠 Skills Demonstrated

![SIEM Alert Triage](https://img.shields.io/badge/SIEM%20Alert%20Triage-0EA5E9?style=for-the-badge&labelColor=0D1117)
![XSS Analysis](https://img.shields.io/badge/XSS%20Analysis-1E90FF?style=for-the-badge&labelColor=0D1117)
![Payload Analysis](https://img.shields.io/badge/Payload%20Analysis-38BDF8?style=for-the-badge&labelColor=0D1117)
![IP Reputation Analysis](https://img.shields.io/badge/IP%20Reputation%20Analysis-0284C7?style=for-the-badge&labelColor=0D1117)
![Raw Log Analysis](https://img.shields.io/badge/Raw%20Log%20Analysis-334155?style=for-the-badge&labelColor=0D1117)
![HTTP Status Code Analysis](https://img.shields.io/badge/HTTP%20Status%20Code%20Analysis-475569?style=for-the-badge&labelColor=0D1117)
![Incident Documentation](https://img.shields.io/badge/Incident%20Documentation-1E293B?style=for-the-badge&labelColor=0D1117)

---

<!-- ===================== FOOTER ===================== -->
<div align="center">

### 📚 Explore More Labs

**[⬅️ Back to the LetsDefend SOC Analyst Learning Path](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath)** · **[👤 View My GitHub Profile](https://github.com/kentbalmoria7-oss)**

<sub>🛡️ <i>Detect early. Investigate thoroughly. Document everything.</i></sub>

</div>
