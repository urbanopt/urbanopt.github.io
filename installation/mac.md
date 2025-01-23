---
layout: default
title: Mac Installation
parent: Installation
nav_order: 1
---

# Mac Installation Instructions

An URBANopt<sup>&trade;</sup> installer (Mac OSX >= 10.12) is available to install the URBANopt CLI, Ruby, and OpenStudio SDK at the same time.  If you'd rather install the dependencies manually, view the [manual install](#manual-install) section below.

For CLI usage examples, see our [Getting Started page](../getting_started/getting_started.md)

We also have [tutorial videos](../resources/tutorials/tutorials.md) available to guide you through the installation steps.

## Install with the URBANopt Installer

Follow the steps below or watch the [Mac Installer Video](https://urbanopt-tutorial.s3.amazonaws.com/videos/02_Mac_Installer.mp4).

1. Download the desired version of the  [.dmg package](http://urbanopt-cli-installers.s3-website-us-west-2.amazonaws.com/).

1. Use the GUI installer and choose a directory to install. Once installed, open a terminal and run the provided setup script. The ```setup-env.sh``` generates env variables and stores them in a file ```.env_uo.sh``` in your home directory:

    ```bash
    /Applications/UrbanOptCLI_X.X.X/setup-env.sh
    . ~/.env_uo.sh
    ```

1. When launching new shell terminals, run ```. ~/.env_uo.sh``` to setup the environment.

## Manual Install

Follow the steps below or watch the [Mac Manual Installation Video](https://urbanopt-tutorial.s3.amazonaws.com/videos/04_Mac_Manual_Install.mp4).

1. Install Ruby 3.2.2.  We recommend using [rbenv](https://github.com/rbenv/rbenv#installation) to manage and install [Ruby](https://github.com/rbenv/rbenv#installing-ruby-versions)
    - `brew install rbenv`
    - `rbenv install 3.2.2`
    - Do not forget the `rbenv init` step of rbenv installation
    - Once installed, you may check which versions of Ruby have been installed and which one is active with: `rbenv versions`
    - Set your current directory to use Ruby 3.2.2 with: `rbenv local 3.2.2`
    - Full documentation for rbenv can be found at the [rbenv github site](https://github.com/rbenv/rbenv#command-reference)

1. Install Bundler version 2.4.10:

    ```terminal
    gem install bundler -v 2.4.10
    ```

1. Install [OpenStudio 3.9.0](https://github.com/NREL/OpenStudio/releases/tag/v3.9.0)

1. Add the `RUBYLIB` path as an "environment variable", pointing to the OpenStudio Ruby location you just installed.  You can use a text editor such as TextEdit, Sublime Text, vi or nano to open `.bash_profile` (or `.zshenv` if using zsh, the default since MacOS 10.15 Catalina).  The following is an example using nano:

    1. Type the following in the terminal: `nano ~/.bash_profile` (or `nano ~/.zshenv`)
    1. Copy and paste `export RUBYLIB=/Applications/OpenStudio-X.X.X/Ruby` (where X.X.X is the OpenStudio version installed) at the top of the file
    1. Press `control-x` to exit, then `y` to save the changes, and the `return` key to go back to the terminal

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

The URBANopt CLI includes District Energy System (DES) support.  This functionality is implemented in Python and Modelica and requires additional dependencies before use.

While the GeoJSON Modelica Translator will be installed automatically with the UO CLI `install_python` command, follow the [DES Installation Instructions](./des_installation.md) to install additional dependencies related to this workflow.
