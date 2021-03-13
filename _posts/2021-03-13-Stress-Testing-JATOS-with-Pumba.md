---
layout: post
title: Stress testing JATOS with Pumba
author: Kristian Lange
email: lange.kristian@gmail.com
---

> Pumba is a chaos testing command line tool for Docker containers. Pumba disturbs your containers by crashing containerized application, emulating network failures and stress-testing container resources (cpu, memory, fs, io, and others).

https://github.com/alexei-led/pumba

## Pumba installation on Linux

```shell
$ curl -SL https://github.com/alexei-led/pumba/releases/download/0.7.7/pumba_linux_amd64 -O
$ sudo mv pumba_linux_amd64 /usr/bin/pumba
$ sudo chmod +x /usr/bin/pumba
$ pumba --version
```

## Run some tests

Since JATOS' docker image does not have _tc_ Linux tool for network emulation installed, I have to add `--tc-image gaiadocker/iproute2` to every execution.

* Delay every request by 10s and add a random delay variation (jitter) of 3s for 10 minutes:

  ```shell
  pumba netem --tc-image gaiadocker/iproute2 --duration 600s delay --time 10000 --jitter 3000 --distribution normal my-docker-container-name
  ```
  
* Drop 50% of packages for 10 minutes: 

  ```shell
  pumba netem --tc-image gaiadocker/iproute2 --duration 600s loss --percent 50 my-docker-container-name
  ```

* Cotrrupt 10% of packages 10 minutes: 

  ```shell
  pumba netem --tc-image gaiadocker/iproute2 --duration 600s corrupt --percent 10 my-docker-container-name
  ```
