---
layout: default
title: OpenDSS Installation
parent: Installation
nav_order: 4
---

# OpenDSS Installation for use in CLI

OpenDSS functionality is available through the URBANopt DiTTo Reader, which:

- converts URBANopt<sup>&trade;</sup> output files for a scenario into OpenDSS input files
- runs the OpenDSS workflow
- stores OpenDSS results into the URBANopt scenario directory

## Requirements

While most of the URBANopt SDK uses Ruby, the OpenDSS part of the workflow is implemented in Python:

- Python 3.7 or higher

## Installation

1. Ensure that you have python version 3.7 or higher installed on your system.

	**Attention Pyenv users:** You must install python with the ‘enable-shared’ flag:
```bash
	env PYTHON_CONFIGURE_OPTS='--enable-shared' pyenv install 3.7.X
```

1. Install pip.  The following command should return the version if pip is already installed, and fail otherwise:
```bash
	pip --version
```

1. Install the URBANopt Ditto Reader package:
```bash
	pip install urbanopt-ditto-reader
```

This command will also automatically install the [ditto repository](https://github.com/NREL/ditto).

To verify your install, list the packages installed using the ```pip list``` command. The listing of packages installed should include urbanopt-ditto-reader.

```
$ pip list
Package                 Version Location
----------------------- ------- ------------------------------------------
ditto.py				0.2.0
networkx            	2.5
numpy               	1.201
OpenDSSDirect.py    	0.6.0
pandas              	1.2.2
pip                 	21.0.1
setuptools          	53.0.0
traitlets           	5.0.5
urbanopt-ditto-reader 	0.3.6
```

### Troubleshooting for WINDOWS users

Configuring the following environment variables may help if you are having issues running this workflow.
- Ensure that the python directory and the python/Scripts directory are in your PATH variable
- Ensure that the python paths from above are listed before the path that starts with `%USERPROFILE%` and points to `AppData\Local\Microsoft\WindowsApps`
- Add an environment variable `PYTHONPATH` that points to your python directory
```bash
		PYTHONPATH = C:\Users\<username>\AppData\Local\Programs\Python\Python37
```
- Add an environment variable `LIBPYTHON` that points to your python DLL
```bash
		LIBPYTHON = C:\Users\<username>\AppData\Local\Programs\Python\Python37\python37.dll
```

Once you have made these changes, **close and reopen your Terminal/GitBash window** for these changes to take effect.

Visit the [OpenDSS page](../workflows/opendss/opendss.md#usage) for instructions on how to run the OpenDSS workflow.
