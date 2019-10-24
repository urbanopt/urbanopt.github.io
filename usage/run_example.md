---
layout: default
title: Example Project
parent: Usage
nav_order: 1
---

Move to the top-level directory of the example project you just cloned. Run the following commands to execute the Rake tasks defined in the Rakefile of the current working directory.

1. `bundle install` to ensure all dependencies in your Gemfile are available
1. `bundle update` to update your gems to the latest available versions
1. `bundle exec rake openstudio:runner:init`  
This command creates a `runner.conf` file that can be used to configure URBANopt. We recommend setting `num_parallel` to the smaller of the number of Features you have or cores in your CPU.
1. `bundle exec rake <rake task>` to execute Rake tasks
