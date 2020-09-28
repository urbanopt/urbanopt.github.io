---
layout: default
title: Windows Installation
parent: Installation
nav_order: 2
---

# Windows Installation Instructions

As of version 0.3.1, an URBANopt installer (64-bit Windows 7 – 10) is available to install the URBANopt CLI, Ruby 2.5.x, and OpenStudio SDK at the same time.  If you'd rather install the dependencies manually, view the [manual install](#manual-install) section below.  

For CLI usage examples, see our [example project](../usage/run_project.md)

## Install with the URBANopt installer

1. Download the desired version of the [.exe installer](http://urbanopt-cli-installers.s3-website-us-west-2.amazonaws.com/). 

1. Use the GUI installer and choose a directory to install. Once installed, open a terminal (Powershell, Windows CMD and GitBash are supported) and run the provided setup script for that shell (below are the setup scripts for each respective shell environment).


### Bash (or GitBash for Windows)
```terminal
c:/urbanopt-cli-X.X.X/setup-env.sh  
. ~/.env_uo.sh  
```

### Powershell
```terminal
c:\urbanopt-cli-X.X.X\setup-env.ps1  
. ~\.env_uo.ps1  
```
### Windows Command Prompt
```terminal
c:\urbanopt-cli-X.X.X\setup-env.bat  
%HOMEPATH%\.env_uo.bat  
```

1. When launching new shell terminals, run the correct environment config to setup the environment. 


## Manual Install

1. Install [Ruby 2.5 (x64)](https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-2.5.5-1/rubyinstaller-2.5.5-1-x64.exe)  

1. Include path to Ruby by adding the following to your environment variables path: 

	`C:\Ruby25-x64\bin`
1. Create a new environment variable `HOME` and set the variable value to the following: 

	`C:\Users\<user_name>`

	Detailed instructions for [creating environment variables](https://helpdeskgeek.com/how-to/create-custom-environment-variables-in-windows/) can be found online.
1. Install Bundler version 2.1:

	```terminal
	gem install bundler -v 2.1
	```


1. Install [OpenStudio 3.0.1](https://github.com/NREL/OpenStudio/releases/tag/v3.0.1)  

1. Create file `C:\ruby-2.5.5-1-x64-mingw32\lib\ruby\site_ruby\openstudio.rb` and edit it to contain the path to your installed OpenStudio (where X.X.X is the OpenStudio version installed):

	```ruby
	require 'C:\openstudio-X.X.X\Ruby\openstudio.rb'
	```

1. Verify your OpenStudio and Ruby configuration:

	```terminal
	ruby -e "require 'openstudio'" -e "puts OpenStudio::Model::Model.new"
	```

	Expected output:

	```terminal
	OS:Version,
	 {<long-uuid>},                          !- Handle
	 3.0.0;                                  !- Version Identifier`
	 ```

<!-- 1. Install [Git](https://git-scm.com/) if not already installed. A list of [optional git
   GUIs](https://github.com/NREL/OpenStudio/wiki/Using-OpenStudio-with-Git-and-GitHcub) can
  be found here,
   along with some help using git with OpenStudio. 

1. Configure git to allow long path names in git:

	```terminal
	git config --global core.longpaths true
	``` -->

1. Install the URBANopt™ Command Line Interface (CLI):

    ```terminal
    gem install urbanopt-cli
    ```

## URBANopt CLI Usage

1. View available CLI commands with:

    ```terminal
    uo --help
    ```

1. For detailed instructions, see our [example project](../usage/run_project.md)
