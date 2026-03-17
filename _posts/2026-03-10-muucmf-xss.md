---
title: 	Muucmf T6 CMS contains Reflected XSS in /admin/Member/index.html?search=
date: 2026-03-10 00:00:00 +0800
categories: [Vulnerability Research, Web Security]
tags: [cve, xss,  poc, exploit]     # TAG names should always be lowercase
---

## 1. Description:

Reflected Cross-Site Scripting (XSS) in MuuCmf T6 v1.9.5.20260309 allows a remote attacker to execute arbitrary JavaScript code in the context of the user's browser session via the search parameter to the /admin/Member/index.html endpoint.

Vulnerability: Reflected XSS

CVSS score: 8.8 (High)

Vector String: CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H

## 2. Ananlysis

In the `muucmf-master\app\admin\view\member\index.html` file, line `49` the value of input tag is using input from user without filter or santitize and the data is reflected in website leak to XSS vulnerability.

![img-description](./assets/muucmf_poc/XSS_POC1.png)

## 3. POC

Inject XSS payload to endpoint `http://127.0.0.1/admin/Member/index.html?search=` and the payload will execute.

![img-description](./assets/muucmf_poc/XSS_POC2.png)

**Video demo:**

{% include embed/youtube.html id='sSoJBgII7ls' %}
