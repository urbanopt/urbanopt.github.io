---
layout: default
title: Workflows
has_children: true
nav_order: 4
---

# URBANopt Workflows

In this section you can find out more details about each of the workflows supported by URBANopt.

## Features

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

