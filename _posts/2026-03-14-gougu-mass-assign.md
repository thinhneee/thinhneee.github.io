---
title: 	Gougu CMS contains Mass Assignment in /home/login/reg
date: 2026-03-14 00:00:00 +0800
categories: [Vulnerability Research, Web Security]
tags: [cve, business logic errors,  poc, exploit]     # TAG names should always be lowercase
---

## 1. Description:

Mass Assignment in GouguCMS v4.08.18 allows an unauthenticated attacker to elevate privileges to VIP users by injecting the level parameter during the user registration process at the /home/login/reg endpoint.

Vulnerability: Mass Assignment

CVSS score: 8.2 (High)

Vector String: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N

## 2. Ananlysis

In the `gougucms-master\app\home\controller\Login.php` file, line `115`, the `reg_submit` function get parameters from HTTP request, and the parameters inserted to the database without any filtering. Even `reg_submit` using `validate` function, but not enough, it just check the type or empty. If the input contains the values same with columns in database, it will be insert to it. It leads to attacker can create a user with VIP and can access to assets where VIP users can.

![img-description](./assets/gougu_poc/poc1.png)

## 3. POC

Step 1: Create a new user

Step 2: Using BS, intercept the request and add `level` parameter.

**Video demo:**

{% include embed/youtube.html id='JlQHuTOA0fc' %}