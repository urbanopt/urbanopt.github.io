---
layout: default
title: Cloud Installation
parent: Installation
nav_order: 6
---

# Commercial Software Cloud-Based Integration of URBANopt

This page provides documentation to support commercial software cloud-based integration of URBANopt™. The process is divided into multiple steps that include preparing URBANopt inputs and projects, setting up the cloud server, and finally retrieving outputs and reformatting them if necessary. Please note that this is only general guidance on scoping potential approaches and that each specific implementation will vary based on the use case, software architecture, etc. Guidance is NOT meant to be exhaustive, and the required approaches/steps/order of implementation can also vary substantially. Also, guidance may change in the future as URBANopt and supporting platform capabilities evolve. Please contact the URBANopt team for specific guidance/questions when scoping potential integration, as well as any recommendations for improving this documentation.

**URBANopt has a defined JSON Schema and supporting data files required to run an URBANopt analysis.  If you have your own software and would like to integrate URBANopt, development efforts to Extract, Transform, and Load (ETL) URBANopt inputs and outputs should be taken into consideration and planned. Please see the section on preparing URBANopt Project Inputs via an Inputs Translator for guidance on this topic as well as Reading URBANopt Outputs via an Outputs Translator.**



## Setting up a Cloud/OS Server

The following are a couple of key approaches to consider if you want to run URBANopt in the cloud.

### Option 1 - Running on a server in the cloud

![cloud integration option 1](../../doc_files/cloud_integration_option_1.jpg)

*Figure 1 - Option 1: Setting everything on a server in the cloud*

In many cases, the most straight-forward path is to deploy a standalone VM instance, such as an AWS EC2 instance, using a supported platform image and an installer to install URBANopt. For example, on AWS EC2, you can deploy an Ubuntu 18.04 AMI and then download the .deb package and install it. For example:

```terminal
sudo apt install ./URBANoptCLI-0.8.3.6a224192d0-Linux.deb
```

