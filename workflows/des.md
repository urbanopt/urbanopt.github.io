---
layout: default
title: District Energy Systems (DES)
parent: Workflows
nav_order: 3
---

# District Energy Systems (DES)

## Installation

The DES workflow has python and Modelica dependencies that must be installed in addition to the URBANopt CLI dependencies prior to use. Visit the [DES Installation page](../installation/des_installation.md) to install the dependencies.

## Usage


There are three major steps to running the DES workflow:

1. generating the GeoJSON and System Parameter JSON files,
1. creating of the Modelica package containing the district system, and
1. running the Modelica package.

Refer to the [Getting Started page](../getting_started/getting_started.md) to view examples of how to use the DES workflow with the URBANopt CLI.


## Overview

District energy systems have been leveraged for hundreds of years to move energy (typically waste heat from industrial processes) to effectively maintain comfort in neighboring buildings; however, modeling the potential and effectiveness of these systems has been a challenge due to complexity. The URBANopt DES workflow aims to make DES analysis more approachable in hopes of encouraging adoption through better evaluation of new systems or expansions of in situ systems. The URBANopt DES workflow leverages a tool called the GeoJSON to Modelica Translator (GMT) to enable the analysis of DES systems.

The GeoJSON Modelica Translator (GMT) is a one-way trip from GeoJSON in combination with a well-defined instance of the system parameters schema to a Modelica package with multiple buildings loads, energy transfer stations, distribution networks, and central plants. The project will eventually allow multiple paths to build up different district heating and cooling system topologies; however, the initial implementation is limited to 1GDH and 4GDHC.

The project is motivated by the need to easily evaluate district energy systems. The goal is to eventually cover the various generations of heating and cooling systems as shown in the figure below. The need to move towards 5GDHC systems results in higher efficiencies and greater access to additional waste-heat sources.

