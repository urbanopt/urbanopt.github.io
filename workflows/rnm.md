---
layout: default
title: RNM
parent: Workflows
nav_order: 7
---

# RNM Analysis

## Usage

The RNM functionality is available workflow is available via the `opendss` URBANopt CLI command. For in-terminal help:
```bash
uo rnm --help
```


An example:
1. Create an rnm example project by including the `-t` flag (or --streets) to include streets in the FeatureFile:
	```bash
	uo create --project-folder <path/to/new/project> --streets
	```

1. Run the project as you normally would, making sure you use the streets feature file, and a scenario file created from it.  RNM supports both the basic workflow and the REopt functionality
	```bash
	uo run --feature <path/to/streets/FEATUREFILE.json> --scenario <path/to/SCENARIOFILE.csv>
	```
1. Post-process using the default post-processor to generate the feature_reports used by the RNM workflow:
	```bash
	uo process --default --feature <path/to/streets/FEATUREFILE.json> --scenario <path/to/SCENARIOFILE.csv>
	```

1. Use the `rnm` command to run the RNM analysis.  You can access the list of options and guidance with the following command:

	```bash
	uo rnm -h
	```

	The CLI options are:
	- `--scenario` &mdash; Required, Path to scenario csv file
	- `--feature` &mdash; Required, Path to feature json file
	- `--reopt` &mdash; Optional, Use to specify a run with additional REopt functionality.
	- `--extended-catalog` &mdash; Optional, Use to specific the path to the extended electrical catalog to use in the analysis. Do not include this option if you want to use the default catalog.
	- `--average-peak-catalog` &mdash; Optional, Use to specific the path to the average peak catalog to use in the analysis. Do not include this option if you want to use the default catalog.
	- `--opendss` &mdash; Optional, If specified, an OpenDSS-compatible electrical database will be created for use in future OpenDSS analyses.

Visit the [Getting Started page](../getting_started/getting_started) for detailed usage examples.

## Background information

