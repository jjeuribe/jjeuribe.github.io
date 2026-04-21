---
title: DATACOM - Conducting a Cybersecurity Risk Assessment
image:
  path: /assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/1_JZT4mXcRX2hxK0OjuwOwKQ.webp
date: 2026-04-21 08:00:00 -0600
categories: [Cybersecurity, Blue Team]
tags: [cybersecurity, soc-analyst, risk-management, osint]     # TAG names should always be lowercase
---

During this job simulation you'll take on the role of a cybersecurity consultant at Datacom. You'll investigate a cyberattack and conduct a comprehensive risk assessment for a client. Give it a try and explore the lab here [Data Cybersecurity]( https://www.theforage.com/simulations/datacom/cybersecurity-zm6d)

## Scenario

One of the Datacom's client has fallen victim to a cyberattack carried out by a well-known *APT (Advanced Persistent Threat)* group known as **APT34**. The attack is believed to be sponsored by a foreign government, and has left the client's network compromised, with valuable customer data and intellectual property stolen. 

Your mission is to conduct initial research on this APT group. You’ll need to get familiar with **APT34’s Tactics, Techniques, and Procedures (TTPs)**, as well as the common vulnerabilities they exploit to break into networks. Then, as a cybersecurity consultant, you’ll provide recommendations for improving the client's cybersecurity posture.

## APT breach - Analyzing the impact on information security

To conduct your research, you can use the following **Open-Source Intelligence (OSINT)** tools to gather information on the **APT34** group: 

 * [Mandiant Security Blog](https://www.mandiant.com/resources/blog)
 * [CrowdStrike](https://www.crowdstrike.com)
 * [Recorded Future](https://www.recordedfuture.com)
 * [CyberScoop](https://www.cyberscoop.com)
 * [Dark Reading](https://www.darkreading.com)
 * [The CyberWire](https://thecyberwire.com)
 * [SecureWorks](https://www.secureworks.com)
 * [ThreatConnect](https://www.threatconnect.com)
 * [Kaspersky Lab](https://www.kaspersky.com/)
 * [Symantec Threat Intelligence](https://www.symantec.com/threat-intelligence)

You can also use the **[MITRE ATT&CK Framework](https://attack.mitre.org)** to identify and categorize cyber threats, and then develop a solid defense strategy to protect your client’s network and systems from future attacks.

### Inside the Threat - Profiling APT34

**APT34**'s profile was built around the following key questions that reveal who **APT34** is, what motivates them, and how they operate. 

* What is their history?
* Which nation/state are they associated with?
* Do they target specific industries?
* What are their motives?
* What **TTPs (Tactics, Techniques, and Procedures)** do they use to execute their attacks?

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251022195649.png)

### Building a Stronger Shield - Recommended Security Actions

Defending against **APT34 (OilRig)** takes a mix of solid basics and a few focused security controls. This is what our client can do: 

* **Strengthen Email Security**: Use email filters and sandboxing to detect phishing attachments or links. Turn on anti-phishing protection in Microsoft 365 or Google Workspace. Consider implementing DMARC, DKIM, and SPF records to reduce domain spoofing, and last but no least, train staff regularly so they can spot fake emails and avoid clicking on suspicious links. 
* **Protect User Accounts**: Enforce Multi-factor Authentication (MFA) for all users, especially admins, executives, and remote access accounts. Require strong, unique passwords, and consider the use of a password manager. Disable unused accounts and remove default or shared credentials. Monitor for suspicious logins, specially from new devices, unusual locations or IPs.
* **Limit PowerShell and Admin Tools**: Use PowerShell logging and use *constrained language mode* to prevent malicious scripts. Restrict who can run admin tools like **PsExec**, **WMI**, or **RDP**, only authorized admins should have that kind of access.  Always apply the **principle of least privilege**, give users only what they need to do their job. 
* **Endpoint Protection**: Use antivirus software to detect and block malware. Apply device control policies to restrict the use of external devices such as USBs or portable drives to prevent data exfiltration. Implement regular patch management and vulnerability scanning to ensure OS and third-party software are up to date reducing exposure to exploits. Consider using disk encryption to protect sensitive data at rest. 
* **Secure Web Servers and Cloud Accounts**: Keep all web apps and plugins updated, remove unused ones. Regularly check for web shells or unexpected files on servers. For cloud accounts (like Office 365 or Azure), audit access logs and use conditional access policies. 
* **Network Segmentation & Security**: Segment the network into smaller, isolated networks, this way if one segment gets compromised, the infection won't spread. Use Firewalls to control traffic between networks or zones, blocking unauthorized access. 
* **Incident detection & Response**: Use IDS/IPS to monitor network traffic and spot suspicious activity. Use EDR/XDR to protect individual devices by detecting unusual behavior or malware on them. Set up SIEM alerts for events like suspicious authentication, command-line activity, or potential data exfiltration. Regularly practice incident response drills so everyone knows what to do if an incident occurs. 
* **Security Awareness**: Always keep staff trained, **this APT loves social engineering**. Run phishing simulations.

## Cybersecurity Risk Assessment

Your initial research on the **APT34** group was a crucial step because it helped you understand their methods, motives, and targets. 

Your next task is to conduct a comprehensive **risk assessment** for the client. As a cybersecurity consultant, you understand that conducting a risk assessment is an essential component of any effective cybersecurity strategy. This involves identifying, evaluating, and prioritizing potential threats and vulnerabilities to determine the level of risk the organization faces, and develop a plan to mitigate those risks in order to protect the **confidentiality**, **integrity**, and **availability** of their information and systems.

### Define the Context - Know What You are Protecting

As the saying goes: *"You can't protect what you don't know"*. That's why the first step is to **identify your client's key assets**.  

The following assets have been identified as requiring protection against unauthorized access, loss, or exposure.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251018191123.png)

* **Confidential Customer Data**: It includes all PII such as names, addresses, contact details, account credentials, and payment information collected from customers. 
* **Proprietary Business Information**: This refers to internal operational and strategic data. For example, business plans, pricing strategies, contracts, internal reports, and supplier information. 
* **Financial Information**: It includes financial records and transactions. For example, budgets, payroll data, banking details, tax records, accounting systems. 
* **Intellectual Property**: It includes patents, trademarks, designs, research data, product blueprints, source code, and other creative or technical assets developed by the company. 
* **Physical Infrastructure & Equipment**: It covers all hardware, servers, networking devices, on-premises and cloud systems.

### Define the Risk Matrix - Visualize and Prioritize Risks

When making decisions based on risk priorities, organizations should evaluate the **likelihood** and **consequence** of every risk, as well as their tolerance. The level of cybersecurity required will depend on how much risk the organization is willing to accept. 
 
 One effective way to prioritize risks is by using the **risk matrix**, which helps identify priority as the intersection of **likelihood** of occurrence and its **consequence**. In this process, **likelihood** refers to the chance of a risk occurring, while **consequence** describes how severe the impact would be. The **risk rating** will represent the overall level of risk, calculated by combining both likelihood and consequence.

With this in mind, let's first define the levels of **likelihood**:

* **Rare**: Very unlikely to happen. It would need unusual circumstances. 
* **Unlikely**: It could happen, but not expected under normal conditions. 
* **Possible**: It might happen sometimes. It has occurred before in similar cases. 
* **Likely**: Expected to happen regularly or in many situations. 
* **Almost Certain**: Very likely to happen. It occurs frequently. 

Then, the level of **consequences**: 

* **Severe**: Critical Impact; Core operations fail, survival at risk. Major losses or legal issues
* **Major**: Major disruption; serious operational/delays or financial impact. 
* **Moderate**: Noticeable disruption; reduced performance but business still operational. 
* **Minor**: Small Impact; It causes short delays but easy to fix. 
* **Insignificant**: Minimal Impact; routine fix. 

Finally, let's put this information into our **risk matrix** as such:

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251009160820.png)

### Define Three Risk Scenarios - Highlight Key Threats

During a risk assessment, it's important to identify the potential risks scenarios that could affect your client's key assets. These scenarios help visualize where threats may come from and how they might impact your client's operations. 

With that in mind, we consider the following three potential scenarios:

#### Cyberattack

A cyberattack is a deliberate attempt carried out by and individual or group to breach the security of a computer system, network or device in order to steal, damage, or disrupt information and services. 

* **Sources or Cause of Risk**: Cyber attacks can come from Cybercriminals seeking financial gain, Nation-state actors conducting espionage or sabotage, Hacktivists targeting organizations for political or social reasons, or even Insiders (employees or contractors) who misuse access, intentionally or by mistake.
* **Consequences of Risk**: Data loss or theft, System downtime, Financial loss, Reputation damage, Operational disruption, Legal or regulatory penalties.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251018192102.png)

#### Employee Negligence 

It happens when staff accidentally or carelessly compromise an organization's security or operations, putting the company assets at risk. This can include actions like mishandling sensitive data, ignoring security policies, sharing login credentials or failing for phishing scams. 

* **Sources or Cause of Risk**: Negligence often comes from lack of staff training, human error, fatigue or distraction. 
* **Consequences of Risk**: Data exposure, Financial loss, Reputation damage, Regulatory issues.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251018192403.png)

#### Natural Disaster 

It's an unexpected event caused by natural forces that can disrupt operations or damage company's physical assets, and pose a threat to employee safety.

 * **Sources or Cause of Risk**: Natural events beyond human control like earthquakes, floods, fires, storms, or pandemics. 
 * **Consequence of Risk**: Physical damage, loss of life, disruption of supply chains, financial losses, and Data loss.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251018192457.png)

### Assess Risk Rating for Each Risk Scenario - Inherent Risk Levels

For each risk scenario you must analyze the situation *without the fence and padlock in place*, that is, no security measures or protections in place. This will help you determine the **inherent risk**, or the level of threat the organization would face if no security controls or defenses existed. 

This will serve as the **baseline** to measure how effective current or future security measures are at reducing risks.

#### Cyberattack

With no security measures in place, the **likelihood** of a cyber attack is considered as *Likely*, because organizations are a common target for threat actors. The **consequences** of a breach for the company are considered *Major* because of the data loss or theft, system downtime, reputational damage, financial loss, operational disruption, legal or regulatory penalties. Therefore, the inherent risk for the company in case of a cyberattack is considered *VERY HIGH*.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251019142815.png)

#### Employee Negligence

With no preventive measures in place, the **likelihood** of human error is thought to be *Likely* because employees who are not aware of the security protocols, don't take security seriously or don't understand the consequences of their actions. The **consequences** are considered *Major*, as these incidents can affect operations or expose sensitive data. Therefore, the inherent risk for the company in case of employee negligence is rated as *VERY HIGH*.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251020142238.png)

#### Natural Disaster 

Although the **likelihood** of a natural disaster is *Rare*, the potential **consequences** for the company are *Severe*: damage to infrastructure, loss of life, data loss, or prolonged downtime. For this reason the inherent risk in case of a natural disaster is rated as *HIGH*.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251019143102.png)

### Assess Risk Rating for Each Risk Scenario with Existing Measures - Current Risk Levels

Now, you need to calculate the risk for each scenario, but this time with the *fence and padlock* in place, that is, considering the existing security measures the client already has. With the current security protections active, both the **likelihood** and **impact** of each event should be reduced compared to when no defenses were in place.

#### Cyberattack

Current *Security measures* implemented by the client: 

* **Firewall and IDS/IPS - Good**: The organization has properly configured Firewalls and Intrusion Detection Systems that monitor network traffic and block suspicious activity. However, log reviews and rule updates are not done frequently, leaving some blind spots that could be improved.
* **Antivirus Protection - Excellent**: All company devices are quipped with updated antivirus software that automatically scans, detects, and removes malware.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251020125717.png)

While the current security measures effectively lowered the **likelihood** of a successful cyberattack, the potential **impact** remains as *Major*. The overall risk is still *VERY HIGH*, though the organization's defensive strength and detection capabilities have clearly improved.

#### Employee Negligence

Current *Security measures* implemented by the client: 

* **Security Awareness & Training Program - Moderate**:  Employees receive periodic training sessions, but the material is too general and doesn't include phishing simulations or hands-on exercises. This limits how much employees can apply what they learn in real situations. 
* **Access Control - Good**: The company follows a least-privilege approach, granting users only the access they need. Access review is done quarterly, however, some legacy systems still grant broader access than necessary, creating occasional gaps in control. 
* **Data Loss Prevention (DLP) and Monitoring Tools – Weak**: While the organization uses basic monitoring tools, it lacks a full DLP solution. This makes it difficult to detect or block sensitive data being sent outside the organization through email or removable drives.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251020135801.png)

With the current security measures in place, the organization is doing a good job managing risks related to employee negligence. Both the **likelihood** and **impact** of incidents have decreased, reducing the overall risk from *VERY HIGH* to *MEDIUM*. This means the implemented controls are effectively reducing mistakes and limiting their potential damage.

#### Natural Disaster

Current *Security measures* implemented by the client: 

* **Building Reinforcement - Good**: The organization’s facilities are built to withstand moderate natural events like heavy rain or mild earthquakes. However, they may not hold up against stronger disasters, such as major earthquakes or severe flooding. Some areas could benefit from structural improvements or additional safety inspections. 
* **Backup Power Generators - Excellent**: The company has reliable backup generators that automatically activate during power outages, keeping critical systems and operations running without interruption. Regular maintenance ensures these generators remain in top condition.
* **Data Backup & Disaster Recovery Plan - Moderate**: The organization performs regular data backups, but not all backups are stored off-site or in the cloud. Recovery procedures exist but haven’t been tested recently, which could delay restoration if a disaster strikes.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251020165802.png)

