---
layout: default
title: Getting Started
nav_order: 1
---
<div class="row">
	<div class="col">
		<p>To run URBANopt&trade;, first follow the <a href="../installation/installation" class="bold">installation instructions</a> to install the URBANopt CLI and all of its dependencies.</p>
		<p>Once the CLI is installed, help is available by typing <code><span class="code-text">uo --help</span></code> from the command line.  Detailed help for each command can be found with <code><span class="code-text">uo [the command name] --help</span></code>. The main CLI commands are: <code><span class="code-text">create</span></code>, <code><span class="code-text">run</span></code>, <code><span class="code-text">process</span></code>, <code><span class="code-text">visualize</span></code>, <code><span class="code-text">opendss</span></code>, and <code><span class="code-text">delete</span></code>.</p>
		<p>Before you start, think about the capabilities and analyses you want to utilize and setup your project accordingly. The best way to start is to use the example project made with the CLI.  Once you are familiar with the commands you can customize your project.</p> 
	</div>
	<div class="col" style="padding-left:20px;">
	<h2 style="padding-bottom:20px;">Important Notes</h2>
	<p>Keep the project directory path short to avoid errors related to long paths, especially when running on Windows.  For more information on this error, refer to the <a href="../developer_resources/known_issues" class="bold">known issues section</a>.</p>
	<p>We recommend calling all URBANopt commands from outside of the project you created, using relative or absolute paths to the relevant files.</p>
	<div style="float:right;padding-top:30px;padding-right:20px;"><a href="../resources/definitions" class="btn btn-uo white-text">Definitions</a></div>
	</div>
</div>
<hr class="blue-line">

<h1>Steps</h1>
<p>The overall URBANopt process is shown in the graphic below.
<img src="../doc_files/process-steps.png" alt="UO process steps image">
<p>Read through each section below for more information on each step of the process&mdash;including CLI command examples&mdash;and for details on the different workflows available.</p> 
<div class="row blue-section">
	<div class="col-8">
		<h2 class="white-text" id="step1">1. Create Project</h2>
		<p>Set up the project directory structure and example files</p>
	</div>
	<div class="col-4 my-auto"><img src="../doc_files/started__step1_graphic.png" alt="image of an open directory folder and its contents"></div>
</div>
<p>Expand the sections below and choose the option that is right for your project. Most of the options will create both a project directory structure and example project files that will allow you to try out the additional commands.  Visit the <a href="documentation/example" class="bold">example project page</a> to learn more about the example project.</p>

