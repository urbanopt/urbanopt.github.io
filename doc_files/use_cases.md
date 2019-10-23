# Analysis-Scale Requirements

Identifying the target scale for URBANopt analyses is important to differentiate URBANopt from other energy modeling applications.

- Collocated buildings/systems (e.g. districts and neighborhoods) are the target scope
- Non-collocated buildings (e.g. all post offices in a portion of a city) are possible but not the target scope
- Single building analyses are better-performed using the Parametric Analysis Tool (PAT) or the OpenStudio Application
- Entire zip codes/cities are out of scope (ComStock/ResStock application)
- Approximate ideal size (across all scenarios): 100 buildings, 1-10 district systems
- Approximate maximum size (across all scenarios): 1,000 buildings, 10-100 district systems

# Deployment Requirements

- Supported on Windows, Mac, and Unix
- Easy to integrate and ship with third-party user interfaces and applications
- Integrate with Python-based workflows
- Integrate with Ruby-based workflows
- Easy to install and get started with examples quickly
- Easy to specify versions of OpenStudio, OpenStudio Standards, OpenStudio Measures
- Scenarios can be run on local computer; portions of scenarios can be run for testing
- Scenarios can be sent to single large remote compute node to run, download results
- Scenarios are run with a Make/Rake type build system, detects which jobs are out of date and runs only out of date jobs

# Use Cases

This list of use cases is not exhaustive but is more comprehensive than we will implement in the three year URBANopt project.  This helps define a total space the design must consider. Prioritization of specific use cases based on Technical Advisory Group feedback and needs of case studies in years two and three will determine which features and workflows are implemented.  

# Core Use Cases

Core use cases are central to URBANopt, they will be exercised in most URBANopt workflows. The following core use cases have previously been implemented in the initial proof of concept URBANopt platform:

- Specify building geometry for scenario using geographic information system (GIS) footprint and number of stories
- Create OpenStudio Model (OSM) for each building in scenario
- Include surrounding building geometry as shading in OSM
- Specify building-level fuel types for space cooling and space heating
- Apply generic OpenStudio Measures to buildings in workflow to model energy conservation measures (ECMs)
- Run each OSM for all buildings in scenario
- Associate buildings in scenario with one or more district systems in scenario
- Create OSM for district system, import loads from connected buildings in scenario
- Apply generic OpenStudio Measures to district systems to model ECMs
- Use combined water flow rate (at assumed temperature) required by buildings as input to the district system model
- Run each OSM for all district systems in scenario
- Aggregate all electrical loads associated with “transformer” district system type in a scenario, simulate transformer losses
- Export utility costs based on tariffs in EnergyPlus IDF format

The following core use cases have not been previously implemented but are highly desirable.  These will likely be implemented in the three year URBANopt project:

- Vary building geometry between scenarios (e.g. number and footprints of buildings are different in different scenarios)
- Specify building geometry with area and number of stories and location (latitude/longitude)
- Specify building geometry using GIS base and podium footprints and number of stories
- Specify 3D building geometry with all interior zone detail (e.g. OSM level detail)
- Import building model simulation results in CSV format (from some other simulator)
- Support different thermal zoning and space type assignments for GIS footprints
- Export geometry parameters needed for costing (e.g. wall area construction schedule)
- Specify service water heating fuel type in addition to space heating fuel type
- Implement modeling for parking structures including exterior lighting loads
- Allow comparison and scaling of GIS geometry to property tax record area
- Allow a separate heating and cooling plant to serving one building.
- Assign different utility rate tariffs to each building and energy system

The following core use cases have not been previously implemented but are desirable.  These may be implemented in the three year URBANopt project depending on priorities:

- Specify building geometry using GIS floorplan with interior zoning/space types for each floor (allow reusing floorplan on many floors)
- Specify 3D massing (exterior envelope), support slicing by floors and different thermal zoning and space type assignment algorithms
- Allow full building geometry to be considered for shading and view factor calculations but simplification of geometry via multipliers for simulation speed
- Perform detailed modeling of individual residential units
- Model tall buildings (special pumping, etc)
- Include tree geometry for shading
- Model locations of HVAC system outdoor air nodes and heat rejection nodes
- Allow multiple district thermal systems per building, e.g. when there are multiple cooling district systems serving different buildings systems (data centers, labs).
- Allow multiple transformers per building (need to associate specific E+ meters with specific transformers)
- Instantaneously generate energy estimates with no simulation using benchmark (e.g. from Portfolio Manager) or reference models (e.g. ResStock/ComStock)
- Electric Distribution Use Cases

The following core use cases have not been previously implemented but are highly desirable for GEB analyses.  These will likely be implemented in the three year URBANopt project:

- Identify transformer pinch points visually based on cumulative hours exceeding kVA limits - a quick estimate is possible without OpenDSS integration
- Identify worst case scenario coincident loads for sizing
- Export electrical distribution topology and load profiles to OpenDSS to study power flow
- Design cost-effective electrical distribution topology given building and distributed energy resource (DER) locations and load/generation profiles using RNM-US
- Upload building load profiles and other information to REopt Lite API for optimal sizing and dispatch of behind-the-meter solar photovoltaics and electric batteries
- Model PV with or without detailed PV geometry and context
- Figure out maximum amount of PV given “PV-possible area”
- Export electrical system parameters needed for costing
- Investigate tradeoffs between building efficiency, building demand flexibility, DERs, and distribution system infrastructure at district-scale

The following core use cases have not been previously implemented but are desirable for GEB analyses.  These may be implemented in the three year URBANopt project depending on priorities:

