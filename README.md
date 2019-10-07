# Welcome to URBANopt

URBANopt is a Software Development Kit (SDK) to aid in the design of districts where the interactions between individual buildings, district energy systems, distributed energy resources, and electrical system designs are considered. Including these interactions allows URBANopt to address important questions in low energy, grid aware, future thinking urban districts such as tradeoffs between building height and PV (photovoltaic) production, investments in building efficiency vs distributed renewable generation, coordination of multiple buildings to optimize grid metrics, and performance gains of shared thermal district systems vs conventional single building systems. For example, load diversity between commercial and residential buildings may allow for system time sharing or even complementary heat transfer between buildings using a district thermal energy system.  
A high-level introduction to the intent and purpose of URBANopt can be found [here](https://www.nrel.gov/buildings/urbanopt.html).

Urban planning firms, utility management facilities, architecture firms, energy consultancies and researchers can use URBANopt to create customized workflows to perform a specific environmental design task of interest eg. capturing interactions between individual buildings, district energy systems, distributed energy resources, and various other interactions.

URBANopt is a package of code for software engineers to use. **This is not a standalone program for end users.** This SDK is the engine, the building blocks that can be used to construct a modern, intuitive, performant software package supported by the scientific data provided by the National Renewable Energy Lab ([NREL](https://www.nrel.gov)). The goal of URBANopt is to enable a thriving public/private partnership of software companies to build enduse enevironmental design tools using URBANopt SDK modules. These developers can use URBANopt modules, customize URBANopt modules, or create new modules to implement the desired workflows for their tools.

The initial URBANopt modules are written in Ruby. Hence, each URBANopt module is a Ruby Gem hosting a group of Ruby files. Each Ruby file defines a class with unique methods and functionality. The logical structure that describe the connection of classes within a gem is described in a JSON schema or multiple JSON schemas located in the schema folders for each module.

Currently URBANopt include 3 main modules: urbanopt-core-gem, urbanopt-scenario-gem, and urbanopt-geojson-gem. These modules are combined to run an example project generating scenario level and feature level results. This documentation will walk users through the following steps: installing required softwares,  running the example project using rake tasks, adding your own measures, modifying mapper classes, and adding a custom post processor. In addition, advanced users should refer to the module docs to further customize and modify their modules. These docs include a detailed description of all the classes and methods in each URBANopt module.

**This documentation is intended for power users to operate from the command line*

## Table of Contents

- [Introduction](#welcome-to-urbanopt)
- [Mac installation](#mac-installation)
- [Windows installation](#windows-installation)
- [Linux installation](#linux-installation)
- [Basic usage](#basic-usage)
  - [Set up](#set-up)
  - [Run example project](#run-example-project)
  - [Overview of rake tasks](#overview-of-rake-tasks)
- [Adding your own measures](#adding-your-own-measures)
- [Modifying mapper classes](#modifying-mapper-classes)
- [Adding a custom post processor](#adding-a-custom-post-processor)
- [Advanced usage](#advanced-usage)
  - [geoJSON docs](#geojson-docs)
  - [geoJSON github](#geojson-github)
  - [Scenario docs](#scenario-docs)
  - [Scenario github](#scenario-github)

## Mac installation

Install [Ruby 2.2.4](https://github.com/rbenv/rbenv)  
Install Bundler 1.17 by typing into your command line:

```bash
gem install bundler -v 1.17
```

If necessary, create a `.gemrc` file in your home directory that contains:

```yml
:backtrace: false
:bulk_threshold: 1000
:sources:
- http://rubygems.org
:update_sources: true
:verbose: true
```

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
Add path to Ruby in the new OpenStudio folder by pasting into your `.bash_profile` or similar file:

```bash
export RUBYLIB=/Applications/OpenStudio-2.8.1/Ruby
```

## Windows installation

Install Ruby using the [RubyInstaller](https://rubyinstaller.org/downloads/archives/) for [Ruby 2.2.4 (x64)](https://dl.bintray.com/oneclick/rubyinstaller/rubyinstaller-2.2.4-x64.exe)  
Install Devkit using the [mingw64](https://dl.bintray.com/oneclick/rubyinstaller/DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe) installer  
Include path to Ruby by adding to your environment variables path: `C:\Ruby22-x64\bin`
Create a new environment variable `HOME` and set the variable value to: `C:\Users\<user_name>`

Install Bundler 1.17 by typing into your command line

```bash
gem install bundler -v 1.17
```

If you have a secure firewall that prevents **bundler** from installing, follow these steps:

- Type into the command line:
  - `gem sources -c`
  - `gem sources -a http://rubygems.org/`
- Accept the lowered security of `http` instead of `https`
- Create file: `C:\Users\<user_name>\.gemrc`. Move to that directory at the command prompt and do this by typing:
<!-- TODO: confirm creating files from a different directory, so this can be cleaned up. -->

```bash
type nul > .gemrc
```

Edit this file to contain:

```yml
:backtrace: false
:bulk_threshold: 1000
:sources:
- http://rubygems.org
:update_sources: true
:verbose: true
```

Now you are able to install **bundler** with

```bash
gem install bundler -v 1.17
```

in your command line

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
Create file `C:\ruby-2.2.4-x64-mingw32\lib\ruby\site_ruby\openstudio.rb` and edit it to contain:

```ruby
require 'C:\openstudio-2.8.1\Ruby\openstudio.rb'
```

Verify your OpenStudio and Ruby configuration by typing this into your command line:

```bash
ruby -e "require 'openstudio'" -e "puts OpenStudio::Model::Model.new"
```

Expected output is

```bash
OS:Version,
 {5f1cd617-3f1e-47e0-ab6c-3a27160f114b}, !- Handle
 2.7.1;                                  !- Version Identifier`
 ```

## Linux installation

Install [OpenStudio 2.8.1](https://github.com/NREL/OpenStudio/releases/tag/v2.8.1)  
Manually install [libpng12](https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html)  
Gdbm libs:

```bash
/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm.so.3: cannot open shared object file: No such file or directory
```

and

```bash
/usr/local/openstudio-2.8.1/bin/openstudio: error while loading shared libraries: libgdbm_compat.so.3: cannot open shared object file: No such file or directory
```

You need to create symlinks to the appropriate version (`libgdbm.so.5` and `libgdbm_compat.so.4`)

## Basic Usage

### **Set up**

Install [Git](https://git-scm.com/) if not already installed  
NREL provides a list of optional git GUI's, and some help using git with OpenStudio, [here](https://github.com/NREL/OpenStudio/wiki/Using-OpenStudio-with-Git-and-GitHub)  
Make a Fork of our example project: <https://github.com/urbanopt/urbanopt-example-geojson-project>

- Check out to a short path (e.g. `C:\urbanopt-project`) to avoid problems with long file names on Windows
  - Reference: <https://ourcodeworld.com/articles/read/109/how-to-solve-filename-too-long-error-in-git-powershell-and-github-application-for-windows>
  - Solution:

```bash
git config --system core.longpaths true
```

### **Run example project**

Run the following commands to execute the Rake tasks defined in the Rakefile of the current working directory.

1. `bundle install` :
Use this to ensure all dependencies in your Gemfile are available.
1. `bundle exec rake` :
Use this to execute the Rake tasks.
1. `bundle update` :
Use this to update your gems to the latest available versions.

To run specific rake tasks:

```bash
bundle exec rake <name of the Rake task>
```

### **Rake tasks**

#### *run_all*

In the Rake file methods are defined to create a Scenario CSV for each scenario, that take as arguments:

- The `geojson` file (for example: `industry_denver.geojson`)
- `mappers_files_dir` the file path for the `baseline.osw` file and the mapper classes.  
- `csv` file (for example: `baseline_scenario.csv`)

The `run_all` Rake task creates and runs a `ScenarioRunnerOSW` for each scenario, passing the scenario method as an argument.

- `run_baseline`, `run_high_efficiency` and `run_mixed rake` tasks can be used for running individual scenarios.

#### *post_process_all*

This rake task creates and runs `ScenarioDefaultPostProcessor` for each scenario and saves the results.

- `post_process_baseline`, `post_process_high_efficiency`, `post_process_mixed` rake tasks can be used for individual scenarios.

#### *update_all*

This rake task combines the `run_all` and `post_process_all` tasks.

#### *default*

This runs the `update_all` rake task.

#### *clear_all*

This rake task clears the scenario results from any previous runs.

- `clear_baseline`, `clear_high_efficiency`, `clear_mixed` rake tasks can be used for individual scenarios.

### **Adding your own measures**

The `mappers` folder contains `baseline.osw` which serves as a simulation input for
URBANopt. It is a workflow of OpenStudio and URBANopt measures and dictates the sequence of running
these measures. Measures are added to create building models and test different energy conservation measures
for different scenarios.

- `set_run_period`: It is an OpenStudio Measure used to define the number of timesteps per hour and specify
  the begin and end date for running the simulation.

- `ChangeBuildingLocation`: It is an OpenStudio Measure used to load the EPW file and
  specify to the EPW weather file.

- [`create_bar_from_building_type_ratios`](https://github.com/NREL/openstudio-model-articulation-gem/tree/develop/lib/measures/create_bar_from_building_type_ratios):
  It is an OpenStudio Model Articulation Measure to create a core
  and perimeter bar sliced by space type. It takes in one or more building types as user
  arguments to create space type collections.

- [`create_typical_building_from_model
  1`](https://github.com/NREL/openstudio-model-articulation-gem/tree/develop/lib/measures/create_typical_building_from_model): It is an OpenStudio Model Articulation Measure that creates a custom prototype
    building with user-defined geometry and assigns constructions, schedules, internal loads,
    HVAC and other loads such as exterior lights and service water heating based on the
    space and sub space types. The `add_hvac` is set to `false` by default.

- [`blended_space_type_from_model`](https://github.com/NREL/openstudio-model-articulation-gem/tree/develop/lib/measures/blended_space_type_from_model):
  It is an OpenStudio Model Articulation Measure that used is used to create a single
  space type that represents the loads and schedules of a collection of space types. It
  removes all previous space type assignments and hard assigns internal loads from spaces
  included in the building floor area. A blended space type will be created from the
  original internal loads and assigned at the building level.

- [`urban_geometry_creation`](https://github.com/urbanopt/urbanopt-geojson-gem/tree/develop/lib/measures/urban_geometry_creation):
  It is an URBANopt GeoJSON measure that is used to create geometry for a particular
    building, accounting for surrounding buildings as shading.

- `create_typical_building_from_model 2`: It is added in the workflow after urban
  geometry creation and the `add_hvac` argument is now set to `true`, to add HVAC system
  for the blended space types. The rest of the arguments for adding constructions, space
  type loads etc. are set to `false`.

- `ReduceElectricEquipmentLoadsByPercentage`: It is an OpenStudio Measure and is used
  to reduce the equipment load by a certain amount. The measure is skipped for the
  baseline scenario. For the high efficiency scenario, the skip measure argument is
  set to false and the measure is implemented.

- `ReduceLightingLoadsByPercentage`: It is an OpenStudio Measure and is used
  to reduce the lighting load by a certain amount. The measure is skipped for the
  baseline scenario. For the high efficiency scenario, the skip measure argument is
  set to false and the measure is implemented.

- [`default_feature_reports`](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports):
  It is an URBANopt Scenario Measure and it creates a `default_feature_reports.json` used
  by URBANopt Scenario Default Post Processor.

Additional measures can be added to the workflow by adding the measure name and directory
name along with the measure arguments.

### **Modifying mapper classes**

The Simulation Mapper Class derives from the
[SimulationMapperBase](https://github.com/urbanopt/urbanopt-scenario-gem/blob/develop/lib/urbanopt/scenario/simulation_mapper_base.rb)
class. It maps the features in the feature file to arguments required as simulation
inputs in the `baseline.osw`. Currently, it does so for the *Building* feature_type. When the Scenario is run, a new Simulation Mapper instance
is created and the `create_osw` method is implemented for each Feature assigned to the
Simulation Mapper Class in the Scenario CSV.

On adding additional measures to the `baseline.osw` file, the necessary features from the feature
files must be mapped to measure arguments in the Simulation Mapper Class.

The *`OpenStudio::Extension.set_measure_argument`* method sets the feature from the
feature file as simulation input, passing the measure and
argument name and the corresponding feature from the feature file as arguments.

The `HighEfficiency` class inherits from the `BaselineMapper`
class and can override measures that were skipped in the `BaselineMapper`.

### **Adding a custom post processor**

A customized post processor should be added to the rake file replacing the current post processor.  The current post processor defined in the rake file is `default_post_processor` :

  ```ruby
  default_post_processor = URBANopt::Scenario::ScenarioDefaultPostProcessor.new(baseline_scenario)
  scenario_result = default_post_processor.run
  scenario_result.save
  ```

`default_post_processor` is an object of ScenarioDefaultPostProcessor class this class can be customized in the Scenario Gem. Advanced users should refer to scenario docs to learn about the all the methods and classes that are used to aggreagate all the properties that describe a feature report (reporting_periods, construction_cost, program, etc.). Users can edit these methods or add new methods that extend or customize the post processor functionality.

#### Feature reports
<!-- TODO: add reference as hyperlinks -->
This scenario post process require feature reports to aggregate results from feature simulations. A reporting measure is used to query and report specific output data from an Openstudio simulation of each feature. The current default reporting measure is the [default_feature_reports](https://github.com/urbanopt/urbanopt-scenario-gem/tree/develop/lib/measures/default_feature_reports). This measure writes a `default_feature_reports.json` file containing information on all features in the simulation. It also writes a `default_feature_reports.csv` containing timeseries data for all the features.

<!-- TODO: add reference -->
Users can create their own OpenStudio reporting measure to generate customized simulation reports. For example, users can request results for different reporting frequencies or query and report additional outputs that are important for their own projects; e.g. reporting specific construction costs. User can then add the new reporting measure to the openstudio `workflow.osw` file **TODO** and rerun the simulation.  

The `DefaultPostProcessor` reads these feature reports and aggregates them to create a `ScenarioReport`.

## Advanced Usage

To customize or develop URBANopt, please use the following documentation and source code to aid you:

- ### [geoJSON docs](https://urbanopt.github.io/urbanopt-geojson-gem/)

- ### [geoJSON github](https://github.com/urbanopt/urbanopt-geojson-gem)

- ### [Scenario docs](https://urbanopt.github.io/urbanopt-scenario-gem/)

- ### [Scenario github](https://github.com/urbanopt/urbanopt-scenario-gem)

[Contents](#table-of-contents)
