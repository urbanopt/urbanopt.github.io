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
    uo -p <FOLDERNAME>
    ```
    This creates a project folder with an example FeatureFile, and downloads weather files to the
    Weather folder for the sample FeatureFile.

    Alternatively, create an empty base project folder by using: 
    ```terminal
    uo -e -p <FOLDERNAME>
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

1. Aggregate simulated features into a [Scenario](../overview/definitions.md) report by using:

    ```terminal
    uo -a -f <FEATUREFILE> -s <SCENARIOFILE>
    ```

1. Delete an outdated [Scenario](../overview/definitions.md) run by using:

    ```terminal
    uo -d -s <SCENARIOFILE>
    ```

# Workflow Details

The figure below describes the workflow that takes place for the *run* and *post_process* calls.

![workflow_diagram](../doc_files/CLI_workflow_diagram.jpg)


The following figure represents how Simulation Mapper Classes can be assigned to different Features from the FeatureFile in the Scenario CSV.

![scenario_mapper](../doc_files/scenario_mapper.jpg)
