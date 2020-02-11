---
layout: default
title: Mac Installation
parent: Installation
nav_order: 1
---

# Mac Installation Instructions

## Dependencies

1. Install Ruby 2.2.4.  We recommend using [rbenv](https://github.com/rbenv/rbenv#installation) to manage and install [Ruby 2.2.4](https://github.com/rbenv/rbenv#installing-ruby-versions)  
 **Note**: URBANopt will update to a newer Ruby version when OpenStudio 3.0 is released

1. Install Bundler version 1.17:

	```terminal
	gem install bundler -v 1.17
	```

1. Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
1. Add the `RUBYLIB` path as an "environment variable", pointing to the OpenStudio Ruby location you just installed.  You can use a text editor such as vi or nano to open `.bash_profile` (or `.zshenv` if using zsh, the default since MacOS 10.15 Catalina).  The following is an example using nano:

    1. Type the following in the terminal: `nano ~/.bash_profile` (or `nano ~/.zshenv`)
    1. Copy and paste `export RUBYLIB=/Applications/OpenStudio-2.8.1/Ruby` at the top of the file
    1. Press `control-x` to exit, then `y` to save the changes, and the `return` key to go back to the terminal

1. Install [Git](https://git-scm.com/) if not already installed (unlikely). A list of [optional git
   GUIs](https://github.com/NREL/OpenStudio/wiki/Using-OpenStudio-with-Git-and-GitHub)
   can be found here,
   along with some help using git with OpenStudio.

## Basic Set-up

1. Fork the [URBANopt example project](https://github.com/urbanopt/urbanopt-example-geojson-project)
1. Clone the forked repo down to your local machine  

1. Follow the instructions in the [Example Project](../usage/run_example) section to install the URBANopt example project.  This will install the URBANopt SDK.
