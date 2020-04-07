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
    uo -p <path/to/PROJECT_DIRECTORY_NAME>
    ```

    This creates a project folder containing the [example project](example.md), and downloads related weather files and detailed models to the appropriate folders.

    Create an empty base project folder by using:

    ```terminal
    uo -e -p <path/to/PROJECT_DIRECTORY_NAME>
    ```
    
    This creates project folder without an example FeatureFile and an empty weather folder. You can
    download weather files and add to this folder from energyplus.net/weather.

    Overwrite an existing folder by using:

    ```terminal
    uo -o -p <path/to/PROJECT_DIRECTORY_NAME>
    ```

    This deletes anything in the named folder and creates a fresh project directory. Can be combined with `-e` to overwrite a directory with a new empty URBANopt project directory.

1. Put your [FeatureFile](../overview/definitions.md) in the root of the folder you just created, or use the provided example.
1. We recommend calling all URBANopt commands from _outside_ the project folder you created, using absolute paths to the relevant files.
1. Create [ScenarioFiles](../overview/definitions.md) for all Features in the FeatureFile based off the example _mappers_ using:

    ```terminal
    uo -m -f <path/to/FEATUREFILE>
    ```

    Or create ScenarioFiles for a single [Feature](../overview/definitions.md) by specifying the Feature_ID in the arguments.

    ```terminal
    uo -m -f <path/to/FEATUREFILE> -i <FEATURE_ID>
    ```

    You may write your own mapper file for your own specific use case as needed, as well as make your own ScenarioFile by hand.  You may also make edits to the ScenarioFiles to mix and match mappers.

1. Simulate energy usage of each feature or for a single Feature by specifying the appropriate
   ScenarioFile by using:

    ```terminal
    uo -r -f <path/to/FEATUREFILE> -s <path/to/SCENARIOFILE>
    ```

    Note that there is a *runner.conf* file automatically created in the project folder.  This file is used to configure the number of features to process in parallel as well as a few other parameters.  Make edits to this file prior to running the above command.

1. Gather simulated features into a [Scenario](../overview/definitions.md) report by using:

    ```terminal
    uo -g -t <TYPE> -f <path/to/FEATUREFILE> -s <path/to/SCENARIOFILE>
    ```

    Valid `TYPE`s are: `default`, `opendss`, `reopt-scenario`, `reopt-feature`

1. Delete an outdated [Scenario](../overview/definitions.md) run by using:

    ```terminal
    uo -d -s <path/to/SCENARIOFILE>
    ```

# Workflow Details

The figure below describes the workflow that takes place for the *run* and *post_process* calls.

![workflow_diagram](../doc_files/CLI_workflow_diagram.jpg)


The following figure represents how Simulation Mapper Classes can be assigned to different Features from the FeatureFile in the Scenario CSV.

![scenario_mapper](../doc_files/scenario_mapper.jpg)