<ul class="jk_accordion">
  <li class="acc">
    <input id="accordion1" type="checkbox" />
    <label for="accordion1">Use the default workflow</label>
    <div class="show">
      <p>Use the command below to create a default project:</p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">uo create --project-folder &lt;path/to/PROJECT_DIRECTORY_NAME&gt;</span></code></pre>
      </div>
      <p>This creates a project folder containing the example project using the default geometry workflow with <code>urban-geometry-creation-zoning</code> measure, and downloads related weather files and detailed models to the appropriate directories.</p>
      <p>For more details on the various geometry workflows, refer to the <a href="../usage/geometry_workflows" class="bold">geometry_workflows page</a>.</p>  
    </div>
  </li>
  <li class="acc"><input id="accordion2" type="checkbox" /><label for="accordion2">Use Createbar geometry workflow</label>
  <div class="show">
    <p>This project uses the <code>create_bar_from_building_type_ratio</code> measure to create building geometry.</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">uo create --project-folder --createbar &lt;path/to/PROJECT_DIRECTORY_NAME&gt;</span></code></pre>
    </div>
    <p>For more details on the various geometry workflows, refer to the <a href="../usage/geometry_workflows" class="bold">geometry_workflows page</a>.</p>
  </div>
  </li>
  <li class="acc"><input id="accordion3" type="checkbox" /><label for="accordion3">Use Floorspace geometry workflow</label>
  <div class="show">
    <p>This creates building geometry from floor plans with stub space types drawn using FloorSpaceJS.</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">uo create --project-folder --floorspace &lt;path/to/PROJECT_DIRECTORY_NAME&gt;</span></code></pre>
    </div>
    <p>For more details on the various geometry workflows, refer to the <a href="../usage/geometry_workflows" class="bold">geometry_workflows page</a>.</p>
  </div></li>
  <li class="acc"><input id="accordion4" type="checkbox" /><label for="accordion4">Include Residential buildings in your project</label>
  <div class="show">
    <p>As of version 0.4.0, URBANopt support a workflow that combines commercial building types and residential building types (Single-family Detached only for now, Single-family Attached and Multifamily will be included in a future release).</p>
    <p>To create a project that contains all files required to run this combined workflow, add the <code>--combined</code> option to the create command:</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo create --combined --project-folder &lt;path/to/PROJECT_DIRECTORY_NAME&gt;</span></code></pre>
    </div>
    <p>The rest of the CLI commands are the same as for the default workflow. Make sure that you use and inspect the <code>example_project_combined.json</code> FeatureFile in your project directory to see an example of a residential feature specification (feature IDs 14, 15, and 16) and the additional fields required for residential building types.</p>
    <p>Residential building energy models in URBANopt are created using the OpenStudio-HPXML workflow. Visit the <a href="../usage/residential_workflows" class="bold">Residential Workflows page</a> to learn more.</p>
  </div>
  </li>
  <li class="acc"><input id="accordionA" type="checkbox" /><label for="accordionA">Include OpenDSS functionality in your project</label>
    <div class="show">
      <p>In order to use the OpenDSS functionality successfully, the FeatureFile should contain Electrical Connectors and Junctions.  Use the command below to create an example project containing a FeatureFile with electrical network defined within it (<code>example_project_with_electric_network.json</code>). You can also use your own Feature File as long as the electrical infrastructure is defined and connected to the buildings accordingly.</p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo create --electric --project-folder &lt;path/to/PROJECT_DIRECTORY_NAME&gt;</span></code></pre>
      </div>
      <p>View the <a href="#opendss" class="bold">OpenDSS section</a> under <a href="#analyses" class="bold">Additional Capabilities</a> below for more details on the OpenDSS functionality.</p>
    </div>
  </li>
</ul>
<p>Other Options</p>
<ul class="jk_accordion">
  <li class="acc"><input id="accordion5" type="checkbox" /><label for="accordion5">Create an empty project directory structure</label>
    <div class="show">
    <p>This option creates a project directory structure without an example FeatureFile or weather files. You can download weather files and add to this folder from the <a href="https://energyplus.net/weather" target="_blank" class="bold">EnergyPlus Website</a>.</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">uo create --empty --project-folder &lt;path/to/PROJECT_DIRECTORY_NAME&gt;</span></code></pre></div>
    </div>
  </li>
  <li class="acc"><input id="accordion6" type="checkbox" /><label for="accordion6">Overwrite an existing project</label>
  <div class="show">
    <p>By default, the CLI will abort if the project directory being created already exists. To overwrite an existing folder, use the <code>--overwrite</code> option. This deletes anything in the named folder and creates a fresh project directory. Can be combined with the <code>-e</code> option to overwrite a directory with a new empty URBANopt project directory.</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">uo create --overwrite --project-folder &lt;path/to/PROJECT_DIRECTORY_NAME&gt;</span></code></pre>
    </div>
  </div>
  </li>
</ul>
<div class="row blue-section">
  <div class="col-7">
    <h2 class="white-text" id="step2">2. Set up Scenario</h2>
    <p>Assign a Scenario Mapper to each feature <br/>
    Configure additional settings</p>
  </div>
  <div class="col-5 my-auto lr-pad-0"><img src="../doc_files/started__step2_graphic.png" width="100%" alt="image of CSVs being inserted into a directory folder"></div>
