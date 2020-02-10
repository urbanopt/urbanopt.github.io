---
layout: default
title: Linux Installation
parent: Installation
nav_order: 3
---

# Linux Installation Instructions

## Dependencies

**_Linux installation has not been tested exhaustively. Please submit an issue if you run into installation errors_**

1. Install Ruby 2.2.4.  We recommend using `rbenv` to manage and install [Ruby 2.2.4](https://github.com/rbenv/rbenv#installation)  
 **Note**: URBANopt will update to Ruby 2.5 when OpenStudio 3.0 is released
 1. Install Bundler version 1.17:

	```terminal
	gem install bundler -v 1.17
	```
1. Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
1. Manually install [libpng12](https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html)  
If you get gdbm libs errors like the following, you need to create symlinks to the appropriate version (`libgdbm.so.5` and `libgdbm_compat.so.4`):

	```terminal
	/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm.so.3: cannot open shared object file: No such file or directory
	```

	or:

	```terminal
	/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm_compat.so.3: cannot open shared object file: No such file or directory
	```


1. Add the `RUBYLIB` environment variable path pointing to OpenStudio Ruby location by pasting the following line into your `.bash_profile` or similar file: 

	```terminal
		export RUBYLIB=/usr/local/openstudio-2.8.1/Ruby
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
