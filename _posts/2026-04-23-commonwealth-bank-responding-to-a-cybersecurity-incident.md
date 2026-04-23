---
title: Commonwealth Bank - Responding to a Cybersecurity Incident
image:
  path: /assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/0_bAw5dC4mpEb7nIwM.webp
  alt: Photo by Peter Conrad on Unsplash
date: 2026-04-23 07:00:00 -0600
categories: [Cybersecurity, Blue Team]
tags: [cybersecurity, soc-analyst, incident-response]     # TAG names should always be lowercase
---

As a cybersecurity analyst, one of your day-to-day responsibilities is responding to **security incidents**. When such a situation occurs, you're expected to notify the appropriate teams, gather information from affected users, and assist your organization in recovering from the impact. 

A structured and effective way to handle incidents is by following the **NIST Incident Response Lifecycle**. In this exercise, you’ll apply this security framework step by step to analyze and respond to an incident scenario.

As always, you can access and try out the lab yourself [here](https://www.theforage.com/virtual-experience/x52Jy9s26xNbZkTQ7/commonwealth-bank/intro-cybersecurity-rdxl/incident-response). Alright, let’s jump right into it!

## Scenario 

The IT Service Department has flagged a critical issue that requires immediate attention from your cybersecurity team. The incident includes many staff complaints and a notion that the bank's network may have been compromised. 

Your task as a **Cybersecurity Analyst** is to investigate the nature of the incident and identify the type of cyberattack that has occurred. You must identify the necessary steps to **contain, eradicate, and recover** from the incident effectively. Finally, you’ll outline the **post-incident activities** and recommend **preventive measures** to help the organization avoid similar attacks in the future.

Use the following questions to conduct your investigation: 

1. What kind of attack has happened and why do you think so?
2. As a cyber security analyst, what are the next steps to take? List all that apply.
3. How would you contain, resolve and recover from this incident? List all answers that apply.
4. What activities should be performed post-incident?

Refer to the following online sources for additional context and guidance: 

1. [Top 10 Common types of Cybersecurity Attacks (infocyte.com)](https://www.datto.com/blog/cybersecurity-101-intro-to-the-top-10-common-types-of-cybersecurity-attacks)
2. [11 Types of Phishing + Real-Life Examples (pandasecurity.com)](https://www.pandasecurity.com/en/mediacenter/tips/types-of-phishing/)
3. [8 Critical steps to take after a ransomware attack: Ransomware response guide for businesses](https://blog.emsisoft.com/en/36921/8-critical-steps-to-take-after-a-ransomware-attack-ransomware-response-guide-for-businesses/)
4. [Battling Ransomware: How to Respond to a Ransomware Incident](https://www.forbes.com/sites/forbestechcouncil/2018/12/27/battling-ransomware-how-to-respond-to-a-ransomware-incident/?sh=b464b4864dc6)
5. [Frequently Asked Questions - Ransomware - Information Security Office](https://security.berkeley.edu/faq/ransomware/)
6. [What to do before and after a cybersecurity breach?](https://www.american.edu/kogod/research/cybergov/upload/what-to-do.pdf)
7. [Computer Security Incident Handling Guide](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

## Incident Overview

At **10:30 am.**, the IT Service Desk at the bank received a report from a bank employee who had received an email appearing to come from the HR department. The email requested all staff to update their timesheets through the company’s support portal so managers could approve them on time before the next payday. 

The bank employee clicked the link in the email, which opened a webpage that looked just like the company's real portal. But after the employee entered their username and password, a strange error page like the one below appeared:

![Desktop View](/assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/20251031113843.png)

By **2:00 pm.**, the IT Service Desk received eight additional reports of emails similar to the one reported earlier in that day. Investigation revealed that 62 employees in the Risk Department had received the same fake email over the past two days.

These emails contained a link to a fake HR portal website designed to steal usernames and passwords and to trick users into downloading a malicious program onto their computers.

At **3:50 pm.**, the IT Service Desk started receiving calls and emails from multiple employees saying that they can’t open shared files. They report that whenever they try to open a Word document they’ve always been able to access before, they now get an error message instead.

## NIST Incident Response in Action

The **NIST Incident Response Lifecycle** is a NIST Special Publication (800-61 Revision 2) titled _Computer Security Incident Handling Guide_, and it provides a structured way to effectively respond to incidents. 

It consist of four main phases: 

1. **Preparation**: Get ready before an incident occurs. Think of it as setting up your defenses, tools, and playbooks before the game starts. 
2. **Detection & Analysis**: Identify and confirm incidents. Here, your goal is to detect suspicious activity and determine if it's an actual security incident. 
3. **Containment, Eradication, and Recovery**: Once the security incident is confirmed, your next step is to limit the damage, remove the threat, and restore normal operations.
4. **Post-Incident Activity**: After everything is resolved, it's a good idea to review the incident, that is, understanding what happened, documenting lessons learned, and improving processes to prevent future response. 

One thing to note is that the **NIST Incident Response Lifecycle** isn't a strictly linear process. It's cyclical, this means phases can overlap as new information and discoveries emerge. In this case, you're going to focus only on phases 2, 3, and 4, since that's where the actual incident response and follow-up take place.

### Phase 2 - Detection & Analysis

![Desktop View](/assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/20251109165513.png)

Analysis Summary: 

* The incident is confirmed as a coordinated **spear-phishing attack combined with ransomware delivery**. 
* The malicious email link leads victims to a fraudulent web portal designed for credential theft. 
* The same email deceives employees into downloading and installing ransomware. 
* The inability to access shared files indicates encryption by ransomware software.
* The attack represents a **high likelihood of both data compromise and operational disruption**, requiring immediate containment and mitigation actions.

### Phase 3 - Containment, Eradication & Recovery

#### Containment

![Desktop View](/assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/20251109165600.png)

#### Eradication

![Desktop View](/assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/20251109165653.png)

#### Recovery

![Desktop View](/assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/20251109165746.png)

### Phase 4 - Post-Incident Activity

#### Incident Summary

![Desktop View](/assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/20251109165907.png)

#### Incident Timeline

* **10:30 a.m.** - First phishing report.
* **2:00 p.m.** - Eight additional similar reports, campaign confirmed.
* **3:50 p.m.** - File encryption begins, ransomware confirmed.
* **4:15 p.m.** - Systems isolated and password resets initiated.
* **6:30 p.m.** - Malicious domains blocked and affected machines quarantined.
* **Next day** - Restoration from clean backups and patch deployment completed.

#### Lessons Learned

![Desktop View](/assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/20251109165939.png)

#### Follow-Up Actions

![Desktop View](/assets/2026-04-23-commonwealth-bank-responding-to-a-cybersecurity-incident/20251109170013.png)

## Wrapping Up

Cyber incidents can happen to anyone, but following the **NIST Incident Response Lifecycle** keeps you on track from start to finish. By detecting threats early, containing the damage, and carefully recovering systems, you can respond efficiently and confidently. 

Remember, it’s not just about fixing the problem, it’s about **learning from each security incident, improving your defenses, and being ready for the next challenge**. I hope you've found this walkthrough helpful and interesting! See you next time 👋!
