---
layout: default
title: Release Notes
parent: For Developers
nav_order: 3
---

## Release Notes and Known Issues

### All Versions

1. **CERTIFICATE HAS EXPIRED** You may encounter a 'certificate has expired' error when running REopt&trade; optimizations. The error text will look something like this:

	```bash
	 error from REopt API: SSL_connect returned=1 errno=0 state=error: certificate verify failed (certificate has expired)
	 ```
	To resolve this issue in DEVELOPMENT mode, run the following command and then try again:

	```bash
	bundle exec certified-update
	```
	
	If you are using an URBANopt installer, locate the path within the installed application of the 'certified-update' executable and run it.  For example, on MAC the path would be something like:
	```bash
	/Applications/URBANoptCLI_X.X.X/gems/ruby/2.7.0/gems/certified-1.0.0/bin/certified-update
	```
	And on windows it may look like:
	```bash
	c:\urbanopt-cli-X.X.X\gems\ruby\2.7.0\gems\certified-1.0.0\bin\certified-update
	```

1. **LONG FILEPATHS ISSUE** Users (windows users especially) may run into an error while running URBANopt.  The error will be encountered either when running ‘bundle install’ in the project directory or in the in.osw.log file of a specific feature simulation and will look like this:

	```bash
	Errno::ENOENT: No such file or directory @ rb_sysopen –
	```
	This will occur in the `uo run` command during installation of either the openstudio-standards gem or another openstudio gem.  This error occurs because the filepath to your project directory is too long.  Move the directory to a shallower path on your system and try again.

1. **OSAF** Starting with OpenStudio version 3.3.0, major biannual releases of OpenStudio SDK / OpenStudio Analysis Framework (OSAF) will *not* include the URBANopt SDK due to dependency conflicts. URBANopt SDK will be released following the OpenStudio release, and then a patch release of the OSAF will be made that includes the URBANopt dependency.  Visit the [Release Instructions](release_instructions.md#openstudio---urbanopt-release-process) page for more details.

### Version 0.9.1 and below
1. An unpinned ruby dependency has been updated and is causing an issue with running URBANopt projects.  If you get an error related to `unicode_normalize` similar to this:
	```bash
	lib/openstudio/workflow/util/measure.rb failed with message cannot load such file -- unicode_normalize/normalize.rb 
	      ...
	```

To fix this issue, either download URBANopt CLI 0.9.2 and recreate/update your projects.  
Since this issue is isolated to the files in your project directory, you can also add the following line to the Gemfile *inside your project directory* and re-run your simulation:
	```
		gem 'addressable', '2.8.1'
	```

### Version 0.6.3

1. The REopt assumptions files have the `gcr` field set incorrectly to `1` as a default value and will cause REopt optimizations to fail.  The maximum value for this field is `0.99`. This change can be made in the `base_assumptions.json` (1 occurrence) and `multiPV_assumptions.json` (2 occurrences) files found in the `reopt` folder within the project directory.

### Version 0.4.0

1. If the CLI installation stalls on the `openstudio-standards` gem: 1) exit out of the installation, 2) install openstudio-standards by itself, 3) rerun the CLI install command:

	```bash
	gem install openstudio-standards
	gem install urbanopt-cli
	```

1. This version contains a known bug related to the feature CSV reports and the scenario-level SQL database.  It is recommended that you upgrade to version 0.4.1.

1. URBANopt<sup>&trade;</sup> SDK version 0.4.0 includes OpenDSS support via the URBANopt CLI.  Windows users may experience errors detecting python and urbanopt-ditto-reader when using the `opendss` CLI command.  If you are not able to run OpenDSS through the CLI, the functionality is also available manually by following the general [OpenDSS instructions](../opendss/opendss.md#converting-and-running-opendss).

1. SQLITE/WINDOWS issues: The sqlite3 gem contains native extensions and sometimes causes installation errors for WINDOWS users.  To resolve, ensure your ruby version is installed with "MSYS2 and MINGW development toolchain" (option 3 during installation) and that the following command succeeds before installing the CLI:

	```bash
		gem install sqlite3 -v 1.4.2
	```
1. The residential workflows currently only support the *Single-Family Detached* building type in the **Baseline** Scenario and have not been tested exhaustively.  *Single-Family Attached* and lowrise *Multifamily* building types will be added in future releases of the URBANopt SDK. Review the [modeling notes](../usage/residential_workflows.html#modeling-notes) on the residential workflows page. Please submit a bug report via the [URBANopt GitHub Issues Page](https://github.com/urbanopt/urbanopt-cli/issues) if you run into any issues.


### Version 0.2.0

1.	New example project files: The URBANopt™ SDK version 0.2.0 release comes with new files for the example project (new mappers and a new base_workflow.osw file).  For compatibility purposes and to use all new features, you may want to update any existing projects with these new files.  The example project can be installed via the CLI with the following command:

	```bash
		uo create --project-folder <path/to/PROJECT DIR>
	```

1.	Walk-in refrigeration modeling: The "create_typical_building_from_model" measure in the model articulation gem that supports OpenStudio 2.9.1 adds in capability for modeling walk-in refrigeration to a select number of space types. However, this functionality requires discrete space types to be modeled, and not a whole building or whole story blended space types. As a result, when using the default URBANopt workflow that models a blended space type based on the GeoJSON footprints, the new functionality will not be enabled. If a workflow using the GeoJSON floor area and number of floors is used with the create_bar_from_building_type_ratios measure, then walk-in refrigeration can be added in URBANopt using this version of the model articulation gem.

1. URBANopt CLI users may see 2 lines of warning regarding `DEVELOPER_NREL_KEY`. This does not affect operation in any way. A future patch will remove the warning.

1. Some users may experience issues with running the CLI from within the project directory. Try moving to a higher directory and using absolute or relative paths to the feature_file (-f) and scenario CSV (-s).

1. Many warnings get printed to the terminal when calling the URBANopt CLI from inside your project directory. The work-around to avoid these warnings is to move outside the project directory and use absolute or relative paths when calling URBANopt commands. A hypothetical example: `uo run -s ~/user/uo-project-directory/baseline_scenario.csv -f ~/user/uo-project-directory/example_project.json`
