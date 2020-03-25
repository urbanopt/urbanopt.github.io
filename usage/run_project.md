---
layout: default
title: Running a Project
parent: Usage
nav_order: 2
---

To run URBANopt, first [install](../installation/installation.md) the project dependencies and the URBANopt Command Line Interface.

Once the CLI is installed, help is available by typing `uo -h` from the command line.

1. Create a project folder in your current directory using:

    ```terminal
    uo -p <PROJECT_DIRECTORY_NAME>
    ```

    This creates a project folder containing the [example project](example.md), and downloads related weather files and detailed models to the appropriate folders.

    Alternatively, create an empty base project folder by using: 
    ```terminal
    uo -e -p <PROJECT_DIRECTORY_NAME>
    ```
    
    This creates project folder without an example FeatureFile and an empty weather folder. You can
    download weather files and add to this folder from energyplus.net/weather.
1. Put your [FeatureFile](../overview/definitions.md) in the root of the folder you just created, or use the provided example.
1. For all following commands you must be _inside the project directory_ you created in step 1.
1. Create [ScenarioFiles](../overview/definitions.md) for all Features in the FeatureFile based off the example _mappers_ using:

    ```terminal
    uo -m -f <FEATUREFILE>
    ```

    Or create ScenarioFiles for a single [Feature](../overview/definitions.md) by specifying the Feature ID in the arguments.

    ```terminal
    uo -m -f <FEATUREFILE> -i <FEATURE ID>
    ```

    You may write your own mapper file for your own specific use case as needed, as well as make your own ScenarioFile by hand.  You may also make edits to the ScenarioFiles to mix and match mappers.

1. Simulate energy usage of each feature or for a single Feature by specifying the appropriate
   ScenarioFile by using:

    ```terminal
    uo -r -f <FEATUREFILE> -s <SCENARIOFILE>
    ```
    Note that there is a *runner.conf* file automatically created in the project folder.  This file is used to configure the number of features to process in parallel as well as a few other parameters.  Make edits to this file prior to running the above command.

    Once the simulations completes, the results can be found in the ```run/<SCENARIO_NAME>/<FEATURE_NAME>/default_feature_reports``` directory. The JSON file contains high-level results of the simulation, and the CSV file contains timeseries results.  All timeseries results are in SI units, except for gas timeseries which are in kBtu.

1. Aggregate simulated features into a [Scenario](../overview/definitions.md) report by using:

    ```terminal
    uo -a -f <FEATUREFILE> -s <SCENARIOFILE>
    ```

    The aggregated scenario results can then be found in the ```run/<SCENARIO_NAME>``` directory.  The default_scenario_report JSON file contains high-level aggregated results and the CSV file contains aggregated timeseries results.


1. Delete an outdated [Scenario](../overview/definitions.md) run by using:

    ```terminal
    uo -d -s <SCENARIOFILE>
    ```

# Workflow Details

The figure below describes the workflow that takes place for the *run* and *post_process* calls.

![workflow_diagram](../doc_files/CLI_workflow_diagram.jpg)


The following figure represents how Simulation Mapper Classes can be assigned to different Features from the FeatureFile in the Scenario CSV.

![scenario_mapper](../doc_files/scenario_mapper.jpg)
