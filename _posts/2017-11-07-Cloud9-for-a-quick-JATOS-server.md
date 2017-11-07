---
layout: post
title: Cloud9 for a quick JATOS server
author: Kristian Lange
email: lange.kristian@gmail.com
---

A JATOS user (Hi Spiro!) told me about [Cloud9](https://c9.io/) and I thought I should mention it in a blog post.

It's basically a Linux server that runs in the cloud that you can access via the browser. On top of this it offers a whole web development environment but one can use it just as a bare Ubuntu server. Of course it's working with JATOS. And they have a [free tier](https://c9.io/pricing).

![c9.io screenshot]({{ site.baseurl }}/images/Screenshot from 2017-11-06 20-52-42.png)

It's pretty impressive: You get easy access to the file system, there is shell with root rights, and you can edit your files directly in the browser. You get a domain that ends with _.c9users.io_ but I'm pretty sure one can change to a proper domain if one wants to.

In order to get JATOS running I downloaded the latest release with wget, e.g.

```shell
wget https://github.com/JATOS/JATOS/releases/download/v3.1.10/jatos-3.1.10_linux_java.zip
```

and unzipped it. Cloud9 provides environment variables for IP address and port, `$IP` and `$PORT`, and I used them to configurate JATOS' `loader.sh`. 

![c9.io screenshot]({{ site.baseurl }}/images/Screenshot from 2017-11-06 20-52-15.png)

That's it: then JATOS starts as usual with `./loader start`. 