---
layout: default
title: Windows Installation
parent: Installation
nav_order: 2
---

# Windows Installation Instructions

## Dependencies

1. Install [Ruby 2.2.4 (x64)](https://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-2.2.4-x64.exe)  
**Note**: URBANopt will update to Ruby 2.5 when OpenStudio 3.0 is released

1. Install Devkit using the [mingw64](https://dl.bintray.com/oneclick/rubyinstaller/DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe) installer  
1. Include path to Ruby by adding the following to your environment variables path: 

	`C:\Ruby22-x64\bin`
1. Create a new environment variable `HOME` and set the variable value to the following: 

	`C:\Users\<user_name>`
1. Install Bundler version 1.17:

	```terminal
	gem install bundler -v 1.17
	```

1. Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
1. Create file `C:\ruby-2.2.4-x64-mingw32\lib\ruby\site_ruby\openstudio.rb` and edit it to contain:

	```ruby
	require 'C:\openstudio-2.8.1\Ruby\openstudio.rb'
	```

1. Verify your OpenStudio and Ruby configuration:

	```terminal
	ruby -e "require 'openstudio'" -e "puts OpenStudio::Model::Model.new"
	```

	Expected output:

	```terminal
	OS:Version,
	 {<long-uuid>},                          !- Handle
	 2.8.1;                                  !- Version Identifier`
	 ```

1. Install [Git](https://git-scm.com/) if not already installed. A list of [optional git
   GUIs](https://github.com/NREL/OpenStudio/wiki/Using-OpenStudio-with-Git-and-GitHcub) can
  be found here,
   along with some help using git with OpenStudio. 

1. Configure git to allow long path names in git:

	```terminal
	git config --global core.longpaths true
	```

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
