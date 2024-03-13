Request Structure
=================

The following sections provide the specifications and syntax for each API request schema.

.. literalinclude:: schemas/request/TopLevelRequestPayload.json5
   :language: javascript
   :caption: Top-level request payload schema

.. note::

   _`Objects and arrays` are used as values throughout the API. The difference is:
      - Object: This represents a single item that is not inherently a collection. An example of this is `Air Infiltration`_, which is characteristic of the entire building and thus can only be defined once.
      - Array: This represents an item that is inherently a collection, even if we don’t yet support more than one item. Often it will be an array of objects, where each object defines an item in the collection. `HVAC`_ and `Walls`_ are examples of this.

Address
-------

An address can be specified using either ``addressFull`` *or* ``addressComponents``.

.. _address_full:

Address Full
******************

.. literalinclude:: schemas/request/AddressFull.json5
   :language: json

.. _address_components:

Address Components
******************

.. literalinclude:: schemas/request/AddressComponents.json5
   :language: json


Base Building
-------------

.. literalinclude:: schemas/request/BaseBuilding.json5
   :language: javascript

Building Summary
****************

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

Enclosure
*********

.. literalinclude:: schemas/request/Enclosure.json5
   :language: javascript
   
Air Infiltration
^^^^^^^^^^^^^^^^

