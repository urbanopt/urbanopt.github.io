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

While most of the URBANopt SDK uses Ruby, the OpenDSS part of the workflow is implemented in Python, and as such, has a different set of dependencies that must be installed:

- Git
- Python 3.7 or higher
- Pip
- Various python packages (opendssdirect.py, ditto)

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

1. Clone the urbanopt-ditto-reader to your local machine.  Example:
```bash
	git clone https://github.com/urbanopt/urbanopt-ditto-reader.git
```

1. Open a terminal in the cloned ditto-reader directory and install the ditto-reader dependencies:
```bash
	pip install -e .
```
This command will also automatically clone the [ditto repository](https://github.com/NREL/ditto) to the following location: ```urbanopt-ditto-reader/ditto```.

To verify your install, list the packages installed using the ```pip list``` command. The listing of packages installed should include UrbanoptDittoReader.

```
$ pip list
Package             Version Location
------------------- ------- ------------------------------------------
networkx            2.4
numpy               1.19.1
OpenDSSDirect.py    0.5.0
pandas              1.1.0
pip                 19.2.3
setuptools          41.2.0
traitlets           4.3.3
UrbanoptDittoReader 0.2.0   <path/to/urbanopt-ditto-reader>
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

Note that if you are not able to run the opendss command via the CLI, you can always access it manually by following the general [OpenDSS instructions](../opendss/opendss.md#converting-and-running-opendss).


## Usage

The DiTTo-Reader/OpenDSS workflow is available via the ```opendss``` URBANopt CLI command. 


### Notes
- The ```run``` and ```process``` commands must first be run on a scenario to generate the input files required by OpenDSS. 
- The feature file should contain Electrical Connectors and Junctions for a successful OpenDSS run.
- The location of ditto is assumed to be found at ```urbanopt-ditto-reader/ditto```.
- The equipment file is assumed to be at ```urbanopt-ditto-reader/example/electrical-database.json``` unless otherwise specified with the --equipment flag when issuing the command
- If you want to include reopt results as an input to OpenDSS, make sure to specify the --reopt flag when issuing the command

For usage help:
```bash
uo opendss --help
```

An example:
```bash
uo opendss --scenario baseline_scenario.csv --feature example_project.json
```