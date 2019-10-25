---
layout: default
title: Structure
parent: Overview
nav_order: 2
---

The URBANopt SDK is implemented in Ruby and consists of 3 main modules: `urbanopt-core-gem`, `urbanopt-scenario-gem`, and `urbanopt-geojson-gem`.

![image info](../doc_files/URBANopt_arch_development_v2.jpg)

The **Core** gem defines a FeatureFile class. The feature file can be in any format (CityGML, GeoJSON, etc.) and describes properties of each `Feature`, such as location, floor area, number of stories, building type, cooling source, etc. This Core gem in the SDK architecture allows the development of new modules that are independent of other modules.

The **GeoJSON** gem is an OpenStudio Extension gem that translates a GeoJSON feature file into Openstudio model inputs. The GeoJSON gem reads each feature (_**buildings are the only supported Feature in version 0.1.0e**_) from the FeatureFile (such as this [example FeatureFile](https://github.com/urbanopt/urbanopt-example-geojson-project/blob/develop/industry_denver.geojson)), converts geospatial coordinates to cartesian coordinates, performs auto-zoning to establish perimeter and core zones as needed, and extrudes geometry from 2D footprints to 3D surfaces. These properties from each Feature are translated to OpenStudio model (OSM file) inputs leveraging methods in the OpenStudio Model Articulation Gem and OpenStudio Standards Gem. _Methods to translate district systems and transformers into model inputs will be added in a future release._

The **Scenario** gem does the heavy lifting in URBANopt.  It takes the `Scenario` you want to examine (such as [this example Scenario](https://github.com/urbanopt/urbanopt-example-geojson-project/blob/develop/baseline_scenario.csv)), [runs](../usage/rake_tasks.md) the FeatureFile (translated by the GeoJSON gem) through OpenStudio building energy simulation, and reports results for each feature. These reported results are defined by the [default_feature_report](../customization/feature_reports.md) Measure. A `run` directory gets created in your example project directory with folders for each Scenario and each `feature_id` within each Scenario. Feature level results will be stored in a `default_feature_report` folder within the run directory for each Feature. [Post-Processing](../usage/rake_tasks.md) may then be executed to aggregate all Feature reports of a Scenario into a Scenario level report (e.g. aggregate energy use, aggregated building program information) that is written at the top level of each Scenario folder, inside the `run` folder.

![workflow_diagram](../doc_files/workflow-diagram2.jpg)
