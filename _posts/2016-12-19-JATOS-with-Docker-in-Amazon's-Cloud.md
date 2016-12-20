---
layout: 
title: JATOS with Docker in Amazon's Cloud
---

*This text was originally in JATOS' wiki but I moved it here mostly because I think it doesn't really fit in the wiki. It's a too special use case and it's using a very narrow tool set (Docker Cloud with AWS). Nevertheless it's still correct and valid.*

This is a little tutorial on how to install JATOS on Amazon's Cloud by using Docker Cloud as a deployment service. But this is just an example. There are many other Docker hosting services like like Google Cloud, Digital Ocean, Microsoft Azure, etc. - a nice comparison is in this [blog post of Cris Ward](https://blog.codeship.com/the-shortlist-of-docker-hosting/).

I used Docker Cloud as a helping service to bring my Docker container with JATOS that is stored on Docker Hub to Amazon's EC2 cloud. :)

### Caveats
First you should be aware of the major difference between JATOS installed on your own server and an installation in the cloud. On your own server you have complete control - in the cloud, not so much. This is especially important when it comes to privacy of your stored data. Many studies' ethics demand that your data are stored in a secure place. But on a cloud instance your data are stored on a private company's servers, potentially in a different country. The chances are high your ethics doesn't allow this. On the other side if it is allowed then deploying JATOS in the cloud is a very easy way to bring your study to the Internet.

A short notice about the cost. Of course no cloud solution is for free, but some offer at least a free trial period which might be already enough to get a study on its way. For pricing information, check the cloud service's web page.

### So you still want to go on?

JATOS has a Docker container. It's stored at [Docker Hub](https://hub.docker.com/r/jatos/jatos/).

All this Docker Cloud, Docker Hub, Docker Registry, Docker Engine, Docker Container, Docker xxx can be quite confusing (at least it was to me). So if you have any questions or problems with your cloud, please [contact us](https://github.com/JATOS/JATOS/wiki/Contact-us).

### Registering

First you need to register.

1. Register at [AWS (Amazon Web Services)](https://aws.amazon.com/) (they'll want your credit card)
1. Register at [Docker Cloud](https://cloud.docker.com/)

### Setup on Docker Cloud

Now we follow the first steps of [Docker Cloud's tour](https://cloud.docker.com/onboarding/).

1. As cloud provider choose Amazon. There you have to provide credentials from AWS to Docker Cloud. This is some kind of authentication. Follow this [tutorial](https://docs.docker.com/docker-cloud/infrastructure/link-aws/) (it's easier than it seems).
1. Go back to [Docker Cloud's tour](https://cloud.docker.com/onboarding/) and deploy a node on AWS. You can go with the defaults, but I would suggest to use a micro instance instead of the nano one to have more memory (and it's still included in [Amazon's free tier](http://aws.amazon.com/free/)).
1. Go back to [Docker Cloud's tour](https://cloud.docker.com/onboarding/) and create a service:
  1. Select **Public Repositories** and search for '_jatos/jatos_'. Then select the _jatos/jatos_ one.
  1. In the **Service Configuration** leave everything as it is except the ports. There add a port and use 9000 as Container Port, check Published, and use 80 in Node port. Here is a screen shot of the ports:
![Docker Cloud Screenshot](https://github.com/JATOS/JATOS/wiki/images/screenshot_docker_cloud_port.png)
  1. In **Environment variables** add the environment variable `JAVA_OPTS` with the value `-Dpidfile.path=/dev/null` like in the screenshot:
![Docker Cloud Screenshot](https://github.com/JATOS/JATOS/wiki/images/docker_cloud_env.png)
  1. Then click **Create and deploy**. You get to the Service dashboard and in the Status column you should see 'Starting' and then after a couple of seconds 'Running'. In the back Docker Cloud did all the magic: it deployed the JATOS Docker container to Amazon's EC2.
  1. Check out your freshly created JATOS web page in the cloud: Go to your Amazon AWS account / EC2 / Instances. Click on the instance with xxx-many-numbers-xxx.node.dockerapp.io as the name (there is only one if you are using AWS for the first time). Then have a look at the bottom and open the Description tab. There is information about your cloud instance including your public IP and public DNS. Both lead you to your JATOS web page.
![AWS Screenshot](https://github.com/JATOS/JATOS/wiki/images/AWS_instances_IP_DNS.png)

That's all! You now have a JATOS installation in the cloud.
(The last two points in Docker Cloud's tour ('Create a stack' and 'Repositories') aren't necessary for JATOS since we have only one container and don't intend to use their repository.)

### Some notes

* It is also possible to add **HTTPS** encryption to your AWS instance. This can be achieved with a [load balancer in AWS and you'd need a certificate](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/elb-create-https-ssl-load-balancer.html). But I don't want to explain this here in detail and if you have questions or need advice [contact us](https://github.com/JATOS/JATOS/wiki/Contact-us) directly.
* Be aware that each time you **redeploy** the JATOS docker container you practically install it anew and therefore **delete the study assets and database** of the old one. If you want to keep your old study assets and result data you have to make a backup of them. Have a look at [Updating a JATOS server installation](https://github.com/JATOS/JATOS/wiki/Updating-a-JATOS-server-installation) for instructions.
