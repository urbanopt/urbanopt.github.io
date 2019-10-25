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
- Mixed use*

**Note- The Mixed use building type, requires specifying 4 building  types and their
corresponding fractions of total area. In case number the building  types is less than 4, the
fraction of total area for the remaining building use types can be entered as 0.*

Within the Mapper Classes, functionality can be added to
apply measures based on the Feature properties. For example,  measures can be implemented
only for  certain building type, or can be implemented only if the number of floors is
greater than a particular number etc.

The URBANopt GeoJSON Example Project includes a default Simulation Mapper Class to
translate an URBANopt GeoJSON Feature to an OpenStudio Model. The `HighEfficiency` mapper
class inherits from the `BaselineMapper` class and can override measures that were
skipped in the `BaselineMapper`. 

When the Scenario is run, a new Simulation Mapper instance is created and the `create_osw` method is implemented for each Feature assigned to the Simulation Mapper Class in the Scenario CSV.

The default Simulation Mapper Class can be used directly, extended or modified, or a completely different Simulation Mapper Class can be created.

On adding additional measures to the `baseline.osw` file, the Simulation Mapper Class would need to be extended by adding necessary features from the feature files and mapping them to measure arguments.

The *`OpenStudio::Extension.set_measure_argument`* method sets the feature from the feature file as simulation input, passing the measure and argument name and the corresponding feature property from the feature file as arguments.

For example:

```ruby
OpenStudio::Extension.set_measure_argument(osw, 'urban_geometry_creation', 'feature_id', feature_id)
```

Where `'urban_geometry_creation'` is the measure name, `'feature_id'` is the argument name and `feature_id` is the feature property from the feature file.

To add custom measures, or measures that lie outside of the ruby gems specified in the Gemfile, `@@osw[:measure_paths] << File.join(File.dirname(__FILE__), '../new_measure_folder/')` can be added in the Mapper Class, specifying the file path of the new measures. This adds the `measure_path` in the `baseline.osw`.

### Creating a new Mapper Class

- A new mapper class can be created to inherit from the existing `BaselineMapper`. For
  example, a `MediumEfficiency` mapper class can be created to override some of the
  measure arguments from the `BaselineMapper` in the `create_osw` method. 
- The new mapper class can be assigned to features by adding the mapper class name in the
  scenario csv.

To create a completely new mapper class and scenario CSV: 

- The new mapper class ruby file should be created in the Mappers folder, since the
  `mapper_files_dir` in the `Rakefile` is directed here. The default `BaselineMapper`
  Class can be used as a template, and the `osw_path` would need to be updated as per the
  name of the new `osw` file. 
- The new scenario CSV can be created in the root folder, and the mapper class name should be added in the mapper class column. The existing scenario csv's can be used as reference.
- OpenStudio Measures can be added to the new `osw` file by adding the measure directory
  and measure arguments. In the mapper class features properties from the feature file
  should be mapped to the corresponding measure arguments.
- A new method can be created in the `Rakefile` to call the Mapper Class. The `mapper_files_dir` and `csv_file paths` should correspond to the newly created Mapper Class and csv file paths respectively.
- To implement the mapper class, a new Rake task can be created that creates `ScenarioRunnerOSW` and runs it passing the new method as the argument.
