---
title: Creating a read only backup user with MySQL
slug: creating-read-only-backup-user-mysql
date: 2009-01-06 00:00:00+00:00
tags:
category:
link:
description:
type: text
---

I always have to look up which permissions a user needs to just run mysqldump successfully. This creates a new backup user, without a password, and gives them the least amount of privilege necessary.

<pre>CREATE USER 'backup'@ 'localhost';
GRANT SHOW DATABASES, SELECT, LOCK TABLES, RELOAD ON *.* to backup@localhost;
FLUSH PRIVILEGES;</pre>
            
            