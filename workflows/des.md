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

District energy systems have been leveraged for hundreds of years to move energy (typically waste heat from industrial processes) to effectively maintain comfort in neighboring buildings; however, modeling the potential and effectiveness of these systems has been a challenge due to complexity. The URBANopt DES workflow aims to make DES analysis more approachable, enabling better evaluation of new systems or expansions of in situ systems. The URBANopt DES workflow leverages a tool called the GeoJSON to Modelica Translator (GMT) to enable the analysis of DES systems.

The GeoJSON Modelica Translator (GMT) is a one-way trip from GeoJSON in combination with a well-defined instance of the system parameters schema to a Modelica package with multiple buildings loads, energy transfer stations, distribution networks, and central plants. The project will eventually allow multiple paths to build up different district heating and cooling system topologies; however, the initial implementation is limited to 4GDHC and 5GDHC.

The project is motivated by the need to easily evaluate district energy systems. The goal is to eventually cover the various generations of heating and cooling systems as shown in the figure below. For example, 5GDHC systems can result in higher efficiencies and greater access to additional waste-heat sources.

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

The GMT is designed to enable "easy" swapping of building loads, district systems, and network topologies. Some
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

The Simulation Mapper Class can operate at multiple levels:

1. The GeoJSON level -- input: geojson, output: geojson+
2. The Load Model Connection -- input: geojson+, output: multiple files related to building load models (spawn, rom, csv)
3. The Translation to Modelica -- input: custom format, output: .mo (example inputs: geojson+, system design parameters). The translators are implicit to the load model connectors as each load model requires different parameters to calculate the loads.

In some cases, the Level 3 case (translation to Modelica) is a blackbox method (e.g. TEASER) which prevents a
simulation mapper class from existing at that level.

## Testing and Developer Resources

