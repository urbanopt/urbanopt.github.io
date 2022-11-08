---
layout: default
title: DISCO Installation
parent: Installation
nav_order: 4
---


## DISCO Installation

DISCO is implemented as a python package and will be available for download on PyPi. It will be
installed, along with its dependencies, in a dedicated conda environment. Installation scripts will
be provided for Linux, Mac OS, and Windows platforms. The scripts will be invoked via an URBANopt
CLI command without the user having to manually install python. This installation process should not
interfere with python versions and dependency packages already installed on a user’s machine.

### To Install

Use the following CLI  command: 

```bash
uo install_python
```

To update the versions of the python packages installed, modify the version in the *dependencies.json*
file in the *python_deps* folder for the package.

### To check DISCO installation

To verify your install, list the packages installed using the ```pip list``` command and check for
the NREL-Disco package.