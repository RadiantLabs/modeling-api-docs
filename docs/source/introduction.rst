Introduction
==========================================

Radiant Labs has built a sophisticated software platform for city and state governments, utilities, and contractors to target ideal candidates
for building energy upgrades, fuel switching, renewable energy installations, and electric vehicle deployments. Our software can aggregate 
assessor, building permit, rooftop solar potential, utility bill, manufacturing, and energy program data to generate an hourly energy model 
and anticipated replacement timelines for mechanical
  
This site provides the documentation for our Modeling API, which is Radiant Labs’ primary tool to generate energy models for homes. The Modeling API can take as little information as a building address and generate an hourly energy model as the response. We use a vast array of data sources and databases to determine and infer all of the building characteristics necessary to generate an energy model. If you have additional observed information about the home such as the primary heating fuel, the type of windows, or the amount of attic insulation, you can send that data along in your API request and the model will replace our default and inferred data with that information, increasing the accuracy of the model. Eventually, we will add support for receiving utility bill data to assist in calibrating the model to actual consumption for the highest level of accuracy.

The Modeling API also supports :ref:`automated_measures`, a streamlined approach to specifying improvements to be modeled. Answer simple questions about what upgrades will be performed and we will create an improved model and compare it to the base model to calculate energy savings.

.. note::

   This API and documentation site is under active development.

.. data_sources::

Data Sources
------------

Radiant Labs collects data from a wide range of public and private data sources to develop our “Common Schema”, which is a cleaned, merged data structure providing a host of property characteristics, including building features, owner details, demographics, and regional factors. The following tables provide details on each data source included in our pipeline and information on the recency of the data.

.. public_sources::

Public Sources
**************

============= ======================== ========================================================= ===================
Data Provider Data Source              Description                                               Data Source Version
============= ======================== ========================================================= ===================
EIA           eGrid                    Identifies geospatial electric grid emissions rates       1/30/2023
SafeGraph     Census                   Provides demographic data for the census block group      2020 5-year ACS
DOE           Justice40 Initiative     Identifies disadvantages communities                      7/20/2021
EIA           Electric Service Regions Identifies electric service regions                       12/9/2022
EIA           Electric prices          Identifies average residential electric rates by utility  2022
EIA           Fuel rates               Identifies average residential fuel costs by state        2022
============= ======================== ========================================================= ===================

.. private_sources::

Private Sources
***************

============= ======================== ========================================================================================== ===================
Data Provider Data Source              Description                                                                                Data Source Version
============= ======================== ========================================================================================== ===================
Proprietary   Tax assessor             Building characteristics documented by regional tax assessors                              Live data stream
Proprietary   Building permits         Building permit data categorized by work type. Fill rate and data quality varies by region Live data stream
============= ======================== ========================================================================================== ===================

.. modeling_methodology::

Modeling Methodology
--------------------

Radiant Labs’ primary focus is creating a high accuracy, custom energy model for each unique API request. To create our residential energy models, Radiant Labs constructs a unique representation of each dwelling unit including all major building elements, like enclosure, systems, appliances, lighting, etc. Radiant Labs creates this building representation using a staged approach:

1. Directly apply all known building characteristics using Common Schema
    - Example: Apply known HVAC Heating Type is a gas-fired furnace
2. Use proprietary functions to logically generate building geometries
    - Example: Calculate area of attic using known conditioned floor area, stories count, and footprint area
3. Populate remaining unknown building characteristics using location- and vintage-based building stock defaults
    - Example: Apply most common attic insulation R-value based on building stock characteristics of the location and building vintage

This unique dwelling-specific representation is then modeled using EnergyPlus to calculate an 8,760 hourly energy model based on TMY3 weather data.
