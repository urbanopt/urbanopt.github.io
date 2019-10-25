---
layout: default
title: Rake Tasks
parent: Usage
nav_order: 2
---

Rake tasks are created to run and Post-Process each of the defined Scenarios. For each Scenario a ScenarioCSV object is instantiated. Below is an example of defining a `baseline_scenario` using the following inputs:

- `name` : name of the Scenario
- `run_dir`: the directory to run and save results
- `feature_file_path`: The  path for the `geojson` file (for example: `industry_denver.geojson`)
<!-- TODO: Change this example file when new one is prepared-->
- `csv_file`: `csv` file (for example: `baseline_scenario.csv`)
- `mappers_files_dir`: the file path for the `base_workflow.osw`

````ruby
def baseline_scenario
  name = 'Baseline Scenario'
  run_dir = File.join(File.dirname(__FILE__), 'run/baseline_scenario/')
  feature_file_path = File.join(File.dirname(__FILE__), 'industry_denver.geojson')
  csv_file = File.join(File.dirname(__FILE__), 'baseline_scenario.csv')
  mapper_files_dir = File.join(File.dirname(__FILE__), 'mappers/')
  num_header_rows = 1

  feature_file = URBANopt::GeoJSON::GeoFile.from_file(feature_file_path)
  scenario = URBANopt::Scenario::ScenarioCSV.new(name, root_dir, run_dir, feature_file, mapper_files_dir, csv_file, num_header_rows)
  return scenario
end
````

The following tasks are available in the Rakefile. To list all available tasks in the Rakefile:

```terminal
bundle exec rake -T
```

#### *run_all*

{: .no-toc }

The `run_all` Rake task creates and runs a `ScenarioRunnerOSW` for each Scenario i.e. the
*Baseline*, *HighEfficiency*, and *Mixed* Scenarios, passing the Scenario method as an argument.

- `run_baseline`, `run_high_efficiency` and `run_mixed` rake tasks can be used for running individual Scenarios.

#### *post_process_all*

{: .no-toc }

This task "post-processes" a Scenario by aggregating the [Feature reports](#Feature-reports) into results across the whole Scenario. This rake task creates and runs `ScenarioDefaultPostProcessor` for each Scenario and saves the results.

- `post_process_baseline`, `post_process_high_efficiency`, `post_process_mixed` rake tasks are pre-built and can be used for those individual Scenarios.

#### *update_all*

{: .no-toc }

This rake task combines the `run_all` and `post_process_all` tasks.

#### *default*

{: .no-toc }

This runs the `update_all` rake task.

#### *clear_all*

{: .no-toc }

This rake task clears (deletes) the Scenario results from any previous runs.

- `clear_baseline`, `clear_high_efficiency`, `clear_mixed` rake tasks can be used for individual Scenarios.

![scenario_mapper](../doc_files/scenario_mapper.jpg)
