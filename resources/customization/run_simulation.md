---
layout: default
title: Details of a simulation run
parent: Customization
grand_parent: Resources
nav_order: 10
---

To modify or build your own gem that generates OpenStudio energy simulation results, here are the details of our code.

The first code block below creates and runs a `baseline_scenario` parser for OpenStudio results. It also integrates REopt Lite settings for analysis during post processing. The example is set up so that the **REopt™ Lite** analysis will use assumptions defined in a `reopt/base_assumptions.json` file for a scenario level analysis, as well as for subsequent individual site analyses at the feature level. 

Inputs to this function are defined as follows:

- `name` : name of Scenario
- `root_dir` = Project directory (the one you created using `uo -p`)
- `run_dir`: Directory to run and save results
- `feature_file`: Path for `geojson` file (for example: `example_project.json`)
- `mappers_files_dir`: File path for `base_workflow.osw`
- `csv_file`: `csv` ScenarioFile with a column for mapper class and column for REopt assumptions file (stored within the _reopt_files_dir_ folder). For an example see `baseline_scenario.csv` which is reproduced in part below.

  | Feature Id | Feature Name | Mapper Class                       | REopt Assumptions     |
  |------------|--------------|------------------------------------|-----------------------|
  | 1          | Mixed_use 1  | URBANopt::Scenario::BaselineMapper | base_assumptions.json |
  | 2          | Lodging 1    | URBANopt::Scenario::BaselineMapper | base_assumptions.json |

- `reopt_files_dir`: File path for a folder containing `.json` files for defining _non-default_ **REopt Lite** analysis assumptions. All assumption files **MUST** be saved in this folder. Note this folder is _optional_; if base assumptions are not provided for a feature, then defaults will be used as defined in [https://developer.nrel.gov/docs/energy-optimization/reopt-v1/](https://developer.nrel.gov/docs/energy-optimization/reopt-v1/). If default assumptions are used utility charges will consist of $0.13/kWh without demand charges.
- `scenario_reopt_assumptions_file_name`: Base **REopt Lite** assumption `.json` file within the _reopt_files_dir_ folder to use if running an optimization for an aggregated scenario.

```ruby
def self.run_func
    name = File.basename(@scenario_file_name, File.extname(@scenario_file_name))
    run_dir = File.join(@root_dir, 'run', name.downcase)
    csv_file = File.join(@root_dir, @scenario_file_name)
    featurefile = File.join(@root_dir, @feature_name)
    mapper_files_dir = File.join(@root_dir, 'mappers')
    reopt_files_dir = File.join(@root_dir, 'reopt/')
    num_header_rows = 1
    reopt_files_dir_contents_list = Dir["#{reopt_files_dir}/*"]
    reopt_assumptions_filename = File.basename(reopt_files_dir_contents_list[0])

    if @feature_id
        feature_run_dir = File.join(run_dir, @feature_id)
        # If run folder for feature exists, remove it
        FileUtils.rm_rf(feature_run_dir) if File.exist?(feature_run_dir)
    end

    feature_file = URBANopt::GeoJSON::GeoFile.from_file(featurefile)
    scenario_output = URBANopt::Scenario::REoptScenarioCSV.new(name, @root_dir, run_dir, feature_file, mapper_files_dir, csv_file, num_header_rows, reopt_files_dir, reopt_assumptions_filename)
    scenario_output
end
```
