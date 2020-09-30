---
layout: default
title: Troubleshooting
parent: Installation
nav_order: 5
---

# Troubleshooting Installation Errors

## Bundler Installation Error

If you have a secure firewall that prevents **bundler** from installing properly, type the following into the command line:

`gem sources -c`  
`gem sources -a http://rubygems.org/`

You will need to accept the reduced security of `http` compared to `https`  

If installing bundler **still** doesn't work, create a `.gemrc` file in your home directory:

```terminal
type nul > C:\Users\<user_name>\.gemrc
```

Edit the new `.gemrc` file to contain:

```yml
:backtrace: false
:bulk_threshold: 1000
:sources:
- http://rubygems.org
:update_sources: true
:verbose: true
```

- Now install **bundler**:

```terminal
gem install bundler -v 2.1
```

## Installing OpenStudio in Ubuntu < 18.04:

Manually install [libpng12](https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html)
- If you get gdbm libs errors like the following, you need to create symlinks to the appropriate version (`libgdbm.so.5` and `libgdbm_compat.so.4`):

	```terminal
	/usr/local/openstudio-3.0.0/bin/openstudio: error while loading shared libraries: libgdbm.so.3: cannot open shared object file: No such file or directory
	```

	or:

	```terminal
	/usr/local/openstudio-3.0.0/bin/openstudio: error while loading shared libraries: libgdbm_compat.so.3: cannot open shared object file: No such file or directory
	```
