---
title: Epic google error message fail
date: 2011-02-24T02:22:00.001000
lastmod: 2011-02-24T02:23:18.713000
author: Dan
draft: false
tags:
  - Other
aliases:
  - /2011/02/epic-google-error-message-fail.html
---

I just spent a day without being able to send email because I enabled [two factor authentication](http://googleblog.blogspot.com/2011/02/advanced-sign-in-security-for-your.html) on my work account. That and a complicated gmail setup that apparently appeals to no one else ;)

I forward all my mail from my work, google apps account into my personal gmail account so that all my email is consolidated for reading, filtering, etc. I then use the “[Send mail as](http://mail.google.com/support/bin/answer.py?hl=en&ctx=mail&answer=22370)” functionality to email from my work account. Gmail even allows you to configure alternate SMTP information so that when I send from my work account people don't get a "Sent on behalf of..." in their Outlook.

Well, when I turned on the new two factor authentication for my work email account the smtp password stopped working and this all broke...silently.

No errors when I sent the email, no errors on the SMTP configuration page and worst: **all my emails made it successfully into the "sent” folder!**

I finally just guessed what must be happening and fixed it.

Nice.