The installer provides access to the URBANopt cli (uo cli) and that provides commands for creating, running, and processing results. For more information about the URBANopt cli commands and workflows refer to the [Getting Started page](https://docs.urbanopt.net/getting_started/getting_started.html)


### Option 2 - Connecting to an OpenStudio-Server instance via REST API

![cloud integration option 2](../../doc_files/cloud_integration_option_2.jpg)

*Figure 2 - Option 2: Connecting to OpenStudio server through REST API*

Another option is to use the OpenStudio Analysis Framework (OSAF), also called [OpenStudio-server](https://github.com/NREL/OpenStudio-server).  This approach is useful for applications that are running on serverless computing environments (e.g. AWS Kestrel) that do not have a way to install URBANopt locally. 

[OpenStudio-server](https://github.com/NREL/OpenStudio-server) is a web-based application that runs OpenStudio Analysis workflows as well as URBANopt workflows. It provides a REST API to process workflows and run simulations in the cloud. OpenStudio-server can be deployed using a  [Kubernetes Helm chart](https://github.com/NREL/OpenStudio-server-helm) and works with most cloud providers' Kubernetes stacks such as AWS Elastic Kubernetes Service (EKS). Once deployed, your application client can make REST HTTP requests to the OpenStudio-server without having to run it locally.  For details on how to install OpenStudio-server and set up cloud deployment, please consult [README.md.](https://github.com/NREL/OpenStudio-server-helm/#readme) For information on how to run an URBANopt workflow using OpenStudio-server, please consult this example [Jupyter notebook](https://github.com/NREL/docker-openstudio-jupyter/blob/openstudio/notebooks/create_URBANopt_OSA.ipynb).

After deploying an OpenStudio-server instance, you can create an OSA folder (OpenStudio Analysis folder), which is a zipped folder that includes an URBANopt project packaged to run via the OpenStudio server. After creating the zipped folder, you can send requests to the OpenStudio-server via the openstudio_meta CLI (openstudio_meta ships with Openstudio-server: https://github.com/NREL/OpenStudio-server/blob/develop/bin/openstudio_meta) which includes a function that sends REST HTTP requests to run the analysis in the OpenStudio server.

This [Jupyter notebook](https://github.com/NREL/docker-openstudio-jupyter/blob/openstudio/notebooks/create_URBANopt_OSA.ipynb) demonstrates an example of creating the OSA zipped folder and sending a request to run the URBANopt analysis via the openstudio_meta_cli.  To run the Jupyter notebook you can create a ruby kernel and follow these general steps to install the necessary dependencies to run the commands with the openstudio_meta.

1. Install rest-client in your ruby gems:

   ```terminal
   gem install rest-client
   ```

2.	Install PAT to have access to the openstudio_meta CLI and all gems necessary to run it (install directory on windows is  C/ParametricAnalysisTool-3.5.0).

3.	Start an OSAF server cluster on AWS, Google, Microsoft, etc. using the Helm charts. Note the server IP address (referred to as the HOST in the notebook); you will need it to submit the jobs.

The command to run the analysis is as follows (you will need to replace the values in angle brackets (< >) with your values):


```terminal
<path/to/openstudio-meta> run_analysis --debug --verbose <path/of/ URBANopt_template.json'> <Server IP address> -z <name_of_zipped_folder> -a single_run
```

For example:

```terminal
C:\ParametricAnalysisTool-3.1.0\pat\OpenStudio-server\bin\openstudio_metarun_analysis --debug --verbose 'C:/create_URBANopt_OSA/URBANopt_template.json' 'http://10.40.18.67' -z 'URBANopt' -a single_run
```
Once the job is submitted, you should be able to see the analysis status and the results on the Server Web Interface that you created. 
Note: The user can choose to recreate the functions that send REST HTTP requests in any coding language they want instead of using openstudio_meta CLI, which is in Ruby language. The details of the ruby based client that contain the REST API functions (e.g. new_analysis, run_analysis, download_datapoint) can be found in the [OpenStudio Analysis Gem](https://github.com/NREL/OpenStudio-analysis-gem/blob/develop/lib/openstudio/analysis/server_api.rb). The openstudio_meta -CLI tool makes uses of that API.


## Preparing URBANopt project inputs via an Inputs Translator

To streamline the creation of URBANopt inputs, a translator will first need to be developed. The translator should generally include methods that map project details defined in the commercial software platform to URBANopt inputs as outlined in the following sections. To see an example of an URBANopt project directory structure you can run the CLI to create a URBANopt project. A example project directory is also provided in the [example project repository](https://github.com/urbanopt/urbanopt-example-geojson-project/tree/develop/example_project). It is recommended that users refer to the following tutorials to understand more about URBANopt required files and their structure:   [Project Creation and GeoJSON File Tutorial](https://urbanopt-tutorial.s3.amazonaws.com/videos/05_CreateProject.mp4)  and  [Scenario Creation and Run](https://urbanopt-tutorial.s3.amazonaws.com/videos/06_CreateRunScenario.mp4). 


### Create a valid initial GeoJSON file

The first input of URBANopt is the Feature File which represents a selected set of features for analysis and is in GeoJSON is a widely used format for encoding geographic data. For example, GeoJSON supports representing points, LineString, and Polygon. Geometric objects with additional properties are Feature objects and sets of features are contained in FeatureCollection objects. The [GeoJSON schema](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/geojson_schema.json) should be followed when creating a GeoJSON file.


### Add project-level features to the GeoJSON

After creating the GeoJSON file structure, project-level features can be added to create an URBANopt GeoJSON file. Project-level features are properties that are applied to all the features in the URBANopt GeoJSON file. These properties include top-level inputs such as the simulation timestep, the weather file name, and the climate zone. The schema for these properties is defined in the [site properties schema](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/site_properties.json).

### Add a building feature to the GeoJSON

The next step is to add building features to the GeoJSON file according to the [building properties schema](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/building_properties.json) as a guide. This schema describes (A) the required and optional inputs for representing buildings in the GeoJSON file and (B) the format/type that these inputs should follow.

### Extracting and storing the Weather File

Methods can be created to extract a valid weather file and store it in the directory named “weather”. The required files that should be extracted are the epw, ddy, and stat files. The example URBANopt project uses [weather files for Buffalo NY](https://github.com/urbanopt/urbanopt-cli/tree/develop/example_files/weather). Additional weather files can be found on the [EnergyPlus Website](https://energyplus.net/weather). The translator could have the functionality to connect to the website and download identified weather files or parse them from a weather database if it exists. The weather file can then be saved in the `weather` directory at the top of an URBANopt project directory to be utilized in the simulation.

## Reading URBANopt Outputs Via an Outputs Translator

After running the analysis, the URBANopt output results can be found in the URBANopt project directory, which will follow according to the [URBANopt reporting schema](https://github.com/urbanopt/urbanopt-reporting-gem/tree/develop/lib/urbanopt/reporting/default_reports/schema). For more information about URBANopt outputs refers to the [URBANopt Results Processing](https://urbanopt-tutorial.s3.amazonaws.com/videos/07a_PostProcess.mp4) tutorial.  
The client can create a translator that processes these result files and maps them back in a specific format to be displayed on the client's platform.