========================  =======  ================  ===========  ========  ==================  ============================================== 
Property                  Type     Units             Constraints  Required  Default             Notes                                                                                           
========================  =======  ================  ===========  ========  ==================  ==============================================       
``rate``                  float    see ``rateUnit``  >0           no        BSA
``rateUnit``              string                     see [#]_     no        ACH
``housePressurePa``       float    Pascals           >0           no        50
========================  =======  ================  ===========  ========  ==================  ============================================== 

.. [#] ``rateUnit`` options are "ACH" or "CFM"

Attics
^^^^^^

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
^^^^^

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
^^^^^^^^^^^

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
^^^^^

Each wall that has no contact with the ground and bounds a space is entered in ``...building.enclosure.walls``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       Wall1                                                                               
``type``                           string                     see [#]_        no        BSA
``assemblyEffectiveRValue``        float    F-ft2-hr/Btu      >0              no        BSA
``fractionAreaShared``             float    fraction          0-1             no        PSC                                                                                
``area``                           float    ft2               >0              no        PSC
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``type`` choices are "wood stud", "concrete masonry unit", "structural brick", "steel frame", "stone", "adobe", "log wall", and "solid concrete".

HVAC Systems
************

Each HVAC system can be entered in ``...building.systems.hvac``.

.. literalinclude:: schemas/request/HVAC.json5
   :language: javascript
   
HVAC Cooling Systems
^^^^^^^^^^^^^^^^^^^^

Each space cooling system (other than a heat pump) can be entered in ``...building.systems.hvac.hvacCoolingSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/HVACCoolingSystem.json5
   :language: javascript

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
``coolLoadFraction``               float    fraction                     <=1             yes       1
=================================  =======  ===========================  ==============  ========  ==================  ============================================== 

.. [#] If ``systemType`` is "central air conditioner"
.. [#] ``systemType`` choices are "central air conditioner", "room air conditioner", "evaporative cooler", "packaged terminal air conditioner", and "mini-split".
.. [#] ``compressorType`` choices are "single stage", "two stage", and "variable speed".
.. [#] ``coolEfficiencyUnits`` choices are "percent", "EER", "CEER", and "SEER". The option to use "SEER2" is planned for a future release.

HVAC Heating Systems
^^^^^^^^^^^^^^^^^^^^

Each space heating system (other than a heat pump) can be entered in ``...building.systems.hvac.hvacHeatingSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/HVACHeatinSystem.json5
   :language: javascript

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
``heatLoadFraction``               float    fraction                     0-1             yes       1   
=================================  =======  ===========================  ==============  ========  ==================  ============================================== 

.. [#] Required when ``systemType`` is "furnace" or "boiler".
.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".
.. [#] ``heatEfficiencyUnits`` choices are "AFUE" and "percent".

HVAC Heat Pumps
^^^^^^^^^^^^^^^

Each space conditioning heat pump can be entered in ``...building.systems.hvac.hvacHeatPumps``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/HVACHeatPump.json5
   :language: javascript

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
``heatLoadFraction``               float    fraction                    0-1             yes       1
``coolLoadFraction``               float    fraction                    0-1             yes       1
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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each separate air distribution system can be entered in ``...building.systems.hvac.hvacDistributionSystems.airDistributionSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/AirDistributionSystem.json5
   :language: javascript

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
.. [#] If ``location`` not provided, defaults to the first present space type: "basement conditioned", "basement unconditioned", "crawlspace vented", "crawlspace unvented", "attic vented", "attic unvented", "garage", or "living space".

HVAC Hydronic Distribution Systems
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each separate hydronic distribution system can be entered in ``...building.systems.hvac.hvacDistributionSystems.hydronicDistributionSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/HydronicDistributionSystem.json5
   :language: javascript

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``id``                             id                                   Must be unique  yes       PSC    
``systemType``                     string                                               yes       
``conditionedFloorAreaServed``     float    ft2                         >0              no        PSC
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 


HVAC Control Systems
^^^^^^^^^^^^^^^^^^^^

If any HVAC systems are specified, a single control system can be entered in ``...building.systems.hvac.hvacControlSystems``.

.. literalinclude:: schemas/request/HVACControlSystem.json5
   :language: javascript

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

Water Heating Systems
*********************

Each water heater is entered in ``...building.systems.waterHeatingSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/WaterHeating.json5
   :language: javascript

===========================================  =======  ==========================  ==============  ========  ====================  ============================================== 
Property                                     Type     Units                       Constraints     Required  Default               Notes
===========================================  =======  ==========================  ==============  ========  ====================  ==============================================       
``id``                                       id                                   Must be unique  yes       WaterHeater1    
``systemType``                               string                               see [#]_        yes       storage water heater    
``connectedHeatingId``                       idref                                see [#]_        see [#]_  null
``fuel``                                     string                               see [#]_        no      
``location``                                 string                               see [#]_        no        see [#]_
``tankVolume``                               float    gal                                         no
``dhwLoadFraction``                          float    fraction                    0-1             yes                             sum of dhwLoadFraction must equal 1
``heatCapacityBtuPerHour``                   float    Btu/hr                      >0              no                              autosized by modeling engine if undefined
``energyFactor`` or ``uniformEnergyFactor``  float    fraction                    <1              no        BSA
``hotWaterTemperature``                      float    F                           >0              no        125 
===========================================  =======  ==========================  ==============  ========  ====================  ============================================== 

.. [#] ``systemType`` choices are "storage water heater", "instantaneous water heater", "heat pump water heater", "space-heating boiler with storage tank", and "space-heating boiler with tankless coil".
.. [#] Must reference a defined ``hvacHeatingSystem.id``. 
.. [#] Only required when ``systemType`` is "space-heating boiler with ..."
.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".
.. [#] ``location`` choices are "living space", "basement conditioned", "basement unconditioned", "crawlspace unvented", "crawlspace vented", "attic unvented", "attic vented", "garage", “other exterior”, “other heated space”, and “other non-freezing space”.
.. [#] If ``location`` not provided, defaults to the first present space type:
  
  IECC zones 1-3, excluding 3A: "garage", "living space"

  IECC zones 3A, 4-8, unknown: "basement conditioned", "basement unconditioned", "living space"

Electrical Panels
*****************

Each electrical panel is entered in ``...building.systems.electricalPanels``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/ElectricalPanel.json5
   :language: javascript


Photovoltaics
*************

Each solar electric photovoltaic (PV) system is entered in ``...building.systems.photovoltaics``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/Photovoltaic.json5
   :language: javascript

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

Appliances
**********

Appliances can be entered in ``...building.appliances``.

.. literalinclude:: schemas/request/Appliances.json5
   :language: javascript
   
Clothes Dryers
^^^^^^^^^^^^^^

Clothes dryers can be entered in ``...building.appliances.clothesDryers``. Currently, this array is limited to a maximum size of 1. See note about `objects and arrays`_
for more information.

.. literalinclude:: schemas/request/ClothesDryer.json5
   :language: javascript

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
^^^^^^^^^^^^^^

Cooking ranges can be entered in ``...building.appliances.cookingRanges``. Currently, this array is limited to a maximum size of 1. See note about `objects and arrays`_
for more information.

.. literalinclude:: schemas/request/CookingRange.json5
   :language: javascript

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       CookingRange1
``fuel``                           string                     see [#]_        no        PSC, BSA   
``isInduction``                    boolean                                    no        false   
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".

  
Timelines
---------


Global Controls
---------------


Default Model Controls
----------------------


