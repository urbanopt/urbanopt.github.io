---
layout: default
title: Residential Workflows
parent: Workflows
has_children: true
nav_order: 10
has_toc: false
---

# Residential Workflows

Low-rise residential building energy models in URBANopt<sup>&trade;</sup> are created using the [**OpenStudio-HPXML**](https://github.com/NREL/OpenStudio-HPXML) workflow.
For every residential building feature found in the GeoJSON file, either:

1. an [HPXML](https://hpxml.nrel.gov) file is built to represent living unit(s) of the building, or
1. a pre-built HPXML file is used to represent living unit(s) of the building.

For example, in the case of a single-family detached building one HPXML file is built to represent the single unit.

- [Building Types](building_types.md) lists the supported building types and details for each.
- [Arguments](arguments.md) describes various paths for assignment argument values to GeoJSON features.
- [Occupancy](occupancy.md) includes information about schedule and calculation types.
- [Other Assumptions](other_assumptions.md) describes modeling assumptions for geometry and fuel types.