![image of DES generations](https://raw.githubusercontent.com/urbanopt/geojson-modelica-translator/develop/docs/images/des-generations.png)

The diagram below is meant to illustrate the future proposed interconnectivity and functionality of the
GMT project.

![GMT functionality](https://raw.githubusercontent.com/urbanopt/geojson-modelica-translator/develop/docs/images/des-connections.png)

As shown in the image, there are multiple building loads that can be deployed with the GMT and are described in the [Building Load Models section](#building-load-models) below. These models, and the associated design parameters, are required to create a fully runnable Modelica model. The GMT leverages two file formats for generating the Modelica project: 1) the GeoJSON feature file and 2) a System Parameter JSON file.

### Building Load Models

The building loads can be defined multiple ways depending on the fidelity of the required models. Each of the building load models are easily replaced using configuration settings within the System Parameters file. The models can have mixed building load models, for example the district system can have 3 time series models, an RC model, and a detail Spawn model. The 4 different building load models include:

1. Time Series in Watts: This building load is the total heating, cooling, and domestic hot water loads represented in a CSV type file (MOS file). The units are Watts and should be reported at an hour interval; however, finer resolution is possible. The load is defined as the load seen by the ETS.
1. Time Series as mass flow rate and delta temperature: This building load is similar to the other Time Series model but uses the load as seen by the ETS in the form of mass flow rate and delta temperature. The file format is similar to the other Time Series model but the columns are mass flow rate and delta temperature for heating and cooling separately.
1. RC Model: This model leverages the TEASER framework to generate an RC model with the correct coefficients based on high level parameters that are extracted from the GeoJSON file including building area and building type.
1. Spawn of EnergyPlus: This model uses EnergyPlus models to represent the thermal zone heat balance portion of the models while using Modelica for the remaining components. Spawn of EnergyPlus is still under development and currently only works on Linux-based systems.

## Architecture Overview

The GMT is designed to enable "easy" swapping of building loads, district systems, and newtork topologies. Some
of these functionalities are more developed than others, for instance swapping building loads between Spawn and
RC models (using TEASER) is fleshed out; however, swapping between a first and fifth generation heating system has yet to be fully implemented.

### GeoJSON and System Parameter Files

This module manages the connection to the GeoJSON file including any calculations that are needed. Calculations
can include distance calculations, number of buildings, number of connections, etc.

The GeoJSON model should include checks for ensuring the accuracy of the area calculations, non-overlapping building areas and coordinates, and various others.

### Load Model Connectors

The Model Connectors are libraries that are used to connect between the data that exist in the GeoJSON with a
model-based engine for calculating loads (and potentially energy consumption). Examples includes, TEASER,
Data-Driven Model (DDM), CSV, Spawn, etc.

### Simulation Mapper Class / Translator

The Simulation Mapper Class can operate at mulitple levels:

1. The GeoJSON level -- input: geojson, output: geojson+
2. The Load Model Connection -- input: geojson+, output: multiple files related to building load models (spawn, rom, csv)
3. The Translation to Modelica -- input: custom format, output: .mo (example inputs: geojson+, system design parameters). The translators are implicit to the load model connectors as each load model requires different paramters to calculate the loads.

In some cases, the Level 3 case (translation to Modelica) is a blackbox method (e.g. TEASER) which prevents a
simulation mapper class from existing at that level.

## Testing and Developer Resources

It is possible to test the GeoJSON to Modelica Translator (GMT) by simpling installing the Python package and running the command line interface (CLI) with results from and URBANopt SDK set of results. However, to fully leverage the functionality of this package (e.g., running simulations), then you must also install the Modelica Buildings library (MBL) and Docker. Instructions for installing and configuring the MBL and Docker are available on the [DES Installation page](../installation/des_installation.md).

To simply scaffold out a Modelica package that can be inspected in a Modelica environment (e.g., Dymola) then run the following code below up to the point of run-model. The example generates a complete 4th Generation District Heating and Cooling (4GDHC) system with time series loads that were generated from the URBANopt SDK using OpenStudio/EnergyPlus simulations.

```bash
pip install geojson-modelica-translator

# from the simulation results within a checkout of this repository
# in the ./tests/management/data/sdk_project_scraps path.

# generate the system parameter from the results of the URBANopt SDK and OpenStudio Simulations
uo_des build-sys-param sys_param.json baseline_scenario.csv example_project.json

# create the modelica package (requires installation of the MBL)
uo_des create-model sys_param.json

# test running the new Modelica package (requires installation of Docker)
uo_des run-model model_from_sdk
```

More example projects are available in an accompanying [example repository](https://github.com/urbanopt/geojson-modelica-translator-examples).

Visit the [developer resources page](https://docs.urbanopt.net/geojson-modelica-translator/developer_resources.html) if you are interested in contributing to the GMT project.

## Publications and References

- Long, N., Gautier, A., Elarga, H., Allen, A., Summer, T., Klun, L., Moore, N., Wetter, M.,. (2021). Modeling District Heating and Cooling Systems with Urbanopt, Geojson To Modelica Translator, and the Modelica Buildings Library. Building Simulation 2021, submitte

- Allen, A., Long, N. L., Moore, N., & Elarga, H. (2021). URBANopt District Energy Systems HVAC Measures. National Renewable Energy Laboratory. [https://doi.org/10.11578/dc.20210127.1](https://doi.org/10.11578/dc.20210127.1)

- Long, N., & Summer, T. (2020). Modelica Builder (0.1.0). [https://doi.org/10.11578/dc.20200409.1](https://doi.org/10.11578/dc.20200409.1)

- Long, N., & Summer, T. (2020). Modelica Formatter. [https://github.com/urbanopt/modelica-fmt](https://github.com/urbanopt/modelica-fmt)

- Long, N., Almajed, F., von Rhein, J., & Henze, G. (2021). Development of a metamodelling framework for building energy models with application to fifth-generation district heating and cooling networks. Journal of Building Performance Simulation, 14(2), 203–225. [https://doi.org/10.1080/19401493.2021.1884291.](https://doi.org/10.1080/19401493.2021.1884291) [https://www.tandfonline.com/doi/abs/10.1080/19401493.2021.1884291](https://www.tandfonline.com/doi/abs/10.1080/19401493.2021.1884291)

- Allen, A., Henze, G., Baker, K., Pavlak, G., Long, N., & Fu, Y. (2020). A topology optimization framework to facilitate adoption of advanced district thermal energy systems. IOP Conference Series: Earth and Environmental Science, 588, 022054. [https://doi.org/10.1088/1755-1315/588/2/022054](https://doi.org/10.1088/1755-1315/588/2/022054)

- Allen, A., Henze, G., Baker, K., & Pavlak, G. (2020). Evaluation of low-exergy heating and cooling systems and topology optimization for deep energy savings at the urban district level. Energy Conversion and Management, 222, 113106. [https://doi.org/https://doi.org/10.1016/j.enconman.2020.113106](https://doi.org/https://doi.org/10.1016/j.enconman.2020.113106)

- Allen, A., Henze, G., Baker, K., Pavlak Gregory, & Murphy, M. (2021). Evaluation of Topology Optimization to Achieve Energy Savings at the Urban District Level. 2021 ASHRAE Winter Conference.