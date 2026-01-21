---
layout: default
title: Energy Resilience Planning
parent: REopt
grand_parent: Workflows
nav_order: 5
---
# URBANopt&trade; REopt&reg; Energy Reliability and Backup Power Planning

This document describes the integration of URBANopt with REopt's Energy Resilience Planning (ERP) capability. 

ERP calculates outage survivability metrics, estimating how long an energy system can operate during a grid outage.
It models how resources such as fuel-fired generators and batteries can sustain critical loads during grid outages, considering technology reliability, configuration, and dispatch strategies for detailed planning.

To use ERP within URBANopt, a user can start by creating a REopt example project, which exposes the required ERP inputs. The steps below describe the workflow.

## URBANopt REopt Workflow

1. ### Create a REopt Example Project

    Create one of the example projects. The example command command below creates the most basic URBANopt project:

    ```bash
    uo create --project-folder <path/to/reopt/folder>
    ```

2. ### Create Scenario Files

    Create the URBANopt example scenario files:

    ```bash
    uo create --scenario-file <path/to/FEATUREFILE.json>
    ```
    Next, create the REopt ERP-enabled scenario file based on the desired example scenario (ex: Baseline scenario) using the following command:

    ```bash
    uo create --reopt-erp-scenario-file <path/to/existing/SCENARIOFILE.csv>
    ```
    
    Running this command will create a copy of an existing scenario file and add the 'REopt Assumptions' column to it. After running this command, a `reopt` folder will also be created. This folder contains the REopt assumption files that include the required inputs and default values for running a REopt analysis. For the ERP workflow, two additional assumption files are included: `multiPV_assumptions_ERP.json` and `erp_assumptions.json`. 

    **MultiPV_assumptions_ERP.json File**

    The `multiPV_assumptions_ERP.json` defines an array of PV inputs used to optimize multiple PV systems simultaneously. The `multiPV_assumptions_ERP.json` is assigned to the URBANopt project features in the REopt ERP scenario file by default. The multiPV_assumptions_ERP.json file can be modified by the user as needed, or a new assumption file can be created by the user and assigned in the REopt ERP scenario file. More details on creating a REopt example project and the structure of these assumption files are available in the [REopt Post Processing Section](./reopt_post_processing.md).
 
    In particular, the `multiPV_assumptions_ERP.json` file contains the key input fields required to run an ERP analysis:

    `outage_start_time_steps`: A list of starting time steps representing when the grid outage may start. This input is used for robust optimization across multiple outages. For example, i f the `timestep_per_hour` field is set to 1, then an `outage_start_time_steps` value of [1000, 2500] would indicate that outages will start at hour 1000 and hour 2500 of the simulation.

    `outage_durations`: A list of possible durations for grid outages during the simulation. For each start time listed in the `outage_start_time_steps` field above, an outage duration value should be listed in the `outage_durations` field. For example, if the `outage_durations` field is set to [24, 48], the outage starting at hour 1000 will last 24 hours, and the outage starting at hour 2500 will last 48 hours. The maximum (over `outage_start_time_steps`) of the expected value (over `outage_durations` with probabilities `outage_probabilities`) of outage cost is included in the objective function minimized by REopt. By default the `outage_probabilities` field has not be included in the URBANopt-REopt assumptions file as it defaults to giving each outage equal probability. If the user would like to vary the probability of each outage specified in the `outage_start_time_steps`, they can add a new field named `outage_probabilities` containing a list of probabilities for each outage.

    `critical_load_fraction`: This is the fraction multiplied by the building loads on site, to determine the amount of critical loads that must be met during the outage. By default this is set to 1.0 in the URBANopt-REopt workflow.

    `urdb_label` this should be updated to match the site location. Instructions on making the update are provided in the [REopt Post Processing Section](./reopt_post_processing.md).

    More details on these inputs are provided in the <a href="https://developer.nrel.gov/api/reopt/stable/help/?API_KEY=DEMO_KEY" class="bold" target="_blank" rel="noopener noreferrer">REopt API documentation</a>.

    **ERP_assumptions.json File**

    The `erp_assumptions.json` is a separate file used to run the ERP workflow and contains the following:

    `max_outage_duration`. This is a required input to run the ERP capability. It is set to 24 (hours) by default. If this file is not added as an argument while running the ERP capability, a default of 24 hours is assumed.

    **ERP Workflows**

    The ERP capabilities can be used in two main ways:

    **1 &mdash; Include outage inputs during the REopt optimization**

    In this approach, the outage duration and start timestep are specified in the assumptions file. REopt sizes the energy systems (generators, PV, battery) to meet both economic objectives and the requirement to survive the specified outage. REopt sizes PV and storage *optimally to survive those outages*, assuming perfect foresight. After optimization, the REopt ERP command is run to calculate the backup power metrics for the system that was explicitly designed to withstand the outage.

    **2 &mdash; Run ERP on an existing system without outage-based sizing**

    REopt sizes generation and storage purely based on economic objectives (e.g., cost savings), with no requirement to survive a specified outage. When running the ERP post processing, REopt uses the system sizes and dispatch profile generated previously. It then simulates outages starting at every hour of the year, using the actual battery state of charge at each hour. This is often useful if the user wants to first size for economics, then see how the system might be able to provide backup power without explicitly sizing to survive a specified outage. <em>For this option, use an assumptions file in the Scenario File that does not specify an outage duration and start timestep.</em>

    In both cases the critical load fraction input needs to be provided.

