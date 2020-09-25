---
layout: default
title: High Efficiency Scenario
parent: Scenarios
nav_order: 2
---

# High Efficiency Scenario

The HighEfficiency MapperClass inherits from the Baseline the required input and applies energy efficiency measures. Below is the list of the added OpenStudio measures that define this Mapper: 

## Scenario
The High Efficiency scenario is defined by applying the HighEfficiency MapperClass to all of the 13 builindgs in this sceanrio.

## Measures
The OpenStudio measures used in the HighEfficiency Mapper are as the following: 

- IncreaseInsulationRValueforExteriorWalls: An OpenStudio measure used to increase the R-Value of insulation for exterior walls by a specific value.
- ReduceElectricEquipmentLoadsByPercentage: An OpenStudio measure used to reduce the electric equipment loads by a specific percentage value.
- ReduceLightingLoadsByPercentage: An OpenStudio measure used to reduce the lighting loads by a specific percentage value.