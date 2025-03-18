---
layout: base 
title: Safe Computing 
search_exclude: true
permalink: /safecomputing/
---

## Introduction
Safe computing is essential to protect personal information, maintain system integrity, and ensure ethical use of technology. In today's interconnected world, understanding and implementing safe computing practices is crucial to safeguard against cyber threats, data breaches, and unethical behavior. This lesson aligns with the College Board's principles of safe computing, empowering you to navigate the digital landscape responsibly and securely.


## Key Principles
### 1. **Protect Personal Information (IOC Objective: Data Protection)**
Protecting your personal information is the first step toward safe computing. Cybercriminals often exploit weak security practices to gain unauthorized access to sensitive data. To mitigate these risks:
- Use strong, unique passwords for each account. A strong password typically includes a mix of uppercase and lowercase letters, numbers, and special characters.
- Enable two-factor authentication (2FA) where possible. This adds an extra layer of security by requiring a second form of verification, such as a code sent to your phone.
- Avoid sharing sensitive information online, especially on public platforms or unsecured websites. Be cautious about what you post on social media, as it can be used to guess security questions or passwords.

### 2. **Secure Your Devices (IOC Objective: Device Security)**
Your devices are gateways to your personal and professional life. Keeping them secure is critical:
- Keep your operating system and software up to date. Regular updates often include patches for security vulnerabilities.
- Install and maintain antivirus software to detect and remove malicious programs.
- Use a firewall to block unauthorized access to your network and devices. This acts as a barrier between your device and potential threats.

### 3. **Practice Safe Browsing (IOC Objective: Internet Safety)**
The internet is a vast resource, but it also harbors risks. To browse safely:
- Avoid clicking on suspicious links or downloading unknown files, as they may contain malware or lead to phishing sites.
- Verify website URLs before entering sensitive information. Look for typos or slight variations in domain names that could indicate a fake site.
- Use secure (HTTPS) websites for online transactions. The "S" in HTTPS stands for "secure," indicating that the connection is encrypted.

### 4. **Be Aware of Social Engineering (IOC Objective: Threat Awareness)**
Social engineering attacks exploit human psychology to gain access to sensitive information. To protect yourself:
- Recognize phishing attempts in emails or messages. Look for red flags such as urgent language, unexpected attachments, or requests for personal information.
- Avoid sharing personal information with unverified sources. Always confirm the identity of the requester before providing sensitive data.
- Be cautious of unsolicited requests for sensitive data, even if they appear to come from trusted sources.

### 5. **Backup Data Regularly (IOC Objective: Data Recovery)**
Data loss can occur due to hardware failure, cyberattacks, or accidental deletion. To ensure your data is safe:
- Use cloud storage or external drives to back up important files. This provides a secure copy of your data in case of an emergency.
- Schedule automatic backups to ensure data safety without relying on manual intervention. Regular backups minimize the risk of losing critical information.

## Ethical Computing (College Board Big Idea: Impact of Computing)
Ethical computing involves using technology responsibly and respecting the rights of others. To practice ethical computing:
- Respect intellectual property and copyright laws. Always give credit to original creators and avoid using unauthorized copies of software or media.
- Avoid using pirated software or media, as it not only violates copyright laws but also poses security risks.
- Be mindful of your digital footprint and online behavior. Remember that your actions online can have real-world consequences, both for yourself and others.


## Popcorn Hack: Password Strength Checker
Test your understanding of safe computing by creating a simple program to check the strength of a password. Use the following criteria:
- At least 8 characters long.
- Includes uppercase and lowercase letters.
- Contains numbers and special characters.

### Starter Code (Python)
```python
import re

def check_password_strength(password):
    if len(password) < 8:
        return "Weak: Password must be at least 8 characters long."
    if not re.search(r"[A-Z]", password):
        return "Weak: Include at least one uppercase letter."
    if not re.search(r"[a-z]", password):
        return "Weak: Include at least one lowercase letter."
    if not re.search(r"[0-9]", password):
        return "Weak: Include at least one number."
    if not re.search(r"[!@#$%^&*(),.?\":{}|<>]", password):
        return "Weak: Include at least one special character."
    return "Strong password!"

# Test it
password = input("Enter a password to test: ")
print(check_password_strength(password))
```


## Homework

### 1. Research a Recent Data Breach
Investigate a recent data breach and write a short report covering the following points:
- **How the breach occurred.**
- **What could have been done to prevent it.**
- **Lessons learned.**

### 2. Ethical Dilemma Discussion
Discuss the following scenario:  
*A friend shares a pirated software link with you. What would you do?*  
Consider the ethical and security implications in your response.

### 3. Backup Strategy Plan
Create a backup strategy for a hypothetical small business. Your plan should include:
- **Types of data to back up.**
- **Backup frequency.**
- **Tools or services to use.**


## Conclusion
By following these safe computing practices, which align with the College Board's principles, you can protect yourself and others from potential risks while fostering a secure and ethical digital environment. Safe computing is not just about protecting your own data; it is also about contributing to a safer and more trustworthy digital community. Stay informed, stay vigilant, and always prioritize security and ethics in your digital interactions.


<style>

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    background-color: #f8f9fa;
    color: #333;
    padding: 20px;
    display: flex;
    justify-content: center;
}

/* Main Container */
.container {
    max-width: 900px;
    width: 100%;
    background: white;
    padding: 20px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
}

/* Header Styles */
h1 {
    text-align: center;
    font-size: 2.5em;
    color: #0056b3;
    margin-bottom: 10px;
}

h2 {
    font-size: 1.8em;
    border-bottom: 3px solid #0056b3;
    padding-bottom: 5px;
    margin-top: 20px;
}

h3 {
    font-size: 1.4em;
    color: #004494;
    margin-top: 15px;
}

/* Section Containers */
.section {
    background: #eef5ff;
    padding: 15px;
    margin: 20px 0;
    border-left: 5px solid #0056b3;
    border-radius: 5px;
}

/* Code Blocks */
pre {
    background: #272822;
    color: #f8f8f2;
    padding: 15px;
    border-radius: 5px;
    overflow-x: auto;
}

/* List Styles */
ul {
    margin-left: 20px;
    list-style: square;
}

/* Callout Boxes */
.note {
    background: #e7f3fe;
    padding: 10px;
    border-left: 5px solid #2196F3;
    margin: 10px 0;
    border-radius: 5px;
}

.warning {
    background: #ffebee;
    padding: 10px;
    border-left: 5px solid #d32f2f;
    margin: 10px 0;
    border-radius: 5px;
}

/* Buttons */
.button {
    display: inline-block;
    background: #0056b3;
    color: white;
    padding: 10px 15px;
    text-decoration: none;
    border-radius: 5px;
}

.button:hover {
    background: #004494;
}

/* Footer */
footer {
    text-align: center;
    margin-top: 30px;
    font-size: 0.9em;
    color: #666;
}

</style>