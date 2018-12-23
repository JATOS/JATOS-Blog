---
layout: post
title: Simple setup of a JATOS server with DigitalOcean and Traefik
author: Kristian Lange
email: lange.kristian@gmail.com
---

JATOS greatly simplifies setting up a server to run studies online **but** you still need that server to begin with! If getting your _own_ server (e.g. from your IT department) is not an option, you can set up JATOS on a cloud vendor (AWS, Azure, Google Cloud are the most common ones). 

For a newbie, and someone without any knowledge about server admin, setting up a server in the cloud with any of those services is not as straightforward as it sounds. So I put some effort to find the easiest solution.  

DigitalOcean seems to be, for now, the best option. So I wrote a step-by-step tutorial on how to set up [JATOS on DigitalOcean](http://www.jatos.org/JATOS-on-DigitalOcean.html). It's aimed at people who know a little about computers but don't feel comfortable with server administration or the 'cloud'. If everything works as it should, you won't even need to use a terminal. It uses the cloud provider DigitalOcean that in itself is easy to use.

The [first part](http://www.jatos.org/JATOS-on-DigitalOcean.html#setup-a-simple-jatos-server-on-digitalocean) describes how to set up a simple JATOS server.

Then, in the [second part](http://www.jatos.org/JATOS-on-DigitalOcean.html#add-https-with-traefik-and-use-your-own-domain-name), it continues with adding HTTPS and using your own domain name through [Traefik](https://traefik.io/).

I like to reiterate: always keep in mind that storing your data in the cloud means that you lose some control. A company has the data you stored there and you should really be careful and check what your ethics allow you to do with the data you have. 
