---
layout: default
title: DES Installation
parent: Installation
nav_order: 5
---

# DES Installation

## Installation via URBANopt CLI

1. Run the following URBANopt CLI command to install all URBANopt python dependencies.  As of version 0.9.0, URBANopt requires Python v3.10.
  ```bash
   uo install_python
  ```
2. You will also need to install and configure MBL and Docker as described in the manual section:

  - [Installing and configuring the Modelica Buildings Library (MBL)](#mbl-installation)
  - [Installing and configuring Docker in order to run simulations using JModelica](#docker-installation)

### Usage

#### Create System Parameters File

Once the python dependencies are installed, the `uo des_params` CLI command can be used to build the system parameters file from the URBANopt post-processed results:

```bash 
  uo des_params -y <path/to/system-parameters-file.json> -s <path/to/scenario.csv> -f <path/to/featurefile.json>
```

Use the following for additional help on running this command: 

```bash
 uo des_params --help
```

#### Create Modelica package

Once the system parameters file is generated, the CLI can then be used to create the model with the following command:

```bash
  uo  des_create -y <path/to/system-parameters-file.json> -f <path/to/featurefile.json> -d <path/to/modelica/dir/to/create>
```

Use the following for additional help on running this command: 

```bash
 uo des_create --help
```
The resulting Modelica package will be created and can be opened in a Modelica editor. Open the `package.mo` file in the root directory of the generated package. You will also need to load the MBL into your Modelica editor.

#### Run the Model

After the model is created, use the CLI to run the model using the following
command:

```bash
  uo des_run -m <path/to/modelica/dir>
```

Use the following for additional help on running this command: 

```bash
 uo des_run --help
```

## Manual Installation

There are three major steps to running the GeoJSON to Modelica Translator (GMT):

1. generating the GeoJSON and System Parameter JSON files,
1. creating of the Modelica package containing the district system, and
1. running the Modelica package.

Depending on the use case, the need to run all the steps above may not be needed. For example:
it may be desirable to only generate the Modelica package and then open and run the model
in a Modelica user interface such as Dymola. Or, there may be a case to simply generate the
GeoJSON and System Parameter file from results of an URBANopt SDK simulation result set.

This Installation guide is broken up into three major setup steps:

1. [Installing the GMT](#gmt-installation)
2. [Installing and configuring the Modelica Buildings Library (MBL)](#mbl-installation)
3. [Installing and configuring Docker in order to run simulations using JModelica](#docker-installation)

### GMT Installation

You must have PIP and Python 3.7 or later installed (run `python --version` to see what version you're using). After installing Python and PIP run the following in a terminal (requires Python 3):

```bash
 pip install geojson-modelica-translator
```

After installation of the GMT, a new command line interface (called the URBANopt District Energy Systems [UO DES] CLI) can be used to run various commands. Without needing to install the [MBL](https://github.com/lbl-srg/modelica-buildings/) the user can use the CLI to build the system parameters file from the results of the URBANopt SDK. For more information run the following:

```bash
  uo_des -h
  uo_des build-system-param -h

  # the command below is only an example; however, it will run if the repository
  # is checked out and run in the following path: ./tests/management/data/sdk_project_scraps
  uo_des build-sys-param sys_param.json baseline_scenario.csv example_project.json
```

### MBL Installation


The Modelica Buildings Library contains many models that are needed to assemble the district systems.
Installation of the MBL is done through Git and GitHub. Follow the instructions below to install the MBL needed for the GMT:

- Clone the [MBL](https://github.com/lbl-srg/modelica-buildings/) into a working directory outside of the GMT directory
- Change to the directory inside the modelica-buildings repo you just checked out. (`cd modelica-buildings`)
- Install git-lfs
  - Mac: `brew install git-lfs; git lfs install`
  - Ubuntu: `sudo apt install git-lfs; git lfs install`
- Pull the correct staging branch for this project with: `git checkout v9.0.0`. Note that the git repo will be in a detached head state, which is expected.
- Add the Modelica Buildings Library path to your MODELICAPATH environment variable (e.g., `export MODELICAPATH=${MODELICAPATH}:$HOME/path/to/modelica-buildings`). Restart your terminal to ensure that the MBL library is exported correctly.

Once the MBL is installed, the CLI can be used to create the model with the following command:

```bash
  uo_des create-model -h

  # the command below is only an example; however, it will run if the repository
  # is checked out and run in the following path: ./tests/management/data/sdk_project_scraps
  uo_des create-model sys_param.json
```

The resulting Modelica package will be created and can be opened in a Modelica editor. Open the `package.mo` file in the root directory of the generated package. You will also need to load the MBL into your Modelica editor.

#### Docker Installation

In GMT version 0.3.0, the only method of running the simulations is within Dymola or OpenModelica. We are working on an
approach that will re-enable the Docker-based solution that is described below.

The GMT versions prior to 0.3.0 enabled running of the models using JModelica. It requires the
installation of [Docker](https://docs.docker.com/get-docker/). To configure Docker, do the following:

- Install [Docker](https://docs.docker.com/get-docker/) for your system.
- Configure Docker Desktop to have at least 4 GB Ram and 2 cores. This is configured under the Docker Preferences.
- It is recommended to test the installation of Docker by simply running `docker run hello-world` in a terminal.

After Docker is installed and configured, you can use the CLI to run the model using the following
command:

```bash
  uo_des run-model -h

  # the command below is only an example; however, it will run if the repository
  # is checked out and run in the following path: ./tests/management/data/sdk_project_scraps
  uo_des run-model model_from_sdk
```