3. ### Run URBANopt project
 
    The URBANopt project is run by specifying the feature file and the REopt ERP Scenario file:

    ```bash
    uo run --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv>
    ```

4. ### Default Post Process Results

    Default post processing to generate URBANopt results is performed via the following command:

    ```bash
    uo process --default --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv>
    ```

5. ### REopt Post Process Results

    The Reopt Scenario or Feature Post Processing optimizations are performed with the --reopt-backup-power flag to include the ERP capability as follows:

    To run the Scenario Post Processing:

    ```bash
    uo process --reopt-scenario --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv> --reopt-backup-power
    ```

    To run the Feature Post Processing:

    ```bash
    uo process --reopt-feature --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv> --reopt-backup-power
    ```
    For both the Scenario Post Process and Feature Post Processing, the `erp_assumptions.json` file from the `reopt` folder is used by default without needing to specify it. If users want to specify their own assumption file, they can do so using the `reopt-erp-assumptions-file` flag in the process command:

    ```bash
    uo process --reopt-scenario --feature <path/to/FEATUREFILE.json> --scenario <path/to/ReoptERPScenarioFile.csv> --reopt-backup-power --reopt-erp-assumptions-file <path/to/erp_assumptionsFile.json>
    ```

    On completing the simulation worklow, an ERP report: `scenario_report_reopt_erp_run_resilience.json` is generated in the `reopt` folder of the URBANopt Scenario run directory. This file contains the backup power metrics reported from the ERP simulation. Some of the outputs include:

    **unlimited_fuel_mean_cumulative_survival_by_duration**:  The mean, calculated over outages starting at each hour of the year, of the probability of surviving up to and including each hour of max_outage_duration, if generator fuel is unlimited

    **unlimited_fuel_cumulative_survival_final_time_step**: The probability of surviving the full max_outage_duration, for outages starting at each hour of the year, if generator fuel is unlimited.

    **mean_fuel_survival_by_duration**: The probability, averaged over outages starting at each hour of the year, of having sufficient fuel to survive up to and including each hour of max_outage_duration.

    More details about the outputs can be found in the <a href="https://developer.nrel.gov/api/reopt/stable/erp/outputs?API_KEY=DEMO_KEY" class="bold" target="_blank" rel="noopener noreferrer">REopt API documentation</a>.

For more details on the REopt ERP implementation, visit the <a href="https://github.com/NREL/REopt-Analysis-Scripts/wiki/4.-API-endpoints#hoststableerp" class="bold" target="_blank" rel="noopener noreferrer">REopt ERP API Endpoints</a> documentation.