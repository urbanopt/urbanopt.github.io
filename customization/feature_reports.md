---
layout: default
title: Feature Reports
parent: Customization
nav_order: 5
---

The Scenario Post-Processor requires Feature reports to aggregate results from Feature simulations. A reporting Measure is used to query and report specific output data from an Openstudio simulation of each Feature. The current default reporting Measure is the [default_feature_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports). This Measure writes a `default_feature_reports.json` file containing information on all Features in the simulation. It also writes a `default_feature_reports.csv` containing timeseries data for all the Features.

Users can create their own OpenStudio reporting Measure to generate customized simulation reports. Users can request results for different reporting frequencies or query and report additional outputs that are important for their own projects; e.g. reporting specific construction costs.  Users should refer to the current reporting Measure and this [reporting measure writing guide](http://nrel.github.io/OpenStudio-user-documentation/reference/measure_writing_guide/#reporting-measures) to customize the current `measure.rb` file in [default_feature_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports) or create a new reporting Measure.

In this default reporting Measure, a Feature report object is instantiated. Then information is retrieved from the Openstudio model and stored in this `feature_report`:

````ruby
feature_report = URBANopt::Scenario::DefaultReports::FeatureReport.new
feature_report.id = feature_id
feature_report.feature_type = feature_type
feature_report.timesteps_per_hour = model.getTimestep.numberOfTimestepsPerHour
````

Methods to query results from the output EnergyPlus sql file are also created and used to retrieve results and add them to the corresponding properties in the `feature_report`. In the example below, the `sql_query` and `convert_units` methods are used to query total_site_energy, convert units to kBtu and assign it to the total_site_energy attribute in the Feature report:

```ruby
# sql_query method
def sql_query(runner, sql, report_name, query)
  val = nil
  result = sql.execAndReturnFirstDouble("SELECT Value FROM TabularDataWithStrings WHERE ReportName='#{report_name}' AND #{query}")
  if result.empty?
    runner.registerWarning("Query failed for #{report_name} and #{query}")
  else
    begin
      val = result.get
    rescue StandardError
      val = nil
      runner.registerWarning('Query result.get failed')
    end
  end
  val
end

# unit conversion method to apply unit conversion
def convert_units(value, from_units, to_units)
  value_converted = OpenStudio.convert(value, from_units, to_units)
  if value_converted.is_initialized
    value = value_converted.get
  else
    @runner.registerError("Was not able to convert #{value} from #{from_units} to #{to_units}.")
    value = nil
  end
  return value
end

# query total_site_energy from EnergyPlus sql file and add it to the feature reports
total_site_energy = sql_query(runner, sql_file, 'AnnualBuildingUtilityPerformanceSummary', "TableName='Site and Source Energy' AND RowName='Total Site Energy' AND ColumnName='Total Energy'")
feature_report.reporting_periods[0].total_site_energy = convert_units(total_site_energy, 'GJ', 'kBtu')
```

After assigning all the required results to their Feature report attributes, the feature_report is converted to a hash using the `to_hash` method and saved as `default_feature_reports.json` file. This JSON file is saved in a deafult_feature_reort folder within the run directory for a feature.

```ruby
# Converts to a Hash equivalent for JSON serialization
feature_report_hash = feature_report.to_hash

# save the 'default_feature_reports.json' file
File.open('default_feature_reports.json', 'w') do |f|
  f.puts JSON.pretty_generate(feature_report_hash)
  # make sure data is written to the disk one way or the other
  begin
    f.fsync
  rescue StandardError
    f.flush
  end
end
```

Users can then add any new reporting Measure to the OpenStudio workflow file, as described in the [Adding your own Measures](adding_own_measure.md) section, and re-run the simulation. The current Measure added to the `base_workflow.osw` is the `default_feature_reports`:

```json
{
      "measure_dir_name": "default_feature_reports",
      "arguments": {
        "feature_id": null,
        "feature_name": null,
        "feature_type": null
      }
}
```

The `DefaultPostProcessor` reads these Feature reports and aggregates them to create a `ScenarioReport`.