With the current security measures in place, the organizations shows its strength and resilience against natural disasters. The **impact** of such events decreased from *Severe* to *Moderate*, reducing the overall risk from *HIGH* to *LOW*. This means that the implemented controls,  like building reinforcements, backup generators, and disaster recovery plans, are effectively minimizing potential damage and ensuring business continuity.

### Assess Risk Rating for Each Risk Scenario with Additional Measures - Target Risk Levels

Now it’s time to take your client’s cybersecurity to the next level. You'll add additional security measures on top of the client's existing ones to further reduce the risk. After that, you must calculate the **target risk levels** for each scenario with these additional protections.

#### Cyberattack 

Additional security recommendations: 

* **Review logs and Update rules - Excellent**: Regularly review security logs and update IDS/IPS and firewall rules to improve detection accuracy. 
* **Strengthen Email Security - Good**: Use email filters and sandboxing to detect phishing attachments or links. Turn on anti-phishing protection in Microsoft 365 or Google Workspace. Consider implementing DMARC, DKIM, and SPF records to reduce domain spoofing, and last but no least, train staff regularly so they can spot fake emails and avoid clicking on suspicious links. 
* **Protect User Accounts - Excellent**: Enforce Multi-factor Authentication (MFA) for all users, especially admins, executives, and remote access accounts. Require strong, unique passwords, and consider the use of a password manager. Disable unused accounts and remove default or shared credentials. Monitor for suspicious logins, specially from new devices, unusual locations or IPs.
* **Network Segmentation & Security - Excellent**: Segment the network into smaller, isolated networks, this way if one segment gets compromised, the infection won't spread. Use Firewalls to control traffic between networks or zones, blocking unauthorized access. 
* **Implement a SIEM or XDR solution - Excellent**: Centralize alerts, detect anomalies in real time, and respond quickly to potential incidents. 
* **Penetration Testing - Excellent**: Conduct periodic penetration tests to simulate real-world attacks and identify vulnerabilities in systems, applications, or network configurations.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251021161731.png)

