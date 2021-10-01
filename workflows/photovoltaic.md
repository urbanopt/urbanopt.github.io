---
layout: default
title: Photovoltaic Workflows
parent: Workflows
nav_order: 2
---

# Photovoltaic Workflows

As of version 0.6.3, URBANopt supports community photovoltaic and ground mount photovoltaic workflows. 

## Community Photovoltaic

Community Photovoltaic features can be added to the GeoJSON Feature File as a district system of type "Community Photovoltaic".  View the [district system schema](https://docs.urbanopt.net/urbanopt-geojson-gem/schemas/district-system-properties.html) for more details. The REopt scenario optimization post-processor will use this information in its PV capacity calculations. 

## Ground Mount Photovoltaic 

Ground Mount Photovoltaic features can also be added to the GeoJSON Feature File as a district system feature of type "Ground Mount Photovoltaic". A ground mount pv feature should have an `associated_building_id` element specified, with the ID of a building feature in the Feature File. View the [district system schema](https://docs.urbanopt.net/urbanopt-geojson-gem/schemas/district-system-properties.html) for more details. The REopt optimization post-processors will use this information in their PV capacity calculations.


## Example Project

Use the following CLI command to create a PV-enabled example project:

```bash
uo create --project-folder <path/to/PROJECT_DIRECTORY_NAME> --photovoltaic
```

Visit the [Getting Started](./getting_started/getting_started) page for more information on creating and running an example pv project.