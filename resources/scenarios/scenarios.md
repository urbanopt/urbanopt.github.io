---
layout: default
title: Scenarios
parent: Resources
has_children: true
nav_order: 4
has_toc: false
---

In this section, you can learn more about the pre-built example scenarios and their corresponding mapper classes. Each Feature in a Scenario is assigned a Simulation Mapper Class. A Mapper Class translates the feature information compiled in the FeatureFile to create the simulation inputs. The Mapper creates a simulation input using a template OSW that bundles a set of OpenStudio Measures (base_workflow.osw). Different mapper classes can choose to run or skip the measures in the template OSW, as well as set their arguments. The measures are used to create an input defining specific energy model characteristics for each Feature.  This allows for greater control when defining custom simulation behavior for each individual scenario.

For each pre-built example scenario, we developed a Mapperclass that defines the efficiency level and properties of the scenario. The developed prebuilt scenarios are:

- [Baseline Scenario](baseline.md) defined by a Baseline MapperClass
- [High-Efficiency Scenario](highefficiency.md) defined by a HighEfficiency MapperClass
- [Thermal Energy Storage Scenario](thermalstorage.md) defined by a ThermalStorage MapperClass
- [Electric Vehicle Charging Scenario](evcharging.md) defined by an EvCharging MapperClass
- [Flexible Hot Water Scenario](flexiblehotwater.md) defined by an FlexibleHotWater MapperClass

To learn more about modifying and creating new Mapper Classes, refer to the [customization](../customization/customization.md) section.
