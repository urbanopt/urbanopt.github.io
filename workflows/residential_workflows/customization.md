---
layout: default
title: Customization
parent: Residential Workflows
has_children: true
nav_order: 6
has_toc: false
---

# Residential Workflows Customization


The residential workflows in URBANopt are designed to be flexible and extensible to adapt to specific user
projects. The building models are created on the basis of default assumptions
made for different building components within lookup files, grouped together within a template, and assigned to the
building feature in the feature file as described under [customizable template](https://docs.urbanopt.net/workflows/residential_workflows/residential_workflows.html#customizable-template). To modify the models, these templates and the underlying assumptions
can be customized, or a new template with unique assumptions can be created. The URBANopt example
project includes alternate customizable templates (e.g., for modeling homes where some types appliances are not present and/or efficiency of certain appliances/equipment needs to be adjusted) and illustrates how these could be assigned to the building features.

The following additional customizable templates are included:

- `Residential IECC 2006 - Customizable Template Apr 2022`
- `Residential IECC 2009 - Customizable Template Apr 2022`
- `Residential IECC 2012 - Customizable Template Apr 2022`
- `Residential IECC 2015 - Customizable Template Apr 2022`
- `Residential IECC 2018 - Customizable Template Apr 2022`

The specific assumptions made in these customized templates for different building equipment in the
lookup files are:

- Clothes Dryer : The location is updated to be 'none' and it is assumed that no clothes dryer is
  present.

- Clothes Washer: The location is updated to be 'none' and it is assumed that no clothes dryer is
  present.

- Dishwasher: The location is updated to be 'none' and it is assumed that no clothes dryer is
  present.

- Refrigerator: The efficiency of the appliance is modified.

- Water heater: The efficiency of the appliance is modified.

This customized template is assigned to the low-rise residential building features in the [alternate
combined example project feature file](https://github.com/urbanopt/urbanopt-cli/blob/e7d29764eb9ae837078f92a488adb783a3e52616/example_files/example_project_combined.json). It is to be noted, that these values are meant to be representative
to illustrate how templates can be used to customize
the workflow for different communities and are not based on an actual community or formal study. Users should ensure that specific assumptions in their templates are accurate for the homes/communities they are modeling. 