---
layout: default
title: Workflows
has_children: true
nav_order: 4
---

# URBANopt Workflows

In this section you can find out more details about each of the workflows supported by URBANopt. Check out the sections below for more information on the OpenStudio measures currently used in URBANopt workflows as well as a functionality lookup table.

[Measure Compatibility](#measure-compatibility-details){: .btn .btn-uo .white-text}


[Functionality Lookup](#functionality-example-lookup){: .btn .btn-uo .white-text}


## Measure Compatibility Details

URBANopt<sup>&trade;</sup> workflows use OpenStudio<sup>&reg;</sup> measures to create building models, apply different energy conservation measures (ECMs) for different Scenarios, perform quality checks, and report results back to URBANopt.  The sequence of measures is specified in an OpenStudio workflow (osw) file found in the project directory. To enable/disable a measure, modify its `SKIP` variable in the `base_workflow.osw` file either directly or via the scenario mapper file.

The table below lists the measures that are used in URBANopt workflows, what types of buildings they are compatible with, and whether they are enabled or disabled by default in the workflow file.

<div id="table2div">
	<table>
		<thead>
			<tr>
				<th class="bold">Measure</th>
				<th>Building Types</th>
				<th>Required to Run Baseline Workflow?</th>
				<th>Enabled by default?</th>
				<th>Description</th>
			</tr>
	  </thead>
	  <tbody>
			<tr>
				<th><a target="_blank" href="https://github.com/urbanopt/urbanopt-cli/tree/master/example_files/measures/BuildResidentialModel">Build Residential Model</a></th>
				<td>Residential</td>
				<td>Yes</td>
				<td>Yes</td>
				<td>Builds the OpenStudio Model for an existing residential building</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/set_run_period">Set Run Period</a></th>
				<td>Commercial</td>
				<td>Yes</td>
				<td>Yes</td>
				<td>OpenStudio Measure used to define the number of timesteps per hour and specify the begin and end date for running the simulation</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/ChangeBuildingLocation">Change Building Location</a></th>
				<td>Commercial</td>
				<td>Yes</td>
				<td>Yes</td>
				<td>An OpenStudio Measure used to specify and load the EPW (weather) file.</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-model-articulation-gem/tree/master/lib/measures/create_bar_from_building_type_ratios">Create Bar From Building Type Ratios</a></th>
				<td>Commercial</td>
				<td>Yes (unless using a detailed model)</td>
				<td>Yes</td>
				<td>An OpenStudio Model Articulation Measure used to create a core and perimeter bar sliced by space type. It takes in one or more building types as user arguments to create space type collections.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-model-articulation-gem/tree/master/lib/measures/create_typical_building_from_model">Create Typical Building From Model (1st application)</a></th>
				<td>Commercial</td>
				<td>Yes (unless using a detailed model)</td>
				<td>Yes</td>
				<td>An OpenStudio Model Articulation Measure that creates a custom prototype building with user-defined geometry and assigns constructions, schedules, internal loads, HVAC and other loads such as exterior lights and service water heating based on the space and sub space types. The `add_hvac` is set to `false` by default in this measure because it gets handled later in the process to account for blended space types.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-model-articulation-gem/tree/master/lib/measures/blended_space_type_from_model">Blended Space Type From Model</a></th>
				<td>Commercial</td>
				<td>Yes (unless using a detailed model OR CreateBar geometry workflow)</td>
				<td>Yes</td>
				<td>An OpenStudio Model Articulation Measure that is used to create a single space type that represents the loads and schedules of a collection of space types. It removes all previous space type assignments and hard assigns internal loads from spaces included in the building floor area. A blended space type will be created from the original internal loads and assigned at the building level.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/add_ev_load">Add EV Load</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in the EV Charging Scenario</td>
				<td>An OpenStudio Model Articulation Measure that is used to create a single space type that represents the loads and schedules of a collection of space types. It removes all previous space type assignments and hard assigns internal loads from spaces included in the building floor area. A blended space type will be created from the original internal loads and assigned at the building level.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/add_ems_to_control_ev_charging">Add EMS to Control EV Charging</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in the EV Charging Scenario</td>
				<td>An OpenStudio measure that implements a control system to curtail an electric vehicle (EV) charging load to better align EV charging with expected energy production from a solar PV system.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/urbanopt/urbanopt-geojson-gem/tree/master/lib/measures/urban_geometry_creation">Urban Geometry Creation</a></th>
				<td>Commercial</td>
				<td>Yes in FloorSpace geometry workflow (unless using a detailed model)</td>
				<td>Yes</td>
				<td>This measure reads an URBANopt GeoJSON and creates geometry for a particular building.  Surrounding buildings are included as shading structures.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/urbanopt/urbanopt-geojson-gem/tree/master/lib/measures/urban_geometry_creation_zoning">Urban Geometry Creation Zoning</a></th>
				<td>Commercial</td>
				<td>Yes in Default and CreateBar geometry workflows (unless using a detailed model)</td>
				<td>Yes</td>
				<td>An URBANopt GeoJSON measure that is used to create extruded geometry for building features from
  GeoJSON coordinates with core and perimeter zoning, it can also account for shading from surrounding buildings.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-model-articulation-gem/tree/master/lib/measures/create_typical_building_from_model">Create Typical Building From Model (2nd application)</a></th>
				<td>Commercial</td>
				<td>Yes in Default and FloorSpace geometry workflows (unless using a detailed model)</td>
				<td>Yes</td>
				<td>A second instance of this OpenStudio Measure, which is added in the workflow after urban geometry creation and the add_hvac argument is now set to true, to add HVAC system for the blended space types. The rest of the arguments for adding constructions, space type, loads, etc. are set to false.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/LBNL-ETA/Openstudio-GEB-gem/tree/master/lib/measures/add_chilled_water_storage_tank">Add Chilled Water Storage Tank</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in the Chilled Water Storage Scenario</td>
				<td>This measure adds a chilled water storage tank to a chilled water loop for the purpose of thermal energy storage.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/PredictedMeanVote">Predicted Mean Vote</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Yes</td>
				<td>An OpenStudio measure used to add the necessary data to people objects to support Predicted Mean Vote output data.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/add_ems_emissions_reporting">Add EMS Emissions Reporting</a></th>
				<td>Commercial and Residential</td>
				<td>No</td>
				<td>No</td>
				<td>An OpenStudio measure used to report emissions based on user-provided future and historical years as well as future, historical hourly, and historical annual subregions. 
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/LBNL-ETA/Openstudio-GEB-gem/tree/master/lib/measures/reduce_epd_by_percentage_for_peak_hours">Reduce EPD by Percentage For Peak Hours</a></th>
				<td>Commercial and Residential</td>
				<td>No</td>
				<td>Enabled in the Peak Hours MELs Shedding Scenario</td>
				<td>This measure reduces electric equipment loads by a user-specified percentage for a user-specified time period (usually the peak hours). The reduction can be applied to at most three periods throughout out the year specified by the user. This is applied throughout the entire building.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/LBNL-ETA/Openstudio-GEB-gem/tree/master/lib/measures/AdjustThermostatSetpointsByDegreesForPeakHours">Adjust Thermostat Setpoint by Degrees for Peak Hours</a></th>
				<td>Commercial and Residential</td>
				<td>No</td>
				<td>Enabled in the Peak Hours Thermostat Adjust Scenario</td>
				<td>This measure adjusts heating and cooling setpoints by a user-specified number of degrees and a user-specified time period. This is applied throughout the entire building.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/IncreaseInsulationRValueForExteriorWalls">Increase Insulation RValue for Exterior Walls</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in High Efficiency Scenario</td>
				<td>An OpenStudio measure that is used to increase the R-Value of insulation for exterior walls by a specific value. This measure is skipped in the baseline Scenario and is added for all `MidRiseApartment` OpenStudio building types in the high efficiency Scenario.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/ReduceElectricEquipmentLoadsByPercentage">Reduce Electric Equipment Loads By Percentage</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in High Efficiency Scenario</td>
				<td>An OpenStudio Measure that is used to reduce the equipment load by a certain amount. The Measure is skipped for the baseline Scenario. For the high efficiency Scenario, the `skip_measure` argument is set to false and the measure is implemented.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/ReduceLightingLoadsByPercentage">Reduce Lighting Loads By Percentage</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in High Efficiency Scenario</td>
				<td>An OpenStudio Measure that is used to reduce the lighting load by a certain amount. The measure is skipped for the baseline Scenario. For the high efficiency Scenario, the `skip_measure` argument is set to false and the measure is implemented.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-load-flexibility-measures-gem/tree/master/lib/measures/add_central_ice_storage">Add Central Ice Storage</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in Thermal Storage Scenario</td>
				<td>An OpenStudio measure that adds an ice storage tank to a chilled water loop for the purpose of thermal energy storage.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-load-flexibility-measures-gem/tree/master/lib/measures/add_hpwh">Add Central Ice HPWH</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in Thermal Storage Scenario</td>
				<td>An OpenStudio measure that adds or replaces existing domestic hot water heater with air source heat pump system and allows for the addition of multiple daily flexible control time windows. The heater/tank system may charge at maximum capacity up to an elevated temperature, or float without any heat addition for a specified timeframe down to a minimum tank temperature.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-load-flexibility-measures-gem/tree/master/lib/measures/add_packaged_ice_storage">Add Packaged Ice Storage</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Enabled in Thermal Storage Scenario</td>
				<td>An OpenStudio measure that removes the cooling coils in the model and replaces them with packaged air conditioning units with integrated ice storage.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/urbanopt/urbanopt-reporting-gem/tree/develop/lib/measures/export_time_series_modelica">Export Time Series Modelica</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Yes</td>
				<td>An OpenStudio reporting measure that adds the required output variables and creates a CSV file with plant loop level mass flow rates and temperatures for use in a Modelica simulation.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/urbanopt/urbanopt-reporting-gem/tree/develop/lib/measures/export_modelica_loads">Export Modelica Loads</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>Yes</td>
				<td>An OpenStudio reporting measure uses the results from the EnergyPlus simulation to generate a load file for use in Modelica. This will create a MOS and a CSV file of the heating, cooling, and hot water loads.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/openstudio_results">OpenStudio Results</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>No</td>
				<td>An OpenStudio reporting measure that creates high level tables and charts pulling both from model inputs and EnergyPlus results. It has building level information as well as detail on space types, thermal zones, HVAC systems, envelope characteristics, and economics.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/envelope_and_internal_load_breakdown">Envelope and Internal Load Breakdown</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>No</td>
				<td>An OpenStudio reporting measure that provides annual and monthly views into heat gains and losses for envelope and internal loads.
				</td>
			</tr>
				<tr>
				<th><a target="_blank" href="https://github.com/NREL/openstudio-common-measures-gem/tree/master/lib/measures/generic_qaqc">Generic QAQC</a></th>
				<td>Commercial</td>
				<td>No</td>
				<td>No</td>
				<td>An OpenStudio reporting measure that extracts key simulation results and performs basic model QAQC checks.
				</td>
			</tr>
			<tr>
				<th><a target="_blank" href="https://github.com/urbanopt/urbanopt-scenario-gem/tree/master/lib/measures/default_feature_reports">Default Feature Reports</a></th>
				<td>Commercial and Residential</td>
				<td>Yes</td>
				<td>Yes</td>
				<td>An URBANopt Scenario Measure that creates a `default_feature_reports.json` used by URBANopt Scenario Default Post-Processor.
				</td>
			</tr>
		</tbody>
	</table>
</div>

## Functionality Example Lookup

The following table lists some of the functionality that is available in URBANopt, as well as where examples of that functionality can be found.  Scroll to the right in the table to reveal additional columns and click on the links for more details.
<div id="tablediv">
	<table>
		<thead>
			<tr>
				<th class="bold">Feature</th>
				<th>Commercial Example Project</th>
				<th>Commercial-Residential Example Project (HPXML)</th>
				<th>Example Project with electric network (OpenDSS)</th>
				<th>Example Project with Streets (RNM)</th>
				<th>Photovoltaics Example Project (REopt)</th>
				<th>Floorspace Geometry Example</th>
				<th>CreateBar Geometry Example</th>
				<th class="info-col">Additional Info</th>
			</tr>
	  </thead>
	  <tbody>
			<tr>
				<th>Default Geometry</th>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td></td>
				<td></td>
				<td>See <a href="./geometry_workflows.html#default-workflow" class="bold">Default Geometry Workflow</a> for more details.</td>
			</tr>
			<tr>
				<th>FloorspaceJS Geometry</th>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td>&#10004;</td>
				<td></td>
				<td>See <a href="./geometry_workflows.html#createbar-workflow" class="bold">FloorspaceJS Geometry Workflow</a> for more details.</td>
			</tr>
			<tr>
				<th>CreateBar Geometry</th>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td>&#10004;</td>
				<td>See <a href="./geometry_workflows.html#floorspacejs-workflow" class="bold">CreateBar Geometry Workflow</a> for more details.</td>
			</tr>
			<tr>
				<th>Residential Features</th>
				<td></td>
				<td>&#10004;</td>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td>Residential feature examples can be found by creating the combined example project with the CLI. See <a href="./residential_workflows/residential_workflows.html" class="bold">Residential Workflows</a> for more details, including how to adjust appliance and efficiency assumptions.</td>
			</tr>
			<tr>
				<th>Detailed OSM Example</th>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td></td>
				<td></td>
				<td>Commercial building features with <strong>IDs 7, 8, and 9</strong> have a detailed osm specified in the GeoJSON file and code specified in the Baseline mapper. The detailed models should be placed in the `osm_building` directory within the project folder. Create one of the selected projects with the CLI to investigate this functionality further.</td>
			</tr>
			<tr>
				<th>Detailed HPXML Example</th>
				<td></td>
				<td>&#10004;</td>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td>Residential building feature with <strong>ID 17</strong> has a detailed HPXML specified in the GeoJSON file and code specified in the Baseline mapper. The detailed HPXML models should be placed in the `xml_building` directory within the project folder. Create a 'combined' example project with the CLI to investigate this functionality further.</td>
			</tr>
			<tr>
				<th>DES Workflow</th>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td></td>
				<td></td>
				<td>The District Energy Systems workflow requires that the <em>export_time_series_modelica</em> and <em>export_modelica_loads</em> reporting measures be enabled to generate the results needed by the GeoJSON Modelica Translator. Create one of the selected projects with the CLI to view an example of enabling these measures in the workflow.osw file. See <a href="./des.html" class="bold">DES Workflow</a> for more details.</td>
			</tr>
			<tr>
				<th>QAQC OpenStudio Results Reporting</th>
				 <td>*</td>
				<td>*</td>
				<td>*</td>
				<td>*</td>
				<td>*</td>
				<td></td>
				<td>&#10004;</td>
				<td>QAQC is implemented for commercial buildings as a set of 3 reporting measures. This functionality is only enabled in the CreateBar workflow, but can be easily enabled in the workflows marked with a <em>*</em> by modifying the <a href="https://github.com/urbanopt/urbanopt-cli/blob/v0.8.0/example_files/mappers/Baseline.rb#L1023-L1025" target="_blank">Baseline Mapper</a> in your example project. Note that enabling this functionality will generate additional result files and will increase the size of your simulation. See <a href="./geometry_workflows.html#qaqc-reporting">QAQC Reporting</a> for more information.</td>
			</tr>
			<tr>
				<th>Thermal Comfort Reporting</th>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>Thermal comfort reporting is enabled on commercial building features that do not have a detailed OSM model specified. It is implemented with the  <a href="https://github.com/urbanopt/urbanopt-cli/blob/v0.8.0/example_files/mappers/base_workflow.osw#L118-L126" target="_blank">PredictedMeanVote measure</a>. Comfort results can be found in the <em>comfort_result</em> section of the URBANopt reports.</td>
			</tr>
			<tr>
				<th>Photovoltaics Workflows</th>
				<td></td>
				<td></td>
				<td></td>
				<td></td>
				<td>&#10004;</td>
				<td>;</td>
				<td></td>
				<td>Community photovoltaics is enabled in feature <strong>ID 14</strong> and ground-mount photovoltaics are enabled in feature <strong>IDs 15 and 16</strong> of the PV example project, which can be created with the CLI. Inspect the GeoJSON file to see the required fields, run the project, and post-process the results with the <a href="">REopt post processor</a> to generate the results. See <a href="./photovoltaic.html">Photovoltaics Workflows</a> for more information.</td>
			</tr>
			<tr>
				<th>EV Charging</th>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td></td>
				<td></td>
				<td>EV charging examples are available in commercial building feature <strong>IDs 2-6</strong> of the selected projects.  EV charge scaling by occupancy is enabled in feature <strong>ID 2</strong>. The <a href="https://github.com/urbanopt/urbanopt-cli/blob/v0.8.0/example_files/mappers/EvCharging.rb" target="_blank">EV Charging Mapper</a> is used to enable the measure required for this functionality. See the <a href="../resources/scenarios/evcharging.html"> EV Charging Scenario</a> for more information. </td>
			</tr>
			<tr>
				<th>Carbon Emissions Reporting</th>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td>&#10004;</td>
				<td></td>
				<td></td>
				<td>Carbon Emissions reporting is enabled in commercial building feature IDs <strong>3 & 7</strong>, and residential feature <strong>ID 14</strong> of the selected projects. See <a href="./carbon_emissions.html">Carbon Emissions Reporting</a> for more information. </td>
			</tr>
	  </tbody>
  </table>
</div>
