---
layout: default
title: Overview
has_children: true
has_toc: false
nav_order: 1
---

# URBANopt SDK Documentation

URBANopt (Urban Renewable Building And Neighborhood optimization) is an EnergyPlus and OpenStudio-based simulation platform aimed at district- and campus-scale thermal and electrical analysis.

Use the following links to find out more about the URBANopt project:
- [High-level introduction](https://www.nrel.gov/buildings/urbanopt.html) to the intent and purpose of URBANopt
- [URBANopt project website](https://www.energy.gov/eere/buildings/urbanopt) 
- [URBANopt Design overview](doc_files/design_doc.md)

**_URBANopt is not a standalone program for end users._** URBANopt Software Development Kit (SDK) is a package of modules that can be used to build user-friendly tools. Developers can use existing URBANopt modules, customize URBANopt modules, and create new modules to implement the desired workflows for their tools. Other users of the SDK could include researchers looking to create customized workflows to perform specific environmental design tasks.

URBANopt initially focuses on enabling three types of use cases:

- Design of low energy campuses and districts
- Design and optimization of grid-interactive efficient buildings (GEBs) at a district-scale in conjunction with distributed energy resources (DERs) and electric distribution systems
- Detailed design of next-generation district thermal systems

The URBANopt SDK can aid in the design of districts where the interactions between individual buildings, district energy systems, distributed energy resources, and electrical system designs are considered. Simulating these interactions allows URBANopt to address important questions in low energy, grid aware, future thinking urban districts such as:

- Tradeoffs between building height and photovoltaic (PV) production
- Investments in building efficiency vs. renewable generation
- Coordination of multiple buildings to optimize grid metrics
- Performance gains of shared thermal district systems vs conventional single building systems

Load diversity between commercial and residential buildings may allow for system time sharing or even complementary heat transfer between buildings using a district thermal energy system (see Figure 2 from the [URBANopt project website](https://www.nrel.gov/buildings/urbanopt.html) for an example).
