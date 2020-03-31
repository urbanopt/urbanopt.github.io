---
layout: default
title: REopt Post Processing
parent: REopt
nav_order: 2
---
## Intro

Rake tasks are used to run and post-process each scenario a user defines. **REopt Lite** optimization happens during the post-processing of each scenario, and is facilitated using a _**REoptPostProcessor**_ that can be run on a _**FeatureReport**_, a collection of _**Feature Reports**_, or a _**Scenario Report**_ (either on the aggregate load or on each _**FeatureReport**_ load profile before aggregation). **REopt Lite** non-default assumptions (i.e. utility rate, capital costs) are specified in JSON files that are either loaded from input CSV's for _**FeatureReports**,_ by a  _**URBANopt::Scenario::REoptScenarioCSV**_, or specified within the scenario Rake task (as shown below). 

## Workflow

#### Setting up a new scenario
The first code block below creates and runs a `baseline_scenario` parser for OpenStudio results. It also integrates REopt Lite settings for analysis during post processing. The example is set up so that the **REopt Lite** analysis will use assumptions defined in a `reopt/base_assumptions.json` file for a scenario level analysis, as well as for subsequent individual site analyses at the feature level. 

The inputs to this rake task are defined as follows:

- `name` : name of the Scenario
- `run_dir`: the directory to run and save results
- `feature_file_path`: the  path for the `geojson` file (for example: `example_project.json`)
- `csv_file`: `csv` file formatted with a column for each feature's mapper class and column for the name of the REopt assumptions file to use within the _reopt_files_dir_ folder (For an example see `baseline_scenario.csv` which is reproduced in part below). Note the REopt Assumptions column is ordered fourth from the left, and contains just the name of the assumptions file to use. The actual folder in which the assumptions file is saved is defined in the _reopt_files_dir_ parameter (which is required if any assumptions will be used for Scenario or Feature Reports).

  | Feature Id | Feature Name | Mapper Class                       | REopt Assumptions     |
  |------------|--------------|------------------------------------|-----------------------| 
  | 1          | Mixed_use 1  | URBANopt::Scenario::BaselineMapper | base_assumptions.json | 

