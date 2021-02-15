---
layout: default
title: Residential Workflows
parent: Usage
has_children: true
nav_order: 4
has_toc: false
---

# Residential Workflows

Residential building energy models in URBANopt<sup>&trade;</sup> are created using the [OpenStudio-HPXML](https://github.com/NREL/OpenStudio-HPXML) workflow.
For every residential building feature found in the geojson file, an [HPXML](https://hpxml.nrel.gov) file is built to represent each living unit of the building.
In the case of a single-family detached building (which is the only type of low-rise residential building supported at this time), one HPXML file is built to represent the single unit.
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

Otherwise, argument values will be set according to [these lookup files](https://github.com/urbanopt/urbanopt-example-geojson-project/tree/develop/example_project/mappers/residential) that span across the following categories:

* enclosure (insulation levels, air leakage, etc.)
* HVAC systems (heating/cooling types, efficiencies, etc.)
* appliances (refrigerator, clothes washer, etc.)
* ventilation (mechanical, exhaust)
* water heater

All argument values for the previous categories may be customized by manually adjusting values in the lookup files.
The enumeration names include "Residential IECC 20XX" because a variety of enclosure, window, duct insulation, and whole-home air leakage assumptions are based on the different IECC model code years to illustrate how templates can be used to approximate different levels of efficiency.
Note that not all possible assumptions have been aligned with IECC requirements (e.g., see above regarding defaults), but the users can further customize these templates as needed for specific projects.
