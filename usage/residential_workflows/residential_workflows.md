---
layout: default
title: Residential Workflows
parent: Usage
has_children: true
nav_order: 4
has_toc: false
---

# Residential Workflows

Low-rise residential building energy models in URBANopt<sup>&trade;</sup> are created using the [**OpenStudio-HPXML**](https://github.com/NREL/OpenStudio-HPXML) workflow.
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

See the [Customizable Template](#customizable-template) section below for more information on controlling how these assumptions are made.

A translator measure is then applied to each HPXML file to constuct an OpenStudio(R) building model.

## Supported Building Types

Currently, the following residential building types are supported:

- [**Single-Family Detached**](single_family_detached.md)
- [**Single-Family Attached**](single_family_attached.md)
- [**Low-Rise Multifamily**](multifamily.md)[^1]

Only the *Baseline* and *High Efficiency* Scenarios are supported at this time; any additional mappers will need to be updated manually.

It should be noted that modeling capabilities for these building types are currently in **beta** mode.
This means that testing and development is still in progress, and user feedback is welcome.

[^1]: Mid-Rise and High-Rise Multifamily building prototypes can be found in the commercial building workflows).

## Customizable Template

An optional template enumeration may be specified for each feature in the geojson.
Enumerations that are applicable to residential buildings:

- `Residential IECC 2006 - Customizable Template Sep 2020`
- `Residential IECC 2009 - Customizable Template Sep 2020`
- `Residential IECC 2012 - Customizable Template Sep 2020`
- `Residential IECC 2015 - Customizable Template Sep 2020`
- `Residential IECC 2018 - Customizable Template Sep 2020`

If no template enumeration is specified, argument values will be defaulted according to the [documentation](https://openstudio-hpxml.readthedocs.io/en/latest/workflow_inputs.html) for the OpenStudio-HPXML workflow.
In general, these defaults are based on **ANSI / RESNET / ICC Std. 301 (2006)**.

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

#### Aspect Ratio
The aspect ratio of individual units of the building is assumed to be 2.

#### Foundations
For buildings with a crawlspace foundation, the height of the foundation is assumed to be 3 ft.
For buildings with a basement or ambient foundation, the height of the foundation is assumed to be 8 ft.

#### Roofs
For buildings with an attic (i.e., not a flat roof), the roof type is assumed to be a gable roof.

#### Walls
The average height of walls adjacent to living space is 8 ft.

#### Neighbors
It is assumed that buildings have no neighbors.

#### Orientation
For Single-Family Detached and Single-Family Attached buildings, 100% of the building units are oriented to the South.
For Low-Rise Multifamily buildings, 50% of the building units are oriented to the South while the other 50% are oriented to the North.

#### Garages
For Single-Family Detached buildings with garages, the size of the garage depends on the floor area.
The garage is assumed to be a 1-car (12 ft wide) for buildings 2500 ft<sup>2</sup> or less, and 2-car (24 ft wide) for buildings greater than 2500 ft<sup>2</sup>.
The garage is also assumed to protrude from the building 100% (i.e., no portion of it is tucked within the building).

#### Corridor
For Low-Rise Multifamily buildings, the corridor is assumed to be a "Double Exterior" corridor (i.e., entrances to individual units are from the exterior of the building).

### Fuel Types

#### Appliances
The fuel type of the cooking range, oven, and clothes dryer is assumed to match the fuel type of the heating system.

#### Water Heating
The fuel type of the water heater is assumed to match the fuel type of the heating system.