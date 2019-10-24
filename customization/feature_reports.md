---
layout: default
title: Feature Reports
parent: Customization
nav_order: 3
---
## Feature Reports

The Scenario Post Processor requires feature reports to aggregate results from feature simulations. A reporting measure is used to query and report specific output data from an Openstudio simulation of each feature. The current default reporting measure is the [default_feature_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports). This measure writes a `default_feature_reports.json` file containing information on all features in the simulation. It also writes a `default_feature_reports.csv` containing timeseries data for all the features.

Users can create their own OpenStudio reporting measure to generate customized simulation reports. For example, users can request results for different reporting frequencies or query and report additional outputs that are important for their own projects; e.g. reporting specific construction costs.  Users should refer to the current reporting measure and this [reporting measure writing guide](http://nrel.github.io/OpenStudio-user-documentation/reference/measure_writing_guide/#reporting-measures) to customize the current `measure.rb` file in [default_feature_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports) or create a new reporting measure.

In this default reporting measure, a feature report object is instantiated. Then information is retrieved from the Openstudio model and stored in this `feature_report`:

````ruby
feature_report = URBANopt::Scenario::DefaultReports::FeatureReport.new
feature_report.id = feature_id
feature_report.feature_type = feature_type
feature_report.timesteps_per_hour = model.getTimestep.numberOfTimestepsPerHour
````

Methods to query results from the output EnergyPlus sql file are also created and used to retrieve results and add them to the corresponding properties in the `feature_report`. In the below example the sql_query method and convert_units method are used to query total_site_energy, convert its units to kBtu and then assign it to the total_site_energy attribute in the feature report:

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

After assigning all the required results to their feature report attributes, the feature_report is converted to a hash using the `to_hash` method and saved as `default_feature_reports.json` file. This JSON file is saved in a deafult_feature_reort folder within the run directory for a feature.

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

User can then add any new reporting measure to the OpenStudio `.osw` file, as described [here](#Adding-your-own-measures), and re-run the simulation.
The current measure added to the baseline.osw is the `default_feature_reports`:

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

The `DefaultPostProcessor` reads these feature reports and aggregates them to create a `ScenarioReport`.

### Example: post-processing an added feature report attribute

Lets now try a short example where a new attribute "number of ocuppants" is to be added to the feature_report and to the post_processor.

Below are the steps for this process:

- clone the scenario-gem repository to your local machine
- open the [schema](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/default_reports/schema/scenario_schema.json) file and append the new property "number of occupants" to the properties inside the program component.

```JSON
"Program": {
  "type": "object",
  "properties": {
    "number_of_occupants": {
    "type": "number"
    }
  }
}
```

- go to the [program](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/default_reports/program.rb) class in the [default_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/urbanopt/scenario/default_reports) and add the following:

1. Add `number_of_occupants` to the attribute accesor.

```ruby
  attr_accessor :site_area,......., :number_of_occupants
```

2. Initialize an instance variable `number_of_occupants` by adding it to the initialize method.

```ruby
  def initialize(hash = {})
  ......
    @number_of_occupants = hash[:number_of_occupants]
  ......
  end
```

3. Add a default value for `number_of_occupants` in the defaults method:

```ruby
  def defaults
    hash = {}
    ......
    hash[:number_of_occupants] = nil
    return hash
  end
```

4. Add `number_of_occupants` to the to_hash method.

```ruby
def to_hash
  result = {}
  ......
  result[:number_of_occupants] = @number_of_occupants if @number_of_occupants
  return result
end
```

5. Finally, add `number_of_occupant` to the add_program method that is used by the post processor to aggregate the program attributes values.

```ruby
def add_program(other)
  @number_of_occupants = add_values(@number_of_occupants, other.number_of_occupants)
end
```

- Now let's go to the reporting measure and request the number of occupants from the OpenStudio model and store it in the feature_report. `number of occupants` should be implemented in the `run` method, after the intialization of feature_report:

``` ruby
def run(runner, user_arguments)
  super(runner, user_arguments)
  ....
  # number of occupants. Note: 'building' is an object retrieved from openstudio model
  num_occupants = building.numberOfPeople
  feature_report.program.number_of_occupants = num_occupants
```

- Run the new example project using the modified scenario gem reposiory.<!--TODO explain this process (how to do this by editing the gem file) in the first section. Refer to the first section explaination here >