</div>
<p>If you are not using an example project, ensure that your FeatureFile is in the root of the project directory.  If you are using an example project, an example feature file is provided for you.</p>
<ul class="jk_accordion">
  <li class="acc"><input id="accordion7" type="checkbox" /><label for="accordion7">Create a Scenario CSV File for each mapper</label>
    <div class="show">
    <p>The following command will create a ScenarioFile for each mapper contained in the project directory.  The resulting CSV files will map all features in the FeatureFile to the particular scenario mapper. The scenario mappers currently included in URBANopt as listed above. Visit the <a href="../usage/scenarios/scenarios" class="bold">Scenarios page</a> to learn more about each scenario mappers.</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">uo create --scenario-file &lt;path/to/FEATUREFILE.json&gt;</span></code></pre></div>
    </div>
  </li>
  <li class="acc"><input id="accordion8" type="checkbox" /><label for="accordion8">Enable REopt Functionality</label>
    <div class="show">
      <ol>
        <li>To run a REopt scenario you will need an internet connection so the REopt™ Gem can access the REopt Lite API.</li>
        <li>Obtain an API key from the <a href="https://developer.nrel.gov/" class="bold">NREL Developer Network</a> to use the <strong>REopt Lite API</strong>. Copy and paste your key as an environment variable named <code>GEM_DEVELOPER_KEY</code> on your computer. Step-by-step instructions for creating env variables are found in the <a href="../installation/installation" class="bold">installation docs</a> for your operating system.
          <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text"> GEM_DEVELOPER_KEY = '&lt;insert your NREL developer key here'&gt;</span></code></pre></div>
        </li>
        <li>Extend the Scenario CSV File with REopt information. After following the instructions above to create a basic Scenario CSV File for each mapper, use the command below to create a new Scenario CSV File (named REopt_scenario.csv by default) that has an extra column to map assumption files to features. Use this Scenario CSV File going forward in future steps. </p>
        <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">uo create --reopt-scenario-file &lt;path/to/EXISTING_SCENARIO_FILE.csv&gt;</span></code></pre></div>
        </li>
        <li><p>Configure your REopt assumptions. Two example <strong>REopt Lite</strong> assumption files are located in the <code>reopt</code> folder within your project directory:  <code>base_assumptions.json</code> and <code>multiPV_assumptions.json</code>. These files follow the format outlined in the <a href="https://developer.nrel.gov/docs/energy-optimization/reopt-v1/" target="_blank" class="bold">REopt Lite API documentation</a> and can be customized to your specific project needs. Though CLI commands, they will be updated with basic information from your Feature and Scenario Reports (i.e. latitude, longitude, electric load profile) and submitted to the <strong>REopt Lite API</strong>.</p>
        <p>In particular, you will want to make sure that the <code>urdb_label</code> in the assumptions file maps to a suitable utility rate <em>label</em> from the <a href="https://openei.org/apps/IURDB/" target="_blank" class="bold">URDB</a>. The <em>label</em> is the last term of the URL of a utility rate detail page (e.g. the <em>label</em> for the rate at <a href="https://openei.org/apps/IURDB/rate/view/5b0d83af5457a3f276733305" target="_blank" class="bold">https://openei.org/apps/IURDB/rate/view/5b0d83af5457a3f276733305</a> is 5b0d83af5457a3f276733305).</p>
        <p>Also note that the example <code>reopt/multiPV_assumptions.json</code> file contains an array of PV inputs to allow for the optimization of multiple PV systems at once (e.g. rooftop and ground-mount).</p>
        </li>
      </ol>
      <p>Visit the <a href="../additional_documentation/reopt/reopt" class="bold">REopt page</a> for more details on using REopt with URBANopt.</p>
    </div>
  </li>
</ul>
<p>Other Options</p>
<ul class="jk_accordion">
  <li class="acc"><input id="accordion9" type="checkbox" /><label for="accordion9">Create a scenario CSV File for a single Feature</label>
    <div class="show">
    <p>If you run into the need to run a single feature through the URBANopt process, you will first need to generate a Scenario CSV File containing only that Feature. To accomplish this, specify the Feature_ID in the arguments as shown here:</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text"> uo create --scenario-file &lt;path/to/FEATUREFILE.json&gt; --single-feature &lt;FEATURE_ID&gt;</span></code></pre></div>
    </div>
  </li>
  <li class="acc"><input id="accordion10" type="checkbox" /><label for="accordion10">Create a mixed Scenario CSV File</label>
    <div class="show">
    <p>The default process to create a Scenario CSV File for each mapper assigns the same mapper to all Features in the FeatureFile.  If you wish to customize the scenario so that features are mapped to different mappers, the best course fo action is to first generate the basic Scenario CSV Files for each mapper and then edit them to assign the various mappers.</p>
    <p>The following figure represents how Simulation Mapper Classes can be assigned to different Features from the FeatureFile in the Scenario CSV.</p>
    <img src="../doc_files/scenario_mapper.jpg" alt="diagram showing assigment of different mapper classes to different features">
    </div>
  </li>
  <li class="acc"><input id="accordion11" type="checkbox" /><label for="accordion11">Customize the mappers</label>
    <div class="show">
    <p>In addition to Scenario mappers included in the URBANopt CLI, you can write your own mapper file for a specific use case. You can also edit the existing mappers in your project directory.  Visit the <a href="../customization/customization" class="bold">Customizations page</a> to learn about how to customize your URBANopt workflow.</p>
    </div>
  </li>
</ul>
<div class="row blue-section">
  <div class="col-10">
    <h2 class="white-text" id="step3">3. Run Scenario</h2>
    <p>Simulate Energy Usage of each FeatureFile mapped in a Scenario CSV File</p>
  </div>
  <div class="col-2 my-auto"><img src="../doc_files/started__step3_graphic.png" alt="image of a run icon"></div>
</div>
<p>Expand the sections below to learn more about running a basic scenario and running a reopt-enabled project.</p>
<div class="important-note"><p><strong>Config File</strong>&mdash;there is a <em>runner.conf</em> file automatically created in the project folder. This file can be used to configure the number of features to process in parallel as well as a few other parameters. Make edits to this file prior to running the project.</p></div>
<ul class="jk_accordion">
   <li class="acc"><input id="accordion12" type="checkbox" /><label for="accordion12">Run a basic project</label>
    <div class="show">
    <p>Simulate the energy usage of each feature in a scenario.  Use the following command to specify the appropriate FeatureFile and Scenario CSV File:</p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text"> uo run --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
    </div>
  </li>
  <p>Or</p>
  <li class="acc"><input id="accordion13" type="checkbox" /><label for="accordion13">Run a project with REopt functionality</label>
    <div class="show">
      <p>To simulate energy usage for a scenario with REopt-enabled functionality, add the <code>--reopt</code> flag to the command to extend the Scenario CSV File with REopt functionality:</p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text"> uo run --reopt --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
    </div>
  </li>
</ul>
<div class="row blue-section">
  <div class="col-10">
    <h2 class="white-text" id="step4">4. Post-Process Scenario</h2>
    <p>Simulate Energy Usage of each FeatureFile mapped in the Scenario CSV File</p>
  </div>
  <div class="col-2 my-auto"><img src="../doc_files/started__step4_graphic.png" alt="icon of a report file"></div>
</div>
<p>Expand the sections below to choose the option that is right for your project.</p>
<div class="important-note"><p>You must run the default post-processing command before running any additional analyses such as OpenDSS and DES, or post-processing any other information, such was REopt.</p></div>
<ul class="jk_accordion">
  <li class="acc"><input id="accordion14" type="checkbox" /><label for="accordion14">Post-process general results (default post-processor)</label>
    <div class="show">
    <p>To post-process the simulated features into a Scenario report, call the CLI process command using the <code>--default</code> flag:</p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo process --default --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
    <h3>Output Files</h3>
    <p>The post-process command will aggregated data across all the features in a scenario and generate results files in the scenario results folder: </p>
      <ol>
        <li><strong>JSON </strong>&mdash; a <code>default_scenario_report.json</code> file containing overall results</li>
        <li><strong>CSV Timeseries</strong>&mdash; a <code>default_scenario_report.csv</code> file containing aggregated timeseries data.</li>
      </ol>
    <div class="important-note"><p>If the scenario is consequently post-processed with another option flag (i.e. reopt or opendss, see sections below), the new data will be appended to these existing results files.</p></div>
  </div>
  </li>
</ul>
<p>Other Options</p>
<ul class="jk_accordion">
  <li class="acc"><input id="accordion15" type="checkbox" /><label for="accordion15">Post-process REopt results</label>
    <div class="show">
      <p><strong>REopt Lite</strong> optimization happens during the post-processing of each scenario, after the scenario is run. Two types of REopt optimization are available:</p>
      <ol>
        <li><strong>scenario-level</strong>, which optimizes for the aggregate load of the entire district being simulated, and</li>
        <li><strong>feature-level</strong>, which optimizes each building’s load individually.</li>
      </ol>
      <p>You may chose to optimize by one or both of these approaches according to your project objectives.</p>
      <div class="important-note"><p>Note&mdash;You will need an internet connection so the REopt™ Gem can access the REopt Lite API.</p></div>
      <p><strong>To optimize at the scenario-level, use the <code>--reopt-scenario</code> flag:</strong></p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo process --reopt-scenario --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
      <p>The <code>--reopt-scenario-assumptions-file</code> (or <code>-a</code>) option can be used to specify the path to the assumptions file to use for this optimization. If none are specified, the <code>base_assumptions.json</code> file in the <code>reopt</code> folder of the project directory will be used.</p>
      <p><strong>To optimize at the feature-level, use the <code>--reopt-feature</code> flag:</strong></p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo process --reopt-feature --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/REoptEnabledSCENARIOFILE.csv&gt;</span></code></pre></div>
      <p>For this optimization, the assumptions file is specified per feature in the Scenario CSV file</p>
      <p>Visit the <a href="../additional_documentation/reopt/reopt_post_processing" class="bold">REopt Workflow page</a> for more details on using REopt.</p>
    </div>
  </li>
  <li class="acc"><input id="accordion16" type="checkbox" /><label for="accordion16">Post-process OpenDSS results</label>
    <div class="show">
      <p>To post-process OpenDSS results back into the main Scenario JSON and CSV results files, use the <code>--opendss</code> flag.</p>
      <p>Note%mdash Run this command <strong>after</strong> you <strong>a)</strong> post-process the general scenario results and <strong>b)</strong> run the opendss workflow as described in the Additional Analyses section below.</p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo process --opendss --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
      <p>For more information on the OpenDSS workflow, visit the <a href="../additional_documentation/opendss/opendss" class="bold">OpenDSS page</a>.</p>
    </div>
  </li>
  <li class="acc"><input id="accordionQ" type="checkbox" /><label for="accordionQ">SQL Database Output File</label>
    <div class="show">
      <p>In addition to the JSON and CSV output files, a <strong>SQL Database</strong> file (<code>default_scenario_report.db</code>) containing aggregated energy use across the scenario is also available. The relevant data is in the <strong>ReportData</strong> table to match the structure of the <code>eplusout.sql</code> file generated for each building by EnergyPlus.</p>
      <p>The ReportData table structure is as follows:</p>
      <table>
        <tr>
          <th>TimeIndex</th>
          <th>Year</th>
          <th>Month</th>
          <th>Day</th>
          <th>Hour</th>
          <th>Minute</th>
          <th>Dst</th>
          <th>ReportDataDictionaryIndex</th>
          <th>Value</th>
        </tr>
        <tr>
          <td>Integer</td>
          <td>Var</td>
          <td>Var</td>
          <td>Var</td>
          <td>Var</td>
          <td>Var</td>
          <td>Var</td>
          <td>Integer <br/> 10 = Electricity; 1382 = Gas</td>
          <td>Integer <br/> Amount (J)</td>
        </tr>
      </table>
      <p>Add the <code>--with-database</code> flag to your command to generate this file:</p>
     <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo process --default --with-database --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
    </div>
  </li>
