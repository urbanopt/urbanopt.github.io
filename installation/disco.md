---
layout: default
title: DISCO Installation
parent: Installation
nav_order: 4
---


## DISCO Installation

DISCO is implemented as a python package and will be available for download on PyPi. It will be
installed, along with its dependencies, in a dedicated conda environment, within a *python_deps*
folder on the user's machine. Installation scripts will
be provided for Linux, Mac OS, and Windows platforms. The scripts will be invoked via an URBANopt
CLI command described below, without the user having to manually install python. This installation process should not
interfere with python versions and dependency packages already installed on a user’s machine. URBANopt currently supports **python 3.10** for its workflows.

### To Install

Use the following CLI command to install the **NREL-disco**, **urbanopt-ditto-reader** and
**geojson-modelica-translator** python packages:

```bash
uo install_python
```

This installs to the following versions of the python packages:


|Python Package               |Version |
|:---------------------------:|:------:|
| NREL-disco                  | 0.4.0	 |
| urbanopt-ditto-reader       | 0.6.3	 |
| geojson-modelica-translator | 0.6.0  |


To update these versions, modify the version in the *dependencies.json*
file in the *python_deps* folder for the package.

### To check DISCO installation

To verify your install, list the packages installed using the ```pip list``` command and check for
the NREL-disco package.
