---
layout: default
title: Energy Resilience Planning
parent: REopt
grand_parent: Workflows
nav_order: 5
---
# REopt Energy Resilience Planning

This  document describes the integration of URBANopt with REopt's Energy Resilience Planning capability. 

ERP calculates outage survivability metrics, estimating how long an energy system can operate during a grid outage.
It models how resources such as solar, battery and generators can sustain critical loads during grid outages, considering technology reliability, configuration and dispatch strategies for detailed resilience planning.

To use ERP within URBANopt, a user can start by creating a REopt example project, which exposes the required ERP inputs. The steps below describe the workflow.

## URBANopt REopt Workflow

1. ### Create a REopt Example Project

    Create a REopt project by including the `--photovoltaic` flag (or `-v` as a shortcut) in the create command:

    ```bash
    uo create --project-folder <path/to/reopt/folder> --photovoltaic
    ```

2. ## Create Scenario Files

    An URBANopt scenario file is first created:

    ```bash
    uo create --scenario-file <path/to/FEATUREFILE.json>
    ```
    Next, the REopt ERP scenario file is created using the following command:

    ```bash
    uo create --reopt-erp-scenario-file <path/to/existing/SCENARIOFILE.csv>
    ```
    
    After running this command, a `reopt` folder will be created containing the REopt assumption files that include the required inputs and default values for running a REopt analysis. For ERP, two additional assumption files are included: `multiPV_assumptions_ERP.json` and `erp_assumptions.json`.

    The `multiPV_assumptions_ERP.json` defines an array of PV inputs used to optimize multiple PV systems simultaneously. The `multiPV_assumptions_ERP.json` is assigned to the URBANopt project features in the REopt ERP scenario file by default. The multiPV_assumptions_ERP.json file can be modified by the user as needed. More details on creating a REopt example project and the structure of these assumption files are available in the [REopt Post Processing Section](./reopt_post_processing.md).
 
    The `multiPV_assumptions_ERP.json` file also contains the key input fields required to run an ERP analysis, namely:

    `outage_start_time_steps`: A list of time steps that the grid outage may start. This input is used for robust optimization across multiple outages. The maximum (over outage_start_time_steps) of the expected value (over outage_durations with probabilities outage_probabilities) of outage cost is included in the objective function minimized by REopt

    `outage_durations`: One-to-one with outage_probabilities. A list of possible time step durations of the grid outage. This input is used for robust optimization across multiple outages. The maximum (over outage_start_time_steps) of the expected value (over outage_durations with probabilities outage_probabilities) of outage cost is included in the objective function minimized by REopt.

    `critical_load_fraction`: This is the fraction multiplied by the building loads on site, to determine the amount of critical loads that must be met during the outage. By default these are set to 1.0 in URBANopt.

    `urdb_label` this should be updated as per the site location. Instructions on making the update are provided in the [REopt Post Processing Section](./reopt_post_processing.md).

    More details on these inputs is provided in the <a href="https://developer.nrel.gov/api/reopt/stable/help/?API_KEY=DEMO_KEY" class="bold" target="_blank" rel="noopener noreferrer">REopt API documentation</a>.

    The `erp_assumptions.json` is a separate file used to run the ERP workflow and contains the following:

    `max_outage_duration`. This is a required input to run the ERP capability. By default it is set to 24 hours. This can be updated by the user and provided as a flag when running the ERP analysis. If this file is not added as an argument while running the ERP capability, a default of 24 hours is assumed.

    The ERP capabilities can be used in two main ways:

    **1: Include outage inputs during the REopt optimization**

    In this approach, the outage duration and start timestep are specified in the assumption file. REopt sizes the energy systems (PV, battery, generators) to meet both economic objectives and the requirement to survive the specified outage. REopt sizes PV and storage *optimally to survive those outages*, assuming perfect foresight. After optimization, the REopt ERP command is run to calculate the resilience metrics for the system that was explicitly designed to withstand the outage.

    **2: Run ERP on an existing system without outage-based sizing.**

    REopt sizes PV and storage purely based on economic objectives (e.g., cost savings, emissions), with no resilience requirement. When running the ERP post processing, REopt uses the system sizes and dispatch profile generated previously, It then simulates outages starting at every hour of the year, using the actual battery state of charge at each hour. This is often useful if the user wants to first size for economics, then see how resilient the system is without explicitly the sizing toward resilience. <em>For this option, use an assumptions file that does not specify an outage duration and start timestep.</em>

    In both cases the critical load definition need to be provided

3. ### Run URBANopt project
 
    The URBANopt project is run by specifying the feature file and the REopt ERP Scenario file:

    ```bash
    uo run --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv>
    ```

4. ### Default Post Process Results

    Default post processing to generate URBANopt Results is done:

    ```bash
    uo process --default --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv>
    ```

5. ### REopt Post Process Results

    The Reopt Scenario or Feature Post Processing is done with the --reopt-resilience flag to include the ERP capability as follows:

    To run the Scenario Post Processing:

    ```bash
    uo process --reopt-scenario --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv> --reopt-resilience
    ```

    To run the Feature Post Processing:

    ```bash
    uo process --reopt-feature --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv> --reopt-resilience
    ```

    Optionally, a user-specified REopt ERP assumption file, can be provided. The format for the assumption file must follow what is used in `erp_assumptions.json`. If an assumption file is not provided, the `erp_assumptions.json` is used by default. To specify your own ERP assumptions file, use the following command:

    ```bash
    uo process --reopt-scenario --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv> --reopt-resilience --reopt-erp-assumptions-file <path/to/erp_assumptionsFile.json>
    ```

    On completing the simulation worklow, a resilience report: `scenario_report_reopt_erp_run_resilience.json` is generated in the `reopt` folder in the URBANopt Scenario run directory. This file contains the resilience metrics reported from the ERP simulation. Some of the outputs include:

    **unlimited_fuel_mean_cumulative_survival_by_duration**:  The mean, calculated over outages starting at each hour of the year, of the probability of surviving up to and including each hour of max_outage_duration, if generator fuel is unlimited

    **unlimited_fuel_cumulative_survival_final_time_step**: The probability of surviving the full max_outage_duration, for outages starting at each hour of the year, if generator fuel is unlimited.

    **mean_fuel_survival_by_duration**: The probability, averaged over outages starting at each hour of the year, of having sufficient fuel to survive up to and including each hour of max_outage_duration.

    More details about the outputs can be found in the <a href="https://developer.nrel.gov/api/reopt/stable/erp/outputs?API_KEY=DEMO_KEY" class="bold" target="_blank" rel="noopener noreferrer">REopt API documentation</a>.

For more details on the REopt ERP implementation, visit the <a href="https://github.com/NREL/REopt-Analysis-Scripts/wiki/4.-API-endpoints#hoststableerp" class="bold" target="_blank" rel="noopener noreferrer">REopt ERP API Endpoints</a> documentation.