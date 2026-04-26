---
title: HackThisSite - Basic Level 11 - Exposed .htaccess Files
image:
  path: assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/1771870448718.jpg
  alt: Photo by freepik on Freepik
date: 2026-04-26 08:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, hackthissite, penetration-testing]     # TAG names should always be lowercase
---

This [challenge](https://www.hackthissite.org/missions/basic/11/) is a bit different and harder than the previous ones. In this scenario, Sam has developed a music website, but unfortunately, he doesn't really understands how Apache works. 

When you visit Sam's music website, you're greeted with this screen:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201161424.png)

No login form, no input fields, no buttons, just a single page whose message changes every time you refresh the page. 

At first glance, it looks like there's nothing to attack. However, as mentioned earlier in this challenge series, **one of the first things you should always do during a pentest is inspect the page source**. You’d be surprised how often developers leave useful hints behind:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201161636.png)

When you check the source code, you’ll notice an interesting message: *"our own collection"*. Could this refer to a directory on the server?, if that's the case, then performing directory and file enumeration might reveal something useful under the current challenge directory: 

```
https://www.hackthissite.org/missions/basic/11/
```

Let's find that out by scanning it using `dirb`:  

```bash
> dirb https://www.hackthissite.org/missions/basic/11/
```

That command uses a default wordlist to enumerate hidden files and directories inside `/missions/basic/11/`:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201195231.png)

As you can see, there are a few accessible endpoints. For example, these two refer to the page you're seeing: 

```
/missions/basic/11/index
/missions/basic/11/index.html
```

This other endpoint tells a different story: 

```
/missions/basic/11/index.php
```

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201195838.png)

Oh look!. A login form is revealed. You’ll eventually need a password to complete the challenge, but for now, let’s keep exploring.

The following endpoint might confirm your initial suspicion:  

```
https://www.hackthissite.org/missions/basic/11/note
```

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201200237.png)

They're telling you that  this challenge is indeed a **directory listing mission**, keep this in mind. 

Shortly after, `dirb` reports the following endpoint: 

```
https://www.hackthissite.org/missions/basic/11/e/
```

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260202183244.png)

It says that that directory **IS LISTABLE**, meaning you don’t need to brute‑force it , you can browse it directly. 

So, open that URL in your browser and you’ll see a subdirectory named `l/`. Click it:


![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201200717.png)

Inside `l/`, you’ll find `t/`. Click it:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201211751.png)

Keep doing the same until no more entries are displayed: 

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201211900.png)

Perfect!. You notice a pattern forming the word: `elton`, likely the "collection" mentioned earlier. Proceed to scan it with `dirb` to see what might be inside:

```bash
> dirb https://www.hackthissite.org/missions/basic/11/e/l/t/o/n/
```

Wow!, among the results, there’s something particularly interesting, an exposed **`.htaccess`** file:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260201220125.png)

You see,  an `.htaccess` is a configuration file used by the Apache web server. It basically allows you to control how your website behaves directory by directory without touching the main server configuration file `httpd.conf`.

With an `.htaccess` file you can: 

* Control access. 
* Rewrite URLs.
* Handle errors.
* Define file behavior and MIME types.

And because it often contains sensitive configuration details, **it should never be publicly readable**. If an external user can access it, that’s a **server misconfiguration** and a **real security issue**.

Having said that, let's inspect the contents of the `.htaccess` file: 

```
https://www.hackthissite.org/missions/basic/11/e/l/t/o/n/.htaccess
```

You get the following output:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260202200207.png)

What this does is hide from the directory listing any file starting with `DaAnswer.` (for example, `DaAnswer.txt`, `DaAnswer.php`, etc.), **but it does not prevent direct access to them**. That's why the files are invisible when browsing this directory: 

```
https://www.hackthissite.org/missions/basic/11/e/l/t/o/n/
```

Yet, they remain fully accessible if requested by name. If you could only find a way to guess the filenames correctly, you'd be able to access them. 

Fortunately for you, `dirb` comes to the rescue once again. Since you already know that that directory contains files starting with `DaAnswer.`, why not simply append some common file extensions and check whether any of them are publicly accessible. This is what I mean:   

```
https://target.com/DaAnswer.txt
https://target.com/DaAnswer.php
https://target.com/DaAnswer.old
https://target.com/DaAnswer.zip
https://target.com/DaAnswer.tar.gz
https://target.com/DaAnswer.sql
https://target.com/DaAnswer.conf
https://target.com/DaAnswer.log
https://target.com/DaAnswer.swp
```

If a file is accessible, you'll get a valid HTTP response  `200 OK`. So, let's put this into practice: 

```bash
> dirb https://www.hackthissite.org/missions/basic/11/e/l/t/o/n/ <(echo DaAnswer) -X .txt,.php,.old,.zip,.tar.gz,.sql,.conf,.log,.swp
```

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260202204820.png)

Alright!, there's a promising candidate, let's inspect its content: 

```
https://www.hackthissite.org/missions/basic/11/e/l/t/o/n/DaAnswer.txt
```

In my case, the file returns the following result:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260202210808.png)

The information you see is tricky, but the answer is straightforward. The password is `around`, at least in my case (it may be a different word for you).

To finish the challenge, simply return to the login form you discovered earlier during the initial directory enumeration:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-11-exposed-htaccess-files/20260202211101.png)

Enter the password, submit the form, and that’s it 🎉. You’ve successfully completed the challenge.

## Security Recommendations

Sensitive configuration files such as `.htaccess` should never be publicly accessible. Web servers must be configured to explicitly deny access to dotfiles, and access controls should be enforced at the server level rather than relying on directory listing behavior.

**Directory listing should be disabled** unless it is absolutely necessary, as it can reveal internal directory structure, file naming patterns, and clues that only helps in further exploitation. Even when listings are disabled, developers should assume that attackers can still guess filenames and brute-force common extensions, so sensitive files should never live inside web-accessible directories in the first place.

Finally, security testing should be part of the development lifecycle. Regularly scanning applications with tools like directory enumerators can help detect exposed resources and misconfigurations early, before attackers do.
