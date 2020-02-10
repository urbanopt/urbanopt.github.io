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
1. Add the `RUBYLIB` environment variable path pointing to OpenStudio Ruby location by pasting the following line into your `.bash_profile` or similar file:

	```terminal
		export RUBYLIB=/Applications/OpenStudio-2.8.1/Ruby
	```

1. Install [Git](https://git-scm.com/) if not already installed. A list of [optional git
   GUIs](https://github.com/NREL/OpenStudio/wiki/Using-OpenStudio-with-Git-and-GitHub)
   can be found here,
   along with some help using git with OpenStudio.

## Basic Set-up

1. Install the URBANopt Command Line Interface (CLI):

    ```terminal
    gem install uo_cli
    ```

1. View available CLI commands with:

    ```terminal
    uo -h
    ```

1. For detailed instructions, see our [example instructions](../usage/run_example.md)
