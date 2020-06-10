---
layout: default
title: Linux Installation
parent: Installation
nav_order: 3
---

# Linux Installation Instructions

## Dependencies

**_Linux installation has not been tested exhaustively. Please submit a bug report via the Github issue page if you run into installation errors_**

1. Install Ruby 2.5 (anything in the 2.5.x range will work).  We recommend using `rbenv` to manage and install [Ruby 2.5](https://github.com/rbenv/rbenv#installation)  
 
 1. Install Bundler version 2.1:

	```terminal
	gem install bundler -v 2.1
	```
1. Install [OpenStudio 3.0](https://github.com/NREL/OpenStudio/releases/tag/v3.0.0)  
1. Manually install [libpng12](https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html)  
If you get gdbm libs errors like the following, you need to create symlinks to the appropriate version (`libgdbm.so.5` and `libgdbm_compat.so.4`):

	```terminal
	/usr/local/openstudio-3.0.0/bin/openstudio: error while loading shared libraries: libgdbm.so.3: cannot open shared object file: No such file or directory
	```

	or:

	```terminal
	/usr/local/openstudio-3.0.0/bin/openstudio: error while loading shared libraries: libgdbm_compat.so.3: cannot open shared object file: No such file or directory
	```


1. Add the `RUBYLIB` environment variable path pointing to OpenStudio Ruby location by pasting the following line into your `.bash_profile`, `.zshenv` or similar file: 

	```terminal
		export RUBYLIB=/usr/local/openstudio-X.X.X/Ruby
	```

	(where X.X.X is the OpenStudio version installed)

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