With these additional security measures in place, the organization has made a real improvement in its security posture. The **likelihood** of a successful cyberattack has dropped from *Possible* to *Unlikely*, mainly because phishing, weak passwords, user accounts, and network paths are much better protected through stronger email defenses, MFA, and network segmentation. 

The **impact** has been reduced too, from *Major* to *Moderate* thanks to a better visibility, faster detections, and improved containment. Even if an attacker manages to get in, the damage would be more limited and recovery much quicker. As a result, the **overall risk rating** has decreased from *VERY HIGH* to *MEDIUM*, showing clear progress in the organization’s security posture.

#### Employee Negligence

Additional security recommendations:

* **Practical Training & Phishing Simulations - Good**: Reinforce employee awareness with regular, practical exercises that simulate real phishing attempts and social engineering attacks. 
* **Implement DLP (Data Loss Prevention) solution - Good**: Deploy a DLP system to monitor and block the transfer of sensitive information via email, removable devices, or cloud storage.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251021161823.png)

With the additional security measures in place, the organization has significantly reduced the risk of employee negligence. The **likelihood** of an incident has gone from *Possible* to *Unlikely*, thanks to practical training, phishing simulations, and better awareness among staff.

At the same time, the **impact** has dropped from *moderate* to *minor*, because of the implementation of a DLP solution. Overall, these enhancements have lowered the **overall risk level** from *MEDIUM* to *LOW*, showing now that the organization is much better protected against human errors.