- Model electric storage with “best practice” controls
- Apply stochastic schedules, loads, etc to generate load diversity
- Incorporate electric vehicle charging models with building loads
- District Thermal System Use Cases

The following core use cases have not been previously implemented but are highly desirable.  These will likely be implemented in the three year URBANopt project:

- Calculate piping length for district system in URBANopt scenario
- Export district energy systems (DES) parameters needed for costing
- Simulate tradition four-pipe hot and cold water DES in Modelica
- Simulate ambient loop (5th Generation District Heating and Cooling) DES with large temperature deadband and multiple diverse loads in Modelica
- Generate reduced order models (ROM) for buildings in scenario to integrate with DES models in Modelica
- Simulate new industrial waste heat energy sources (complimentary scope from concurrent AMO project)
- Simulate combined heat and power systems (complimentary scope from concurrent AMO project)

The following core use cases have not been previously implemented but are desirable.  These may be implemented in the three year URBANopt project depending on priorities:

- Model thermal storage with “best practice” controls
- Model river and sea water cooling
- Model geothermal, lake, pond heat sources
- Create equivalent representation of specific OpenStudio Measures for ROMs
- Combine multiple buildings on a shared district thermal system into a single OpenStudio model with district system included
- Determine the optimal topology layout for DESs. The topologies could be in the form of radial, mesh, hybrid, or any other format.
- Determine the optimal control settings for a mesh networked 5th generation bi-directional DES. Includes analysis using MPC for optimal control.

# Results Use Cases

The following core use cases have not been previously implemented but are highly desirable.  These will likely be implemented in the three year URBANopt project:

- Calculate scenario energy use
- Calculate scenario energy cost
- Use utility rate data from the Utility Rate Database (URDB)
- Apply tariff to precalculated energy use
- Calculate scenario total first cost given project specific construction and material costs
- Calculation of construction cost should not require re-running energy simulations
- Calculate scenario lifecycle costs (energy and construction costs)
- Compare costs between scenarios
- Export building model simulation results to CSV format (for some other simulator)
- Support custom post processing for results analysis or custom modeling
- Build visualizations/dashboard using D3, Tableau, Microsoft BI, etc
- Pre-run multiple scenarios, present results in custom GUI such as Design Explorer

The following core use cases have not been previously implemented but are desirable.  These may be implemented in the three year URBANopt project depending on priorities:

- Visualize energy use on geometry for each scenario using timeslider to see when and where energy is being used
- Calculate scenario cash flows over multiple years given staged build out and/or ECM application
- Compare modeled energy use to measured, benchmark, or reference model use

# Urban Weather Use Cases

The impacts of urban microclimate are of great interest to the research community.  URBANopt does not plan to implement tools for modeling urban microclimate effects.  However, existing tools will be evaluated and may be integrated with URBANopt based on the needs of case studies.

- Calculate surface temperatures of all surfaces in URBANopt scenario, includes building surfaces but also ground (e.g. grass, lake, parking lot) and sky temps
- Calculate view factors to other buildings and other surfaces using radiance
- Include view factors to other buildings and other surfaces as input to OSM building model, include simulated surface temperatures if available or allow use of default
- Compute adjusted urban weather file
- Compute exterior shading for entire district using radiance, use this as input to multiple EnergyPlus simulations
- Calculate reflectance off of other buildings, ground cover, add to direct solar
- Model exterior boundary conditions of tall buildings
- Calculate impact of trees and greenspace on urban weather
- Calculate outdoor thermal comfort, add comfort to walk score
- Use CFD simulation results (temperatures, pressures per surface) as input to EnergyPlus

# Future Use Cases

The following use cases have been identified as desirable but are not likely to be considered in the current three year URBANopt project.  If these functionalities are developed in other projects they may be considered for integration with URBANopt for use in case studies.

- Multi-objective optimization of multiple systems (buildings, DERs, DESs) in a scenario
- Optimization of particular systems (e.g. buildings, district system, distribution grid, PV/storage sizing) may be possible using non-URBANopt workflows.  
- Automatic calibration of individual buildings, DERs, or DESs to measured use
- Calibration may be possible using non-URBANopt workflows
- Automatic calibration of aggregated buildings, DERs, or DESs use to measured use
- Manual calibration may be possible by running multiple scenarios
- Run scenario multiple times with stochastic simulations to generate output distributions
- Support CityGML as an input format
- Support BuildingSync as an input format for buildings
- Automated report generation (e.g. in Microsoft Word format)
- Co-simulate buildings, electrical distribution, and district thermal energy systems

# Software Design Strategies

After reviewing the use cases and requirements above, it becomes clear that URBANopt should support multiple input formats for specifying geometry and properties associated with features in each scenario.  Third party applications are well suited to manage this type of data and already export data in common formats.  URBANopt desires to map these file formats to multiple simulation engines (OpenStudio, Modelica, REopt Lite, OpenDSS, and RNM-US).  The key to supporting this flexibility is to decouple geometry and properties from the simulation mapping code:

Static geometry and properties can be written in existing file formats (GeoJSON, CityGML, etc.) by many tools; reuse these formats
Simulation mapping (e.g. how to convert geometry and properties to simulations inputs) should be captured in scripted programing languages
It should be easy for a third-party application to update the geometry and properties in a static file and then rerun the simulation mapping scripts
New geometry and property file formats can be supported by writing new simulation mapping scripts
New simulation engines can also be supported by writing new simulation mapping scripts

[Back to main documentation](../README.md#welcome-to-urbanopt)
