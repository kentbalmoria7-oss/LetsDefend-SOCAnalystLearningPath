# Phishing Email Challenge - LetsDefend Lab Walkthrough

## Overview

In this lab, we are analyzing an email to determine whether it is malicious and to gain a better understanding of the challenges involved in email threat analysis.

![image alt](https://github.com/kentbalmoria7-oss/LetsDefend-SOCAnalystLearningPath/blob/ef90cd22941fc47db9e830a4e1627073e9af46a9/Screenshot%202026-10-02%20050644.png)
---

## Lab Objectives

- Extract and inspect email headers (SPF, DKIM, DMARC, Sender IP).
- Analyze the email body for suspicious URLs, domain typosquatting, or social engineering indicators.
- Examine attachments for potential malware payloads.
- Map findings to the Cyber Kill Chain and propose remediation steps.

## Scenario

You received an email sent to an address exposed in a data breach, claiming to concern an urgent PayPal matter. The goal is to investigate and analyze the message to determine whether it is malicious.