</ul>

<div class="row blue-section">
  <div class="col-12">
    <h2 class="white-text" id="analyses">Additional Capabilities (Optional)</h2>
    <p>Add Electrical Distribution or Distributed Thermal Systems Analysis to your Scenario</p>
  </div>
</div>
<p>Expand the sections below to learn more about optional capabilities available in URBANopt</p>
<ul class="jk_accordion">
   <li class="acc" id="opendss"><input id="accordion17" type="checkbox" /><label for="accordion17">OpenDSS Functionality</label>
    <div class="show">
      <p><strong>OpenDSS</strong> is an open-source tool that is popular for simulating electrical distribution systems. The <strong>DIstribution Transformation TOol (DiTTo)</strong> is an open source and many-to-many conversion tool that has been developed by NREL to simplify converting data between distribution models. Finally, the <strong>URBANopt DiTTo Reader</strong> package and CLI provide the link between URBANopt and OpenDSS.</p> 
      <p>The entire DiTTo-Reader to OpenDSS workflow is available in URBANopt via the opendss URBANopt CLI command.</p>
      <div class="important-note">
        <p> Since the DiTTo Reader and OpenDSS functionality is written in python, additional dependencies will need to be installed if you wish to use this workflow. Visit the <a href="../additional_documentation/installation/ditto_reader.md" class="bold">OpenDSS Installation page</a> to install OpenDSS and URBANopt DiTTo Reader.
      </p>
      </div>
      <p>Once you have installed Python and urbanopt-ditto-reader, you can use the <code>opendss</code> CLI command to access the OpenDSS functionality. You can use the <code>opendss</code> CLI command after you have run the scenario (using a FeatureFile that contains a fully-connected electrical network) and post-processed the general results with the <code>--default</code> post-processor.  The OpenDSS workflow will use these results file in the processing.</p>
      <h3>Step-by-Step of the entire OpenDSS-enabled workflow:</h3>
      <ol class="t">
        <li class="t">Create an example project with the <code>--electric</code> option or use your own FeatureFile containing electrical network information.</li>
        <li class="t">Create the Scenario CSV file and run the project as explained above in steps 2 and 3.  You can run the basic project or the REopt-enabled project</li>
        <li class="t">Post-process the general results with the <code>uo process --default</code> command described above in step 4.  If you have a REopt-enabled project, also post-process the results with the desired REopt post-processor (either <code>--reopt-scenario</code> or <code>--reopt-feature</code>)</li>
        <li class="t">Run OpenDSS:
          <p>To run the general OpenDSS workflow, use the following command:</p>
           <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo opendss --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
          <p>To run the REopt-enabled OpenDSS workflow, include the <code>--reopt</code> flag in the command:</p>
          <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo opendss --reopt --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
          <p>In addition to the required `--scenario` and `--feature` options, there are optional options that can be specified, as listed below.  You can also run the CLI help command for more information and examples: `uo opendss -h`</p>
          <ul class="t">
            <li class="t"><code>--equipment</code>: Path to custom equipment file. View the <a href="../workflows/opendss">OpenDSS page</a> for more info.</li>
            <li class="t"><code>--start-time</code>: Beginning of the period for OpenDSS analysis. Defaults to beginning of simulation time Format: "YYYY/MM/DD HH:MM:SS" (use quotes).</li>
            <li class="t"><code>--end-time</code>: End of the period for OpenDSS analysis. Defaults to end of simulation time. Format "YYYY/MM/DD HH:MM:SS" (use quotes).</li>
            <li class="t"><code>--timesteps</code>: Number of minutes per timestep in the OpenDSS simulation </li>
          </ul>
          <p> Alternatively, a config JSON file can be used to set the OpenDSS options. An <a href="https://github.com/urbanopt/urbanopt-ditto-reader/blob/develop/urbanopt_ditto_reader/example_config.json" target="_blank">example config JSON file</a> is available. Note the key names are slightly different than the CLI option names. This config file can be passed into the CLI command:</p>
          <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo opendss --config &lt;path/to/config.json&gt;</span></code></pre></div>
        </li>
        <li class="t"><p>Finally, post-process the results with the <code>--opendss</code> post-processor to pull the openDSS results back into the main result files.</p>
       <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo process –opendss –feature &lt;path/to/FEATUREFILE.json&gt; –scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
        </li>
      </ol>
    </div>
  </li>
  <li class="acc"><input id="accordion18" type="checkbox" /><label for="accordion18">DES Functionality</label>
    <div class="show">
      <p><strong>DES functionality is available in URBANopt CLI version 0.5.2 and above.</strong></p>
      <p>Once a scenario has been run and processed as explained in the sections above, a district thermal simulation can then be run using the output from the SDK. While Additional district energy system simulation capabilities will be added in the future, only timeseries simulations of 4th generation district heating & cooling systems are currently available.</p>
      <p>Follow the steps below to configure, create, and run your DES simulation:</p>
      <ol class="t">
        <li class="t">Build a system parameters JSON config file from the existing URBANopt processed results:
           <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo des_params --sys-param-file &lt;path/to/create/new/sys_params.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt; --feature &lt;path/to/FEATUREFILE.json&gt; --model-type time_series</span></code></pre></div>
        </li>
        <li class="t">Create a Modelica model directory and give it a name:
          <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo des_create --sys-param &lt;path/to/sys_params.json&gt; --feature &lt;path/to/FEATUREFILE.json&gt; --des-name &lt;path/to/create/new/modelica_dir&gt; --model-type time_series</span></code></pre></div>
        </li>
        <li class="t"> Run the Modelica simulation:
           <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text"> uo des_run --model &lt;path/to/modelica_dir&gt;</span></code></pre></div>
        </li>
      </ol>
    </div>
  </li>
  <li class="acc"><input id="accordionV" type="checkbox" /><label for="accordionV">Validate Results</label>
    <div class="show">
    <p>URBANopt provides a method to validate the EUI results from your full year simulation against generally used values. This can be used to confirm that your simulation results are within the right ballpark. The schema file is included in the project_dir and can be customized if your buildings are unusual. </p>
    <p>This functionality requires the <code>--scenario_file</code> and <code>--feature_file</code> options to be specified</p>
    <p>Currently only supports validating eui, and requires the path to the validation_schema that you are using.  An example validation schema can be found in the project directory.</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo validate --eui validation_schema.yaml --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt;</span></code></pre></div>
    <p>Optionally, you can specify the units to work with.  Valid options are <code>SI</code> and <code>IP</code>; defaults to <code>IP</code> if not specified.</p>
    <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">  uo validate --eui validation_schema.yaml --feature &lt;path/to/FEATUREFILE.json&gt; --scenario &lt;path/to/SCENARIOFILE.csv&gt; --units IP</span></code></pre></div>
    </div>
  </li>
