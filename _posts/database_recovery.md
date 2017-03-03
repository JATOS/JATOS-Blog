---
layout: post
title: A failed attempt to recover a H2 database of JATOS
---

I was just asked to help with a broken JATOS instance. The logs were full of error messages like

```
IOException occurred reading text org.hibernate.HibernateException: IOException occurred reading text
```

```
java.io.IOException: Block not found in id [1, -70, -16, 5, 7] [1.4.192/50] at org.h2.mvstore.StreamStore$Stream.read(StreamStore.java:514) ~[com.h2database.h2-1.4.192.jar:1.4.192]
```

```
java.lang.IllegalStateException: Reading from cache:nio:C:/jatos-2.2.4_win_java/database/jatos.mv.db failed; file length 868352 read length 98304 at 15183555 [1.4.192/1]
```
