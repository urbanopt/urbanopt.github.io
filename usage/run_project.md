---
layout: default
title: Running a Project
parent: Usage
nav_order: 2
---

To run URBANopt<sup>&trade;</sup>, first [install](../installation/installation.md) the project dependencies and the URBANopt Command Line Interface.

Once the CLI is installed, help is available by typing `uo --help` from the command line. Detailed help for each command can be found with `uo <command> --help`

1. Create a project folder in your current directory using:

    ```terminal
    uo create --project-folder <path/to/PROJECT_DIRECTORY_NAME>
    ```

    This creates a project folder containing the [example project](example.md) using the
    default geometry workflow with `urban-geometry-creation-zoning` measure, and downloads
    related weather files and detailed models to the appropriate folders.

    Users can add a subcommand to use an alternate geometry creation workflow. The following
    alternate geometry methods can be used:

   * **createbar** 

    This uses the `create_bar_from_building_type_ratio` measure to create building geometry.
    ```terminal
    uo create --project-folder --createbar <path/to/PROJECT_DIRECTORY_NAME>
    ```
   
   * **floorspace** 

    This creates building geometry from floor plans with stub space types drawn using
    `FloorSpaceJS`.
    
    ```terminal
    uo create --project-folder --floorspace <path/to/PROJECT_DIRECTORY_NAME>
    ```
    For more details on these geometry workflows, refer to [geometry_workflows](geometry_workflows.md).


    Create an empty base project folder by using:

    ```terminal
    uo create --empty --project-folder <path/to/PROJECT_DIRECTORY_NAME>
    ```
    
    This creates project folder without an example FeatureFile and an empty weather folder. You can
    download weather files and add to this folder from energyplus.net/weather.

    Overwrite an existing folder by using:

    ```terminal
    uo create --overwrite --project-folder <path/to/PROJECT_DIRECTORY_NAME>
    ```

    This deletes anything in the named folder and creates a fresh project directory. Can be combined
    with `-e` to overwrite a directory with a new empty URBANopt project directory.
    
    *Note: Keep the length of the path to the project directory short to avoid an error due to long
    paths, especially while running in Windows. For more information on this error, refer to [known issues](../developer_resources/known_issues.md)*.

1. Put your [FeatureFile](../overview/definitions.md) in the root of the folder you just created, or use the provided example.
1. **We recommend calling all URBANopt commands from _outside_ the project folder you created, using relative or absolute paths to the relevant files.**

1. There are two ways to create [ScenarioFiles](../overview/definitions.md). One way, which creates a ScenarioFiles for all Features in the FeatureFile based off the example _mappers_, is to run:

    ```terminal
    uo create --scenario-file <path/to/FEATUREFILE.json>
    ```

    Alternatively, to create a ScenarioFile for a single [Feature](../overview/definitions.md), specify the Feature_ID in the arguments as shown here:

    ```terminal
    uo create --scenario-file <path/to/FEATUREFILE.json> --single-feature <FEATURE_ID>
    ```

    If you will be post-processing with **REopt Lite** data, you will need to also now convert your scenario CSV file with:

    ```terminal
    uo create --reopt-scenario-file <path/to/EXISTING_SCENARIO_FILE.csv>
    ```

    This command will create a new scenario CSV (named REopt_scenario.csv by default) that has an extra column to map assumption files to features. Use this scenario CSV file going forward in future steps.

    You may write your own mapper file for your own specific use case as needed, as well as make your own ScenarioFile by hand.  You may also make edits to the ScenarioFiles to mix and match mappers.

1. Simulate energy usage of each feature or for a single Feature by specifying the appropriate ScenarioFile by using:

    ```terminal
    uo run --feature <path/to/FEATUREFILE.json> --scenario <path/to/SCENARIOFILE.csv>
    ```

    **Note:** To simulate energy usage for a scenario that will require additional **REopt Lite** post-processing, run this command instead.

    ```terminal
    uo run --reopt --feature <path/to/FEATUREFILE.json> --scenario <path/to/SCENARIOFILE.csv>
    ```

    Also note that there is a *runner.conf* file automatically created in the project folder.  This file is used to configure the number of features to process in parallel as well as a few other parameters.  Make edits to this file prior to running the above command.

1. If you intent to post process with **REopt Lite** (i.e. using `reopt-scenario`, `reopt-feature`), please now refer to the instructions outlined in [REopt Post Processing](../reopt/reopt_post_processing.md). Otherwise, post-process simulated features into a [Scenario](../overview/definitions.md) report by using `default` as the `type` :

    ```terminal
    uo process --<TYPE> --feature <path/to/FEATUREFILE.json> --scenario <path/to/SCENARIOFILE.csv>
    ```

    Valid `TYPE`s are: `default`, `opendss`, `reopt-scenario`, `reopt-feature`
1. Visualize the results of post-processing:

    * To visualize and compare the results of post-processing for **all scenarios**:

    ```terminal
    uo visualize --scenario <path/to/FEATUREFILE>
    ```
    The resulting visualizations can be viewed in the `scenario_comparison.html` file in the run folder.

    * To visualize and compare the results of post-processing for **all features** in the scenario:

    ```terminal
    uo visualize --feature <path/to/SCENARIOFILE>
    ```
    The resulting visualizations can be viewed in the `feature_comparison.html` file in the scenario folder.

    *Note : You need to run the `default` post-process command before visualizing the results.* 

1. Delete an outdated [Scenario](../overview/definitions.md) run by using:

    ```terminal
    uo delete --scenario <path/to/SCENARIOFILE>
    ```

# Workflow Details

The figure below describes the workflow that takes place for the *run* and *post_process* calls.

![workflow_diagram](../doc_files/CLI_workflow_diagram.jpg)


The following figure represents how Simulation Mapper Classes can be assigned to different Features from the FeatureFile in the Scenario CSV.

![scenario_mapper](../doc_files/scenario_mapper.jpg)
