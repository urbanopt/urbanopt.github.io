---
layout: default
title: Cloud Installation
parent: Installation
nav_order: 3
---

# Running URBANopt in the Cloud 

This page provide documentation to support commercial software cloud-based integration of URBANopt. This process is divided into multiple steps illustrated in Figure 1 and described below. 

![cloud integration](../../doc_files/cloud_integration.jpg)

**I– Inputs Translator**

First, a translator will need to be developed to streamline the creation of URBANopt inputs.  The translators should include methods that map project details defined in the commercial software platform to URBANopt inputs as the following:

***- Create a valid initial GeoJSON file***
 
The first input of URBANopt is the Feature File which represents a selected set of features for analysis and is in a geoJSON format.  GeoJSON is a widely used format for encoding geographic data. For example, the geoJSON supports representing points, LineString, and Polygon. Geometric objects with additional properties are Feature objects and sets of features are contained by FeatureCollection objects. The GeoJSON schema is represented [here](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/geojson_schema.json) and should be followed when creating a GeoJSON file.

***- Add project-level features to the GeoJSON***

After creating the geoJSON file structure project-level features should be added to the geoJSON file. Project-level features are properties that are applied to all the features in the geoJSON file.  These properties include top-level inputs such as the simulation timesteps_oh per_hour, the weather file name, and the climate zone. The Shema for these properties is defined in the [site_properties.json](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/site_properties.json).

***- Add building feature to the GeoJSON***

Then to correctly add building features in the GeoJSON file the [building properties schema](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/building_properties.json). 
This schema describes the required and optional inputs that should be included in the GeoJSON file and describe the format/type that these inputs should follow.  The buildings representation with this GeoJSON file should be formatted and translated to follow the building properties schemas.

***- Extracting and storing the Weather File***

Methods should be created to extract a valid weather file and store it in the directory named “weather”.  The weather file that should be extracted should be in EnergyPlus Weather File (EPW) format.  EPW files can be found at (https://energyplus.net/weather). The translator should have the functionality to connect to the website and download identified EPW files or parse the EPW file from a weather database if it exists. The weather file should then be saved in the “weather” directory at the top of an URBANopt project directory to be utilized in the simulation.

**II- Cloud/OS server setup**

If you want to run URBANopt in the cloud, such as on AWS, you have a couple of options. 

- The easiest path is to deploy a standalone VM instance, such an AWS EC2 instance, using a supported platform image and use an [installer](http://urbanopt-cli-installers.s3-website-us-west-2.amazonaws.com/) to install URBANopt. For example, on AWS EC2, you can deploy an Ubuntu 18.04 AMI and then download the [.deb package](http://urbanopt-cli-installers.s3-website-us-west-2.amazonaws.com/) and install it. e.g. `sudo apt install ./URBANoptCLI-0.8.3.6a224192d0-Linux.deb`

   <br>    
    
- Another option is to use the OpenStudio Analysis Framework (OSAF), also called [OpenStudio-server](https://github.com/NREL/OpenStudio-server). This is a web-based application that runs OpenStudio Analysis workflows as well as URBANopt workflows.  It provides an REST API to process workflows and run the simulations in the cloud. OpenStudio-server can be deployed using a [Kubernetes Helm chart](https://github.com/NREL/OpenStudio-server-helm) and works with most cloud providers' Kubernetes stacks such as AWS Elastic Kubernetes Service (EKS). Once deployed, your application client can make REST HTTP requests to the OpenStudio-server without having to run it locally. This is especially useful for applications that are running on serverless computing enviroments (e.g. AWS Kestrel ) that do not have a way to install URBANopt locally.   For details on how to install OpenStudio-server please consult the [README.md](https://github.com/NREL/OpenStudio-server-helm/#readme) to understand how to setup the cloud deployment. For information on how to run a URBANopt workflow using OpenStudio-server, please consult (link to doc). 