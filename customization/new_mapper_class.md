---
layout: default
title: Creating new Mapper Classes
parent: Customization
nav_order: 3
---

A new mapper class can be created to inherit from the existing `BaselineMapper`. For example, a `MediumEfficiency` mapper class can be created to override some of the measure arguments from the `BaselineMapper` in the `create_osw` method.  
The new mapper class can be assigned to features by adding the mapper class name in the Scenario csv.

**To create a completely new Mapper Class and Scenario CSV:**

- The new Mapper Class ruby file should be created in the Mappers folder. The default Simulation Mapper Class can be used as a template, and the `osw_path` would need to be updated as per the name of the new OpenStudio workflow file.
- The new Scenario CSV can be created in the root folder, and the Mapper Class name should be added in the Mapper Class column. The existing Scenario CSVs can be used as reference.
- OpenStudio Measures can be added to the new OpenStudio workflow file by adding the measure directory and measure arguments. In the mapper class, Feature properties from the FeatureFile should be mapped to the corresponding Measure arguments.

