---
layout: default
title: Feature Reports
parent: Customization
nav_order: 7
---

The URBANopt<sup>&trade;</sup> Scenario Post-Processor requires Feature reports to aggregate results from Feature simulations. A reporting Measure is used to query and report specific output data from an Openstudio simulation of each Feature. The current default reporting Measure is the [default_feature_reports](https://github.com/urbanopt/urbanopt-reporting-gem/tree/develop/lib/measures/default_feature_reports). This Measure writes a `default_feature_reports.json` file containing information on all Features in the simulation. It also writes a `default_feature_reports.csv` containing timeseries data for all the Features.

Users can create their own OpenStudio reporting Measure to generate customized simulation reports. Users can request results for different reporting frequencies or query and report additional outputs that are important for their own projects(e.g. reporting specific construction costs). Users should refer to the current reporting Measure and this [reporting measure writing guide](http://nrel.github.io/OpenStudio-user-documentation/reference/measure_writing_guide/#reporting-measures) to customize the current `measure.rb` file in [default_feature_reports](https://github.com/urbanopt/urbanopt-reporting-gem/tree/develop/lib/measures/default_feature_reports) directory or create a new reporting Measure.


**Initializing feature_report object and assigning information to its attributes:**

In this default reporting measure, a feature_report object is instantiated. Then information is retrieved from the Openstudio model and stored in this `feature_report`. In the example below a feature_report is initialized first. Then the openstudio model is acquired from the openstudio runner. Afterwards, the numberof timesteps per huour is retrieved from the openstudio model and assigned to the feature_report attribute (`timestep_per_hour`)

````ruby
feature_report = URBANopt::Reporting::DefaultReports::FeatureReport.new

# get the last model
model = runner.lastOpenStudioModel
model = model.get

# get timestamp from model
timesteps_per_hour = model.getTimestep.numberOfTimestepsPerHour

# assign the model's retrieved timestamp per hour to the feature_report attribute (`timestep_per_hour`)
feature_report.timesteps_per_hour = timesteps_per_hour

````


**Adding queried results to JSON feature reports:**

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

After assigning all the required results to their Feature report attributes, the feature_report is converted to a hash using the `to_hash` method and saved as `default_feature_reports.json` file. This JSON file is saved in a default_feature_reort folder within the run directory for a feature.

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

**Adding Timeseries results to CSV feature reports:**

Users can also query and add new timeseries results to CSV feature report files. [OpenStudio API](https://openstudio-sdk-documentation.s3.amazonaws.com/cpp/OpenStudio-2.9.0-doc/utilities/html/classopenstudio_1_1_sql_file.html) include methods to query timeseries data from the sql file. These methods are utilized to query timeseries reults of interest. The queried timeseries values are then converted to specified units and saved in the `default_feature_eport.csv` file. Below is a simplified example of this process where two requested timeseries names are added to the csv report (`Electricity:Facility` and `Gas:Facility`).


First, the requested timeseries names are defined. Then the `sql_file.timeSeries` and `.get.values` methods are envoked sequentially to get the timeseires values for each of the requested names (refer to [OpenStudio API](https://openstudio-sdk-documentation.s3.amazonaws.com/cpp/OpenStudio-2.9.0-doc/utilities/html/classopenstudio_1_1_sql_file.html) for further details on these methods).

In the next step the initial units are acquired using the `.get.units` methods and stored in `old_unit` variable, while the target units are specified and stored in `new_units` variable. Then unit conversion is performed using `OpenStudio.convert` method to convert all the timeseries values from the old units to the desired new units.

```ruby
# timeseries we want to report
requested_timeseries_names = ['Electricity:Facility', 'Gas:Facility']

# all numeric timeseries values, transpose of CSV file (e.g. values[key_cnt] is column, values[key_cnt][i] is column and row)
values = []
# number of values in each timeseries
key_cnt = 0

# loop over requested timeseries
requested_timeseries_names.each_index do |i|
  timeseries_name = requested_timeseries_names[i]

  # get the actual timeseries
  ts = sql_file.timeSeries(ann_env_pd.to_s, reporting_frequency.to_s, timeseries_name)
  values[key_cnt] = ts.get.values

  # unit conversion
  old_unit = ts.get.units if ts.is_initialized
  # identify the target nits for the conversion
  if timeseries_name.include? 'Gas'
    new_unit = 'kBtu'
  elsif old_units.to_s = 'J'
    new_unit = 'kWh'
  end
  # loop through each value and apply unit conversion
  os_vec = values[key_cnt]
  for i in 0..os_vec.length - 1
    unless new_unit == old_unit || !ts.is_initialized
      os_vec[i] = OpenStudio.convert(os_vec[i], old_unit, new_unit).get
    end
  end
end
```

These timeseries values can then be saved in a CSV file (`default_feature_reports.csv`) as the following:

```ruby
# Save the 'default_feature_reports.csv' file
File.open('default_feature_reports.csv', 'w') do |file|
  file.puts(final_timeseries_names.join(','))
  (0...n).each do |l|
    line = []
    values.each_index do |j|
      line << values[j][l]
    end
    file.puts(line.join(','))
  end
end
```

**Adding the reporting measure to the OSW file:**

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
