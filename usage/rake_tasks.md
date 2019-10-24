---
layout: default
title: Rake Tasks
parent: Usage
nav_order: 2
---

### Rake tasks

Rake tasks are created to run and post process each of the defined scenarios. For each scenario a ScenarioCSV object is instantiated. Below is an example of defining a baseline_scenario using te following inputs:

- `name` : name of the scenario
- `run_dir`: the director to run and save results
- `feature_file_path`: The  path for the `geojson` file (for example: `industry_denver.geojson`)
- `csv_file`: `csv` file (for example: `baseline_scenario.csv`)
- `mappers_files_dir`: the file path for the `baseline.osw`**TODO-change name of baseline.osw** file and the mapper classes.

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

To list all available tasks in the Rakefile:

```terminal
bundle exec rake -T
```

Details about individual tasks:

#### *run_all*
{: .no-toc }

The `run_all` Rake task creates and runs a `ScenarioRunnerOSW` for each scenario i.e. the
*Baseline*, *HighEfficiency* and *Mixed scenario*, passing the scenario method as an argument.

- `run_baseline`, `run_high_efficiency` and `run_mixed` rake tasks can be used for running individual scenarios.

#### *post_process_all*
{: .no-toc }

The default "post-process" of a scenario aggregates the [Feature reports](#Feature-reports) into results across the whole scenario. This rake task creates and runs `ScenarioDefaultPostProcessor` for each scenario and saves the results.

- `post_process_baseline`, `post_process_high_efficiency`, `post_process_mixed` rake tasks are pre-built and can be used for those individual scenarios.

#### *update_all*
{: .no-toc }

This rake task combines the `run_all` and `post_process_all` tasks.

#### *default*
{: .no-toc }

This runs the `update_all` rake task.

#### *clear_all*
{: .no-toc }

This rake task clears the scenario results from any previous runs.

- `clear_baseline`, `clear_high_efficiency`, `clear_mixed` rake tasks can be used for individual scenarios.