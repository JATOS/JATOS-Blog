---
layout: post
title: How much memory does JATOS need?
author: Kristian Lange
email: lange.kristian@gmail.com
---

JATOS 3 stores its user session in a local cache and initially I gave this cache without much thinking 256 MB of memory. 256 MB is ridiculously high for a cache that just stores a couple of user sessions. But hey I have 16 GB on my laptop - who cares. Actually people do care. Especially those who want to run JATOS on systems with limited resources like an AWS EC2 micro instance with just 1 GB. If you just have 1 GB then 256 MB is a lot.

The easy solution for this issue was to reduce the cache size to just 25 MB (still more than enough). But it made me thinking: how much memory does JATOS actually need?

**For the more impatient reader: It's usually under 400 MB on a system with low memory and under 700 MB without memory restrictions.**

But now to the boring details. Actually it's not easy to determine the memory usage of a Java application. Java runs on the JVM (Java Virtual Machine) and what the JVM is doing memory-wise can be pretty complex. First there is something called garbage collection. Every now and the JVM decides to do some house keeping and clean the memory of old objects: garbage collection. If one looks at a graph of the heap memory (that's where the JVM keeps the objects) it has this typical zigzag curve: old object's memory piles up until the next garbage collection kicks in and let the heap memory drop again.

![JVM gc]({{ site.baseurl }}/images/JVM-garbage-collection.png)

How much heap memory an app is allowed to use can be configured. JATOS is build with the Play Framework and Play allows the [run parameter -J-Xms and -J-Xmx](https://www.playframework.com/documentation/2.5.x/ProductionConfiguration#JVM-configuration) to specify minumum and maximum heap memory.

Next to the heap memory is the Metaspace. This is where the JVM stores all the Java classes and since those usually don't change much over the time of an application execution the Metaspace is mostly constant - with JATOS it's always around 75 MB.

Then the JVM needs memory for itself to do what the JVM does. Optimization. Stuff. I'm not an expert - ask someone else. But then there is this interesting fact: The JVM can actually use less memory by doing less of its optimization, garbage collection, and stuff. There is a trade-off between high performance and low memory consumption - one can't have both. 

So, now to the numbers. I did some tests with JATOS. I let it run the [Study, Group, and Batch Session Example Study](http://www.jatos.org/Example-Studies.html#study-group-and-batch-session-example-study). It's a group study and it uses the group session and the batch session (both use a WebSockets each). Altogether it uses many of JATOS features and therefore should have a comparatively high memory consumption. I let the study run with 10, 20, or 30 workers at the same time and [determined the memory usage with /proc/JATOS-PID/stats (looking for VmRSS and VmHWM)](http://elinux.org/Runtime_Memory_Measurement).

|            | no mem restrictions |
|-----------:|:-------------------:|
| 10 workers | 485 MB              |
| 20 workers | 605 MB              |
| 30 workers | 637 MB              |

Next I told the JVM to use maximal 64 MB heap memory and started JATOS with
`./loader.sh start -J-Xmx64m`.

|            | 64 MB heap memory |
|-----------:|:-----------------:|
| 10 workers | 271 MB            |
| 20 workers | 340 MB            |
| 30 workers | 358 MB            |

_I did my tests on my laptop with Ubuntu 16.04 and the JVM OpenJDK 64-Bit Server, version 1.8.0_131._

### Conclusions

* Clearly visible the more workers JATOS has to serve in parallel the more memory usage JATOS will have.
* JATOS will use more memory (for optimization and hopefully performance) if you give it more memory.
* JATOS can run 30 workers while using under 400 MB if one restricts the JVM's memory either by enforcing it with -J-Xmx or by using a system with limited memory.
* If given a system with 1 GB (like AWS EC2 micro) JATOS should run happily on it.

