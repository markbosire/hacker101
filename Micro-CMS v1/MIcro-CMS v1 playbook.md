# Micro-CMS v1 CTF Playbook

## Introduction

This playbook provides guidance for navigating the Micro-CMS v1 Capture The Flag (CTF) challenge. Micro-CMS v1 is a vulnerable web application where participants aim to exploit security vulnerabilities to capture flags.

## Pre-requisites

Ensure the following tools are available:

- Browser Developer Tools
- Burp Suite or similar proxy tool
- Terminal or command prompt

## Challenge Overview

Micro-CMS v1 contains common web application vulnerabilities such as Cross-Site Scripting (XSS), SQL Injection, and potentially more. The objective is to identify and exploit these vulnerabilities to capture flags.

## Initial Reconnaissance

1. **URL Discovery:**
   - Identify the URL of the Micro-CMS v1 instance.

2. **Manual Browsing:**
   - Manually explore the application to understand its functionality.
   - Look for any input fields, forms, or areas where user input is accepted.

## Exploitation Techniques

### Cross-Site Scripting (XSS)

1. **Blacklist Filters:**
   - Test for XSS vectors in user-input fields.
   - Check for blacklist filter evasion techniques.

2. **Event Handlers:**
   - Exploit XSS using event handlers in various HTML tags.
   - Utilize different HTML versions (4 and 5) and their event handlers.

3. **Control Character Bypasses:**
   - Test XSS vectors with control characters based on browser types.

### SQL Injection

1. **Input Fields:**
   - Identify input fields susceptible to SQL Injection.



### Pseudo-protocols

1. **JavaScript Pseudo-protocols:**
   - Exploit vulnerabilities using javascript: and data: pseudo-protocols.



## Post-Exploitation

1. **Flag Retrieval:**
   - Once vulnerabilities are exploited, retrieve flags placed within the application.

2. **Documentation:**
   - Document the steps taken, including payloads and successful exploitation methods.

## Defensive Techniques

1. **Filter Evasion Prevention:**
   - Implement secure coding practices to prevent XSS and SQL Injection.
   - Use context-sensitive escaping and proper encoding.

2. **Security Headers:**
   - Implement HTTP security headers to enhance application security.

## Conclusion

Micro-CMS v1 CTF provides an opportunity to practice web application security skills. Thoroughly document your steps, and stay updated on secure coding practices to defend against similar vulnerabilities in real-world scenarios.

**Happy hacking!** Stay curious and secure! 🚀✨
