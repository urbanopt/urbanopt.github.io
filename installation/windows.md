---
layout: default
title: Windows Installation
parent: Installation
nav_order: 2
---

# Windows Installation Instructions

## Dependencies

1. Install [Ruby 2.5 (x64)](https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-2.5.5-1/rubyinstaller-2.5.5-1-x64.exe)  

	Make sure that you select option 3: **MSYS2 and MINGW development toolchain** during the installation process:
	![installer options](../doc_files/ruby_windows.png)

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

## Basic Set-up

1. Install the URBANopt Command Line Interface (CLI):

    ```terminal
    gem install urbanopt-cli
    ```

1. View available CLI commands with:

    ```terminal
    uo --help
    ```

1. For detailed instructions, see our [example instructions](../usage/run_project.md)

## OpenDSS and DiTTo Reader Set-up

As of version 0.4.0, the URBANopt CLI includes DiTTo/OpenDSS support.  Since this functionality is implemented in Python, a different set of dependencies must be installed in order to use it.  

If you'd like to use this functionality, follow the [OpenDSS installation](./ditto_reader.md) instructions.  

Note that Windows users may experience some difficulty during the install (particularly with the environment variable setup).  If you are not able to access the opendss command via the CLI, you can always access it manually by following the general [OpenDSS instructions](../opendss/opendss.md#converting-and-running-opendss).
