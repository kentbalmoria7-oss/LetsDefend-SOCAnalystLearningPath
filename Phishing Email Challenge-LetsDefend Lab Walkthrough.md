# Phishing Email Challenge - LetsDefend Lab Walkthrough

## Overview

Phishing email analysis involves safely isolating and inspecting suspicious emails, headers, and links to identify malicious activity without risking endpoint compromise.

![image alt](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ef90cd22941fc47db9e830a4e1627073e9af46a9/Screenshot%202026-10-02%20050644.png)
---

## Lab Objectives

Extract Core IoCs: Inspect raw email headers, Return-Path data, and embedded hyperlinks to isolate key technical artifacts.

Evaluate Threat Intelligence: Run extracted domains, URLs, and file hashes through tools like VirusTotal, URLhaus, and WHOIS to assess reputation.

Inspect Payloads Safely: Preview linked landing pages using sandboxed or visual tools (e.g., URL2PNG, Hybrid Analysis) inside an isolated virtual machine.

Determine Attack Verdict: Synthesize findings to confirm whether the message is a legitimate notification or a malicious phishing campaign.

## Scenario

You received an email sent to an address exposed in a data breach, claiming to concern an urgent PayPal matter. The goal is to investigate and analyze the message to determine whether it is malicious.

![image alt](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/dc15b4f01887be2c74eaa80ea217a76bd61cf215/1/paypal.png)

> **Note:** As a cybersecurity best practice, any suspicious emails, links, or file attachments should be inspected and detonated within an isolated sandbox environment to safely determine if they are malicious.






# 🔍 Email Header Analysis — Step 1
---
## 📌 STEP 1: CHECK THE RETURN PATH

* **Action:** View the raw source code of the email header.
* **Method:** Use the search feature (`CTRL + F`) to search for `Return-Path`.

> **Note:** The return path (also known as the envelope sender, `MAIL FROM` address, or reverse path) is a hidden email header that specifies where mail servers should send delivery failure notifications or bounced emails.
---
### 🎯 Finding / Answer
`bounce@rjttznyzjjzydnillquh.designclub.uk.com`
