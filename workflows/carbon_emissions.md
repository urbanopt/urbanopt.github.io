---
layout: default
title: Carbon Emissions
parent: Workflows
nav_order: 5
---

# **Carbon Emissions Reporting**

The following documents the capabilities added to URBANopt&trade; to calculate carbon emissions associated with building electricity, natural gas, propane and fueloil use. 
Carbon emissions calculations are added to URBANopt to enable users to plan/design for low/net zero carbon buildings, neighborhoods, and urban districts. Calculated emissions values are direct emissions that occur from sources controlled or owned by an organization. Users can calculate emissions and compare the results across multiple UO Scenarios.


# Electricity Emissions

The measure used to calculate electricity emissions, called add_ems_emissions_reporting, is in the [openstudio-common-measures](https://github.com/NREL/openstudio-common-measures-gem) GitHub Repository. The functionality and the translated URBANopt user inputs are briefly defined below.

## Inputs 

URBANopt emissions inputs, along with their choices, are listed below:

1. *emissions*
	This is a required Boolean input that enables or disables emissions calculations. Options are: true or false. 

1. *emissions_future_subregion*
	This is an optional string input that defines the eGRID subregion. Users should refer to the [eGRID map](https://www.wri.org/data/map-egrid-subregions) to define this input. 
	
	Options:
	- AZNM
	- CAMX
	- ERCT
	- FRCC
	- MORE
	- MROW
	- NEWE
	- NWPP
	- NYST
	- RFCE
	- RFCM
	- RFCW
	- RMPA
	- SPNO
	- SPSO
	- SRMV
	- SRMW
	- SRSO
	- SRTV
	- SRVC

	If not defined the subregion will be selected based on a default mapper that maps states to eGRID Subregions. Warning: in reality, some states ( 'ND', 'IN', 'MN', 'SD', 'IA', 'WV', 'OH', 'NE' ) map to two eGRID Subregions. The default mapper maps the state to a subregion that covers the most zip codes in the state. The user should take care to select the appropriate subregion.  

	Default mapping:  
	- FL: FRCC
	- MS: SRMV
	- NE: MROW
	- OR: NWPP
	- CA: CAMX
	- VA: SRVC
	- AR: SRMV
	- TX: ERCT
	- OH: RFCW
	- UT: NWPP
	- MT: NWPP
	- TN: SRTV
	- ID: NWPP
	- WI: MROE
	- WV: RFCW
	- NC: SRVC
	- LA: SRMV
	- IL: SRMW
	- OK: SPSO
	- IA: MROW
	- WA: NWPP
	- SD: MROW
	- MN: MROW
	- KY: SRTV
	- MI: RFCM
	- KS: SPNO
	- NJ: RFCE
	- NY: NYCW
	- IN: RFCW
	- VT: NEWE
	- NM: AZNM
	- WY: RMPA
	- GA: SRSO
	- MO: SRMW
	- DC: RFCE
	- SC: SRVC
	- PA: RFCE
	- CO: RMPA
	- AZ: AZNM
	- ME: NEWE
	- AL: SRSO
	- MD: RFCE
	- NH: NEWE
	- MA: NEWE
	- ND: MROW
	- NV: NWPP
	- CT: NEWE
	- DE: RFCE
	- RI: NEWE

1. *emissions_hourly_historical_subregion*
	This is an optional string input that defines AVERT regions. Users should refer to the [EPA AVERT regions](https://www.epa.gov/avert/avert-tutorial-import-regional-data-file) to assign this input.

	Options: 
	- California
	- Carolinas 
	- Central 
	- Florida 
	- Mid-Atlantic 
	- Midwest 
	- New England 
	- New York 
	- Northwest 
	- Rocky Mountains 
	- Southeast 
	- Southwest 
	- Tennessee  
	- Texas

	If not defined by the user, the AVERT region will be selected based on a default mapper that maps states to AVERT regions. Warning: in reality, some states map to multiple AVERT regions. The default mapper maps the state to a AVERT region that covers the most zipcodes in the state. User should take care to select the appropriate region.

	Default Mapping:
	- FL: Florida
	- MS: Midwest
	- NE: Midwest
	- OR: Northwest
	- CA: California
	- VA: Carolinas
	- AR: Midwest
	- TX: Texas
	- OH: Midwest
	- UT: Northwest
	- MT: Northwest
	- TN: Tennessee
	- ID: Northwest
	- WI: Midwest
	- WV: Midwest
	- NC: Carolinas
	- LA: Midwest
	- IL: Midwest
	- OK: Central
	- IA: Midwest
	- WA: Northwest
	- SD: Midwest
	- MN: Midwest
	- KY: Tennessee
	- MI: Midwest
	- KS: Central
	- NJ: Mid-Atlantic
	- NY: New York
	- IN: Midwest
	- VT: New England
	- NM: Southwest
	- WY: Rocky Mountains
	- GA: SRSO
	- MO: Midwest
	- DC: Mid-Atlantic
	- SC: Carolinas
	- PA: Mid-Atlantic
	- CO: Rocky Mountains
	- AZ: Southwest
	- ME: New England
	- AL: Southeast
	- MD: Mid-Atlantic
	- NH: New England
	- MA: New England
	- ND: Midwest
	- NV: Northwest
	- CT: New England
	- DE: Mid-Atlantic
	- RI: New England

1. *emissions_future_year*
	This optional string input defines the hourly emissions factors for future years of interest. 

	Options are below (in two year increments): 
	- 2020
	- 2022
	- 2024
	- 2026
	- 2028
	- 2030
	- 2032
	- 2034
	- 2036
	- 2038
	- 2040
	- 2042
	- 2044
	- 2046
	- 2048
	- 2050 

	The default is 2030. Data source: Hourly marginal emissions factors for future years are based on the long-run marginal emissions rates for a region's load (co2_lrmer_enduse) from NREL's [Cambium tool](https://scenarioviewer.nrel.gov/) LowRECost scenario.

1. *emissions_ hourly _historical_year*
	This optional string input defines the hourly emission factors for historical years of interest. Options are: “2019”. The default is 2019.  Data source: Hourly marginal emissions factors for historical years are based on data from EPA's AVoided Emissions and geneRation Tool [AVERT](https://www.epa.gov/avert). 

1. *emissions_annual_historical_year*

	This optional string input defines the annual average emission factors for historical years of interest. 
	Options are: 
	- 2017
	- 2009 
	- 2010 
	- 2012 
	- 2014 
	- 2016 
	- 2018 
	- 2019

	The default is 2010. Data source: Annual emission factors for historical years are based on the annual CO2e total output emissions rate (SRC2ERTA) from EPA's [Emissions & Generation Resource Integrated Database (eGRID)](https://www.epa.gov/egrid)).  

These inputs are used to retrieve hourly or annual data of electricity emissions factors, defined in kgCO2e/MWH, stored in the measure resource folder (https://github.com/NREL/openstudio-common-measures-gem/tree/develop/lib/measures/add_ems_emissions_reporting/resources) . Emission factors are then multiplied by the associated facility total electricity energy use profiles to calculate emissions in metric tons (mt) and emissions intensity in kgCO2e/sqft.

To activate electricity carbon emissions calculations, inputs should be defined in the geoJSON FeatureaFile. Users can enable the emissions calculations for all of the features by adding the emissions inputs to the site/project properties at the top of the geoJSON file. 
When adding emissions inputs in the project properties, URBANopt will apply these inputs to all of the building features in the features array. Below is an example of the enabling emissions calculations via adding project properties.

```json
"project": {
    "id": "7c33a001-bccb-413e-ac87-67558b5d4b07",
    "name": "New Project",
    "surface_elevation": null,
    "import_surrounding_buildings_as_shading": null,
    "weather_filename": "USA_NY_Buffalo-Greater.Buffalo.Intl.AP.725280_TMY3.epw",
    "tariff_filename": null,
    "climate_zone": "6A",
    "cec_climate_zone": null,
    "begin_date": "2017-01-01T07:00:00.000Z",
    "end_date": "2017-12-31T07:00:00.000Z",
    "timesteps_per_hour": 1,
    "default_template": "90.1-2013",
    "emissions": true,
    "emissions_future_subregion": "NYSTc",
    "emissions_hourly_historical_subregion": "New York",
    "emissions_annual_historical_subregion": "NYCW",
    "emissions_future_year": "2030",
    "emissions_hourly_historical_year": "2019",
    "emissions_annual_historical_year": "2019"
  }
```
# Natural Gas, Propane, and FuelOil #2 Emissions

Emisison factors for natural gas, propane, and fueloil#2 are based on EPA eGRID data and calculated using 20-year GWP horizon based on ASHRAE 189.1 :
	
	natural gas :  277.358126 KG/MWH
	propane : 323.896704 KG/MWH
	Fueloil #2 : 294.962046 KG/MWH

These emission factors are then multiplied by the associated facility total energy use profile for the corresponding fuel type to calculate emissions in metric tons (mt) and emissions intensity in kgCO2e/sqft.

# Outputs

Emissions results are reported in URBANopt reports. For each feature, timeseries results are reported in the CSV report and aggregate values are reported in URBANopt JSON reports. Bellow are descriptions for the emissions results variables.

### JSON reports results

```json
"emissions": {
	"future_annual_electricity_emissions_mt":
	Future electricity emissions in metric tons (mt) calculated based on an annual emissions factor value for the selected future year

	"future_hourly_electricity_emissions_mt":  
	Future electricity emissions in metric tons (mt) calculated based on hourly emissions factor values for the selected future year

	"historical_annual_electricity_emissions_mt": 
	Historical electricity emissions in metric tons (mt) calculated based on an annual emissions factor value for the selected historical year

	"historical_hourly_electricity_emissions_mt":  
	Historical electricity emissions in metric tons (mt) calculated based on hourly emissions factor values for the selected historical year

	"future_annual_electricity_emissions_intensity_kg_per_ft2": 
	Future electricity emissions intensity in kg/ft2 calculated based on an annual emissions factor value for the selected future year

	"future_hourly_electricity_emissions_intensity_kg_per_ft2": 
	Future electricity emissions intensity in kg/ft2 calculated based on hourly emissions factor values for the selected future year

	"historical_annual_electricity_emissions_intensity_kg_per_ft2": 
	Historical electricity emissions intensity in kg/ft2 calculated based on an annual emissions factor value for the selected historical year

	"historical_hourly_electricity_emissions_intensity_kg_per_ft2":
	Historical electricity emissions intensity in kg.ft2 calculated based on hourly emissions factor values for the selected historical year

	"natural_gas_emissions_mt": 
	Natural Gas emissions in mt calculated by multipling the facility natural gas energy use (MWH) by the natural gas emission factor (277.358126 KG/MWH) and convertd to metric ton(mt)

	"natural_gas_emissions_intensity_kg_per_ft2":
	Natural Gas emissions intensity in kg/ft2 calculated by dividing the natural gas emissions (kg) for a building by its floor area(ft2)
	
	"propane_emissions_mt":
	Propane emissions in mt calculated by multipling the facility propane energy use (MWH) by the natural gas emission factor (323.896704 KG/MWH) and convertd to metric ton (mt)

	"propane_emissions_intensity_kg_per_ft2": 
	Propane emissions intensity in kg/ft2 calculated by dividing the natural gas emissions (kg) for a building by its floor area(ft2)

	"fueloil_no2_emissions_mt": 
	FuelOilNo2 emissions in mt calculated by multipling the facility FuelOilNo2 energy use (MWH) by the FuelOilNo2 emission factor (294.962046 KG/MWH) and convertd to metric ton (mt)
	
	"fueloil_no2_emissions_intensity_kg_per_ft2": 
	FuelOilNo2 emissions intensity in kg/ft2 calculated by dividing the FueLOilNo2 emissions (kg) for a building by its floor area(ft2)
}
					
```
### Timeseries CSV results

- Future_Annual_Electricity_Emissions (mt)
- Future_Hourly_Electricity_Emissions (mt)
- Historical_Annual_Electricity_Emissions (mt)
- Historical_Hourly_Electricity_Emissions (mt)
- Natural_Gas_Emissions (mt)
- Propane_Emissions (mt)
- FuelOilNo2_Emissions (mt)
- Future_Annual_Electricity_Emissions_Intensity (kg/sqft)
- Future_Hourly_Electricity_Emissions_Intensity (kg/sqft)
- Historical_Annual_Electricity_Emissions_Intensity (kg/sqft)
- Historical_Hourly_Electricity_Emissions_Intensity (kg/sqft)
- Natural_Gas_Emissions_Intensity (kg/sqft)
- Propane_Emissions_Intensity (kg/sqft)
- FuelOilNo2_Emissions_Intensity (kg/sqft)
