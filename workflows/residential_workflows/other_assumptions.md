---
layout: default
title: Other Assumptions
parent: Residential Workflows
grand_parent: Workflows
nav_order: 4
---

## Other Assumptions

- [Geometry](#geometry)
- [Fuel Types](#fuel-types)

The building footprint drawn and contained in the GeoJSON does not determine the footprint of individual modeled units.
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