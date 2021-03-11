---
layout: default
title: Residential Workflows
parent: Usage
has_children: true
nav_order: 4
has_toc: false
---

# Residential Workflows

Low-rise residential building energy models in URBANopt<sup>&trade;</sup> are created using the [OpenStudio-HPXML](https://github.com/NREL/OpenStudio-HPXML) workflow.
For every residential building feature found in the geojson file, an [HPXML](https://hpxml.nrel.gov) file is built to represent each living unit of the building.
For example, in the case of a single-family detached building one HPXML file is built to represent the single unit.
HPXML files are built based on feature information contained in the geojson file as well as on sets of default assumptions contained in the following [lookup files](https://github.com/urbanopt/urbanopt-example-geojson-project/tree/develop/example_project/mappers/residential):

* clothes_dryer.tsv
* clothes_washer.tsv
* cooling_system.tsv
* dishwasher.tsv
* enclosure.tsv
* heat_pump.tsv
* heating_system.tsv
* mechanical_ventilation.tsv
* exhaust.tsv
* refrigerator.tsv
* water_heater.tsv

Argument values found in these lookup files span across the following categories:

* enclosure (insulation levels, air leakage, etc.)
* HVAC systems (heating/cooling types, efficiencies, etc.)
* appliances (refrigerator, clothes washer, etc.)
* ventilation (mechanical, exhaust)
* water heater

(See [below](#customizable-template) for more information on controlling how these assumptions are made.)

A translator measure is then applied to the HPXML file to constuct an OpenStudio(R) building model.

## Supported Building Types

Currently, the following residential building types are supported:

- [Single-Family Detached](single_family_detached.md)
- [Single-Family Attached](single_family_attached.md)
- [Low-Rise Multifamily](multifamily.md)[^1]

Only the *Baseline* and *High Efficiency* Scenarios are supported at this time; any additional mappers will need to be updated manually.

[^1]: Mid-Rise and High-Rise Multifamily building prototypes can be found in the commercial building workflows).

## Customizable Template

An optional template enumeration may be specified for each feature in the geojson.
Enumerations that are applicable to residential buildings:

- `Residential IECC 2006 - Customizable Template Sep 2020`
- `Residential IECC 2009 - Customizable Template Sep 2020`
- `Residential IECC 2012 - Customizable Template Sep 2020`
- `Residential IECC 2015 - Customizable Template Sep 2020`
- `Residential IECC 2018 - Customizable Template Sep 2020`

If no template enumeration is specified, argument values will be defaulted according to the [documentation](https://openstudio-hpxml.readthedocs.io/en/latest/hpxml_to_openstudio.html) for the HPXMLtoOpenStudio translator measure.
In general, these defaults are based on ANSI / RESNET / ICC Std. 301 (2006).

All argument values for the previous categories may be customized by manually adjusting values in the lookup files.
The enumeration names include "Residential IECC 20XX" because a variety of enclosure, window, duct insulation, and whole-home air leakage assumptions are based on the different IECC model code years to illustrate how templates can be used to approximate different levels of efficiency.
Note that not all possible assumptions have been aligned with IECC requirements (e.g., see above regarding defaults), but the users can further customize these templates as needed for specific projects.

## Stochastic Schedules

Occupant-related schedules are generated on-the-fly, and vary unit-to-unit for a given building.
They are generated using time-inhomogenous Markov chains derived from American Time Use Survey data, supplemented with sampling duration and power level from NEEA RBSA data, as well as DHW draw duration and flow rate data from Aquacraft/AWWA data.

In terms of repeatability, stochastic schedule generation uses a pseudo-random number generator that takes a seed as an argument.
The seed is determined by multiplying the feature ID by the unit number of the building being simulated.
If the feature ID is not represented by an integer, the seed will default to a value of 1.

## Other Assumptions

The building footprint drawn and contained in the geojson does not determine the footprint of individual modeled units.
Floor area is divided by the number of residential units to determine the floor area of each individual unit.
Individual footprints are determined using this unit floor area and an aspect ratio of 2 (i.e., front/back walls are twice as long as left/right walls).

A summary of modeling assumptions baked into the baseline mapper is given below.
In the future, updates/improvements could be made to expose these arguments as inputs to the models.
For example, aspect ratio could be either user-defined or determined from the drawn building footprint.
Another example is allowing building orientation to be user-defined, or determining it based on the "front" of the building.

### Geometry

#### Corridor
- geometry_corridor_position = 'Double Exterior'

#### Foundations
- geometry_foundation_height = 3 ft if geometry_foundation_type is Crawlspace
- geometry_foundation_height = 8 ft if geometry_foundation_type is Basement
- geometry_foundation_height = 8 ft if geometry_foundation_type is Ambient

#### Roofs
- geometry_roof_type = gable if geometry_roof_type != 'flat'

#### Walls
- geometry_wall_height = 8 ft

#### Neighbors
- neighbor_front_distance = 0 ft
- neighbor_back_distance = 0 ft
- neighbor_left_distance = 0 ft
- neighbor_right_distance = 0 ft

#### Aspect Ratio
- geometry_aspect_ratio = 2.0 (FB/LR)

#### Orientation
- geometry_orientation = 100% South for Single-Family Detached and Single-Family Attached
- geometry_orientation = 50% South and 50% North for Low-Rise Multifamily

#### Garages
- geometry_garage_width = 12 ft if geometry_cfa <= 2500 sqft
- geometry_garage_width = 24 ft if geometry_cfa > 2500 sqft
- geometry_garage_protrusion = 100%

### Fuel Types

#### Appliances
- cooking_range_oven_fuel_type = heating_system_fuel
- clothes_dryer_fuel_type = heating_system_fuel

#### Water Heating
- water_heater_fuel_type = heating_system_fuel