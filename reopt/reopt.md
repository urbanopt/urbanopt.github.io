---
layout: default
title: REopt
has_children: true
nav_order: 4
---

[REopt Lite](https://reopt.nrel.gov/tool) is a technoeconomic decision making model for distributed energy resource (DER) deployment. Given a set of inputs it leverages mixed-integer linear programming to select the optimal design and hourly annual dispatch of solar PV, storage, wind and diesel generator technologies - it also provides key economic metrics for the system. Integration of this model with URBANopt allows individual buildings (i.e Feature Reports) and collections of buildings (i.e. Scenario Reports) to be assessed for cost-optimal DER solutions. 

The [URBANopt REopt Gem](https://github.com/urbanopt/urbanopt-reopt-gem) can be used directly as shown in the [example REopt project](https://github.com/urbanopt/urbanopt-example-reopt-project) or through the [URBANopt CLI](https://github.com/urbanopt/urbanopt-cli). The **URBANopt REopt Gem** extends the [URBANopt Scenario Gem](https://github.com/urbanopt/urbanopt-scenario-gem) with the following functionality:

- Reads Feature and Scenario Reports and parses their latitude, longitude, electric load profile, and available roof area
- Makes calls to the [REopt Lite API](https://developer.nrel.gov/docs/energy-optimization/reopt-v1/) for optimal distributed energy resource (DER) technology sizing, dispatch, and financial metrics based on parameters stored in an input `.json` file
- Optionally makes calls to the **REopt Lite API** for resilience statistics on an optimized system
- Saves responses from the **REopt Lite API** to a local file (by default in a `reopt` folder in the Scenario or Feature Report directory)
- Updates Feature or Scenario Report's `distributed_generation` attributes based on a **REopt Lite API** response
- Updates a Feature or Scenario Report's `timeseries_csv` based on a **REopt Lite API** response

