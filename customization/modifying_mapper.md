---
layout: default
title: Modifying Mapper Classes
parent: Customization
nav_order: 2
---

The Simulation Mapper Class maps the Features in the FeatureFile to arguments required as simulation inputs in the OpenStudio workflow files (OSW).

Recall that a Feature refers to a single object in a district energy analysis, such as a
building, district system, or transformer. The FeatureFile includes all data for all
Features and is written by a third party user interface; for our example we use the
GeoJSON format. The input GeoJSON file must be valid and must meet the additional requirements for data supported or required by the appropriate URBANopt GeoJSON Gem [sub-schema](https://github.com/urbanopt/urbanopt-geojson-gem/tree/develop/lib/urbanopt/geojson/schema). In version 0.1.0, the Simulation Mapper only supports mapping the
[*Building*](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/building.rb)
feature_type and has the capability to support mapping the following building types from
the
[building_properties](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/building_properties.json)
schema in the GeoJSON Gem:

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

**Note - The Mixed use building type can accomodate up to 4 building types and their corresponding fractions of total area. If the number of building types is less than 4, non-existent building use types must be added but the fraction of total area can be entered as 0.*

Within the Mapper Classes, functionality can be added to
apply measures based on the Feature properties. For example, Measures can be implemented only for certain building type, or can be implemented only if the number of floors is greater than a particular number, etc.

The URBANopt GeoJSON Example Project includes a default Simulation Mapper Class to translate an URBANopt GeoJSON Feature to an OpenStudio Model. The `HighEfficiency` mapper class inherits from the `BaselineMapper` class and can override measures that were skipped in the `BaselineMapper`.

When the Scenario is run, a new Simulation Mapper instance is created and the `create_osw` method is implemented for each Feature assigned to the Simulation Mapper Class in the Scenario CSV.

The default Simulation Mapper Class can be used directly, extended, or modified. Alternatively, a completely different Simulation Mapper Class [can be created](new_mapper_class.md).

When adding additional Measures to the OpenStudio workflow file, the Simulation Mapper Class would need to be modified to add any necessary Feature properties from the FeatureFiles and mapping them to new Measure arguments.

The *`OpenStudio::Extension.set_measure_argument`* method sets the Feature property from the FeatureFile as simulation input, passing the Measure and argument name and the corresponding Feature property from the FeatureFile as arguments.

For example:

```ruby
OpenStudio::Extension.set_measure_argument(osw, 'urban_geometry_creation', 'feature_id', feature_id)
```

Where `'urban_geometry_creation'` is the measure name, `'feature_id'` is the argument name and `feature_id` is the Feature property from the FeatureFile.

To add custom Measures, or Measures that lie outside of the ruby gems specified in the Gemfile, add the following line (which specifies the file path of the new Measures) to the Mapper Class: `@@osw[:measure_paths] << File.join(File.dirname(__FILE__), '../new_measure_folder/')`  
This adds the `measure_path` to the OpenStudio workflow file.
