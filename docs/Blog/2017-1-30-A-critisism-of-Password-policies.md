---
layout: post
title: A criticism of password policies
category: blue
---

In my short time in the industry I have noticed some disturbing things about passwords and the common policies that govern them. It seems that no matter the requirements that are imposed on any given corporation there is a lot of overlap no matter if you are trying comply with PCI, HIPPA, NIST and others. This poses a problem because you will see a lot of common policies across a ton of different industries
and the most common policies I see are something along these lines:

```
Password Policy

8 characters Minimum

1 Uppercase

1 Number

1 Special Character

Passwords must be changed once every 90 days
```

While that may seem okay at first glance; over time you begin to see a lot of this

`Password1!`

Yes, that’s right! It is the year 2017 and I am still seeing this password used in almost every environment I have seen. While xkcd may have gotten something right with their comic on passwords and entropy, they got their math wrong.

![xkcd comic](../img/30Jan17/password_strength.png "xkcd Password Strength Comic")

So, let’s break this down and get the math right. The English language contains 26 lowercase and 26 uppercase characters so right there in a single 1 character string you have 52 possible characters. Now if we consider including the numbers 0-9 that’s an additional 10 characters with for a total of 62 and then we add the special characters which adds an additional 32 possible characters. Wait wait wait I hear you saying 32 characters? Bro do you even count? Yes, I do and you are not considering characters like the brackets, greater or less than and quotes as well as tilde and backtick. So, if you are not using Alt-codes, the typical QWERTY keyboard can give you a total of 94 characters to work with. So, to do this calculation at home you take the number of characters of your password and you rise it to the power of how many characters in that set you have.

So, for the password in the comic _Tr0ub4dor&3_ you have upper, lower, numeric and special characters across 11 characters so the math looks like 11^94 or 7.77 * 10^97. Now to determine if it is strong or not, we run it against an unrealistically strong cracker. So, for this example let’s say I am going to try to break this at 1 trillion password attempts per second. It would still take 2.466 * 10^78 years at the most to break out using a brute force method.

Now let’s take a look at their proposed solution of _correct horse battery staple_. In this case, you have 28 characters including spaces between the words using just the lowercase set so we have 27 characters to work with so 28^27 is 1.18 * 10^39. Which is a significantly smaller space than the first password it would still take 37.5 * 10^18 years at the most to break out fully.

Let's recap so far, we have seen 2 examples of really good passwords that should last you until long after your death. However, they are both weak to one specific type of attack: The Man-in-the-middle (MiTM) attack. In this attack the attacker sits in-between the user and the authentication service as to intercept the authentication tokens as they pass from the user to the service. At this point all the entropy in the world won’t save you because the attacker already has your password. So how do we combat this? My recommendation is to implement two factor authentication (2FA) on every service you use. This is a type of authentication that uses a 6-8 digit numeric code that changes once every 30-60 seconds. This is sent to you out of band through something like an SMS message or RSA token. If that code is wrong, even if the username and password is right, the service will not authenticate you. In doing so we can limit an attackers window of opportunity to crack a password to 30-60 seconds.

But how can we make it stronger?

Well the use of passphrase while unpopular is always a feasible option. _So, for example this very sentence could be your next password._ But that rises another question if we begin using this type of password wont attackers just start cracking based on words in the English language with spaces in between? Yes, I predict they would but here we run into the entropy question again as the full 20 volume Oxford English Dictionary contains 171,476 individual entries for words so for the example above of an 11-word sentence it would take a cracker trying a trillion word combinations per second 2.255 * 10^178554 years to completely crack out. Which is longer than the projected heat death of the known universe and we didn’t even calculate for punctuation, commas, spaces or even uppercase characters which would take even longer.

But how do we make this a better password policy using this criterion. Here are my proposed changes

```
Password Policy

10-word minimum

1 Uppercase letter

1 Punctuation Mark

2FA Enabled

Passwords must be changed once every year or whenever a user feels they might have been compromised
```
