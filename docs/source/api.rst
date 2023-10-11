API
===

..
   .. _overview:

   Overview
   --------

.. _authentication:

Authentication
--------------

All requests to the Modeling API must include authentication credentials.

API access tokens are applied to your requests to protect sensitive PII and are compliant with common privacy and security standards.

A valid token must be included as part of the HTTP Authorization header, using the `bearer` HTTP authorization scheme. The value of the header will be ``Bearer <token>``, where you replace <token> with a valid token. Valid tokens will be provided to established customers of the API. It is planned to add the ability to rotate tokens and set expiration dates in future releases.

.. code-block::

  Authorization: Bearer <token>


Here is an example using cURL:

.. code-block:: shell

  curl -v 'https://api.radiantlabs.co/v1/timelines' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer <tokenValue>' \
    --data-raw '{"addressFull":"501 RANDALL RD, BALLSTON SPA, NY 12020"}' \
    --compressed


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

.. note::

   _`Objects and arrays` are used as values throughout the API. The difference is:
      - Object: This represents a single item that is not inherently a collection. An example of this is `Air Infiltration`_, which is characteristic of the entire building and thus can only be defined once.
      - Array: This represents an item that is inherently a collection, even if we don’t yet support more than one item. Often it will be an array of objects, where each object defines an item in the collection. `HVAC`_ and `Walls`_ are examples of this.

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
   
Clothes Dryers
~~~~~~~~~~~~~~

