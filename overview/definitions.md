---
layout: default
title: Definitions
parent: Overview
nav_order: 1
---

**Feature:** A Feature is a single object in a district energy analysis such as a building, district system, or transformer.  Each Feature has an associated geometry definition of some type (e.g. position, 2D footprint, 3D mass).  Each Feature has an associated Feature ID and Feature Type (e.g. Building, District System) as well as other properties related to the Feature Type (e.g. District System Type, Building Type, Number of Stories, Square Footage, etc.). *`Building` is the only Feature Type implemented in version 0.1.0.*

**Feature File:** A FeatureFile includes data for all Features in a given Scenario. The same FeatureFile may be used by multiple Scenarios, but a FeatureFile does not include information about multiple Scenarios. The FeatureFile is written by a third-party user interface. URBANopt will initially support GeoJSON format via the urbanopt-geojson-gem.

**Scenario:** A Scenario represents one potential realization of a district for energy analysis purposes.  Each Scenario references a FeatureFile and assigns a Simulation Mapper Class to each Feature in that file.  In this way, all Features in the FeatureFile can be translated to simulation input files.
