---
layout: default
title: Other Details
parent: Residential Workflows
grand_parent: Workflows
nav_order: 4
---

## Other Details

Other modeling details are described for the following categories:

- [Modeling Notes](#modeling-notes)
- [GeoJSON Schema](#geojson-schema)
- [Assumptions](#assumptions)

### Modeling Notes

- The building footprint drawn and contained in the GeoJSON does not determine the footprint of individual modeled units.
- Floor area is divided by the number of residential units to determine the floor area of each individual unit.
- Individual footprints are determined using this unit floor area and an aspect ratio of 2 (i.e., front/back walls are twice as long as left/right walls).
- Single-Family Detached home models may contain unconditioned non-living spaces that are included as part of the total building area, such as a garage. As a result energy use intensities (EUIs) for homes, often calculated in units of kBtu/sqft/yr, will vary based on the unconditioned floor area if total building area is used for the calculation. Alternatively, conditioned floor area can be used for such calculations.
- Home models for all building types may be heated only, cooled only, or both heated and cooled.
  - Partial Conditioning: heating and cooling may be applied to just a portion of the living space of the home or to the entire living space. Representation of partial conditioning of the living space of a home is accomplished by adding ideal air load system to heat and cool the unconditioned portion of the living area. In this situation, district heating or cooling loads may show up in end uses for the home.
  - Undersized Mechanical System: District heating or cooling loads may also show up in end uses when a designed mechanical system cannot meet the load required to maintain thermostat temperatures. An example would be an evaporative cooling system in a hot humid climate.
  - For both the partially conditioned and undersized examples, it is possible for reporting or post processing to filter out these unintended district heating and cooling loads.
- It is important to know, that unlike the commercial models that will result in unmet heating or cooling hours, the residential models will not have any unmet heating or cooling hours. To understand how the HVAC system is conditioning for home models, users should look at district heating and cooling loads.

### GeoJSON Schema

The [URBANopt GeoJSON schema](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/building_properties.json) differentiates between sets of **required** and **optional** fields for Single-Family Detached, Single-Family Attached, and Low-Rise Multifamily residential buildings.

**Required** fields:

|             Field             |  Data Type   |                                                                                             Enums                                                                                                                     |                                    Notes                                    |
| ----------------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| floor_area                    | number       |                                                                                                                                                                                                                       | Total conditioned floor area.                                               |
| footprint_area                | number       |                                                                                                                                                                                                                       | First floor conditioned floor area.                                         |
| number_of_stories_above_ground| integer      |                                                                                                                                                                                                                       |                                                                             |
| number_of_stories             | integer      |                                                                                                                                                                                                                       | Includes foundations.                                                       |
| number_of_bedrooms            | integer      |                                                                                                                                                                                                                       | Must be > 0.                                                                |
| foundation_type               | string       | (1) slab<br>(2) crawlspace - vented<br>(3) crawlspace - unvented<br>(4) crawlspace - conditioned<br>(5) basement - unconditioned<br>(6) basement - conditioned<br>(7) ambient                                         | Excluding (4) and (6) for Low-Rise Multifamily.                             |
| attic_type                    | string       | (1) attic - vented<br>(2) attic - unvented<br>(3) attic - conditioned<br>(4) flat roof                                                                                                                                | Excluding (3) for Low-Rise Multifamily. Stories > 1 for (3).                |

**Optional** fields:

|             Field             |     Type     |                                                                                             Enums                                                                                                                                 |                                    Notes                                    |
| ----------------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| roof_type                     | string       | (1) Gable<br>(2) Hip                                                                                                                                                                                                              | NA when attic type is flat roof.                                            |
| occupancy_calculation_type    | string       | (1) asset<br>(2) operational                                                                                                                                                                                                      |                                                                             |
| number_of_occupants           | integer      |                                                                                                                                                                                                                                   | For operational calculations.                                               |
| system_type                   | string       | (1) electric resistance<br>(2) furnace<br>(3) boiler<br>(4) central air conditioner<br>(5) room air conditioner<br>(6) evaporative cooler<br>(7) air-to-air heat pump<br>(8) mini-split heat pump<br>(9) ground-to-air-heat-pump  |                                                                             |
| heating_system_fuel_type      | string       | (1) electricity<br>(2) natural gas<br>(3) fuel oil<br>(4) propane<br>(5) wood                                                                                                                                                     |                                                                             |
| onsite_parking_fraction       | number       | (1) No (0)<br>(2) Yes (1)                                                                                                                                                                                                         | For Single-Family Detached only.                                            |
| hpxml_directory               | string       |                                                                                                                                                                                                                                   | Relative to the ``xml_building`` folder. Required fields become optional.   |
| template                      | string       |                                                                                                                                                                                                                                   | See [Customizable Template](building_inputs.md#customizable-template)       |
| characterize_residential_buildings_from_buildstock_csv    | string       |                                                                                                                                                                                                       | See [ResStock Samples](building_inputs.md#resstock-samples)                 |
| resstock_buildstock_csv_path                              | string       |                                                                                                                                                                                                       | See [ResStock Samples](building_inputs.md#resstock-samples)                 |
| uo_buildstock_mapping_csv_path                            | string       |                                                                                                                                                                                                       | See [ResStock Samples](building_inputs.md#resstock-samples)                 |

### Assumptions

A summary of modeling assumptions baked into the baseline mapper is given below.
In the future, updates/improvements could be made to expose these arguments as inputs to the models.
For example, aspect ratio could be either user-defined or determined from the drawn building footprint.
Another example is allowing building orientation to be user-defined, or determining it based on the "front" of the building.

#### Geometry: Aspect Ratio
The aspect ratio of individual units of the building is assumed to be 2.

#### Geometry: Foundations
For buildings with a crawlspace foundation, the height of the foundation is assumed to be 3 ft.
For buildings with a basement or ambient foundation, the height of the foundation is assumed to be 8 ft.

#### Geometry: Walls
The average height of walls adjacent to living space is 8 ft.

#### Geometry: Neighbors
It is assumed that buildings have no neighbors.

#### Geometry: Orientation
For Single-Family Detached and Single-Family Attached buildings, 100% of the building units are oriented to the South.
For Low-Rise Multifamily buildings, 50% of the building units are oriented to the South while the other 50% are oriented to the North.

#### Geometry: Garages
For Single-Family Detached buildings with garages, the size of the garage depends on the floor area.
The garage is assumed to be a 1-car (12 ft wide) for buildings 2500 ft<sup>2</sup> or less, and 2-car (24 ft wide) for buildings greater than 2500 ft<sup>2</sup>.
The garage is also assumed to protrude from the building 100% (i.e., no portion of it is tucked within the building).

#### Geometry: Corridor
For Low-Rise Multifamily buildings, the corridor is assumed to be a "Double Exterior" corridor (i.e., entrances to individual units are from the exterior of the building).

#### Fuel Types: Appliances
The fuel type of the cooking range, oven, and clothes dryer is assumed to match the fuel type of the heating system.

#### Fuel Types: Water Heating
The fuel type of the water heater is assumed to match the fuel type of the heating system.