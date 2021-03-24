---
layout: default
title: Home
has_children: false
has_toc: false
nav_order: 0
---

<img src="doc_files/URBANopt-Logo-Horizontal-2Color.jpg" width="80%" height="80%" alt="urbanopt logo">

# URBANopt SDK Documentation

URBANopt<sup>&trade;</sup> (Urban Renewable Building And Neighborhood optimization) is an EnergyPlus<sup>&trade;</sup>- and OpenStudio<sup>&copy;</sup>-based simulation platform aimed at district- and campus-scale thermal and electrical analysis for Community and Urban District Energy Modeling.

**_URBANopt is not a standalone program for end users._** URBANopt is a Software Development Kit (SDK) &mdash; a collection of open source modules focused on underlying analytics for a variety of multi-building design and analysis use cases. Commercial software developers can use existing URBANopt modules, customize URBANopt modules, and create new modules to help implement the desired workflows for their end user tools. Other users of the SDK could include researchers looking to create customized workflows to perform specific environmental design tasks.

<img src="doc_files/uo_highlevel.png" alt="graphic showing urbanopt at a high level" />


## Use Cases

The URBANopt project is focused on enabling three primary use cases:

1. Design of low energy campuses and districts through multi-building efficiency scenario analysis 
2.  Design and optimization of grid-interactive efficient buildings (GEBs) at a district-scale in conjunction with distributed energy resources (DERs) and electric distribution systems
3. Detailed design of next-generation district thermal systems

<img src="doc_files/use_cases.png" alt="main urbanopt capabilities" width="100%" height="100%">


The URBANopt ecosystem is shown below. 

<img src="doc_files/uo_ecosystem.png" width="100%" height="100%" alt="uo ecosystem diagram">

Current capabilities include:

- Commercial and Residential Building modeling analyses
- Grid-Interactivity analyses (via REopt and OpenDSS)
- District Energy Systems design (via DES)
- Optimization (OSAF)
- Electric Vehicle Storage and Thermal Energy Storage

## Quicklinks

[Getting Started](./getting_started/getting_started){: .btn .btn-uo .white-text} &mdash; Visit the [Getting Started page](./getting_started/getting_started) for detailed instructions on how to use URBANopt in a variety of workflows.  

[Workflows](./workflows/workflows){: .btn .btn-uo .white-text} &mdash; For more details about the workflows enabled through URBANopt, visit the [Workflows](./workflows/workflows) section.

[Resources](./resources/about){: .btn .btn-uo .white-text} &mdash; Visit the [Resources](./resources/about) section for general information on URBANopt structure and customizations.

[For Developers](./developer_resources/developer_resources){: .btn .btn-uo .white-text} &mdash; Visit the [Developer Resources](developer_resources/developer_resources) section for details on how to develop and test new URBANopt functionality as well as a compatibility matrix for all URBANopt dependencies.
