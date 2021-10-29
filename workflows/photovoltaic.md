---
layout: default
title: Photovoltaic Workflows
parent: Workflows
nav_order: 2
---

# Solar Photovoltaics Workflows

As of version 0.6.3, URBANopt&trade; supports community photovoltaic and ground mount photovoltaic workflows in addition to the rooftop photovoltaic workflow supported by previous versions. 

## Community Photovoltaics

Community Photovoltaic features can be added to the GeoJSON Feature File as a district system of type "Community Photovoltaic".  View the [district system schema](https://docs.urbanopt.net/urbanopt-geojson-gem/schemas/district-system-properties.html) for more details. The REopt&trade; scenario optimization post-processor will use this information in its PV capacity calculations when run with the [multi-PV assumptions file](#assumptions-files). PV generation results can be found in the `scenario_optimization` output files in the scenario directory.

## Ground Mount Photovoltaics

Ground Mount Photovoltaic features can also be added to the GeoJSON Feature File as a district system feature of type "Ground Mount Photovoltaic". A ground mount PV feature should have an `associated_building_id` element specified, with the ID of a building feature in the Feature File. View the [district system schema](https://docs.urbanopt.net/urbanopt-geojson-gem/schemas/district-system-properties.html) for more details. The REopt optimization post-processors will use this information in their PV capacity calculations. A `gcr` field representing the ground coverage ratio, or the percentage of the area given by the ground mount feature that will be covered by PV, can be found in the REopt [assumptions file](#assumptions-files), and is defaulted to 1.

## Rooftop Photovoltaics

Rooftop photovoltaics can be optimized through the REopt post-processors. The default `base` and `multi-PV` [assumptions files](#assumptions-files) can be used for the feature optimization and scenario optimization post-processors. A `gcr` field representing the ground coverage ratio, or the percentage of the building's available roof area that will be covered by PV, can be specified in the REopt assumptions file, and is defaulted to 1. The building's available roof area is automatically calculated by the default URBANopt post-processor. Ground Mount and Rooftop photovoltaics can be explored simultaneously by using the `multi-PV` assumptions file.

## Example Project

Use the following CLI command to create a PV-enabled example project:

```bash
uo create --project-folder <path/to/PROJECT_DIRECTORY_NAME> --photovoltaic
```

Once you have a project directory, you will want to create the Scenario CSV and enable REopt functionality:

```bash
uo create --scenario-file <path/to/FEATUREFILE_WITH_PV.json>
```

```bash
uo create --reopt-scenario-file <path/to/EXISTING_SCENARIO_FILE.csv>
```

### Assumptions Files
The command above creates a `reopt` directory within the project directory as well as a REopt enabled Scenario CSV. This directory contains two example assumptions file: one `base` assumptions file and a `multi-pv` assumptions file that should be used when optimizing both rooftop and ground mount PV.  Feel free to modify these files, or create new ones based on the default files. 

To specify which files to use depends on the type of reopt optimization post-process is run.  

- For the feature-level optimization, the assumptions files are listed in the REopt Scenario CSV file.  
- For the scenario-level optimization, use the `--reopt-scenario-assumptions-file` flag to specify the assumptions file to use when issuing the `uo process --reopt-scenario` command. The `base` assumptions file will be used as a default if the flag is not found.


### Outputs

PV generation results can be found in the `scenario_optimization` output files in the scenario directory when the `--reopt-scenario` post-processor is run.  Results are also found in the `feature_optimization` output files within each feature's individual directory when the `--reopt-feature` post-processor is run.

The results from the scenario and feature level optimization can also be visualized using the
instructions provided on the [Getting Started](./getting_started/getting_started) page. 

An example visualization of scenario optimization is shown below, showing the breakdown Electricity
and Natural Gas consumed and Electricity Produced for the scenario.  

![](../doc_files/visualization_solar.PNG)


Visit the [Getting Started](./getting_started/getting_started) page for more information on creating and running an example PV project.