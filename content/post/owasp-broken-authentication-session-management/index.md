---
title: OWASP Mutillidae II - Username Enumeration Vulnerability
date: 2024-03-15
description: OWASP Mutillidae II - Broken Authentication and Session Management - Username Enumeration Vulnerability via responses
tags: 
    - OWASP Mutillidae II
categories:  
    - OWASP Mutillidae II
draft: true
---

An attacker can determine whether a given username is valid by watching for changes in the website's behavior, a technique known as username enumeration.

This vulnerability is quite simple to exploit. In essence, it's typing usernames into the Username text field and hitting submit to see how the website responds. We can ascertain the existence of that username based on the response from the website.


![](login-adrian.jpeg)