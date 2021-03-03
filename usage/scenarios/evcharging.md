---
layout: default
title: EV charging Scenario
parent: Scenarios
grand_parent: Usage
nav_order: 3
---
The EV Charging Scenario adds loads due to [Electric Vehicle (EV)](#ptes) charging associated with Building
Features in the FeatureFile. The EV Charging MapperClass inherits from the High Efficiency
MapperClass and adds EV loads in addition to high efficiency measures.

## Measures

The two measures used for this scenario are in the
[openstudio-common-measures](https://github.com/NREL/openstudio-common-measures-gem "GitHub
Repository") gem. The functionality and available user inputs are briefly described below and links
to additional documentation are provided.

### <a name="ptes"></a> add_ev_load Measure

The measure is based on static profiles of power draw created using EVI-Pro Tool, to reflect
different potential scenarios for EV charging behavior. The charging
profiles are divided into **residential**, **public** and **workplace** charging stations. The vehicle
population is assumed to be the same as the building occupancy, derived from the occupancy schedules
in OpenStudio Standards used in the model. The magnitude of the load based on the
proportion of vehicles at site that are EVs can be configured as well.

Users can specify their inputs for the following arguments:

### <a name="its"></a> EV charging behavior

*Business as usual* : This represents home dominant charging behavior where the majority of the
electrical energy consumed by EV charging is during evening hours and overnight.

*Free workplace charging at project site* : Indicates that the peak power draw from EV charging on
weekdays occurs during morning hours, due to EV charging at workplaces. Overnight residential
charging remains a significant share of the total electricity use for EV charging.

*Free workplace charging across metro area* : This reduces the Home EV charging relative to the Free
workplace charging at project site scenario, for people who work elsewhere and can charge their
vehicles for free at those workplaces.

### <a name="its"></a> Delay type

Represents charging flexibility scenarios applied to workplace charging. Users can select from the
following scenarios:

*Minimum delay* : EVs begin charging immediately upon arriving at work

*Maximum delay* : EVs are plugged in immediately but do not begin charging until necessary

*Minimum power* : EVs are charged at minimum rate over the parking event.

### <a name="its"></a> Charging station type

Users can select from the following options:

*Typical Home* : Representing Single-Family, Multifamily, Lodging URBANopt building types.

*Typical Work* : Representing URBANopt building types such as Office, Laboratory, Education, Healthcare etc.

*Typical Public* : Representing URBANopt building types such as Retail, Shopping mall, Food services etc.

### <a name="its"></a> EV percent

Denotes percentage of vehicles between 0 to 100 that are electric on site.

### <a name="ptes"></a> add_ems_to_control_ev_charging Measure

This measure uses EnergyPlus' EnergyManagementSystem objects to control an electric vehicle charging
load to better align the charging power draw with expected solar PV power production. The measure is
intended ofr use at a 15-minute timestep. *The measure should be run after applying the `Add EV Load
measure`.* It is structured around the assumption of an office building occupancy schedule, with occupants requiring vehicles to be charged by 7pm. Load shifting events are characterized by declining levels of solar radiation, which is used as a proxy for diminishing power output from on-site solar PV. Load shifting occurs only on weekdays, when commercial buildings would typically have higher EV charging loads.

Users need to specify the `Curtailment Fraction` argument for the measure. It is a number between 0 and 1 that denotes the fraction by which EV charging
is curtailed during load shifting events. The default value set in the measure is 0.5.

More information about the measure can be found [here](https://www.nrel.gov/docs/fy20osti/77438.pdf).


## Using or Modifying the EV Charging scenario

To run and post-process the EV Charging scenario simply specify the ev_charging_scenario.csv file when executing at the command line.

```
uo run -s <path to ev_charging_scenario.csv> -f <path to example_project.json>
```

After running the EV charging scenario the default post process command can be used to generate Default
Feature and Scenario reports. The results of EV charging are under the
`ExteriorEquipment(Electricity)`.

```
uo process -d -s <path to ev_charging_scenario.csv> -f <path to example_project.json>
```

## Using EV Charging in Your Own Project

The following steps are required to use the `add_ev_load` measure in your project:

- To add the EV charging load to a building Feature, set `ev_charging` to true for the feature in the
  FeatureFile.
- Specify additional inputs  such as charging station type, EV percent etc. If
  additional inputs are not specified, default values for the measure arguments will be assumed.

For example:

```
    {
      "type": "Feature",
      "properties": {
        "id": "2",
        "name": "Restaurant 1",
        "type": "Building",
        "building_type": "Food service",
        "floor_area": 22313,
        "footprint_area": 22313,
        "number_of_stories": 1,
        "ev_charging": true,
        "ev_charging_behavior": "Business as Usual",
        "ev_percent": 100,
        "ev_curtailment_frac": 0.5
      }
```

Within the .osw file, add the measure and set argument values. For example:

```
    {
      "measure_dir_name": "add_ev_load",
      "arguments": {
        "__SKIP__": false,
        "chg_station_type": "Typical Public",
        "delay_type": "Min Delay",
        "charge_behavior": "Business as Usual",
        "ev_percent": 100
      }
    }
```

Within the mapper add any measure arguments that differ from those set in the .osw file. For example:

```ruby
OpenStudio::Extension.set_measure_argument(osw, 'add_ev_load', 'ev_percent', 50)
```

The  steps for adding the `add_ems_to_control_ev_charging` are the same as adding the `add_ev_load`
measure. 

*Note: The `add_ev_load` measure must be run before running the
`add_ems_to_control_ev_charging` measure. This can be done by adding the `add_ev_load` measure
before the `add_ems_to_control_ev_charging` measure in the .osw file*
