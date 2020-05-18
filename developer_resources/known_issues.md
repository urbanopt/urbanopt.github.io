---
layout: default
title: Known Issues
parent: Developer Resources
nav_order: 3
---

## Known Issues and Notes

### Version 0.2.0

1.	New example project files: The URBANopt SDK version 0.2.0 release comes with new files for the example project (new mappers and a new base_workflow.osw file).  For compatibility purposes and to use all new features, you may want to update any existing projects with these new files.  The example project can be installed via the CLI with the following command:

```bash
	uo create --project-folder <path/to/PROJECT DIR> 
```

1.	Project filepath length issue: Users (windows users especially) may run into an error while running URBANopt.  The error will be encountered either when running ‘bundle install’ in the project directory or in the in.osw.log file of a specific feature simulation and will look like this:
Errno::ENOENT: No such file or directory @ rb_sysopen –
This will occur during installation of either the openstudio-standards gem or the model-articulation gem.  This error occurs because the filepath to your project directory is too long.  Move the directory to a shallower path on your system and try again.

1.	Walk-in refrigeration modeling: The "create_typical_building_from_model" measure in the model articulation gem that supports OpenStudio 2.9.1 adds in capability for modeling walk-in refrigeration to a select number of space types. However, this functionality requires discrete space types to be modeled, and not a whole building or whole story blended space types. As a result, when using the default URBANopt workflow that models a blended space type based on the GeoJSON footprints, the new functionality will not be enabled. If a workflow using the GeoJSON floor area and number of floors is used with the create_bar_from_building_type_ratios measure, then walk-in refrigeration can be added in URBANopt using this version of the model articulation gem.

 1. URBANopt CLI users may see 2 lines of warning regarding `DEVELOPER_NREL_KEY`. This does not affect operation in any way. A future patch will remove the warning.

 1. Some users may experience issues with running the CLI from within the project directory. Try moving to a higher directory and using absolute paths to the feature_file (-f) and scenario CSV (-s).

 1. Many warnings get printed to the terminal when calling the URBANopt CLI from inside your project directory. The work-around to avoid these warnings is to move outside the project directory and use absolute paths when calling URBANopt commands. A hypothetical example: `uo -r -s ~/user/uo-project-directory/baseline_scenario.csv -f ~/user/uo-project-directory/example_project.json`
