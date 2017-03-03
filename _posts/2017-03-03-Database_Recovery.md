---
layout: post
title: A Failed Attempt to Recover JATOS' H2 Database
author: Kristian Lange
email: lange.kristian@gmail.com
---

Recently I was asked to help with a broken JATOS instance. The logs were full of error messages like

```
IOException occurred reading text org.hibernate.HibernateException: IOException occurred reading text
```

```
java.io.IOException: Block not found in id [1, -70, -16, 5, 7] [1.4.192/50] at org.h2.mvstore.StreamStore$Stream.read(StreamStore.java:514) ~[com.h2database.h2-1.4.192.jar:1.4.192]
```

```
java.lang.IllegalStateException: Reading from cache:nio:C:/jatos-2.2.4_win_java/database/jatos.mv.db failed; file length 868352 read length 98304 at 15183555 [1.4.192/1]
```

Doesn't look nice, does it? Somehow the H2 database got corrupted.

Usually the H2 database JATOS is using as a default is very reliable and so far I've never had an incident where data were damaged.

Unfortunately in this case the experimenter running this JATOS had recently gathered some data and hadn't backed them up. So the only hope to rescue those data was to try to recover from the corrupted database file. 

Apparently H2 comes shipped with a [recovery tool](http://www.h2database.com/javadoc/org/h2/tools/Recover.html). I've never attempted a recovery so far.

First one has to download the H2 .jar file with the same version as the one your JATOS is using. In my case it is JATOS 2.2.4 and this version uses H2 version 1.4.192. You can look up H2 version in JATOS' [build.sbt](https://github.com/JATOS/JATOS/blob/master/build.sbt) (and its history if you don't use the current JATOS version). H2's webpage provides only the latest version but you can find older ones in other places, e.g. on [www.versioneye.com](https://www.versioneye.com). 

The database files one can find in the folder where JATOS is installed and there in the folder _database_. There are two files: _jatos.mv.db_ and _jatos.trace.db_ and both we need. I recommend not to attempt the recovery on the original database files. Copy them to some other place together with H2's .jar file.

Now it was time to start the recovery. Since it is a .jar we need Java. Then it's just:
```
java -cp h2*.jar org.h2.tools.Recover
```
It will find the database files if they are in the same folder.

Unfortunately the attempt was a failure.

```
java -cp h2-1.4.192.jar org.h2.tools.Recover
Exception in thread "main" java.lang.IllegalStateException: Reading from cache:nio:/h2_recover/jatos.mv.db failed; file length 868352 read length 128 at 21963215 [1.4.192/1]
	...
Caused by: java.io.EOFException
	at org.h2.mvstore.DataUtils.readFully(DataUtils.java:431)
	... 15 more
```

The _jatos.mv.db_ file itself seems to be corrupted. Nothing I can do there (as far as I know).

Some advice in the end. The cause for the database corruption here was likely a file access from the OS while JATOS was running. **So, please**
* **do not copy the JATOS database files while JATOS is still running.**
* **If you want to run JATOS on two instances, you have to stop running JATOS and copy the JATOS folder including its db file before.**
* **do backup. To backup the database copy the /database folder in the JATOS folder to a safe place.**
