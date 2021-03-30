---
layout: default
title: High Efficiency Scenario
parent: Scenarios
grand_parent: Resources
nav_order: 2
---

The High Efficiency scenario is defined by applying the HighEfficiency MapperClass to each feature in the FeatureFile.
The HighEfficiency MapperClass inherits the required basic inputs from the Baseline MapperClass and applies additional energy efficiency measures. Below is the list of the added OpenStudio measures that define this Mapper.

## Measures
The OpenStudio measures used in the HighEfficiency Mapper are as the following: 

- **IncreaseInsulationRValueforExteriorWalls**: An OpenStudio measure used to increase the R-Value of insulation for exterior walls by a specific value.
- **ReduceElectricEquipmentLoadsByPercentage**: An OpenStudio measure used to reduce the electric equipment loads by a specific percentage value.
- **ReduceLightingLoadsByPercentage**: An OpenStudio measure used to reduce the lighting loads by a specific percentage value.
- **BuildResidentialModel**: An OpenStudio "meta-measure" that calls measures from the OpenStudio-HPXML workflow. Applies only to the three low-rise residential building types. Increases wall assembly R-value by 20% and reduces usage multipliers by 10%.