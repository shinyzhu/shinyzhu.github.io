---
title: "Migrating Your Outlook Authentication Today"
description: "Which just replaced the auth and keep all your libs work"
date: 2024-09-17T12:29:00+08:00
lastmod: 2024-09-17
tags: [oauth, devtools]
categories: Blog
featured_image: "/posts/2024/migrating-your-outlook-authentication-today/migrating-outlook-auth.jpg"
images: ["/posts/2024/migrating-your-outlook-authentication-today/og-migrating-outlook-auth.jpg"]
---

## Migrating Your Outlook Authentication Today

### Motivation and Objective

> **Transition Timeline**
>
> September 16th, 2024 
>
> - Basic Authentication no longer available to access any Outlook account

According to the [Modern Authentication Methods now needed to continue syncing Outlook Email in non-Microsoft email apps](https://support.microsoft.com/en-us/office/modern-authentication-methods-now-needed-to-continue-syncing-outlook-email-in-non-microsoft-email-apps-c5d65390-9676-4763-b41f-d7986499a90d) support doc. You need to **migrate the basic authentication to modern methods** like OAuth.

Here is what I've done and some difficulties encountered.

Please also check out the code from [shinyzhu/outlook-bot-tool](https://github.com/shinyzhu/outlook-bot-tool) repo on GitHub.

### The Implementation

I use the [device_code_flow](https://learn.microsoft.com/en-us/entra/identity-platform/v2-oauth2-device-code) to authenticate and get `access_token`, then use the `imaplib` to fetch emails and `smtplib` to send emails out. 

> I DON'T use the Microsoft Graph API here. You don't have to use it either.

And it also provides local file [token cache](https://learn.microsoft.com/en-us/python/api/msal/msal.token_cache.serializabletokencache?view=msal-py-latest). So you don't need to login every time.

Example output:

Once you logged and authorized the app. The user info, access token and refresh token will be saved to a local JSON file.

```sh
(ohoutlook) outlook-bot-tool[main*] % python mailbot.py
INFO:root:No accounts found in cache
INFO:root:🔑 Device code flow initiated
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code IQA3Y48WV to authenticate.

----------------------------------------------------
Open: https://microsoft.com/devicelogin
Code: IQA3Y48WV
----------------------------------------------------

INFO:root:🔐 Token acquired from remote
====================================================================================================
Subject: New app(s) connected to your Microsoft account
From: Microsoft account team
        <account-security-noreply@accountprotection.microsoft.com>
Date: Wed, 21 Aug 2024 21:21:03 -0700
====================================================================================================
Subject: New app(s) connected to your Microsoft account
From: Microsoft account team
        <account-security-noreply@accountprotection.microsoft.com>
Date: Wed, 21 Aug 2024 21:26:32 -0700
```

Then for later calling, you don't need to login again because the token is fetched from the cache file:

```sh
(ohoutlook) outlook-bot-tool[main*] % python mailbot.py
INFO:root:🔐 Token acquired from cache
Email sent
```

You can reuse the code in my repo. And just replace the authentication code. It should work fine.

### The Problems

#### Can't create a Microsoft Entra application

![create-entra-app](/posts/2024/migrating-your-outlook-authentication-today/create-entra-app.png)

Follow this to **Register an application**:

The ability to create applications outside of a directory has been deprecated. You may get a new directory by [joining the M365 Developer Program](https://aka.ms/joinM365DeveloperProgram) or [signing up for Azure](https://aka.ms/signUpForAzure).

### Resources

- Office/Exchange Doc: [Authenticate an IMAP, POP or SMTP connection using OAuth](https://learn.microsoft.com/en-us/exchange/client-developer/legacy-protocols/how-to-authenticate-an-imap-pop-smtp-application-by-using-oauth)
- https://jwt.ms/