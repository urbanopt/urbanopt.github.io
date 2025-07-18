---
layout: default
title: Example Project
parent: Resources
nav_order: 3
---

You have access to a ***<u>hypothetical</u>*** URBANopt example project that was created to demonstrate various capabilities of the URBANopt<sup>&trade;</sup> SDK (e.g. modeling of various building types and heights in one district). An actual location for the <u>hypothetical</u> project was needed to demonstrate geospatial aspects of the URBANopt SDK.

The <u>hypothetical</u> example project’s location is within the boundary of an actual district project that was a participant in the U.S. Department of Energy Zero Energy District Accelerator. See “Western New York Manufacturing Zero Energy District” in [this NREL paper](https://www.nrel.gov/docs/fy18osti/71841.pdf) for a description of the actual project. The design in the <u>hypothetical</u> URBANopt example project has no relation to the designs and site requirements of the actual development project.

The <u>hypothetical</u> example project design and building typologies are shown in the figure below.

![example_project_layout](../doc_files/building_types_ISO_with_res.jpg)

The example project can be installed via the Command Line Interface with the following command. For more information, visit the [Getting Started](../getting_started/getting_started) page.

```terminal
uo create --project-folder <path/to/PROJECT_DIRECTORY_NAME>
```

Working with the example project, you'll be able to:

1. Experiment with two pre-built example scenarios: a *baseline* scenario that creates the geometry, and a *high-efficiency* scenario that also applies energy efficiency measures.

1. Use the urbanopt geometry creation measures found in the 'base_workflow.osw'.  For a description of the measures found in the base URBANopt workflow, visit the [base workflow](customization/base_workflow.md) page.

1. Specify an existing detailed model for a feature by setting the *detailed_model_filename* property in the Feature file, which will bypass the urbanopt geometry creation measures.

1. Use site-level data (such as climate zone, weather filename, etc.) to set building measure arguments (in the BaselineMapper.rb file)
