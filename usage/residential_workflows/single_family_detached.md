---
layout: default
title: Single-Family Detached
parent: Residential Workflows
grand_parent: Usage
nav_order: 1
---

## Single-Family Detached

Consider the highlighted "Single-Family Detached" building footprint with the following high-level geojson inputs:

* 1 story above ground
* unvented crawlspace foundation
* vented attic

![single_family_detached](../../doc_files/single-family-detached-footprint.jpg)

An example 3D rendering of the single-family detached building is shown below.

![single_family_detached](../../doc_files/single-family-detached-1.jpg)

(Note that the footprint of the modeled unit is always rectangular even though the geojson footprint may not be.)

The 3D building surfaces stored in HPXML and OSM models represent the area and orientation of ground and exterior exposure of surfaces, but do not represent their position relative to each other.
An example geometry rendering for a translated HPXML file is given below. 

![single_family_detached](../../doc_files/single-family-detached-2.jpg)


### Modeling Notes

- "Single-Family Detached" home models may contain unconditioned non-living spaces that are included as part of the total building area, such as a garage. As a result energy use intensities (EUIs) for homes, often calculated in units of kBtu/sqft/yr, will vary based on the unconditioned floor area if total building area is used for the calculation. Alternatively, conditioned floor area can be used for such calculations.  EUIs are not currently used as part of the URBANopt reporting.
- "Single-Family Detached" home models may be heated only, cooled only, or both heated and cooled. 
  - Partial Conditioning: heating and cooling may be applied to just a portion of the living space of the home or to the entire living space. Representation of partial conditioning of the living space of a home is accomplished by adding ideal air load system to heat and cool the un-conditioned portion of the living area. In this situation, district heating or cooling loads may show up in end uses for the home.
  - Undersized Mechanical System: District heating or cooling loads may also show up in end uses when a designed mechanical system cannot meet the load required to maintain thermostat temperatures. An example would be an evaporative cooling system in a hot humid climate. 
  - For both the partially conditioned and undersized examples, it is possible for reporting or post processing to filter out these unintended district heating and cooling loads.
- It is important to know, that unlike the commercial models that will result in unmet heating or cooling hours, the residential models will not have any unmet heating or cooling hours. To understand how the HVAC system is conditioning for "Single-Family Detached" home models, users should look at district heating and cooling loads.


### GeoJSON Schema

The [URBANopt geojson schema](https://github.com/urbanopt/urbanopt-geojson-gem/blob/develop/lib/urbanopt/geojson/schema/building_properties.json) differentiates between sets of required and optional fields for "Single-Family Detached" residential buildings:

Required fields:

|             Field             |     Type     |                                                                                             Enums                                                                                             |                                    Notes                                    |
| ----------------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| floor_area                    | number       |                                                                                                                                                                                               | Conditioned floor area.                                                     |
| number_of_stories_above_ground| integer      |                                                                                                                                                                                               |                                                                             |
| number_of_stories             | integer      |                                                                                                                                                                                               | Includes foundations.                                                       |
| number_of_bedrooms            | integer      |                                                                                                                                                                                               |                                                                             |
| foundation_type               | string       | slab<br>crawlspace - vented<br>crawlspace - unvented<br>basement - unconditioned<br>basement - conditioned<br>ambient                                                                         |                                                                             |
| attic_type                    | string       | attic - vented<br>attic - unvented<br>attic - conditioned<br>flat roof                                                                                                                        |                                                                             |

Optional fields:

|             Field             |     Type     |                                                                                             Enums                                                                                             |                                    Notes                                    |
| ----------------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| system_type                   | string       | electric resistance<br>furnace<br>boiler<br>central air conditioner<br>room air conditioner<br>evaporative cooler<br>air-to-air heat pump<br>mini-split heat pump<br>ground-to-air-heat-pump  |                                                                             |
| heating_system_fuel_type      | string       | electricity<br>natural gas<br>fuel oil<br>propane<br>wood                                                                                                                                     |                                                                             |
| template                      | string       |                                                                                                                                                                                               | See [Customizable Template](residential_workflows.md#customizable-template) |

An example "Single-Family Detached" building feature snippet is shown below.

  ```json
  {
    "type": "Feature",
    "properties": {
      "id": "14",
      "name": "Residential 1",
      "type": "Building",
      "building_type": "Single-Family Detached",
      "floor_area": 3055,
      "footprint_area": 3055,
      "number_of_stories_above_ground": 1,
      "number_of_stories": 1,
      "number_of_bedrooms": 3,
      "foundation_type": "crawlspace - unvented",
      "attic_type": "attic - vented",
      "system_type": "Residential - furnace and central air conditioner",
      "heating_system_fuel_type": "natural gas",
      "template": "Residential IECC 2015 - Customizable Template Sep 2020"
    }
  ```
