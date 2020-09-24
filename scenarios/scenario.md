---
layout: default
title: Scenarios
has_children: true
nav_order: 4
---

In this section, you can learn more about the pre-built example scenarios and their corresponding mapper classes. Each Feature in a Scenario is assigned with a Simulation Mapper Class. A Mapper Class translates the information in a Feature to simulation input. This Mapper creates a simulation input using a template that bundles a set of OpenStudio Measures. It uses these measures to create an input defining specific energy model characteristics for the Feature.  This gives great control to define custom simulation behavior for a scenario. 

For each of our pre-built example scenario, we developed a Mapperclass that define the efficiency level and properties of the scenario. The developed prebuilt scenarios are as the following: 

- Baseline scenario defined by a Baseline MapperClass
- High-Efficiency Scenario defined by a HighEfficiency MapperClass
- Thermal Energy Storage Scenario defined by a ThermalStorage MapperClass

The following subsections will describe each scenario and the measures included in their corresponding MapperCLass. For modifying and creating new Mapper Classes please refer to [customization section](../customization.md).