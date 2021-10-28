---
layout: default
title: Flexible Water Heating Scenario
parent: Scenarios
grand_parent: Resources
nav_order: 5
---

The Flexible Water Heating Scenario enhances the energy efficiency of the modeled Scenario by adding a heat pump water heater, instead of conventional hot water systems, to each building defined in the FeatureFile with a commercial building type. It also allows the users to set its characteristics and control its operation to provide electrical demand flexibility. This is applied explicitly to service & domestic hot water, and it's not for water heating for space heating applications. The Scenario MapperClass inherits from the High Efficiency MapperClass and provides the capability to replace the domestic water heater with an air source heat pump system and allows for the addition of multiple daily flexible control time windows. The heater/tank system may charge at maximum capacity up to an elevated temperature or float without any heat addition for a specified timeframe down to a minimum tank temperature.


## Measure

The add_hpwh measure allows selection between three heat pump water heater modeling approaches in EnergyPlus. The user may select between the pumped-condenser or wrapped-condenser objects. They may also elect to use a simplified calculation that does not use the heat pump objects but instead uses an electric resistance heater and approximates the equivalent electrical input that would be required from a heat pump. This expedites simulation at the expense of accuracy. Note that the accuracy is severely impacted when attempting to simulate load flexibility; therefore, it is not recommended to use the simplified approach beyond approximating the impact of a heat pump relative to electric resistance heating with a constant tank temperature setpoint.


The flexibility of the system is based on user-defined temperatures and times, which are converted into schedule objects. There are four flexibility options:
1.	None: normal operation of the DHW system at a fixed tank temperature setpoint.
1.	Charge - Heat Pump: the tank is charged to a maximum temperature using only the heat pump.
1.	Charge - Electric: the tank is charged using internal electric resistance heaters to a maximum temperature.
1.	Float: all heating elements are turned-off for a user-defined time period unless the tank temperature falls below a minimum value.
The heat pump will be prioritized in a low tank temperature event, with the electric resistance heaters serving as back-up.

**A more details description for usinf the Add HPWH (Heat Pump Water Heater) Measure is provided in this [General Reference Guide](https://github.com/NREL/openstudio-load-flexibility-measures-gem/blob/master/lib/measures/add_hpwh/docs/Flexible%20Domestic%20Hot%20Water%20Implementation%20Guide.pdf).**

## Using or Modifying the FlexibleHotWater Scenario

To run and post-process the *FlexibleHotWater* scenario simply specify the `flexiblehotwater_scenario.csv` file when executing at the command line. The *FlexibleHotWater* scenario may also be used with the REopt workflow.

```bash
uo run -s <path to flexiblehotwater_scenario.csv> -f <path to example_project.json>
```

```bash
uo run -r -s <path to flexiblehotwater_scenario.csv> -f <path to example_project.json>
```

To modify the *FlexibleHotWater* scenario in order to explore other load flexibility options, edit the `FlexibleHotWater.rb` mapper and `base_workflow.osw` as necessary.

## Using the add_hpwh Measure in Your Own Project

To use the `add_hpwh` measure with your own project, you must install the [openstudio-load-flexibility-measures gem](https://github.com/NREL/openstudio-load-flexibility-measures-gem):

```bash
gem install openstudio-load-flexibility-measures
```

and then require it within your mapper file:

```bash
require 'openstudio/load_flexibility_measures'
```

Then within your .osw file, add the measure and set any common argument values. For example:

```bash
{
      "measure_dir_name":"add_hpwh",
      "arguments":{
        "__SKIP__":true
        }
    }
```

Note that the `add_hpwh` measure is an EnergyPlus&trade; measure and must be placed after all OpenStudio&copy; measures and before reporting measures.

Then specify within the mapper any measure arguments that differ from those set in the .osw file. Users can apply the add_hpwh measure, add a sizing multiplier to the tank capacity to cover flex periods, update maximum tanks and minimum temperature setpoints, and manage water heat charge float periods by building. An example of such arguments is defined in the MapperClass:

```bash
OpenStudio::Extension.set_measure_argument(osw, 'add_hpwh', '__SKIP__', false)
# Add a sizing multiplier to the tank capacity to cover flex periods
OpenStudio::Extension.set_measure_argument(osw, 'add_hpwh', 'vol', 2)
# Update maximum tank and minimum temperature setpoints
OpenStudio::Extension.set_measure_argument(osw, 'add_hpwh', 'max_temp', 185)
OpenStudio::Extension.set_measure_argument(osw, 'add_hpwh', 'min_temp', 125)

# Manage water heat charge float periods by building
OpenStudio::Extension.set_measure_argument(osw, 'add_hpwh', 'flex0', 'Charge - Heat Pump')
OpenStudio::Extension.set_measure_argument(osw, 'add_hpwh', 'flex_hrs0', '16:00-17:00')
OpenStudio::Extension.set_measure_argument(osw, 'add_hpwh', 'flex1', 'Float')
OpenStudio::Extension.set_measure_argument(osw, 'add_hpwh', 'flex_hrs1', '17:01-19:00')
```
