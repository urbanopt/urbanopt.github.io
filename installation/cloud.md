---
layout: default
title: Cloud Installation
parent: Installation
nav_order: 3
---

# Running URBANopt in the Cloud 

If you want to run URBANopt in the cloud, such as on AWS, you have a couple of options. 

- The easiest path is to deploy a standalone VM instance, such an AWS EC2 instance, using a supported platform image and use an [installer](http://urbanopt-cli-installers.s3-website-us-west-2.amazonaws.com/) to install URBANopt. For example, on AWS EC2, you can deploy an Ubuntu 18.04 AMI and then download the [.deb package](http://urbanopt-cli-installers.s3-website-us-west-2.amazonaws.com/) and install it. e.g. `sudo apt install ./URBANoptCLI-0.8.3.6a224192d0-Linux.deb`

   <br>    
    
- Another option is to use the OpenStudio Analysis Framework (OSAF), also called [OpenStudio-server](https://github.com/NREL/OpenStudio-server). This is a web-based application that runs OpenStudio Analysis workflows as well as URBANopt workflows.  It provides an REST API to process workflows and run the simulations in the cloud. OpenStudio-server can be deployed using a [Kubernetes Helm chart](https://github.com/NREL/OpenStudio-server-helm) and works with most cloud providers' Kubernetes stacks such as AWS Elastic Kubernetes Service (EKS). Once deployed, your application client can make REST HTTP requests to the OpenStudio-server without having to run it locally. This is especially useful for applications that are running on serverless computing enviroments (e.g. AWS Kestrel ) that do not have a way to install URBANopt locally.   For details on how to install OpenStudio-server please consult the [README.md](https://github.com/NREL/OpenStudio-server-helm/#readme) to understand how to setup the cloud deployment. For information on how to run a URBANopt workflow using OpenStudio-server, please consult (link to doc). 