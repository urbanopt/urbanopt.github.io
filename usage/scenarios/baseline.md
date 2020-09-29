---
layout: default
title: Baseline Scenario
parent: Scenarios
nav_order: 1
---

The Baseline scenario is defined by applying the baseline MapperClass to each feature in the FeatureFile. The Baseline MapperClass includes measures that create the basic required inputs for an OpenStudio model and characterize it with baseline energy properties.

## Measures
The OpenStudio measures used in the Baseline Mapper are as follows: 

- **set_run_period**: An OpenStudio Measure used to define the number of timesteps per hour and specify the start and end date for running the simulation.
- **ChangeBuildingLocation**: An OpenStudio Measure to specify and load the energyPlusWeather file.
- **create_bar_from_building_type_ratios**: An OpenStudio Model Articulation Measure used to create a core and perimeter bar sliced by space type. It takes in one or more building types as user arguments to create space type collections.
- **create_typical_building_from_model 1**: An OpenStudio Model Articulation Measure that creates a custom prototype building with user-defined geometry and assigns constructions, schedules, internal loads, HVAC and other loads such as exterior lights and service water heating based on the space and sub space types. The `add_hvac` is set to `false` by default in this measure because it gets handled later in the process to account for blended space types.
- **blended_space_type_from_mode**: An OpenStudio Model Articulation Measure that is used to create a single space type that represents the loads and schedules of a collection of space types. It removes all previous space type assignments and hard assigns internal loads from spaces included in the building floor area. A blended space type will be created from the original internal loads and assigned at the building level.
- **urban_geometry_creation_zoning**: An URBANopt GeoJSON measure that is used to create extruded geometry for building features from
  GeoJSON coordinates with core and perimeter zoning, it can also account for shading from
  surrounding buildings.
- **create_typical_building_from_model 2**: A second instance of this Measure, which is added in the workflow after urban geometry creation and the `add_hvac` argument is now set to `true`, to add HVAC system for the blended space types. The rest of the arguments for adding constructions, space type, loads, etc. are set to `false`.
- **default_feature_reports**: An URBANopt Scenario Measure that generates feature reports (JSON and CSV files that include feature results), which are used by the scenarioDefaultPostProcessor.