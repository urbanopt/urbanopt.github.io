---
layout: default
title: Example Project
parent: Usage
nav_order: 1
---

The URBANopt example project is a ***<u>hypothetical</u> project only*** that was created
to demonstrate various capabilities of the URBANopt SDK in version 0.1.0 (e.g. modeling
of various building types and heights in one district). An actual location for the
<u>hypothetical</u> project was needed to demonstrate geospatial aspects of the URBANopt SDK.
The <u>hypothetical</u> example project’s location is within the boundary of an actual
district project that is a participant in the U.S. Department of Energy Zero Energy
District Accelerator. See “Western New York Manufacturing Zero Energy District” in [this
NREL paper](https://www.nrel.gov/docs/fy18osti/71841.pdf) for a description of the actual
project. The design in the <u>hypothetical</u> URBANopt example project has no relation
to the designs and site requirements of the actual development project.

The example project design and building typologies are shown in the figure below.

To run URBANopt, use the [URBANopt Command Line Interface (CLI)](https://github.com/urbanopt/uo-cli). Help for the CLI is always available by typing `uo -h` from the command line.

1. Create a project folder using `uo -p`
1. Put your [FeatureFile](../overview/definitions.md) in the root of the folder you just created.
1. Create a [ScenarioFile](../overview/definitions.md) using `uo -m`
    - This assumes the FeatureFile you provided describes a baseline, or a "before" version of your district.
1. Simulate energy usage of each feature in your FeatureFile by using `uo -r -f -s`
1. Aggregate all those features into a [Scenario](../overview/definitions.md) by using `uo -a -f -s`

![example_project_layout](../doc_files/building_types_ISO.jpg)
