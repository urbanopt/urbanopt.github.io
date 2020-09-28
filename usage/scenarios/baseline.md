---
layout: default
title: Baseline Scenario
parent: Scenarios
nav_order: 1
---
# Baseline Scenario

The Baseline MapperClass includes measures that create the basic required inputs for an OpenStudio model and characterize it with baseline energy properties. Below is the list of OpenStudio measures used in this mapper: 

## Scenario
The Baseline scenario is defined by applying the baseline MapperClass to each of the 13 builindgs in this sceanrio.

## Measures
The OpenStudio measures used in the Baseline Mapper are as the following: 

- set_run_period: An OpenStudio Measure used to define the number of timesteps per hour and specify the start and end date for running the simulation.
- ChangeBuildingLocation: An OpenStudio Measure to specify and load the energyPlusWeather file.
- create_typical_building_from_model: An OpenStudio Model Articulation Measure that takes a custom building from user-defined geometry and assigns constructions, schedules, internal loads, HVAC, and other loads (such as exterior lights and service water heating) based on space and subspace types.
- urban_geometry_creation: An URBANopt GeoJSON measure to create geometry along with spaces for a particular building, accounting for shading from surrounding buildings.
- default_feature_reports: An URBANopt Scenario Measure that generates feature reports (JSON and CSV files that include feature results), which are used by the scenarioDefaultPostProcessor.