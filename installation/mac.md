---
layout: default
title: Mac Installation
parent: Installation
nav_order: 1
---

- We recommend using `rbenv` to install [Ruby 2.2.4](https://github.com/rbenv/rbenv#installation)  
(URBANopt will update to Ruby 2.5 when OpenStudio 3.0 is released)  
- Install Bundler version 1.17:

```terminal
gem install bundler -v 1.17
```

If you have a secure firewall that prevents **bundler** from installing properly, create a `.gemrc` file in your home directory that contains:

```yml
:backtrace: false
:bulk_threshold: 1000
:sources:
- http://rubygems.org
:update_sources: true
:verbose: true
```

Now install **bundler**:

```terminal
gem install bundler -v 1.17
```

- Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
Add path to Ruby in the new OpenStudio folder by pasting into your `.bash_profile` or similar file: `export RUBYLIB=/Applications/OpenStudio-2.8.1/Ruby`