It is possible to test the GeoJSON to Modelica Translator (GMT) by separately installing the Python package and running the command line interface (CLI) with results from an URBANopt SDK set of simulations. Follow the instructions to [install the GMT](https://docs.urbanopt.net/geojson-modelica-translator/getting_started/) as a standalone package with its own CLI.

More example projects are available in an accompanying [example repository](https://github.com/urbanopt/geojson-modelica-translator-examples).

Visit the [developer resources page](https://docs.urbanopt.net/geojson-modelica-translator/developer_resources/) if you are interested in contributing to the GMT project.

## Publications and References

- Allen, Amy, Jing Wang, Shadi Abdel Haleem, Matt Mitchell, Nicholas Long, Gregor Henze, Jay Tulley. 2025. [“From Theory to Practice: Feasibility Study of a Thermal Microgrid at a DoD Installation.”](https://technologyportal.ashrae.org/papers/paperdetail/11765) 2025 ASHRAE Annual Conference - Phoenix 131 Part 2 (June).

- Zwickl-Bernhard, Sebastian, Nicholas Long, Simon Jordan, Felix Bauer, Juliet G. Simpson, and Whitney Trainor-Guitton. 2025. [“Optimizing District Energy Systems under Uncertainty: Insights from a Case Study from Washington D.C., USA.”](https://doi.org/10.1016/j.enconman.2025.119979) Energy Conversion and Management 341 (October): 119979. 

- Simpson, Juliet G., Nicholas Long, and Guangdong Zhu. 2024. [“Decarbonized District Energy Systems: Past Review and Future Projections.”](https://doi.org/10.1016/j.ecmx.2024.100726) Energy Conversion and Management: X 24 (October).

- Lämmle, M., Allen, A., Henze, G., Pless, S. (2022). [Valuation of Novel Waste Heat Sources and a Path Towards Adoption](https://www.nrel.gov/docs/fy22osti/83352.pdf). ACEEE Summer Study on Energy Efficiency in Buildings.

- Hinkelman, Kathryn, Jing Wang, Wangda Zuo, Antoine Gautier, Michael Wetter, Chengliang Fan, and Nicholas Long. (2021). [Modelica-Based Modeling and Simulation of District Cooling Systems: A Case Study](https://www.sciencedirect.com/science/article/pii/S0306261922001210). Applied Energy.

- Long, N., Gautier, A., Elarga, H., Allen, A., Summer, T., Klun, L., Moore, N., Wetter, M.,. (2021). [Modeling District Heating and Cooling Systems with URBANopt, GeoJSON to Modelica Translator, and the Modelica Buildings Library](../doc_files/modeling_des_paper.pdf). In Building Simulation 2021. Bruges, Brussels.

- Kathryn Hinkelman, Jing Wang, Chengliang Fan, Wangda Zuo, Antoine Gautier, Michael Wetter, Nicholas Long. (2021). [A Case Study on Condenser Water Supply Temperature Optimization with a District Cooling Plant.](https://doi.org/10.3384/ecp21181587) Proceedings of 14th Modelica Conference 2021, Linköping, Sweden, September 20-24, 2021, 181, 587–595.

- Allen, A., Long, N. L., Moore, N., & Elarga, H. (2021). [URBANopt District Energy Systems HVAC Measures](https://doi.org/10.11578/dc.20210127.1) National Renewable Energy Laboratory.

- Allen, A., Henze, G., Baker, K., Pavlak Gregory, & Murphy, M. (2021). [Evaluation of Topology Optimization to Achieve Energy Savings at the Urban District Level](https://www.nrel.gov/docs/fy21osti/77625.pdf). 2021 ASHRAE Winter Conference.

- Long, N., Almajed, F., von Rhein, J., & Henze, G. (2021). Development of a metamodelling framework for building energy models with application to fifth-generation district heating and cooling networks. Journal of Building Performance Simulation, 14(2), 203–225. [https://doi.org/10.1080/19401493.2021.1884291.](https://doi.org/10.1080/19401493.2021.1884291) [https://www.tandfonline.com/doi/abs/10.1080/19401493.2021.1884291](https://www.tandfonline.com/doi/abs/10.1080/19401493.2021.1884291)

- Allen, A., Henze, G., Baker, K., Pavlak, G., Long, N., & Fu, Y. (2020). A topology optimization framework to facilitate adoption of advanced district thermal energy systems. IOP Conference Series: Earth and Environmental Science, 588, 022054. [https://doi.org/10.1088/1755-1315/588/2/022054](https://doi.org/10.1088/1755-1315/588/2/022054)

- Allen, A., Henze, G., Baker, K., & Pavlak, G. (2020). Evaluation of low-exergy heating and cooling systems and topology optimization for deep energy savings at the urban district level. Energy Conversion and Management, 222, 113106. [https://doi.org/https://doi.org/10.1016/j.enconman.2020.113106](https://doi.org/https://doi.org/10.1016/j.enconman.2020.113106)

- Long, N., & Summer, T. (2020). Modelica Builder (0.1.0). [https://doi.org/10.11578/dc.20200409.1](https://doi.org/10.11578/dc.20200409.1)

- Long, N., & Summer, T. (2020). Modelica Formatter. [https://github.com/urbanopt/modelica-fmt](https://github.com/urbanopt/modelica-fmt)

- von Rhein, J., Henze, G. P., Long, N., Fu, Y. (2019). [Development of a topology analysis tool for fifth-generation district heating and cooling networks.](https://www.sciencedirect.com/science/article/pii/S0196890419306211?via%3Dihub). Energy Conversion and Management, 196 (March), 705–716.

- Buffa, S., Cozzini, M., D’Antoni, M., Baratieri, M., Fedrizzi, R. (2019). [5th generation district heating and cooling systems: A review of existing cases in Europe. Renewable and Sustainable Energy Reviews (pp. 504–522)](https://doi.org/10.1016/j.rser.2018.12.059). Elsevier Ltd.