#### Natural Disaster

Additional security recommendations:

* **Implement Off-site or Cloud backups - Good**: Store critical data off-site or in cloud environments to ensure business continuity even if local systems are damaged or inaccessible.  
* **Test Backup scenarios regularly - Excellent**: Conduct routine restoration tests to confirm that backups can be quickly recovered, and that recovery procedures actually work under real conditions. 
* **Review and Update the Disaster Recovery Plan - Excellent**: Keep the organization’s disaster recovery and business continuity plans up to date, reflecting changes in systems, infrastructure, and personnel.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251021161920.png)

With the additional measures in place, the organization’s resilience against natural disasters has improved significantly. While the **likelihood** remains *rare*, because natural disasters are inherently uncommon, the **impact** has dropped from *Moderate* to *Minor*. This improvement comes from implementing off-site or cloud backups, testing backup scenarios, and strengthening disaster recovery procedures, which ensure that critical data and operations can be quickly restored if an event occurs.

As a result, the **overall risk rating** has decreased from *LOW* to *VERY LOW*, showing that these additional protections effectively minimize potential damage and enhance business continuity.

### Cybersecurity Risk Assessment Report

 Finally, you will provide a risk assessment report to the client summarizing your findings and recommendations for mitigating risks and improving the institution's security posture.

![Desktop View](/assets/2026-04-21-datacom-conducting-a-cybersecurity-risk-assessment/20251023192721.png)

And that's it!. Through this challenge, you learned how to evaluate existing controls, propose new ones, and understand how each security measure contributes to reducing overall risk. 

Remember, cybersecurity isn’t about eliminating risk completely, but managing it and minimizing it wisely. Every action you take, no matter how small, helps protect what truly matters: **your people, your data, and your reputation**. 

Keep sharpening your skills, stay curious, and continue improving, because every step forward in cybersecurity truly counts 🚀.
