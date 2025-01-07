---
title: OWASP Mutillidae II - Sensitive Information Disclosure
date: 2024-03-10
description: OWASP Mutillidae II - Sensitive Information Disclosure
tags: 
    - OWASP Mutillidae II
categories:  
    - OWASP Mutillidae II
toc: false
---

What is sensitive information disclosure vulnerability? When a website inadvertently provides users with sensitive information, this is known as sensitive information disclosure. We'll be demonstrating a hardcoded credentials vulnerability utilizing the Client-side Comments page.

### First, open the Client-side Comments page by navigating to the following location in the menu: HTML/Javascript comments

![Sensitive Information Disclosure](1.png)

### Right click, then choose View Page Source

![Sensitive Information Disclosure](2.png)

### The hard-coded login credentials are located in a comment at the bottom of the source code-containing page

![Sensitive Information Disclosure](3.png)
