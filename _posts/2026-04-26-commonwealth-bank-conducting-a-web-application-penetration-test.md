---
title: Commonwealth Bank - Conducting a Web Application Penetration Test
image:
  path: assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/1771202553648.jpg
date: 2026-04-26 09:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, penetration-testing]     # TAG names should always be lowercase
---

As a cybersecurity generalist at **Commonwealth Bank**, having a basic understanding of penetration testing is essential. **Penetration testing** simulates real-world attacks to identify vulnerabilities in systems and evaluate the effectiveness of security controls. By testing regularly, organizations can spot and remediate weaknesses before they can be exploited, maintaining a strong **security posture** this way.

During this task, you will complete the **"Basic"** web challenges on [HackThisSite](https://www.hackthissite.org/) (I recommend you creating an account). The 11 levels range from easy to difficult and are designed to help you identify and exploit common web application vulnerabilities.

After completing the challenges, you will create a **Penetration Testing Report** that includes:

* An executive summary.
* Vulnerability descriptions.
* Key findings and impact.
* Security recommendations for each level.

I encourage you to try the challenge yourself on [Forage.com](https://www.theforage.com/virtual-experience/x52Jy9s26xNbZkTQ7/commonwealth-bank/intro-cybersecurity-rdxl/penetration-testing), or follow along as I walk through this penetration testing report. So, let's get started. 

## Executive Summary

This report presents the results of a penetration testing exercise conducted against the “Basic” web challenge from [HackThisSite.org](http://hackthissite.org).The objective of this assessment was to identify security weaknesses, demonstrate how they could be exploited, and evaluate their potential impact on confidentiality, integrity, and availability.

The testing illustrates how common web application misconfigurations and vulnerabilities can be leveraged by an attacker to gain unauthorized access to sensitive information or functionality. If present in a production environment, such weaknesses could result in data exposure, operational disruption, financial loss, and reputational damage.

This report documents the identified vulnerabilities, demonstrates proof-of-concept exploitation, and provides remediation recommendations to improve the overall security posture of the application.

## Overview of the Web Application tested 

[HackThisSite.org](http://hackthissite.org) is an online training platform that provides a legal and controlled environment for individuals to develop cybersecurity skills through structured challenges and Capture the Flag (CTF) exercises. Established in 2003, the platform fosters a community focused on ethical hacking, programming, and security research. The “Basic” challenge series is designed to introduce participants to fundamental web security concepts and common vulnerability patterns.

## Finding Severity Ratings

The table below defines the *severity levels* and corresponding *CVSS score ranges* used throughout this vulnerability assessment to evaluate and communicate risk.

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213141242.png)

## Internal Penetration Test Findings

The following tables provide an overview of the identified vulnerabilities during the assessment, including their impact and the corresponding remediation recommendations.

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213141332.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213141351.png)

## Technical Findings

### **Finding IPT-001**: [Hardcoded Password in Source Code](https://jjeuribe.github.io/posts/hackthissite-basic-level-1-hardcoded-password-vulnerability/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213141649.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213141740.png)

#### Remediation

Never embed credentials in source code or client-side files, instead, store them securely using environment variables or secrets manager. Always perform authentication server-side, hash and salt passwords properly, and integrate secret scanning in your development workflow to catch accidental exposures. Educating developers on secure coding practices and following OWASP guidelines will further reduce these risks.

### **Finding IPT-002**: [No Password File](https://jjeuribe.github.io/posts/hackthissite-basic-level-2-no-password-file/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142012.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142030.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142034.png)

#### Remediation

Always fail securely, if a password file is missing or unreadable, the system should deny access rather than defaulting to an empty password. On top of that, store sensitive data like passwords in encrypted files or secure vaults, not in plain text. Implement input sanitization & validation and avoid assumptions about user input. Finally, regularly audit and test your authentication mechanisms to catch mistakes or oversights before they can be exploited.

### **Finding IPT-003**: [Publicly Accessible Sensitive Files](https://jjeuribe.github.io/posts/hackthissite-basic-level-3-publicly-accessible-sensitive-files/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142236.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142302.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142309.png)

#### Remediation

Storing passwords in publicly accessible files is a major security risk and should be avoided. Passwords should **never** be hardcoded or saved in web-accessible directories. Instead, they should be stored securely in a server-side database with strong **hashing** (e.g, bcrypt) and proper access controls.

Any script that processes sensitive data, should **sanitize and validate all user input** to prevent code injection and unauthorized file access. Additionally, sensitive files should be placed **outside the web root** or protected with proper server permissions to ensure they cannot be directly accessed via a URL.

Regularly reviewing file permissions and server logs can also help detect misconfigurations or attempted attacks early.

### **Finding IPT-004**: [Insecure Password Recovery via Request Manipulation](https://jjeuribe.github.io/posts/hackthissite-basic-level-4-insecure-password-recovery-via-request-manipulation/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142533.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142553.png)
![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142600.png)

The following email landed in inbox `myemail@hotmail.com`:

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213142653.png)

#### Remediation

Credentials should never be hardcoded or sent via email. Instead, applications must use secure, single‑use password reset links for account recovery.

Sensitive actions like account recovery must require proper authentication and enforce strict server‑side validation. Additionally, password-related endpoints should be protected with rate limiting and monitoring to detect abuse, and passwords must always be stored using strong hashing and salting algorithms to prevent disclosure even if the system is compromised.

### **Finding IPT-005**: [Insecure Password Recovery via Hidden Fields](https://jjeuribe.github.io/posts/hackthissite-basic-level-5-insecure-password-recovery-via-hidden-fields/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163210.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163232.png)
![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163239.png)

The following email landed in inbox `myemail@hotmail.com`:

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163258.png)

