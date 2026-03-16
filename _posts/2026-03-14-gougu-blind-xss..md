---
title: 	Gougu CMS contains Blind XSS
date: 2026-03-14 00:00:00 +0800
categories: [Vulnerability Research, Web Security]
tags: [cve, xss,  poc, exploit]     # TAG names should always be lowercase
---

## 1. Description:

Blind Cross-Site Scripting (Blind XSS) in GouguCMS v4.08.18 allows a low-privileged user to steal administrative session cookies or perform unauthorized administrative actions by injecting a malicious payload into the record endpoint. The payload is stored in the database and executed when an administrator views the activity logs or records in the backend dashboard. 

Vulnerability: Stored XSS

CVSS score: 8.8 (High)

Vector String: CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H

## 2. Ananlysis

In `\gougucms-master\app\admin\view\user\record.html` file, the project uses the $.ajax function to retrieve data from the `/admin/user/record` endpoint. On line `68`, the value of the `value.content` attribute (this is user-controlled data that has already been stored in the database) is directly appended to the HTML string without any validation or encryption.

Note that on line `72`, the HTML variable containing this raw information is passed to the `$('#logs').append(html)` function. The underlying operation of the `.append()` function in the jQuery library will parse and execute the HTML/Script tags present in the string pass; an attacker could inject malicious payloads (e.g., <img src=x onerror=alert(1)>). When an administrator accesses this record management page, malicious JavaScript code will automatically activate in their browser context, leading to the risk of session hijacking or unauthorized actions performed without administrator privileges.

![img-description](./assets/gougu_poc/poc2.png)

## 3. POC

**Step 1:** In a normal user, inject the XSS payload `<script>alert(document.cookie)</script>` to a submit form.

**Step 2:** If admin or high authenticated users view the Operational records, the XSS payload will trigger.


**Video demo:**

{% include embed/youtube.html id='9uGhUC5xT3Q' %}