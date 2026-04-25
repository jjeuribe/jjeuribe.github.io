---
title: HackThisSite - Basic Level 6 - Weak Encryption
image:
  path: /assets/2026-04-25-hackthissite-basic-level-6-weak-encryption/1770238220705.jpg
  alt: Photo by Markus Winkler on Unsplash
date: 2026-04-25 08:00:00 -0600
categories: [Cybersecurity, Red Team]
tags: [cybersecurity, hackthissite, penetration-testing]     # TAG names should always be lowercase
---

In this [challenge](https://www.hackthissite.org/missions/basic/6/), Sam has encrypted his password using a well-known and publicly available encryption method. Your task is to figure out which encryption system Sam used.

To get started, enter any arbitrary text, such as the word `world`, and hit the **encrypt** button:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-6-weak-encryption/20260128122746.png)

The output is:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-6-weak-encryption/20260128123325.png)

As you can see, the word `wptoh` is the encrypted form (ciphertext) of the plaintext `world`:

```
  world       →     wptoh
plaintext        ciphertext
```

This encryption method is a classical substitution cipher in which each character is shifted forward by an increasing offset based on its position in the string. For example, the first character is shifted by **0**, the second by **1**, the third by **2**, and so on. However, in this scenario, the shifts are applied using the [**ASCII values**](https://www.asciitable.com/) of the characters.

This can be illustrated as follows:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-6-weak-encryption/20260204145519.png)

And that's essentially how the plaintext `world` becomes `wptoh`. 

You can apply the same principle **in reverse** to decrypt Sam’s encrypted password`4746j8:=`, that is, by subtracting the positional shift from each character’s ASCII value. This is what I mean:

![Desktop View](/assets/2026-04-25-hackthissite-basic-level-6-weak-encryption/20260204145551.png)

After reversing the transformation, you recover Sam's original plaintext password: `4623f346`

## Security Recommendations

**Simple ciphers can be broken very easily once someone understands the pattern**. Passwords should never be encrypted in a way that can be reversed, instead, they should be stored using strong password hashing methods like **bcrypt**, **Argon2**, or **PBKDF2**, which are built specifically to protect passwords. On top of that, adding **randomness (a salt)** to the encryption process makes each password unique and much harder for attackers to crack, even if two users choose the same password.

It’s also important to use trusted, well-known security libraries rather than writing your own encryption code.
