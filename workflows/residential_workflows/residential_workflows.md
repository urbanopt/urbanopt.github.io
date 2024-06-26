---
layout: default
title: Residential Workflows
parent: Workflows
has_children: true
nav_order: 10
has_toc: false
---

# Residential Workflows

Low-rise residential building energy models in URBANopt<sup>&trade;</sup> are created using the [OpenStudio-HPXML](https://github.com/NREL/OpenStudio-HPXML) workflow.
For every residential building feature found in the GeoJSON file, either:

1. an [HPXML](https://hpxml.nrel.gov) file is built to represent living unit(s) of the building, or
1. a pre-built HPXML file is used to represent living unit(s) of the building.

For example, in the case of a single-family detached building one HPXML file is built to represent the single dwelling unit.
In the case of a multifamily building one HPXML file is built containing multiple dwelling units.

The following sections describe the types of buildings supported by this workflow and how input values are assigned to various arguments.

- [Building Types](building_types.md) lists the supported residential building types, including feature properties, example 3D renderings, and feature snippets.
- [Building Inputs](building_inputs.md) describes various paths for assigning building model input values for GeoJSON features.
- [Building Occupancy](building_occupancy.md) includes information about schedule and calculation types.
- [Other Details](other_details.md) describes modeling notes, GeoJSON schema details, and assumptions related to geometry and fuel types.
