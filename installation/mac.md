---
layout: default
title: Mac Installation
parent: Installation
nav_order: 1
---

# Mac Installation Instructions

As of version 0.3.1, an URBANopt<sup>&trade;</sup> installer (Mac OSX >= 10.12) is available to install the URBANopt CLI, Ruby, and OpenStudio SDK at the same time.  If you'd rather install the dependencies manually, view the [manual install](#manual-install) section below.

For CLI usage examples, see our [Getting Started page](../getting_started/getting_started.md)

## Install with the URBANopt Installer

1. Download the desired version of the  [.dmg package](http://urbanopt-cli-installers.s3-website-us-west-2.amazonaws.com/).

1. Use the GUI installer and choose a directory to install. Once installed, open a terminal and run the provided setup script. The ```setup-env.sh``` generates env variables and stores them in a file ```.env_uo.sh``` in your home directory: 

    ```bash
    /Applications/UrbanOptCLI_X.X.X/setup-env.sh  
    . ~/.env_uo.sh
    ```

1. When launching new shell terminals, run ```. ~/.env_uo.sh``` to setup the environment. 


## Manual Install

1. Install Ruby 2.7.2 (anything in the 2.7.x range will work).  We recommend using [rbenv](https://github.com/rbenv/rbenv#installation) to manage and install [Ruby 2.7](https://github.com/rbenv/rbenv#installing-ruby-versions)
    - `brew install rbenv`
    - `rbenv install 2.7.x`
    - Do not forget the `rbenv init` step of rbenv installation
    - Once installed, you may check which versions of Ruby have been installed and which one is active with: `rbenv versions`
    - Set your current directory to use Ruby 2.7.x with: `rbenv local 2.7.x`
    - Full documentation for rbenv can be found at the [rbenv github site](https://github.com/rbenv/rbenv#command-reference)

1. Install Bundler version 2.1:

	```terminal
	gem install bundler -v 2.1
	```

1. Install [OpenStudio 3.3.0](https://github.com/NREL/OpenStudio/releases/tag/v3.3.0)  

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

## OpenDSS and DiTTo Reader Installation

As of version 0.4.0, the URBANopt CLI includes DiTTo/OpenDSS support.  Since this functionality is implemented in Python, a different set of dependencies must be installed in order to use it.  

If you'd like to use this functionality, follow the [OpenDSS installation](./ditto_reader.md) instructions.

## DES Installation

As of version 0.5.2, the URBANopt CLI includes DES support.  This functionality is implemented in Python and Modelica requires that various dependencies be installed before use. 

Follow the [DES Installation Instructions](./des_installation.md) to install these dependencies.
