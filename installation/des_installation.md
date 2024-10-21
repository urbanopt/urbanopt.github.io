---
layout: default
title: DES Installation
parent: Installation
nav_order: 5
---

# DES Installation

## Installation via URBANopt CLI

1. Run the following URBANopt CLI command to install all URBANopt python dependencies, which includes the GeoJSON-to-Modelica-Translator (GMT).  As of version 0.9.0, URBANopt requires Python v3.10.
  ```bash
   uo install_python
  ```
2. You will also need to install and configure MBL and Docker as described in the manual section:

  - [Installing and configuring the Modelica Buildings Library (MBL)](#mbl-installation)
  - [Installing and configuring Docker to run simulations using OpenModelica](#docker-installation)

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
  uo  des_create -y <path/to/system-parameters-file.json> -f <path/to/featurefile.json> -n <path/to/modelica/dir/to/create>
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

Follow the instructions to [install the GMT](https://docs.urbanopt.net/geojson-modelica-translator/getting_started.html) as a standalone package with its own CLI.
