---
layout: default
title: Troubleshooting
parent: Installation
nav_order: 4
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
gem install bundler -v 1.17
```