# Welcome to URBANopt

URBANopt (Urban Renewable Building And Neighborhood optimization) is an EnergyPlus and OpenStudio-based simulation platform aimed at district- and campus-scale thermal and electrical analysis.

The URBANopt project website can be found [here](https://www.nrel.gov/buildings/urbanopt.html).  
A high-level introduction to the intent and purpose of URBANopt can be found [here](https://www.energy.gov/eere/buildings/urbanopt).

**_URBANopt is not a standalone program for end users._** URBANopt Software Development Kit (SDK) is a package of modules that can be used to build a user-friendly tool. Developers can use existing URBANopt modules, customize URBANopt modules, and create new modules to implement the desired workflows for their tools. Other users of the SDK could include researchers looking to create customized workflows to perform a specific environmental design task.

URBANopt initially focuses on enabling three types of use cases:

- Design of low energy campuses and districts.
- Design and optimization of grid-interactive efficient buildings (GEBs) at a district-scale in conjunction with distributed energy resources (DERs) and electric distribution systems.
- Detailed design of next-generation district thermal systems.

The URBANopt SDK can aid in the design of districts where the interactions between individual buildings, district energy systems, distributed energy resources, and electrical system designs are considered. Including these interactions allows URBANopt to address important questions in low energy, grid aware, future thinking urban districts such as:

- Tradeoffs between building height and photovoltaic (PV) production
- Investments in building efficiency vs. renewable generation
- Coordination of multiple buildings to optimize grid metrics
- Performance gains of shared thermal district systems vs conventional single building systems.

For example, load diversity between commercial and residential buildings may allow for system time sharing or even complementary heat transfer between buildings using a district thermal energy system (see Figure 2 from the [URBANopt project website](https://www.nrel.gov/buildings/urbanopt.html) for an example).

Currently URBANopt includes 3 main modules: urbanopt-core-gem, urbanopt-scenario-gem, and urbanopt-geojson-gem. The logical structure of the connection between classes within a gem is described in JSON schemas located in the schema folders for each module. These modules are combined to run an example project generating scenario level and feature level results.

## Table of Contents

- [Introduction](#welcome-to-urbanopt)
- [Mac installation](#mac-installation)
- [Windows installation](#windows-installation)
- [Linux installation](#linux-installation)
- [Basic usage](#basic-usage)
  - [Set up](#set-up)
  - [Run example project](#run-example-project)
  - [Overview of rake tasks](#rake-tasks)
- [Adding your own measures](#adding-your-own-measures)
- [Modifying mapper classes](#modifying-mapper-classes)
- [Adding a custom post processor](#adding-a-custom-post-processor)
- [Advanced documentation & source code](#advanced-usage)

## [Mac installation](#table-of-contents)

Install [Ruby 2.2.4](https://github.com/rbenv/rbenv#installation)  
(URBANopt will update to Ruby 2.5 when OpenStudio 3.0 is released)  
Install Bundler version 1.17:

```terminal
gem install bundler -v 1.17
```

If it doesn't already exist, create a `.gemrc` file in your home directory that contains:

```yml
:backtrace: false
:bulk_threshold: 1000
:sources:
- http://rubygems.org
:update_sources: true
:verbose: true
```

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
Add path to Ruby in the new OpenStudio folder by pasting into your `.bash_profile` or similar file: `export RUBYLIB=/Applications/OpenStudio-2.8.1/Ruby`

## [Windows installation](#table-of-contents)

Install [Ruby 2.2.4 (x64)](https://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-2.2.4-x64.exe)  
(URBANopt will update to Ruby 2.5 when OpenStudio 3.0 is released)  
Install Devkit using the [mingw64](https://dl.bintray.com/oneclick/rubyinstaller/DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe) installer  
Include path to Ruby by adding to your environment variables path: `C:\Ruby22-x64\bin`  
Create a new environment variable `HOME` and set the variable value to: `C:\Users\<user_name>`  
Install Bundler version 1.17:

```terminal
gem install bundler -v 1.17
```

If you have a secure firewall that prevents **bundler** from installing properly, type into the command line:

- `gem sources -c`
- `gem sources -a http://rubygems.org/`

You will need to accept the reduced security of `http` compared to `https`

If it doesn't already exist, create a `.gemrc` file in your home directory by typing:

```terminal
type nul > C:\Users\<user_name>\.gemrc
```

Edit the new `.gemrc` file to contain:

```yml
:backtrace: false
:bulk_threshold: 1000
:sources:
- http://rubygems.org
:update_sources: true
:verbose: true
```

Now install **bundler**:

```terminal
gem install bundler -v 1.17
```

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
Create file `C:\ruby-2.2.4-x64-mingw32\lib\ruby\site_ruby\openstudio.rb` and edit it to contain:

```ruby
require 'C:\openstudio-2.8.1\Ruby\openstudio.rb'
```

Verify your OpenStudio and Ruby configuration by typing into your command line:

```terminal
ruby -e "require 'openstudio'" -e "puts OpenStudio::Model::Model.new"
```

Expected output:

```terminal
OS:Version,
 {<long-uuid>},                          !- Handle
 2.8.1;                                  !- Version Identifier`
 ```

## [Linux installation](#table-of-contents)

**_Linux installation/operation has not been tested exhaustively. Please submit an issue if you find something that doesn't work_**

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
Manually install [libpng12](https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html)  
If you get gdbm libs errors like this:

```terminal
/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm.so.3: cannot open shared object file: No such file or directory
```

or this:

```terminal
/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm_compat.so.3: cannot open shared object file: No such file or directory
```

You need to create symlinks to the appropriate version (`libgdbm.so.5` and `libgdbm_compat.so.4`)

## [Basic Usage](#table-of-contents)

### [**Set up**](#table-of-contents)

Install [Git](https://git-scm.com/) if not already installed  
NREL [provides a list](https://github.com/NREL/OpenStudio/wiki/Using-OpenStudio-with-Git-and-GitHub) of optional git GUI's, and some help using git with OpenStudio  
Fork the URBANopt example project: <https://github.com/urbanopt/urbanopt-example-geojson-project>  
Clone your fork down to your local machine  

**If using Windows:**  
Need to allow long path names in git:

```terminal
git config --system core.longpaths true
```

### [**Run example project**](#table-of-contents)

Move to the top level directory of the example you just cloned. Run the following commands to execute the Rake tasks defined in the Rakefile of the current working directory.

1. `bundle install` to ensure all dependencies in your Gemfile are available
1. `bundle exec rake <rake task>` to execute Rake tasks
1. `bundle update` to update your gems to the latest available versions

### [**Rake tasks**](#table-of-contents)

#### *run_all*

In the `Rakefile` methods are defined to create a Scenario CSV for each scenario, that take as arguments:

- The `geojson` file (for example: `industry_denver.geojson`)
- `mappers_files_dir` the file path for the `baseline.osw` file and the mapper classes.  
- `csv` file (for example: `baseline_scenario.csv`)

The `run_all` Rake task creates and runs a `ScenarioRunnerOSW` for each scenario i.e. the
*Baseline*, *HighEfficiency* and *Mixed scenario*, passing the scenario method as an argument.

- `run_baseline`, `run_high_efficiency` and `run_mixed` rake tasks can be used for running individual scenarios.

#### *post_process_all*

The default "post-process" of a scenario aggregates the [Feature reports](#Feature-reports) into results across the whole scenario. This rake task creates and runs `ScenarioDefaultPostProcessor` for each scenario and saves the results.

- `post_process_baseline`, `post_process_high_efficiency`, `post_process_mixed` rake tasks are pre-built and can be used for those individual scenarios.

#### *update_all*

This rake task combines the `run_all` and `post_process_all` tasks.

#### *default*

This runs the `update_all` rake task.

#### *clear_all*

This rake task clears the scenario results from any previous runs.

- `clear_baseline`, `clear_high_efficiency`, `clear_mixed` rake tasks can be used for individual scenarios.

### [**Adding your own measures**](#table-of-contents)

The `mappers` folder contains `baseline.osw` which serves as a simulation input for URBANopt. It is a workflow of OpenStudio and URBANopt measures and dictates the sequence of running these measures. Measures are added to create building models and test different energy conservation measures for different scenarios.

- `set_run_period`: An OpenStudio Measure used to define the number of timesteps per hour and specify the begin and end date for running the simulation.

- `ChangeBuildingLocation`: An OpenStudio Measure used to load the EPW file and specify to the EPW weather file.

- [`create_bar_from_building_type_ratios`](https://github.com/NREL/openstudio-model-articulation-gem/tree/develop/lib/measures/create_bar_from_building_type_ratios): An OpenStudio Model Articulation Measure used to create a core and perimeter bar sliced by space type. It takes in one or more building types as user arguments to create space type collections.

- [`create_typical_building_from_model 1`](https://github.com/NREL/openstudio-model-articulation-gem/tree/develop/lib/measures/create_typical_building_from_model): An OpenStudio Model Articulation Measure that creates a custom prototype building with user-defined geometry and assigns constructions, schedules, internal loads, HVAC and other loads such as exterior lights and service water heating based on the space and sub space types. The `add_hvac` is set to `false` by default.

- [`blended_space_type_from_model`](https://github.com/NREL/openstudio-model-articulation-gem/tree/develop/lib/measures/blended_space_type_from_model): An OpenStudio Model Articulation Measure that used is used to create a single space type that represents the loads and schedules of a collection of space types. It removes all previous space type assignments and hard assigns internal loads from spaces included in the building floor area. A blended space type will be created from the original internal loads and assigned at the building level.

- [`urban_geometry_creation`](https://github.com/urbanopt/urbanopt-geojson-gem/tree/develop/lib/measures/urban_geometry_creation): An URBANopt GeoJSON measure that is used to create geometry for a particular building, accounting for surrounding buildings as shading.

- [`create_typical_building_from_model 2`](https://github.com/NREL/openstudio-model-articulation-gem/tree/develop/lib/measures/create_typical_building_from_model): Added in the workflow after urban geometry creation and the `add_hvac` argument is now set to `true`, to add HVAC system for the blended space types. The rest of the arguments for adding constructions, space type, loads, etc. are set to `false`.

- `ReduceElectricEquipmentLoadsByPercentage`: An OpenStudio Measure that is used to reduce the equipment load by a certain amount. The measure is skipped for the baseline scenario. For the high efficiency scenario, the skip measure argument is set to false and the measure is implemented.

- `ReduceLightingLoadsByPercentage`: An OpenStudio Measure that is used to reduce the lighting load by a certain amount. The measure is skipped for the baseline scenario. For the high efficiency scenario, the skip measure argument is set to false and the measure is implemented.

- [`default_feature_reports`](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports): An URBANopt Scenario Measure that creates a `default_feature_reports.json` used by URBANopt Scenario Default Post Processor.

Additional measures can be added to the workflow by adding the measure name and directory name along with the measure arguments.

### [**Modifying mapper classes**](#table-of-contents)

The Simulation Mapper Class derives from the
[SimulationMapperBase](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/simulation_mapper_base.rb)
class. It maps the features in the feature file to arguments required as simulation
inputs in the `baseline.osw`. 

A feature refers to a single object in a district energy
analysis such as a building, district, system etc. The feature file includes all data for
all the features and is written by a third part user interface, in this case in the
GeoJSON format.
Currently, the Simulation Mapper can be used for mapping the *Building* feature_type.

The URBANopt GeoJSON Example Project includes a default
Simulation Mapper Class to translate an URBANopt GeoJSON Feature to an OpenStudio Model.
 The `HighEfficiency` mapper class inherits from the `BaselineMapper` class and can override measures that were skipped in the `BaselineMapper`.


When the Scenario is run, a new Simulation Mapper instance is created and the
`create_osw` method is implemented for each Feature assigned to the Simulation Mapper
Class in the Scenario CSV.

The default Simulation Mapper Class can be used directly, extended or modified or a
completely different Simulation Mapper Class can be created.

On adding additional measures to the `baseline.osw` file, the Simulation Mapper Class
would need to be modified by adding necessary features from the feature files and mapping
them to measure arguments.

The *`OpenStudio::Extension.set_measure_argument`* method sets the feature from the feature file as simulation input, passing the measure and argument name and the corresponding feature from the feature file as arguments.

To add custom measures, or measures that lie outside of the ruby gems specified in the
Gemfile, `@@osw[:measure_paths] << File.join(File.dirname(__FILE__),
'../new_measure_folder/')` can added in the Mapper Class, specifying the file path of the
new measures. This adds the `measure_path` in the
`baseline.osw`. 


To create a new mapper class: 

- The new mapper class ruby file should be created in the Mappers folder, since the
  `mapper_files_dir` in the `Rakefile` is directed here. The default Simulation Mapper
  Class can be used as template, and the `osw_path`
  would need to be
  updated as per the name of the new `osw` file.
- A new scenario CSV should be created in the root folder, and the mapper class name should be updated
  within the mapper class column. The existing scenario csv's can be used as reference. 
- OpenStudio Measures can be added to the new `osw` file by adding the measure directory
  and measure arguments, and adding the features from the feature file and mapping them to the
  corresponding arguments in the Mapper Class.
- A new method can be created in the `Rakefile` to call the Mapper Class. The
  `mapper_files_dir` and `csv_file paths` should correspond to the newly created Mapper Class and csv file
  paths respectively.
- New Rake tasks can be created and to `.clear`, create a `ScenarioRunnerOSW` as well as `.run`
  and create a `ScenarioDefaultPostProcessor` as well as `.run` the new method.  
  

### [**Adding a custom post processor**](#table-of-contents)

 A customized post processor can be added to the rake file, replacing the default post processor.

 <!-- TODO: Why would someone create a custom post-processor? What benefit could they gain? -->

`default_post_processor` is an object of ScenarioDefaultPostProcessor class. Advanced users should refer to [Scenario documentation](#Advanced-Usage) for customizing all methods and classes used to aggregate the properties that describe a feature report (reporting_periods, construction_cost, program, etc.). Users can edit these methods or add new methods that extend or customize the post processor functionality.

#### [Feature reports](#table-of-contents)

The scenario post process requires feature reports to aggregate results from feature simulations. A reportig measure is used to query and report specific output data from an Openstudio simulation of each feature. The current default reporting measure is the [default_feature_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports). This measure writes a `default_feature_reports.json` file containing information on all features in the simulation. It also writes a `default_feature_reports.csv` containing timeseries data for all the features.

Users can create their own OpenStudio reporting measure to generate customized simulation reports. For example, users can request results for different reporting frequencies or query and report additional outputs that are important for their own projects; e.g. reporting specific construction costs. Users should refer to this [reporting measure writing guide](http://nrel.github.io/OpenStudio-user-documentation/reference/measure_writing_guide/#reporting-measures) to customize the `measure.rb` file in [default_feature_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports) or create a new reporting measure.

User can then add any new reporting measure to the openstudio `.osw` file, as described [here](#Adding-your-own-measures), and re-run the simulation.
The current measure added to the baseline.osw is the `default_feature_reports`:

```json
{
      "measure_dir_name": "default_feature_reports",
      "arguments": {
        "feature_id": null,
        "feature_name": null,
        "feature_type": null
      }
}
```

The `DefaultPostProcessor` reads these feature reports and aggregates them to create a `ScenarioReport`.

## [Advanced Usage](#table-of-contents)

To customize or develop URBANopt, please use the following documentation and source code to aid you:

- [GeoJSON documentation](https://urbanopt.github.io/urbanopt-geojson-gem/)
- [GeoJSON Github](https://github.com/urbanopt/urbanopt-geojson-gem)
- [Scenario documentation](https://urbanopt.github.io/urbanopt-scenario-gem/)
- [Scenario Github](https://github.com/urbanopt/urbanopt-scenario-gem)
- [Core Github](https://github.com/urbanopt/urbanopt-core-gem)
