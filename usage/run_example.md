---
layout: default
title: Example Project
parent: Usage
nav_order: 1
---

The URBANopt example project is a ***<u>hypothetical</u> project only*** that was created to demonstrate various capabilities of the URBANopt SDK in version 0.1.0 (e.g. modeling of various building types and heights in one district). An actual location for the *hypothetical* project was needed to demonstrate geospatial aspects of the URBANopt SDK. The <u>hypothetical</u> example project’s location is within the boundary of an actual district project that is a participant in the U.S. Department of Energy Zero Energy District Accelerator. See “Western New York Manufacturing Zero Energy District” in [this NREL paper](https://www.nrel.gov/docs/fy18osti/71841.pdf) for a description of the actual project. The design in the <u>hypothetical</u> URBANopt example project has no relation to the designs and site requirements of the actual development project.

To prepare URBANopt, move to the top-level directory of the [example project](https://github.com/urbanopt/urbanopt-example-geojson-project) you just forked and cloned in the previous step. Run the following commands to set up the Rake tasks defined in the Rakefile of the current working directory.

1. `bundle install` to ensure all dependencies in your [Gemfile](https://github.com/urbanopt/urbanopt-example-geojson-project/blob/develop/Gemfile) are available
1. `bundle update` to update your gems to the latest available versions
1. `bundle exec rake openstudio:runner:init`  
This command creates a `runner.conf` file that can be used to configure URBANopt. We recommend setting `num_parallel` to the smaller of the number of Features you have or cores in your CPU.
1. `bundle exec rake <rake task>` to execute [Rake tasks](rake_tasks.md) as described in the next section.
