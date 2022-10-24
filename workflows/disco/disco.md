---
layout: default
title: DISCO
nav_order: 3
parent: Workflows
---

# DISCO

[DISCO](https://github.com/NREL/disco) (Distribution Integration Solution Cost Options) is an open-source, NREL-developed,
python-based software tool for automating distribution analyses at scale. Originally developed to
support photovoltaic (PV) impact analyses, DISCO can also be used to understand the impact of other
distributed energy resources on distribution networks. DISCO analysis is based
on power flow modeling with OpenDSS used as the simulation engine. PyDSS is used to interface with
OpenDSS and provide additional control layers.

DISCO combines many features in forms of analysis modules such as snapshot and dynamic hosting
capacity analysis, grid impact analysis, automated upgrade cost analysis, and cost-benefit analysis
for selected grid control strategies.

The URBANopt-DISCO integration leverages the automated upgrade cost analysis module to determine the distribution system upgrades required
to mitigate any voltage and thermal violations that exist on the feeder and then calculate the cost
associated with those upgrades. This currently only considers traditional infrastructure upgrade
options (reconductoring, upgrade transformers, installing voltage regulators and capacitor banks,
and changing the controls or setpoints on voltage regulators and capacitor banks).