Clothes dryers can be entered in ``...building.appliances.clothesDryers``. Currently, this array is limited to a maximum size of 1. See note about `objects and arrays`_
for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       ClothesDryer1   
``fuel``                           string                     see [#]_        no        BSA  
``combinedEnergyFactor``           float    lb/kWh            >0              no        3.73    
``isVented``                       boolean                                    no        true    
``ventedFlowRate``                 float    ft3/min                           no        150 
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".

Cooking Ranges
~~~~~~~~~~~~~~

Cooking ranges can be entered in ``...building.appliances.cookingRanges``. Currently, this array is limited to a maximum size of 1. See note about `objects and arrays`_
for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       CookingRange1
``fuel``                           string                     see [#]_        no        PSC, BSA   
``isInduction``                    boolean                                    no        false   
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".

.. _schema_automated_measures:

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

Building Summary
~~~~~~~~~~~~~~~~

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

.. _electrical_panel:

Electrical Panel
****************

.. literalinclude:: schemas/request/ElectricalPanel.json5
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
   
Air Infiltration
~~~~~~~~~~~~~~~~

========================  =======  ================  ===========  ========  ==================  ============================================== 
Property                  Type     Units             Constraints  Required  Default             Notes                                                                                           
========================  =======  ================  ===========  ========  ==================  ==============================================       
``rate``                  float    see ``rateUnit``  >0           no        BSA
``rateUnit``              string                     see [#]_     no        ACH
``housePressurePa``       float    Pascals           >0           no        50
========================  =======  ================  ===========  ========  ==================  ============================================== 

.. [#] ``rateUnit`` options are "ACH" or "CFM"

Attics
~~~~~~

The attic is entered in ``...building.enclosure.attics``. Currently, this array must contain exactly 1 attic object. If there is no attic present in house, set ``area`` to 0.
See note about `objects and arrays`_ for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       Attic1  
``area``                           float    ft2               >0              no        PSC
``isVented``                       boolean                                    no        yes
``floorAssemblyEffectiveRValue``   float    F-ft2-hr/Btu      >0              no        BSA
=================================  =======  ================  ==============  ========  ==================  ============================================== 

Roofs
~~~~~

Each roof surface is entered in ``...building.enclosure.roofs``. Currently, this array is limited to a maximum size of 1. See note about `objects and arrays`_
for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       Roof1   
``area``                           float    ft2               >0              no        PSC   
``pitch``                          float    ?:12              >=0             no        6   
``assemblyEffectiveRValue``        float    F-ft2-hr/Btu      >0              no        2.3 
=================================  =======  ================  ==============  ========  ==================  ============================================== 

Foundations
~~~~~~~~~~~

Each foundation is entered in ``...building.enclosure.foundations``. Currently, this array is limited to a maximum size of 1. See note about `objects and arrays`_
for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         must be unique  yes       PSC     
``type``                           string                     see [#]_        no        BSA
``area``                           float    ft2               >0              no        PSC   
``wallHeight``                     float    ft                >=0             no        PSC  
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``type`` choices are "basement conditioned", "basement unconditioned", "crawl vented", "crawl unvented", "slab", and "pier and beam".

Walls
~~~~~

Each wall that has no contact with the ground and bounds a space is entered in ``...building.enclosure.walls``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       Wall1                                                                               
``type``                           string                     see [#]_        no        BSA
``assemblyEffectiveRValue``        float    F-ft2-hr/Btu      >0              no        BSA
``percentageAreaShared``           float    fraction          0-1             no        PSC                                                                                
``area``                           float    ft2               >0              no        PSC
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``type`` choices are "wood stud", "concrete masonry unit", "structural brick", "steel frame", "stone", "adobe", "log wall", and "solid concrete".

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
   
HVAC Cooling Systems
~~~~~~~~~~~~~~~~~~~~

Each space cooling system (other than a heat pump) can be entered in ``...building.systems.hvac.hvacCoolingSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ===========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                        Constraints     Required  Default             Notes
=================================  =======  ===========================  ==============  ========  ==================  ==============================================       
``id``                             id                                    Must be unique  yes       PSC  
``connectedDistributionId``        idref                                                 yes [#]_  PSC  
``systemType``                     string                                see [#]_        yes       PSC, BSA
``coolCapacityBtuPerHour``         float    Btu/hr                       >=0             no                            autosized by modeling engine if undefined
``compressorType``                 string                                see [#]_        no        single stage        only applicable if systemType = "central air conditioner"
``coolEfficiency``                 float    see ``coolEfficiencyUnits``  >0              no        PSC 
``coolEfficiencyUnits``            string                                see [#]_        no        PSC 
``coolLoadPercentage``             float    fraction                     <=1             yes       1
=================================  =======  ===========================  ==============  ========  ==================  ============================================== 

.. [#] If ``systemType`` is "central air conditioner"
.. [#] ``systemType`` choices are "central air conditioner", "room air conditioner", "evaporative cooler", "packaged terminal air conditioner", and "mini-split".
.. [#] ``compressorType`` choices are "single stage", "two stage", and "variable speed".
.. [#] ``coolEfficiencyUnits`` choices are "percent", "EER", "CEER", and "SEER". The option to use "SEER2" is planned for a future release.

HVAC Heating Systems
~~~~~~~~~~~~~~~~~~~~

Each space heating system (other than a heat pump) can be entered in ``...building.systems.hvac.hvacHeatingSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ===========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                        Constraints     Required  Default             Notes
=================================  =======  ===========================  ==============  ========  ==================  ==============================================       
``id``                             id                                    Must be unique  yes       PSC
``connectedDistributionId``        idref                                                 see [#]_  PSC
``systemType``                     string                                                yes       PSC, BSA
``fuel``                           string                                see [#]_        no        PSC
``heatCapacityBtuPerHour``         float    Btu/hr                       >=0             no                            autosized by modeling engine if undefined
``heatEfficiency``                 float    see ``heatEfficiencyUnits``  0-1             no        PSC 
``heatEfficiencyUnits``            string                                see [#]_        no        PSC 
``heatLoadPercentage``             float    fraction                     0-1             yes       1   
=================================  =======  ===========================  ==============  ========  ==================  ============================================== 

.. [#] Required when ``systemType`` is "furnace" or "boiler".
.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".
.. [#] ``heatEfficiencyUnits`` choices are "AFUE" and "percent".

HVAC Heat Pumps
~~~~~~~~~~~~~~~

Each space conditioning heat pump can be entered in ``...building.systems.hvac.hvacHeatPumps``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``id``                             id                                   Must be unique  yes       PSC 
``connectedDistributionId``        idref                                see [#]_        see [#]_  PSC
``systemType``                     string                               see [#]_        yes       PSC
``compressorType``                 string                               see [#]_        no        single stage    
``heatCapacityBtuPerHour``         float    Btu/hr                      >=0             no                            autosized by modeling engine if undefined
``coolCapacityBtuPerHour``         float    Btu/hr                      >=0             no                            autosized by modeling engine if undefined
``heatEfficiency``                 float    Btu/Wh                      >0              no        PSC
``heatEfficiencyUnits``            string                               HSPF [#]_       no        HSPF
``coolEfficiency``                 float    Btu/Wh                      >0              no        PSC
``coolEfficiencyUnits``            string                               SEER [#]_       no        SEER
``heatLoadPercentage``             float    fraction                    0-1             yes       1
``coolLoadPercentage``             float    fraction                    0-1             yes       1
``backupSystem``                   object                                               yes
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 

.. [#] Must reference a defined ``hvacDistributionSystem.id``.
.. [#] Required when ``systemType`` is "air-to-air" or "ground-to-air".
.. [#] ``systemType`` choices are "mini-split", "air-to-air", and "ground-to-air".
.. [#] ``compressorType`` choices are "single stage", "two stage", and "variable speed".
.. [#] The option to use "HSPF2" is planned for a future release.
.. [#] The option to use "SEER2" is planned for a future release.

``backupSystem`` schema for HVAC Heat Pumps:

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``systemType``                     string                               see [#]_        no        integrated  
``heatingSwitchoverTemp``          float    F                                           no                            determined by modeling engine if undefined
``fuel``                           string                               see [#]_        no        electricity         only applicable if backupSystem.systemType = "integrated"
``heatEfficiency``                 float    see ``heatEfficiencyUnit``  0-1             no        1                   only applicable if backupSystem.systemType = "integrated"
``heatEfficiencyUnits``            string                               see [#]_        no        percent             only applicable if backupSystem.systemType = "integrated"
``heatCapacityBtuPerHour``         float    Btu/hr                      >=0             no                            autosized by modeling engine if undefined, only applicable if backupSystem.systemType = "integrated"
``backupHvacId``                   idref                                see [#]_        see [#]_  PSC
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 

.. [#] ``systemType`` choices are "integrated" and "separate".
.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".
.. [#] ``heatEfficiencyUnits`` choices are "AFUE" and "percent".
.. [#] Must reference an defined ``hvacHeatingSystem.id``
.. [#] Required when ``backupSystem.systemType`` is "separate".

HVAC Air Distribution Systems
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Each separate air distribution system can be entered in ``...building.systems.hvac.hvacDistributionSystems.airDistributionSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``id``                             id                                   Must be unique  yes       PSC    
``systemType``                     string                               see [#]_        yes       PSC  
``numberOfReturnRegisters``        integer  count                       >=0             no        PSC
``conditionedFloorAreaServed``     float    ft2                         >0              no        PSC
``ducts``                          object                                               yes
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 

.. [#] ``systemType`` choices are "regular velocity" and "gravity".

``ducts`` object uses the following schema:

=================================  =======  ==========================  ==============  ========  ======================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default                 Notes
=================================  =======  ==========================  ==============  ========  ======================  ==============================================       
``id``                             id                                   Must be unique  yes       PSC
``systemType``                     string                               see [#]_        yes       "supply" and "return"   both supply and return must be defined
``insulationRValue``               float    F-ft2-hr/Btu                >=0             no        0   
``leakageValue``                   float    see ``leakageUnits``        >=0             no        BSA    
``leakageUnits``                   string                               see [#]_        no        Percent 
``location``                       string                               see [#]_        no        see notes [#]_
=================================  =======  ==========================  ==============  ========  ======================  ============================================== 

.. [#] ``systemType`` choices are "supply" and "return".
.. [#] ``leakageUnits`` choices are"CFM25", "CFM50", and "percent".
.. [#] ``location`` choices are "living space", "basement conditioned", "basement unconditioned", "crawlspace unvented", "crawlspace vented", "attic unvented", "attic vented", "garage", "outside", "exterior wall", "under slab", "roof deck", "other heated space", and "other non-freezing space".
.. [#] If ``location`` not provided, defaults to the first present space type: "basement - conditioned", "basement - unconditioned", "crawlspace - conditioned", "crawlspace - vented", "crawlspace - unvented", "attic - vented", "attic - unvented", "garage", or "living space".

HVAC Hydronic Distribution Systems
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Each separate hydronic distribution system can be entered in ``...building.systems.hvac.hvacDistributionSystems.hydronicDistributionSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``id``                             id                                   Must be unique  yes       PSC    
``systemType``                     string                                               yes       
``conditionedFloorAreaServed``     float    ft2                         >0              no        PSC
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 


HVAC Control Systems
~~~~~~~~~~~~~~~~~~~~

If any HVAC systems are specified, a single control system can be entered in ``...building.systems.hvac.hvacControlSystems``.

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``id``                             id                                   Must be unique  yes       HVACControl1
``heatingSeason``                  object                                                                             Thermostat settings for heating season
``coolingSeason``                  object                                                                             Thermostat settings for cooling season
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 

``heatingSeason`` and ``coolingSeason`` objects share the following schema:

=================================  =======  ==========================  ==============  ========  ========================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default                   Notes
=================================  =======  ==========================  ==============  ========  ========================  ==============================================       
``setpointTemp``                   float    F                                           no        Heating: 67, Cooling: 78  
``setbackTemp``                    float    F                                           no        null                      Also referred to as setup temperature for the cooling season.
``setbackStartHour``               integer  time                        0-23            no        null    
``totalWeeklySetbackHours``        integer  hrs/week                    0-168           no        null    
=================================  =======  ==========================  ==============  ========  ========================  ============================================== 


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

.. _improved_electrical_panel:

Improved Electrical Panel
*************************

.. literalinclude:: schemas/request/ImprovedElectricalPanel.json5
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

=================================  =======  ==================  ==============  ==========  ==================  ============================================== 
Property                           Type     Units               Constraints     Required    Default             Notes
=================================  =======  ==================  ==============  ==========  ==================  ==============================================       
``replacementCost``                float    ``units.monetary``  >=0             see [#]_                        Default values not supported currently
``endOfLifeDate``                  date                         in the future   see [#LT]_
``effectiveUsefulLifeDays``        integer  days                >0              see [#LT]_   BSA
``installedDate``                  date                         in the past     see [#LT]_   PSC, BSA
=================================  =======  ==================  ==============  ==========  ==================  ============================================== 

.. [#] Required to run status quo timeline.
.. [#LT] Two of these three properties (``endOfLifeDate``, ``effectiveUsefulLifeDays``, ``installedDate``) are required to be included in the status quo timeline.

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
************

.. literalinclude:: schemas/request/Photovoltaic.json5
   :language: javascript

Each solar electric photovoltaic (PV) system is entered in ``...building.systems.photovoltaics``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ==================  ==============  ========  ==================  ============================================== 
Property                           Type     Units               Constraints     Required  Default             Notes
=================================  =======  ==================  ==============  ========  ==================  ==============================================       
``id``                             id                           Must be unique  yes       not defined 
``location``                       string                       see [#]_        no        "roof"  
``moduleType``                     string                       see [#]_        no        "standard"  
``tracking``                       string                       see [#]_        no        "fixed" 
``arrayOrientation``               string   direction           see [#]_        yes     
``arrayTilt``                      float    degrees             0-90            yes                           Tilt relative to horizontal
``maxPowerOutput``                 float    W                   >0              yes     
``inverterEfficiency``             float    fraction            0-1             no        0.96    
``yearModulesManufactured``        integer  year                >1600           no                            Informs systemLossesFraction, which defaults to 0.14
=================================  =======  ==================  ==============  ========  ==================  ============================================== 

.. [#] ``location`` choices are "ground" and "roof".
.. [#] ``moduleType`` choices are "standard", "premium", and "thin film".
.. [#] ``tracking`` choices are "fixed", "1-axis", "1-axis backtracked", and "2-axis".
.. [#] ``arrayOrientation`` choices are "northeast", "east", "southeast", "south", "southwest", "west", "northwest", and "north".

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

Each water heater is entered in ``...building.systems.waterHeatingSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

===========================================  =======  ==========================  ==============  ========  ====================  ============================================== 
Property                                     Type     Units                       Constraints     Required  Default               Notes
===========================================  =======  ==========================  ==============  ========  ====================  ==============================================       
``id``                                       id                                   Must be unique  yes       WaterHeater1    
``systemType``                               string                               see [#]_        yes       storage water heater    
``connectedHeatingId``                       idref                                see [#]_        see [#]_  null
``fuel``                                     string                               see [#]_        no      
``location``                                 string                               see [#]_        no        see [#]_
``tankVolume``                               float    gal                                         no
``dhwLoadPercentage``                        float    fraction                    0-1             yes                             sum of dhwLoadPercentage must equal 1
``heatCapacityBtuPerHour``                   float    Btu/hr                      >0              no                              autosized by modeling engine if undefined
``energyFactor`` or ``uniformEnergyFactor``  float    fraction                    <1              no        BSA
``hotWaterTemperature``                      float    F                           >0              no        125 
===========================================  =======  ==========================  ==============  ========  ====================  ============================================== 

.. [#] ``systemType`` choices are "storage water heater", "instantaneous water heater", "heat pump water heater", "space-heating boiler with storage tank", and "space-heating boiler with tankless coil".
.. [#] Must reference a defined ``hvacHeatingSystem.id``. 
.. [#] Only required when ``systemType`` is "space-heating boiler with ..."
.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".
.. [#] ``location`` choices are "living space", "basement conditioned", "basement unconditioned", "crawlspace unvented", "crawlspace vented", "attic unvented", "attic vented", "garage", "outside", "exterior wall", "under slab", "roof deck", "other heated space", and "other non-freezing space".
.. [#] If ``location`` not provided, defaults to the first present space type:
  
  IECC zones 1-3, excluding 3A: "garage", "living space"

  IECC zones 3A, 4-8, unknown: "basement - conditioned", "basement - unconditioned", "living space"

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
