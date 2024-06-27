---
layout: default
title: Building Occupancy
parent: Residential Workflows
grand_parent: Workflows
nav_order: 3
---

## Building Occupancy

The user has control over both occupant-related schedule types and occupancy calculation types:

- [Schedule Types](#schedule-types)
- [Calculation Types](#calculation-types)

### Schedule Types

Occupant-related schedules can be either defaulted (smooth) or stochastically generated, and may vary either building-to-building or unit-to-unit.
The default behavior is to use stochastically generated schedules that vary unit-to-unit, but the user has control to both use defaulted schedules and vary them building-to-building.
Note that there are runtime impacts associated with using stochastically generated schedules and for varying schedules unit-to-unit.

Stochastic schedules are generated using time-inhomogenous Markov chains derived from American Time Use Survey data, supplemented with sampling duration and power level from NEEA RBSA data, as well as DHW draw duration and flow rate data from Aquacraft/AWWA data.

In terms of repeatability, stochastic schedules generation uses a pseudo-random number generator that takes a seed as an argument.
The seed is determined by the index of a given feature relative to all features in the GeoJSON, and then multiplied by the unit number within the building.
For schedules that vary by building, the schedules that correspond to the first unit are used for all units of the building.
Relocating a feature's position within a GeoJSON would change the seed argument for that building.

### Calculation Types

Occupancy-based loads (i.e., plug loads, appliances, hot water, etc.) can be calculated through either:

1. an asset calculation, or
1. an operational calculation.

The default behavior is to perform an asset calculation, but the user has control to enable an operational calculation.

Asset calculations are performed using number of bedrooms and/or conditioned floor area.

Operational calculations are performed using an adjustment for the number of occupants.