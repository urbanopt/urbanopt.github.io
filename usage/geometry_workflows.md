---
layout: default
title: Geometry Workflows
parent: Usage
nav_order: 2
---

The URBANopt<sup>&trade;</sup> project offers different options for creating building geometry to suit various
modelling requirements. This section gives an overview of these workflows.

**Default workflow**

The default workflow in the URBANopt example project uses the `urban-geometry-creation-zoning`
measure in the [GeoJSON gem](https://urbanopt.github.io/urbanopt-geojson-gem/) to create building
features with core and perimeter zoning from GeoJSON footprint coordinates in the Feature File. It has the options to model surrounding buildings as context shading.

More details of the measures used in the
workflow can be found in the [base workflow section](../customization/base_workflow.md).

<div style="text-align:center">

![urbanopt measure workflow diagram](../doc_files/core_perimeter_zoning.jpg)

</div style="text-align:center">

**Createbar workflow**

This workflow uses the
[create_bar_from_building_type_ratios](https://github.com/NREL/openstudio-model-articulation-gem/tree/develop/lib/measures/create_bar_from_building_type_ratios)
measure from the openstudio-model-articulation-gem that attempts to create geometry from high level
building inputs such as number of stories, floor area, and aspect ratio. It can support up to a mix
of four different building types. The space type ratios for each building type are from the DOE
prototype models.

It has an argument for adding a perimeter multiplier that can be used for accurately representing
the exposure conditions for non-rectangular buildings by creating two perpendicular bars.

It also has the capability for specifying the custom height for a space by setting the `Enable
Custom Height Bar Application argument` to true. This can be used for modelling spcace types that
have heights different from the rest of the building, for example to model gyms or auditorium for a
SecondarySchool building type. This is accomplished by pulling space types with a custom height into a separate rectangular building that sits away from the main structure.

It can model circulation space types, when they exist by enabling the `Double Loaded Corridor`
argument. The double loaded corridor leads to space division and thermal zoning instead of typical
sliced core and perimeter zoning. It creates circulation space type running down the center of the
bar, surrounded by spaces of another space type. For an example, for a primary school the corridor will be paired with the classrooms.

This workflow does not take the actual building footprint into account and does not take consider
the impact of self and context shading.

<div style="text-align:center">

![urbanopt measure workflow diagram](../doc_files/create_bar.png)

</div style="text-align:center">


**FloorspaceJS workflow** 

This workflow starts with floor plans with stub space types drawn using FloorSpaceJS. The
FloorspaceJS file is converted to an OpenStudio model by a measure that merges it with a seed model
that is then passed into the `Create Typical Building from Model` measure. The seed model contains
the actual space type assignments that correspond to the stub space type names in the FloorSpaceJS
file and can be taken from the standard space type templates. One of the web tools to create FloorSpaceJS files can be found [here](https://nrel.github.io/floorspace.js/)

The FloorSpaceJS file and the OpenStudio Seed model must have the same name, for example
`feature.js` and `feature.osm` and must be placed in
the osm_building folder in the project. The FloorSpaceJS file name must be also added to the
`detailed_model_filename` property for that feature in the feature file.

*For all these workflows, an existing OpenStudio model of a building feature can be used, if present, by
specifying the name of the model the `detailed_model_filename` property of that Feature in the Feature File and
adding the OpenStudio model in the osm_building folder in the project.*