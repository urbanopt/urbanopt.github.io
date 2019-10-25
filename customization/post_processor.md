---
layout: default
title: Adding a custom Post-Processor
parent: Customization
nav_order: 3
---
## Adding a custom Post-Processor

The Scenario PostProcessor aggregates results from each Feature simulation. They require specific OpenStudio Reporting Measures be run for each Feature to generate required simulation reports (e.g. timeseries CSV data for specific outputs, specific metrics). Creating a new reporting measure to generate the Feature reports is described [here](#Feature-Reports). These individual simulation results should be aligned with the final desired aggregated results. For example, if users decide to customize the reporting measure to report 15min timestep results, additional methods should be developed to allow the post processors to aggregate data at this timestep.

Currently, the default Scenario Post Processors and its corresponding OpenStudio Reporting Measures are implemented in the **Scenario Gem**.  Additional Scenario Post Processors and OpenStudio Reporting Measures can be implemented in other OpenStudio Extension Gems. Users can use this gem as a guide to developing their own Post Processors or just customize this Gem to report and post-process new results of their interest. The current Scenario Processor which is used in this example project rake file is the `default_post_processor`, which is an object of ScenarioDefaultPostProcessor class.

```ruby
default_post_processor = URBANopt::Scenario::ScenarioDefaultPostProcessor.new(baseline_scenario)
scenario_result = default_post_processor.run
scenario_result.save
```

This `defaults_post_processor` aggregate feature reports into scenario level results by leveraging methods of the [default_reports](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/default_reports.rb) file. This file includes classes that are developed in the default_reports [folder](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/urbanopt/scenario/default_reports). Each of these classes corresponds to a component in the deffault_reports [schema](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/default_reports/schema/scenario_schema.json). This schema describes all the main components of the default reports (reporting_period, program, constructon_cost, etc.) and their attributes. Figure 3<!--add figure 'PostProcessor_code_architecture.jpg' -->, decribes the architecture of the Postprocessor, illustrating the higherarchy of the classes and main methods which are leveraged by this PostProcessor to aggregate feature reports into a scneario report. However, advanced users should also refer to [Scenario documentation](#Advanced-Usage) which include the schema and Rdocs describing all methods and classes used to aggregate the properties of a feature report. Users can edit these methods or add new methods that extend or customize the PostProcessor functionality(e.g. reporting and aggregating new properties of interest).