- `mappers_files_dir`: the file path for the `base_workflow.osw`
- `reopt_files_dir`: the file path for a folder containing .json files for defining non-default **REopt Lite** analysis assumptions. All assumption files must be saved in this folder. Note this folder is optional; if base assumptions are not provided for a feature, then defaults will be used as defined in [https://developer.nrel.gov/docs/energy-optimization/reopt-v1/](https://developer.nrel.gov/docs/energy-optimization/reopt-v1/). If default assumptions are used utility charges will consist of $0.13/kWh without demand charges.
- `scenario_reopt_assumptions_file_name`: the base **REopt Lite** assumption .json file within the _reopt_files_dir_ folder to use if running an optimization for an aggregated scenario.

````ruby
def baseline_scenario
  name = 'Baseline Scenario'
  run_dir = File.join(File.dirname(__FILE__), 'run/baseline_scenario/')
  feature_file_path = File.join(File.dirname(__FILE__), 'example_project.json')
  csv_file = File.join(File.dirname(__FILE__), 'baseline_scenario.csv')
  mapper_files_dir = File.join(File.dirname(__FILE__), 'mappers/')
  reopt_files_dir = File.join(File.dirname(__FILE__), 'reopt/')
  scenario_reopt_assumptions_file_name = 'base_assumptions.json'
  num_header_rows = 1

  feature_file = URBANopt::GeoJSON::GeoFile.from_file(feature_file_path)
  scenario = URBANopt::Scenario::REoptScenarioCSV.new(name, root_dir, run_dir, feature_file, mapper_files_dir, csv_file, num_header_rows, reopt_files_dir, scenario_reopt_assumptions_file_name)
  return scenario

end
````

##### Assumptions JSON Files

Assumption files are they key to customizing your distributed energy resource sizing assumptions. If all buildings have the same techno-economic assumptions (i.e. utility rate tariff, solar array type and orientation, capital costs) you may choose to associate the same assumptions file with all buildings.

To establish unique parameter sets for each building, or groups of buildings, create multiple assumptions files then associate them with the features in the CSV file as described above.

###### Multiple PV Analysis
 Note: **REopt Lite** accepts either a single set of PV assumptions or a list of assumptions in the Scenario>Site>PV parameter. If you choose to enter a list (i.e. separate PV's for roof vs solar, and/or roof aspects) then you can give each PV setting a _pv_name_ and restrict it to either site level "roof" or "ground" area restrictions. By default a PV is constrained by both roof and land area if these site level attributes are available.

For a example of an assumptions file making use of multiple PV systems, see the (REopt Example Project)[https://github.com/urbanopt/urbanopt-example-reopt-project/blob/52d9e237053d3b9bc818b407c808948dd139b8b6/reopt/multiPV_assumptions.json#L49].

#### Running a scenario and post processing with REopt Lite

The tasks described below are in the Rakefile. To list all available tasks in the Rakefile:

```terminal
bundle exec rake -T
```

#### *run_all*

{: .no-toc }

The `run_all` Rake task creates and runs a `ScenarioRunnerOSW` for each Scenario e.g. the
*Baseline*, *HighEfficiency*, and *Mixed* Scenarios, passing the Scenario method as an argument.

- `run_baseline`, `run_high_efficiency` and `run_mixed` rake tasks can be used for running individual Scenarios.

Note, this only runs the initial OpenStudio simulation. It does not run the REopt Lite analysis.

#### *post_process_all*

{: .no-toc }

This task "post-processes" a scenario by aggregating the _**Feature reports**_ into results summarizing the whole scenario, then runs **REopt Lite** post-processing as defined in the post-process function (i.e. `post_process_baseline`). Accordingly, each post-processing rake task creates and runs _**ScenarioDefaultPostProcessor**_ then subsequently a _**REoptPostProcessor**_ for each scenario and saves the results to a file on disk.

When **REopt Lite** is run on a _**ScenarioReport**_ or _**FeatureReport**_ the object's `distributed_generation` attributes (including system financial and sizing attributes) are updated as shown in an example below. 

```
  "distributed_generation": {
        "lcc_us_dollars": 100000000.0,
        "npv_us_dollars": 10000000.0,
        "year_one_energy_cost_us_dollars": 7000000.0,
        "year_one_demand_cost_us_dollars": 3000000.0,
        "year_one_bill_us_dollars": 10000000.0,
        "total_energy_cost_us_dollars": 70000000.0,
        "total_solar_pv_kw": 30000.0,
        "total_wind_kw": 0.0,
        "total_generator_kw": 0.0,
        "total_storage_kw": 2000.0,
        "total_storage_kwh": 5000.0,
        "solar_pv": [{
          "size_kw": 30000.0
        }],
        "wind": [{
          "size_kw": 0.0
        }],
        "generator": [{
          "size_kw": 0.0
        }],
        "storage": [{
          "size_kw": 2000.0,
          "size_kwh": 5000.0
        }]
      }
```

Moreover, the following optimal dispatch fields are added to its `timeseries CSV`.

|            new column name                        |  unit  |
| --------------------------------------------------| ------ |
| REopt:ElectricityProduced:Total(kW)               | kW     |
| REopt:Electricity:Load:Total(kW)                  | kW     |
| REopt:Electricity:Grid:ToLoad(kW)                 | kW     |
| REopt:Electricity:Grid:ToBattery(kW)              | kW     |
| REopt:Electricity:Storage:ToLoad(kW)              | kW     |
| REopt:Electricity:Storage:ToGrid(kW)              | kW     |
| REopt:Electricity:Storage:StateOfCharge(kW)       | kW     |
| REopt:ElectricityProduced:Generator:Total(kW)     | kW     |
| REopt:ElectricityProduced:Generator:ToBattery(kW) | kW     |
| REopt:ElectricityProduced:Generator:ToLoad(kW)    | kW     |
| REopt:ElectricityProduced:Generator:ToGrid(kW)    | kW     |
| REopt:ElectricityProduced:PV:Total(kW)            | kW     |
| REopt:ElectricityProduced:PV:ToBattery(kW)        | kW     |
| REopt:ElectricityProduced:PV:ToLoad(kW)           | kW     |
| REopt:ElectricityProduced:PV:ToGrid(kW)           | kW     |
| REopt:ElectricityProduced:Wind:Total(kW)          | kW     |
| REopt:ElectricityProduced:Wind:ToBattery(kW)      | kW     |
| REopt:ElectricityProduced:Wind:ToLoad(kW)         | kW     |
| REopt:ElectricityProduced:Wind:ToGrid(kW)         | kW     |


**NOTE**: A REopt Lite solution may contain multiple PV systems. In this case the aggregate generation from all PV systems will be reported in the PV columns.


The following shows how to post-process a ScenarioReport in aggregate. This is suitable for community-scale optimizations. 

````ruby
  puts 'Post Processing Baseline Scenario...'
  default_post_processor = URBANopt::Scenario::ScenarioDefaultPostProcessor.new(baseline_scenario)
  scenario_report = default_post_processor.run
  scenario_report.save
  scenario_base = default_post_processor.scenario_base
  reopt_post_processor = URBANopt::REopt::REoptPostProcessor.new(scenario_report, scenario_base.scenario_reopt_assumptions_file, scenario_base.reopt_feature_assumptions, DEVELOPER_NREL_KEY)

  # Run Aggregate Scenario
  scenario_report_scenario = reopt_post_processor.run_scenario_report(scenario_report: scenario_report, save_name: 'baseline_global_optimization')
````

Alternatively, the following shows how to post-process a Scenario Report in for each of its Feature Reports before aggregating into a summary in the Scenario Report.

````ruby
  puts 'Post Processing Baseline Scenario...'
  default_post_processor = URBANopt::Scenario::ScenarioDefaultPostProcessor.new(baseline_scenario)
  scenario_report = default_post_processor.run
  scenario_report.save
  scenario_base = default_post_processor.scenario_base
  reopt_post_processor = URBANopt::REopt::REoptPostProcessor.new(scenario_report, scenario_base.scenario_reopt_assumptions_file, scenario_base.reopt_feature_assumptions, DEVELOPER_NREL_KEY)

  # Run features individually - this is an alternative approach to the previous step, in your analysis depending on project ojectives you maye only need to run one
  scenario_report_features = reopt_post_processor.run_scenario_report_features(scenario_report: scenario_report, save_names_feature_reports: ['baseline_local_optimization']*scenario_report.feature_reports.length, save_name_scenario_report: 'baseline_local_optimization')
````


#### *update_all*

{: .no-toc }

This rake task combines the `run_all` and `post_process_all` tasks.

#### *default*

{: .no-toc }

This runs the `update_all` rake task.

#### *clear_all*

{: .no-toc }

This rake task clears (deletes) the Scenario results from any previous runs.

- `clear_baseline`, `clear_high_efficiency`, `clear_mixed` rake tasks can be used for
  individual Scenarios.

The figure below describes the workflow that takes place on  implementing the *run* and *post_process* rake tasks.

![workflow_diagram](../doc_files/reopt-workflow-diagram.png)


The following figure represents how Simulation Mapper Classes can be assigned to different
Features from the FeatureFile in the Scenario CSV.

![scenario_mapper](../doc_files/reopt-scenario-mapper.png)
