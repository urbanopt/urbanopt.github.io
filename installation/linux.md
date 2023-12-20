---
layout: default
title: Linux Installation
parent: Installation
nav_order: 3
---

# Linux Installation Instructions

As of version 0.3.1, an URBANopt<sup>&trade;</sup> installer (Ubuntu 18.04) is available to install the URBANopt CLI, Ruby, and OpenStudio SDK at the same time.  If you'd rather install the dependencies manually, view the [manual install](#manual-install) section below.

For CLI usage examples, see our [Getting Started page](../getting_started/getting_started.md)

## Install with the URBANopt Installer

1. Download the desired version of the [.deb package](http://urbanopt-cli-installers.s3-website-us-west-2.amazonaws.com/).

1. This will install to ```/usr/local/``` directory.
e.g.:  ```/usr/local/urbanopt-cli-0.3.1/```

1. To run the UrbanOpt CLI, first run the ```setup-env.sh``` script that generates environmental variables and stores these in ```env_uo.sh``` in your home directory.

```bash
/usr/local/urbanopt-cli-X.X.X/setup-env.sh
. ~/.env_uo.sh
```

1. When launching new shell terminals, run ```. ~/.env_uo.sh``` to setup the environment.

## Manual Install

**_Linux installation has not been tested exhaustively. Please submit a bug report via the [Github issue page](https://github.com/urbanopt/urbanopt-cli/issues) if you run into installation errors_**

1. Install Ruby 2.7.2.  We recommend using [rbenv](https://github.com/rbenv/rbenv#installation) to manage and install [Ruby](https://github.com/rbenv/rbenv#installing-ruby-versions)
    - Install rbenv on your system
    - Install your desired Ruby version
    - Do not forget the `rbenv init` step of rbenv installation
    - Once installed, you may check which versions of Ruby have been installed and which one is active with: `rbenv versions`
    - Set your current directory to use Ruby 2.7.2 with: `rbenv local 2.7.2`
    - Full documentation for rbenv can be found at the [rbenv github site](https://github.com/rbenv/rbenv#command-reference)

1. Install Bundler version 2.1:

	```terminal
	gem install bundler -v 2.1
	```

1. Install [OpenStudio 3.7.0](https://github.com/NREL/OpenStudio/releases/tag/v3.7.0)

	OpenStudio is designed to be used on Ubuntu 18.04. For other Ubuntu versions, see the [troubleshooting](troubleshooting.md) page.

1. Add the `RUBYLIB` environment variable path pointing to OpenStudio Ruby location by pasting the following line into your `.bash_profile`, `.zshenv` or similar file:

	```terminal
		export RUBYLIB=/usr/local/openstudio-X.X.X/Ruby
	```

	(where X.X.X is the OpenStudio version installed)

1. Install the URBANopt Command Line Interface (CLI):

    ```terminal
    gem install urbanopt-cli
    ```

## URBANopt CLI Usage

1. View available CLI commands with:

    ```terminal
    uo --help
    ```

1. For detailed instructions, see the [Getting Started page](../getting_started/getting_started.md).

## URBANopt Python Dependencies

As of version 0.9.0, the URBANopt CLI has integrated 3 python workflows: OpenDSS, DES, and DISCO.  To install these python dependencies, a new URBANopt CLI command has been created.  The following command will install python and pip as well as the python packages urbanopt-ditto-reader, geojson-modelica-translator, and nrel-disco.

```terminal
uo install_python
```

The python installation path will be printed in the terminal once python is successfully installed; you may want to save this path for use in future troubleshooting.


## DES Installation

As of version 0.5.2, the URBANopt CLI includes DES support.  This functionality is implemented in Python and Modelica requires that various dependencies be installed before use.

While the GeoJSON Modelica Translator will be installed automatically with the UO CLI `install_python` command, follow the [DES Installation Instructions](./des_installation.md) to install additional dependencies related to this workflow.
