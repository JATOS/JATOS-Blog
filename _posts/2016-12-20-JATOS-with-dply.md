---
layout: post
title: Easy JATOS server with Dply
---

Dply is a new service that allows you to quickly create a temporary cloud server for free - actually only the the first 2 hours are free, then it cost some money (currently it's 3$ per week). The servers are basic and it might take a bit longer than usual for a page to load. But nevertheless it's a great tool to try JATOS in the real internet and maybe run a little study. Find more information about Dply under https://dply.co/how.

And the nice thing is it works with a button press!

[![Dply](https://dply.co/b.svg)](https://dply.co/b/pXk4mgxj)

You'll need a GitHub account and your public ssh-key.

After pressing the above Dply button you'll see a form. You must specify the server name, location (choose one near yourself) and the service plan. Copy-paste your public ssh-key into the appropriate field. You might need it later on if you want to access your server via ssh.

The last field is the 'User-data Script'. It already has a shell script in there. You can change the version of JATOS in the line `wget -P /root https://github.com/JATOS/JATOS/releases/download/v2.2.4/jatos-2.2.4_linux_java.zip`. Apart from this it doesn't need any modifications (although you can add anything you please). 

Now it's time to create the server! You'll see a new page.

- heroku
- simple: no auto start (or backup)
- Ethics!


