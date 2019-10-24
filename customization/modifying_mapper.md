---
layout: default
title: Modifying Mapper Classes
parent: Customization
nav_order: 2
---

## Modifying Mapper Classes

The Simulation Mapper Class derives from the [SimulationMapperBase](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/simulation_mapper_base.rb) class. It maps the features in the feature file to arguments required as simulation inputs in the `baseline.osw`.

Recall that a feature refers to a single object in a district energy analysis, such as a building, district system, or transformer. The feature file includes all data for all the features and is written by a third party user interface; for our example we use the GeoJSON format. Currently, the Simulation Mapper only supports mapping the [*Building*](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/building.rb) feature_type. It has the capability to support the mapping of the following building types from the  [building_properties](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/building_properties.json) schema in the GeoJSON Gem:

- Multifamily (5 or more units)
- Multifamily (2 to 4 units)
- Single-Family
- Office
- Outpatient health care
- Inpatient health care
- Lodging
- Food service
- Strip shopping mall
- Retail other than mall
- Education
- Nursing
- Mixed use

The URBANopt GeoJSON Example Project includes a default Simulation Mapper Class to translate an URBANopt GeoJSON Feature to an OpenStudio Model. The `HighEfficiency` mapper class inherits from the `BaselineMapper` class and can override measures that were skipped in the `BaselineMapper`.

When the Scenario is run, a new Simulation Mapper instance is created and the `create_osw` method is implemented for each Feature assigned to the Simulation Mapper Class in the Scenario CSV.

The default Simulation Mapper Class can be used directly, extended or modified, or a completely different Simulation Mapper Class can be created.

On adding additional measures to the `baseline.osw` file, the Simulation Mapper Class would need to be modified by adding necessary features from the feature files and mapping them to measure arguments.

The *`OpenStudio::Extension.set_measure_argument`* method sets the feature from the feature file as simulation input, passing the measure and argument name and the corresponding feature property from the feature file as arguments.

For example:

```ruby
OpenStudio::Extension.set_measure_argument(osw, 'urban_geometry_creation', 'feature_id', feature_id)
```

Where `'urban_geometry_creation'` is the measure name, `'feature_id'` is the argument name and `feature_id` is the feature property from the feature file.

To add custom measures, or measures that lie outside of the ruby gems specified in the Gemfile, `@@osw[:measure_paths] << File.join(File.dirname(__FILE__), '../new_measure_folder/')` can be added in the Mapper Class, specifying the file path of the new measures. This adds the `measure_path` in the `baseline.osw`.

To create a new mapper class:

- The new mapper class ruby file should be created in the Mappers folder, since the `mapper_files_dir` in the `Rakefile` is directed here. The default Simulation Mapper Class can be used as template, and the `osw_path` would need to be updated as per the name of the new `osw` file.
- A new scenario CSV should be created in the root folder, and the mapper class name should be updated with the name of the new mapper class,  within the mapper class column. The existing scenario csv's can be used as reference.
- OpenStudio Measures can be added to the new `osw` file by adding the measure directory and measure arguments, and adding the features from the feature file and mapping them to the corresponding arguments in the Mapper Class.
- A new method can be created in the `Rakefile` to call the Mapper Class. The `mapper_files_dir` and `csv_file paths` should correspond to the newly created Mapper Class and csv file paths respectively.
- To implement the mapper class, a new Rake tasks can be created that creates `ScenarioRunnerOSW` and runs it passing the new method as the argument.

The Scenario PostProcessor aggregates results from each Feature simulation. They require specific OpenStudio Reporting Measures be run for each Feature to generate required simulation reports (e.g. timeseries CSV data for specific outputs, specific metrics). Creating a new reporting measure to generate the Feature reports is described [here](#Feature-Reports). These individual simulation results should be aligned with the final desired aggregated results. For example, if users decide to customize the reporting measure to report 15min timestep results, additional methods should be developed to allow the post processors to aggregate data at this timestep.

Currently, the default Scenario Post Processors and its corresponding OpenStudio Reporting Measures are implemented in the **Scenario Gem**.  Additional Scenario Post Processors and OpenStudio Reporting Measures can be implemented in other OpenStudio Extension Gems. Users can use this gem as a guide to developing their own Post Processors or just customize this Gem to report and post-process new results of their interest. The current Scenario Processor which is used in this example project rake file is the `default_post_processor`, which is an object of ScenarioDefaultPostProcessor class.

```ruby
default_post_processor = URBANopt::Scenario::ScenarioDefaultPostProcessor.new(baseline_scenario)
scenario_result = default_post_processor.run
scenario_result.save
```

This `defaults_post_processor` aggregate feature reports into scenario level results by leveraging methods of the [default_reports](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/default_reports.rb) file. This file includes classes that are developed in the default_reports [folder](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/urbanopt/scenario/default_reports). Each of these classes corresponds to a component in the deffault_reports [schema](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/default_reports/schema/scenario_schema.json). This schema describes all the main components of the default reports (reporting_period, program, constructon_cost, etc.) and their attributes. Figure 3<!--add figure-->, decribes the architecture of the Postprocessor, illustrating the higherarchy of the classes and main methods which are leveraged by this PostProcessor to aggregate feature reports into a scneario report. However, advanced users should also refer to [Scenario documentation](#Advanced-Usage) which include the schema and Rdocs describing all methods and classes used to aggregate the properties of a feature report. Users can edit these methods or add new methods that extend or customize the PostProcessor functionality(e.g. reporting and aggregating new properties of interest).