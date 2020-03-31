---
layout: default
title: REopt Post Processing
parent: REopt
nav_order: 2
---
## Intro

CLI commands are used to run and post-process each scenario. Onscreen help is always availabe with `uo -h`

**REopt Lite** optimization happens during the post-processing of each scenario. Two types of REopt optimization are available: _scenario-level_, which optimizes for the entire district being simulated, and _feature-level_, which optimizes each building individually. It is possible, although unlikely, for the two types to provide identical optimization outputs.

## Workflow

The `type` of reopt optimization is specified in the CLI call:

This command allows you to post-process a ScenarioReport in aggregate. This is suitable for community-scale optimizations.

```terminal
  uo -g -t reopt-scenario -s baseline_scenario.csv -f example_project.json  
```

Alternatively, this command allows you to post-process a Scenario for each of its Feature Reports before aggregating into a summary in the Scenario Report. This optimizes each building individually, not necessarily for the entire site. 

```terminal
  uo -g -t reopt-feature -s baseline_scenario.csv -f example_project.json  
```

Depending on the `type` of optimization chosen, **REopt Lite** runs on a _**ScenarioReport**_ or _**FeatureReport**_. The resulting `distributed_generation` attributes (including system financial and sizing attributes) are updated as shown in an example below. 

```json
  "distributed_generation": {
        "lcc_us_dollars": 100000000.0,
        "npv_us_dollars": 10000000.0,
        "year_one_energy_cost_us_dollars": 7000000.0,
        "year_one_demand_cost_us_dollars": 3000000.0,
        "year_one_bill_us_dollars": 10000000.0,
        "total_energy_cost_us_dollars": 70000000.0,
        "solar_pv": {
          "size_kw": 30000.0
        },
        "wind": {
          "size_kw": 0.0
        },
        "generator": {
          "size_kw": 0.0
        },
        "storage": {
          "size_kw": 2000.0,
          "size_kwh": 5000.0
        }
      }
```

Moreover, the following optimal dispatch fields are added to its `timeseries CSV`.

|            new column name               |  unit   |
| -----------------------------------------| ------- |
| ElectricityProduced:Total                | kWh     |
| Electricity:Load:Total                   | kWh     |
| Electricity:Grid:ToLoad                  | kWh     |
| Electricity:Grid:ToBattery               | kWh     |
| Electricity:Storage:ToLoad               | kWh     |
| Electricity:Storage:ToGrid               | kWh     |
| Electricity:Storage:StateOfCharge        | kWh     |
| ElectricityProduced:Generator:Total      | kWh     |
| ElectricityProduced:Generator:ToBattery  | kWh     |
| ElectricityProduced:Generator:ToLoad     | kWh     |
| ElectricityProduced:Generator:ToGrid     | kWh     |
| ElectricityProduced:PV:Total             | kWh     |
| ElectricityProduced:PV:ToBattery         | kWh     |
| ElectricityProduced:PV:ToLoad            | kWh     |
| ElectricityProduced:PV:ToGrid            | kWh     |
| ElectricityProduced:Wind:Total           | kWh     |
| ElectricityProduced:Wind:ToBattery       | kWh     |
| ElectricityProduced:Wind:ToLoad          | kWh     |
| ElectricityProduced:Wind:ToGrid          | kWh     |

The figure below describes the workflow that takes place on implementing the `run` and `gather` CLI commands.

![workflow_diagram](../doc_files/cli_workflow_diagram.jpg)

The following figure represents how Simulation Mapper Classes can be assigned to different
Features from the FeatureFile in the Scenario CSV.

![scenario_mapper](../doc_files/reopt-scenario-mapper.png)
