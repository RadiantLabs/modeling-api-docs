API
===

..
   .. _overview:

   Overview
   --------

   .. _authentication:

   Authentication
   --------------

.. _endpoints:

Endpoints
---------

POST /v1/timelines
******************

.. _post_timelines_request:

Top-level request payload

.. literalinclude:: schemas/request/TopLevelRequestPayload.json5
   :language: javascript

.. _post_timelines_response:

Top-level response payload

.. literalinclude:: schemas/response/TopLevelResponsePayload.json5
   :language: javascript

.. _schemas:

Schemas
-------

.. _address_components:

Address Components
******************

.. literalinclude:: schemas/request/AddressComponents.json5
   :language: javascript

.. _appliances:

Appliances
**********

.. literalinclude:: schemas/request/Appliances.json5
   :language: javascript

.. _automated_measures:

Automated Measures
******************

See :ref:`automated_measures` for further details.

.. literalinclude:: schemas/request/AutomatedMeasures.json5
   :language: javascript

.. _base_building:

Base Building
*************

.. literalinclude:: schemas/request/BaseBuilding.json5
   :language: javascript

   ========================  =======  ========  ===========  ========  ==================  ============================================== 
   Property                  Type     Units     Constraints  Required  Default             Notes                                                                                           
   ========================  =======  ========  ===========  ========  ==================  ============================================== 
   ``conditionedFloorArea``  integer  ft2       >0           no        PSC                 If missing from PSC, model will fail           
   ``averageCeilingHeight``  integer  ft        >0           no        8                                                        
   ``bathCount``             integer  count     >0           no        see [#]_
   ``bedroomsCount``         integer  count     >0           no        PSC, BSA
   ``residentCount``         integer  count     >=0          no        BSA                                                       	
   ``storiesCount``          integer  count     >0           no        PSC, BSA
   ``windowToWallFraction``  float    fraction  >0           no        0.14
   ``yearBuilt``             integer  year      >1600        no        PSC                 If missing from PSC, model will fail           
   ========================  =======  ========  ===========  ========  ==================  ============================================== 

   .. [#] ``bedroomsCount``/2 + 0.5

.. _cost:

Cost
****

.. literalinclude:: schemas/request/Cost.json5
   :language: javascript

.. _custom_measure:

Custom Measure
**************

.. literalinclude:: schemas/request/CustomMeasure.json5
   :language: javascript

.. _electrical:

Electrical
**********

.. literalinclude:: schemas/request/Electrical.json5
   :language: javascript

.. _emission_totals:

Emission Totals
***************

.. literalinclude:: schemas/response/EmissionTotals.json5
   :language: javascript

.. _emission_totals_per_fuel_time_series:

Emission Totals per Fuel Time Series
************************************

.. literalinclude:: schemas/response/EmissionTotalsPerFuelTimeSeries.json5
   :language: javascript

.. _enclosure:

Enclosure
*********

.. literalinclude:: schemas/request/Enclosure.json5
   :language: javascript

.. _energy_costs:

Energy Costs
************

.. literalinclude:: schemas/response/EnergyCosts.json5
   :language: javascript

.. _energy_costs_per_fuel_time_series:

Energy Costs per Fuel Time Series
*********************************

.. literalinclude:: schemas/response/EnergyCostsPerFuelTimeSeries.json5
   :language: javascript

.. _energy_costs_rates:

Energy Costs Rates
******************

.. literalinclude:: schemas/request/EnergyCostsRates.json5
   :language: javascript

.. _energy_totals:

Energy Totals
*************

.. literalinclude:: schemas/response/EnergyTotals.json5
   :language: javascript

.. _energy_totals_per_fuel_time_series:

Energy Totals per Fuel Time Series
**********************************

.. literalinclude:: schemas/response/EnergyTotalsPerFuelTimeSeries.json5
   :language: javascript

.. _escalation_rates:

Escalation Rates
****************

.. literalinclude:: schemas/request/EscalationRates.json5
   :language: javascript

.. _global_controls:

Global Controls
***************

.. literalinclude:: schemas/request/GlobalControls.json5
   :language: javascript

.. _hvac:

HVAC
****

.. literalinclude:: schemas/request/HVAC.json5
   :language: javascript

.. _improved_appliances:

Improved Appliances
*******************

.. literalinclude:: schemas/request/ImprovedAppliances.json5
   :language: javascript

.. _improved_building:

Improved Building
*****************

.. literalinclude:: schemas/request/ImprovedBuilding.json5
   :language: javascript

.. _improved_electrical:

Improved Electrical
*******************

.. literalinclude:: schemas/request/ImprovedElectrical.json5
   :language: javascript

.. _improved_enclosure:

Improved Enclosure
******************

.. literalinclude:: schemas/request/ImprovedEnclosure.json5
   :language: javascript

.. _improved_hvac:

Improved HVAC
*************

.. literalinclude:: schemas/request/ImprovedHVAC.json5
   :language: javascript

.. _improved_photovoltaic:

Improved Photovoltaic
*********************

.. literalinclude:: schemas/request/ImprovedPhotovoltaic.json5
   :language: javascript

.. _improved_water_heating:

Improved Water Heating
**********************

.. literalinclude:: schemas/request/ImprovedWaterHeating.json5
   :language: javascript

.. _incentive:

Incentive
*********

.. literalinclude:: schemas/request/Incentive.json5
   :language: javascript

.. _lifetime:

Lifetime
********

.. literalinclude:: schemas/request/Lifetime.json5
   :language: javascript

.. _loan:

Loan
****

.. literalinclude:: schemas/request/Loan.json5
   :language: javascript

.. _model:

Model
*****

.. literalinclude:: schemas/request/Model.json5
   :language: javascript

.. _model_controls:

Model Controls
**************

.. literalinclude:: schemas/request/ModelControls.json5
   :language: javascript

.. _photovoltaic:

Photovoltaic
*************

.. literalinclude:: schemas/request/Photovoltaic.json5
   :language: javascript

.. _response_model:

Response Model
**************

.. literalinclude:: schemas/response/ResponseModel.json5
   :language: javascript

.. _response_timeline:

Response Timeline
*****************

.. literalinclude:: schemas/response/ResponseTimeline.json5
   :language: javascript

.. _timeline:

Timeline
********

.. literalinclude:: schemas/request/Timeline.json5
   :language: javascript

.. _typical:

Typical
*******

.. literalinclude:: schemas/response/Typical.json5
   :language: javascript

.. _units:

Units
*****

.. literalinclude:: schemas/request/Units.json5
   :language: javascript

.. _variable_cost:

Variable Cost
*************

.. literalinclude:: schemas/request/VariableCost.json5
   :language: javascript

.. _water_heating:

Water Heating
*************

.. literalinclude:: schemas/request/WaterHeating.json5
   :language: javascript

.. _weather:

Weather
*******

.. literalinclude:: schemas/request/Weather.json5
   :language: javascript

.. _weather_metadata:

Weather Metadata
****************

.. literalinclude:: schemas/response/WeatherMetadata.json5
   :language: javascript