</ul>
<div class="row blue-section">
  <div class="col-10">
    <h2 class="white-text" id="step5">5. Plot Results</h2>
    <p>Generate visualizations at the feature level and scenario level.</p>
  </div>
  <div class="col-2 my-auto"><img src="../doc_files/started__step5_graphic.png" alt="icon of a chart"></div>
</div>
<p>Once one or more scenarios have been run and post-processed, the results can be visualized either at the scenario level, or for each individual feature within a single scenario.
<ul class="jk_accordion">
  <li class="acc"><input id="accordion19" type="checkbox" /><label for="accordion19">Visualize and compare all Scenarios</label>
    <div class="show">
      <p>Use the following command to visualize and compare the post-processing results for <em>all scenarios</em>:</p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">   uo visualize --scenario &lt;path/to/FEATUREFILE.json&gt;</span></code></pre></div>
      <p>The resulting visualizations can be viewed in the <code>scenario_comparison.html</code> file in the run folder.</p>
    </div>
  </li>
  <li class="acc"><input id="accordion19" type="checkbox" /><label for="accordion19">Visualize and compare all Features within a Scenario</label>
    <div class="show">
      <p>To visualize and compare the post-processing results for <em>all features</em> in a particular scenario:</p>
      <div class="language-terminal highlighter-rouge"><pre class="highlight"><code><span class="code-text">   uo visualize --scenario &lt;path/to/FEATUREFILE.json&gt;</span></code></pre></div>
      <p>The resulting visualizations can be viewed in the <code>feature_comparison.html</code> file in the scenario folder.</p>
    </div>
  </li>
</ul>
<p>Note&mdash;You need to run the default post-process command before visualizing the results.</p>