---
layout: post
title: Get an JATOS server easily with Dply
author: Kristian Lange
email: lange.kristian@gmail.com
---

I was always looking for an easy way to install JATOS on a server (to make it accessible from the Internet); but I never found an easy enough solution. Most server or cloud providers target bigger companies and have a quite complex setup - nothing one wants to do if it's just about trying a little tool like JATOS. But lately I stumbled over Dply. It's still in beta but it might fill this empty spot in the cloud vendor landscape. 

Dply is a new service that allows you to quickly create a temporary cloud server for free - actually only the the first 2 hours are free, they charge afterwards (currently it's 3$ per week). The servers are basic and it might take a bit longer than usual for a page to load. But nevertheless it's a great tool to try JATOS in the real internet and maybe run a little study. Find more information about Dply under [https://dply.co/how](https://dply.co/how).

And the nice thing is it works with a button press!

[![Dply](https://dply.co/b.svg)](https://dply.co/b/pXk4mgxj)

You'll need a GitHub account and your public ssh-key.

### Server Setup

After pressing the Dply button above you'll see a form. You have to specify the server name (anything you want), location (choose one near yourself) and the service plan (get the free one). Keep the OS (Ubuntu). Copy-paste your public ssh-key into the appropriate field. You might need it later on if you want to access your server via ssh.

![Dply form screenshot]({{ site.baseurl }}/images/Screenshot from 2016-12-20 17-55-33.png)

The last field is the 'User-data Script'. It already has a shell script in there. You might have to change the version of JATOS in the line 

`wget -P /root https://github.com/JATOS/JATOS/releases/download/v2.2.4/jatos-2.2.4_linux_java.zip`

Apart from this it doesn't need any modifications (although you can add anything you please to the script). 

Now it's time to create the server. Press the big green button! (The one that says "Create server"). You'll see a new page with a box that says something like 'Ubuntu 16.04 - 138.68.139.250' (you'll have a different IP address). Copy this IP into a new browser tab and it should, if everything went well, show the JATOS login. Actually it might take some time depending on the location and time: I tried London and Frankfurt. London took over 2 min to come up, while Frankfurt was there after about 10 seconds.

![Dply form screenshot]({{ site.baseurl }}/images/Screenshot from 2016-12-20 14-18-31.png)

Thats it. You have a server running JATOS.

If you want you can get a secure shell to your server with root@your-ip-address. If you want to check the start-up logs, they are in `/var/logs/cloud-init-output.log`.

### Some notes in the end

* This is a simple server setup. It doesn't start JATOS automatically after a restart of the server, nor does it have any backup.
* Also be aware that a cloud server might [not be the best place for the sensitve data of your online studies](http://www.jatos.org/Data-Privacy-and-Ethics.html).
