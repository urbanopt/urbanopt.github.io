---
layout: default
title: Definitions
parent: Overview
nav_order: 1
---

### Feature:
A Feature is a single object in a district-scale energy analysis such as a building, district system, or transformer.  Each Feature has an associated geometry definition of some type (e.g. position, 2D footprint, 3D mass).  Each Feature has an associated Feature ID and Feature Type (e.g. Building, District System, ElectricalConnector, etc.) as well as other properties related to the Feature Type (e.g. District System Type, Building Type, Number of Stories, Square Footage, etc.).

### Feature File:
A FeatureFile (e.g. `example_project.json`) includes data describing Features. The same FeatureFile may be used by multiple Scenarios, but a FeatureFile does not include information about multiple Scenarios. The FeatureFile could be written by a third-party application or user interface. URBANopt<sup>&trade;</sup> will initially support GeoJSON format via the urbanopt-geojson-gem.

### Scenario:
A [Scenario](../usage/scenarios/scenarios.md) represents one potential realization of a district for energy analysis purposes.  Each Scenario references a FeatureFile and assigns a Simulation Mapper Class to each included Feature in that file.  In this way, Features in the FeatureFile can be translated to simulation input files.

### Scenario File:
A ScenarioFile (e.g. `baseline.csv`) includes all features that make up the Scenario, which is not necessarily all Features in the FeatureFile. It may optionally reference a REOpt assumptions file for each Feature.
