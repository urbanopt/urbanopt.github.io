---
layout: default
title: Mac Installation
parent: Installation
nav_order: 1
---

# Mac Installation Instructions

## Dependencies

1. Install Ruby 2.2.4.  We recommend using [rbenv](https://github.com/rbenv/rbenv#installation) to manage and install [Ruby 2.2.4](https://github.com/rbenv/rbenv#installing-ruby-versions)
    - `brew install rbenv`
    - `rbenv install 2.2.4`
    - Do not forget the `rbenv init` step of rbenv installation
    - Once installed, you may check which versions of Ruby have been installed and which one is active with: `rbenv versions`
    - Set your current directory to use Ruby 2.2.4 with: `rbenv local 2.2.4`
    - Full documentation for rbenv can be found at the [rbenv github site](https://github.com/rbenv/rbenv#command-reference)

    **Note**: URBANopt will update to a newer Ruby version when OpenStudio 3.0 is released

1. Install Bundler version 1.17:

	```terminal
	gem install bundler -v 1.17
	```

1. Install [OpenStudio 2.9.1](https://github.com/NREL/OpenStudio/releases/tag/v2.9.1)  
1. Add the `RUBYLIB` path as an "environment variable", pointing to the OpenStudio Ruby location you just installed.  You can use a text editor such as TextEdit, Sublime Text, vi or nano to open `.bash_profile` (or `.zshenv` if using zsh, the default since MacOS 10.15 Catalina).  The following is an example using nano:

    1. Type the following in the terminal: `nano ~/.bash_profile` (or `nano ~/.zshenv`)
    1. Copy and paste `export RUBYLIB=/Applications/OpenStudio-X.X.X/Ruby` (where X.X.X is the OpenStudio version installed) at the top of the file
    1. Press `control-x` to exit, then `y` to save the changes, and the `return` key to go back to the terminal

## Basic Set-up

1. Install the URBANopt Command Line Interface (CLI):

    ```terminal
    gem install urbanopt-cli
    ```

1. View available CLI commands with:

    ```terminal
    uo -h
    ```

1. For detailed instructions, see our [example instructions](../usage/run_project.md)
