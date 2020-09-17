---
layout: default
title: Linux Installation
parent: Installation
nav_order: 3
---

# Linux Installation Instructions

## Dependencies

**_Linux installation has not been tested exhaustively. Please submit a bug report via the Github issue page if you run into installation errors_**

1. Install Ruby 2.5 (anything in the 2.5.x range will work).  We recommend using [rbenv](https://github.com/rbenv/rbenv#installation) to manage and install [Ruby 2.5](https://github.com/rbenv/rbenv#installing-ruby-versions)
    - Install rbenv on your system
    - Install your desired Ruby version
    - Do not forget the `rbenv init` step of rbenv installation
    - Once installed, you may check which versions of Ruby have been installed and which one is active with: `rbenv versions`
    - Set your current directory to use Ruby 2.5.x with: `rbenv local 2.5.x`
    - Full documentation for rbenv can be found at the [rbenv github site](https://github.com/rbenv/rbenv#command-reference)
 
1. Install Bundler version 2.1:

	```terminal
	gem install bundler -v 2.1
	```

1. Install [OpenStudio 3.0.0](https://github.com/NREL/OpenStudio/releases/tag/v3.0.0) \

	OpenStudio is designed to be used on Ubuntu 18.04. For other Ubuntu versions, see the [troubleshooting](troubleshooting.md) page.

1. Add the `RUBYLIB` environment variable path pointing to OpenStudio Ruby location by pasting the following line into your `.bash_profile`, `.zshenv` or similar file: 

	```terminal
		export RUBYLIB=/usr/local/openstudio-X.X.X/Ruby
	```

	(where X.X.X is the OpenStudio version installed)

## Basic Set-up

1. Install the URBANopt™ Command Line Interface (CLI):

    ```terminal
    gem install urbanopt-cli
    ```

1. View available CLI commands with:

    ```terminal
    uo --help
    ```

1. For detailed instructions, see our [example instructions](../usage/run_project.md)