#### Remediation

Always assume the client is malicious.

Anything in HTML can be modified by the user: email addresses, hidden fields, form actions, etc., that's why all of it must be validated and authorized server-side, specially when handling sensitive actions such as password recovery.

Passwords should never be sent via email as this exposes credentials to interception and misuse, instead, secure, time-limited password reset tokens should be implemented. Sensitive business logic must reside on the server, not in client-side code, and proper authentication and recovery workflows should be leveraged to ensure that credentials remain protected even if the frontend is manipulated.

### **Finding IPT-006**: [Weak Encryption](https://jjeuribe.github.io/posts/hackthissite-basic-level-6-weak-encryption/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163414.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163436.png)

After reversing the encrypted password `4746j8:=`, the original plaintext password `4623f346` is recovered.

#### Remediation

Simple ciphers can be broken very easily once someone understands the pattern. Passwords should never be encrypted in a way that can be reversed, instead, they should be stored using strong password hashing methods like bcrypt, Argon2, or PBKDF2, which are built specifically to protect passwords. On top of that, adding randomness (a salt) to the encryption process makes each password unique and much harder for attackers to crack, even if two users choose the same password.

It’s also important to use trusted, well-known security libraries rather than writing your own encryption code.

### **Finding IPT-007**: [Command Injection](https://jjeuribe.github.io/posts/hackthissite-basic-level-7-command-injection/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163647.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163706.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163715.png)

#### Remediation

Any input received from users must be strictly sanitized and validated to ensure it contains only expected values. On top of this, developers should avoid invoking shell commands whenever possible, and instead use native language libraries or safer APIs, which greatly reduce the risk of injection attacks.

Additionally, applications should maintain a clear separation between application logic and the operating system. Backend scripts should run with the least privileges required, ensuring that even if an injection occurs, the attacker cannot read sensitive files or traverse directories freely in the system.

### **Finding IPT-008**: [SSI (Server-Side Includes) Injection](https://jjeuribe.github.io/posts/hackthissite-basic-level-8-ssi-injection/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163845.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163906.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163915.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213163921.png)

#### Remediation

Never trust data that you receive from the user, any input received from users must be strictly sanitized and validated to ensure it contains only expected values. On top of that, SSI should be disabled entirely unless it is absolutely required. Modern server-side languages and frameworks provide safer and more maintainable alternatives. If SSI must be used, servers should be configured to disable dangerous directives like `#exec`.

Additionally, sensitive files should be stored outside the web root and protected with restrictive file permissions. This significantly limits the impact of SSI injection vulnerabilities that allow command execution or directory traversal.

### **Finding IPT-009**: [Cross-Directory SSI (Server-Side Includes) Injection](https://jjeuribe.github.io/posts/hackthissite-basic-level-9-cross-directory-ssi-injection/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164107.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164127.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164137.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164145.png)

#### Remediation

Web applications should be treated as isolated units, with strict permissions preventing one application or directory from accessing another, even if they share the same server.

Web servers should enforce the principle of **least privilege** at the filesystem level, ensuring that each application is restricted to its own directory structure and cannot traverse or access unrelated paths. Proper isolation mechanisms such as separate system users, virtual hosts, containers, or chroot environments, should be implemented to ensure access is restricted to a single application context, limiting this way the potential impact of a compromise and reducing the overall attack surface.

### **Finding IPT-010**: [Cookie Manipulation](https://jjeuribe.github.io/posts/hackthissite-basic-level-10-cookie-manipulation/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164238.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164257.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164303.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164309.png)

#### Remediation

Authentication and authorization decisions should **never be trusted to the client side**, as cookies, local storage, and JavaScript values can be easily modified by an attacker. All access control logic must be enforced **server-side**.

Sensitive cookies should be marked with appropriate attributes such as HttpOnly, Secure, and SameSite to reduce the risk of client-side manipulation and cross-site attacks

Implementing proper session management, token signing (like HMAC or JWT with verification), and regular security testing can significantly reduce the risk of authentication bypass vulnerabilities.

### **Finding IPT-011**: [Exposed .htaccess Files](https://jjeuribe.github.io/posts/hackthissite-basic-level-11-exposed-htaccess-files/)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164407.png)

#### Evidence

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164424.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164432.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164439.png)

![Desktop View](/assets/2026-04-26-commonwealth-bank-conducting-a-web-application-penetration-test/20260213164445.png)

#### Remediation

Sensitive configuration files such as `.htaccess` should never be publicly accessible. Web servers must be configured to explicitly deny access to dotfiles, and access controls should be enforced at the server level rather than relying on directory listing behavior.

**Directory listing should be disabled** unless it is absolutely necessary, as it can reveal internal directory structure, file naming patterns, and clues that only helps in further exploitation. Even when listings are disabled, developers should assume that attackers can still guess filenames and brute-force common extensions, so sensitive files should never live inside web-accessible directories in the first place.

Finally, security testing should be part of the development lifecycle. Regularly scanning applications with tools like directory enumerators can help detect exposed resources and misconfigurations early, before attackers do.

## Conclusion and Call to Action

The penetration test revealed significant security flaws, particularly in the areas of *input validation*, *command execution*, and *authentication mechanisms*. The high number of *Critical* and *High* findings indicates that the application is currently **exposed to substantial risk**. It is strongly advised that the relevant teams prioritize the immediate **remediation of all Critical and High vulnerabilities**, following the provided recommendations, **to mitigate the potential for a severe security incident**.
