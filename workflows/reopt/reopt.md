---
layout: default
title: REopt
has_children: true
parent: Workflows
nav_order: 1
---

<div style="text-align:center"><img src="../doc_files/reopt-lite-logo.png" /></div>

**REopt Lite<sup>TM</sup>** is a technoeconomic decision making model for distributed energy resource (DER) design accessible via [web tool](https://reopt.nrel.gov/tool) and [API](https://developer.nrel.gov/docs/energy-optimization/reopt-v1/). Given a set of inputs it leverages mixed-integer linear programming to select the optimal design and hourly annual dispatch of solar PV, storage, wind and diesel generator technologies - it also provides key economic metrics for the system. Integration of this model with URBANopt allows individual buildings (i.e _Feature Reports_) and collections of buildings (i.e. _Scenario Reports_) to be assessed for cost-optimal DER solutions. 

The [URBANopt REopt Gem](https://github.com/urbanopt/urbanopt-reopt-gem) extends the [URBANopt Scenario Gem](https://github.com/urbanopt/urbanopt-scenario-gem) and is intended to be used as a post-processor on _Feature_ and _Scenario_ Reports. The **URBANopt REopt Gem** can be used directly as shown in the [example REopt project](https://github.com/urbanopt/urbanopt-example-reopt-project) or through the [URBANopt CLI](https://github.com/urbanopt/urbanopt-cli). The **URBANopt REopt Gem**  comes with the following functionality:

- Reads _Feature_ and _Scenario_ Reports and parses their latitude, longitude, electric load profile, and available roof area to use as inputs to the **REopt Lite API**. These values can be overwritten with settings from the assumptions file.
- Reads _Feature_ and _Scenario_ Reports populates the ElectricTariff > coincident_peak_load_active_timesteps input to the **REopt Lite API** with the 1-indexed indices of the top 100 timesteps with the largest power demands. At finer **REopt Lite** modeling resolutions than 1 hour, the number of timesteps in this array is determined as 100 * time_steps_per_hour. This value can be overwritten with settings from the assumptions file.
- Makes calls to the **REopt Lite API** for optimal distributed energy resource (DER) technology sizing, dispatch, and financial metrics based using additional customizable input parameters stored in an input `.json` file
- Optionally makes calls to the **REopt Lite API** for resilience statistics on an optimized system (i.e. average outage duration sustained by system)
- Saves responses from the **REopt Lite API** to local files (by default in a `reopt` folder in the Scenario or Feature Report directory)
- Updates Feature or Scenario Report's `distributed_generation` attributes based on a **REopt Lite API** response
- Updates a Feature or Scenario Report's `timeseries_csv` based on a **REopt Lite API** response
