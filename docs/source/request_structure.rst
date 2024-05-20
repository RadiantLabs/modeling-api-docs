Request Structure
=================

The following sections provide the specifications and syntax for each API request schema.

.. literalinclude:: schemas/request/TopLevelRequestPayload.json5
   :language: javascript
   :caption: Top-level request payload schema

As discussed in `Usage Instructions`_, the Defaulting Engine will populate any missing or null building characteristics. In each schema definition table below, information about each key's default is provided. 
  - **_`PSC`**: Property-specific characteristic. This type of default indicates that specific information about the address is collected and applied to that attribute. Thus, each address may have a different default value. For example, ``buildingSummary.conditionedFloorArea`` is collected from various `Data Sources`_ to be populated when missing or null.
  - **_`BSA`**: Building stock assumption. This type of default indicates that location- and vintage-based building stock assumptions are used by the defaulting engine to populate missing and null keys.

.. note::

   _`Objects and arrays` are used as values throughout the API. The difference is:
      - Object: This represents a single item that is not inherently a collection. An example of this is `Air Infiltration`_, which is characteristic of the entire building and thus can only be defined once.
      - Array: This represents an item that is inherently a collection, even if we don’t yet support more than one item. Often it will be an array of objects, where each object defines an item in the collection. `HVAC`_ and `Walls`_ are examples of this.

.. _address:

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

.. _base_building:

Base Building
-------------

.. literalinclude:: schemas/request/BaseBuilding.json5
   :language: javascript

.. _building_summary:

Building Summary
****************

========================  =======  ========  ===========  ========  ==================  ============================================== 
Property                  Type     Units     Constraints  Required  Default             Notes                                                                                           
========================  =======  ========  ===========  ========  ==================  ============================================== 
``conditionedFloorArea``  integer  ft2       >0           no        `PSC`_              If missing from `PSC`_, model will fail           
``averageCeilingHeight``  integer  ft        >0           no        8                                                        
``bathCount``             integer  count     >0           no        see [#]_
``bedroomsCount``         integer  count     >0           no        `PSC`_, `BSA`_
``residentCount``         integer  count     >=0          no        `BSA`_                                                       	
``storiesCount``          integer  count     >0           no        `PSC`_, `BSA`_
``windowToWallFraction``  float    fraction  >0           no        0.14
``yearBuilt``             integer  year      >1600        no        `PSC`_              If missing from `PSC`_, model will fail           
========================  =======  ========  ===========  ========  ==================  ============================================== 

.. [#] ``bedroomsCount``/2 + 0.5

.. _enclosure:

Enclosure
*********

.. literalinclude:: schemas/request/Enclosure.json5
   :language: javascript

.. _air_infiltration:

Air Infiltration
^^^^^^^^^^^^^^^^

========================  =======  ================  ===========  ========  ==================  ============================================== 
Property                  Type     Units             Constraints  Required  Default             Notes                                                                                           
========================  =======  ================  ===========  ========  ==================  ==============================================       
``rate``                  float    see ``rateUnit``  >0           no        `BSA`_
``rateUnit``              string                     see [#]_     no        ACH
``housePressurePa``       float    Pascals           >0           no        50
========================  =======  ================  ===========  ========  ==================  ============================================== 

.. [#] ``rateUnit`` options are "ACH" or "CFM"

.. _attics:

Attics
^^^^^^

The attic is entered in ``...building.enclosure.attics``. Currently, this array must contain exactly 1 attic object. If there is no attic present in house, set ``area`` to 0.
See note about `objects and arrays`_ for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       Attic1  
``area``                           float    ft2               >0              no        `PSC`_
``isVented``                       boolean                                    no        yes
``floorAssemblyEffectiveRValue``   float    F-ft2-hr/Btu      >0              no        `BSA`_
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. _roofs:

Roofs
^^^^^

Each roof surface is entered in ``...building.enclosure.roofs``. Currently, this array is limited to a maximum size of 1. See note about `objects and arrays`_
for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       Roof1   
``area``                           float    ft2               >0              no        `PSC`_   
``pitch``                          float    ?:12              >=0             no        6   
``assemblyEffectiveRValue``        float    F-ft2-hr/Btu      >0              no        2.3 
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. _foundations:

Foundations
^^^^^^^^^^^

Each foundation is entered in ``...building.enclosure.foundations``. Currently, this array is limited to a maximum size of 1. See note about `objects and arrays`_
for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         must be unique  yes       `PSC`_     
``type``                           string                     see [#]_        no        `BSA`_
``area``                           float    ft2               >0              no        `PSC`_   
``wallHeight``                     float    ft                >=0             no        `PSC`_  
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``type`` choices are "basement conditioned", "basement unconditioned", "crawl vented", "crawl unvented", "slab", and "pier and beam".

.. _walls:

Walls
^^^^^

Each wall that has no contact with the ground and bounds a space is entered in ``...building.enclosure.walls``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

=================================  =======  ================  ==============  ========  ==================  ============================================== 
Property                           Type     Units             Constraints     Required  Default             Notes
=================================  =======  ================  ==============  ========  ==================  ==============================================       
``id``                             id                         Must be unique  yes       Wall1                                                                               
``type``                           string                     see [#]_        no        `BSA`_
``assemblyEffectiveRValue``        float    F-ft2-hr/Btu      >0              no        `BSA`_
``fractionAreaShared``             float    fraction          0-1             no        `PSC`_                                                                                
``area``                           float    ft2               >0              no        `PSC`_
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``type`` choices are "wood stud", "concrete masonry unit", "structural brick", "steel frame", "stone", "adobe", "log wall", and "solid concrete".

.. _hvac:

HVAC
****

Each HVAC system can be entered in ``...building.systems.hvac``.

.. literalinclude:: schemas/request/HVAC.json5
   :language: javascript
   
.. _hvac_cooling_systems:

HVAC Cooling Systems
^^^^^^^^^^^^^^^^^^^^

Each space cooling system (other than a heat pump) can be entered in ``...building.systems.hvac.hvacCoolingSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/HVACCoolingSystem.json5
   :language: javascript

=================================  =======  ===========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                        Constraints     Required  Default             Notes
=================================  =======  ===========================  ==============  ========  ==================  ==============================================       
``id``                             id                                    Must be unique  yes       `PSC`_  
``connectedDistributionId``        idref                                                 yes [#]_  `PSC`_  
``systemType``                     string                                see [#]_        yes       `PSC`_, `BSA`_
``coolCapacityBtuPerHour``         float    Btu/hr                       >=0             no                            autosized by modeling engine if undefined
``compressorType``                 string                                see [#]_        no        single stage        only applicable if systemType = "central air conditioner"
``coolEfficiency``                 float    see ``coolEfficiencyUnits``  >0              no        `PSC`_ 
``coolEfficiencyUnits``            string                                see [#]_        no        `PSC`_ 
``coolLoadFraction``               float    fraction                     <=1             yes       1
=================================  =======  ===========================  ==============  ========  ==================  ============================================== 

.. [#] If ``systemType`` is "central air conditioner"
.. [#] ``systemType`` choices are "central air conditioner", "room air conditioner", "evaporative cooler", "packaged terminal air conditioner", and "mini-split".
.. [#] ``compressorType`` choices are "single stage", "two stage", and "variable speed".
.. [#] ``coolEfficiencyUnits`` choices "EER" and "CEER" for room air conditioners and packaged terminal air conditioners. "SEER" and "SEER2" are the choices for central air conditioners and mini-splits. Evaporative coolers do not require `coolEfficiency`. 

.. _hvac_heating_systems:

HVAC Heating Systems
^^^^^^^^^^^^^^^^^^^^

Each space heating system (other than a heat pump) can be entered in ``...building.systems.hvac.hvacHeatingSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/HVACHeatingSystem.json5
   :language: javascript

=================================  =======  ===========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                        Constraints     Required  Default             Notes
=================================  =======  ===========================  ==============  ========  ==================  ==============================================       
``id``                             id                                    Must be unique  yes       `PSC`_
``connectedDistributionId``        idref                                                 see [#]_  `PSC`_
``systemType``                     string                                                yes       `PSC`_, `BSA`_
``fuel``                           string                                see [#]_        no        `PSC`_
``heatCapacityBtuPerHour``         float    Btu/hr                       >=0             no                            autosized by modeling engine if undefined
``heatEfficiency``                 float    see ``heatEfficiencyUnits``  0-1             no        `PSC`_ 
``heatEfficiencyUnits``            string                                see [#]_        no        `PSC`_ 
``heatLoadFraction``               float    fraction                     0-1             yes       1   
=================================  =======  ===========================  ==============  ========  ==================  ============================================== 

.. [#] Required when ``systemType`` is "furnace" or "boiler".
.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".
.. [#] ``heatEfficiencyUnits`` choices are "AFUE" and "fraction".

.. _hvac_heat_pumps:

HVAC Heat Pumps
^^^^^^^^^^^^^^^

Each space conditioning heat pump can be entered in ``...building.systems.hvac.hvacHeatPumps``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/HVACHeatPump.json5
   :language: javascript

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``id``                             id                                   Must be unique  yes       `PSC`_ 
``connectedDistributionId``        idref                                see [#]_        see [#]_  `PSC`_
``systemType``                     string                               see [#]_        yes       `PSC`_
``compressorType``                 string                               see [#]_        no        single stage    
``heatCapacityBtuPerHour``         float    Btu/hr                      >=0             no                            autosized by modeling engine if undefined
``coolCapacityBtuPerHour``         float    Btu/hr                      >=0             no                            autosized by modeling engine if undefined
``heatEfficiency``                 float    Btu/Wh                      >0              no        `PSC`_
``heatEfficiencyUnits``            string                               see [#]_        no        HSPF
``coolEfficiency``                 float    Btu/Wh                      >0              no        `PSC`_
``coolEfficiencyUnits``            string                               see [#]_        no        SEER
``heatLoadFraction``               float    fraction                    0-1             yes       1
``coolLoadFraction``               float    fraction                    0-1             yes       1
``backupSystem``                   object                                               yes
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 

.. [#] Must reference a defined ``hvacDistributionSystem.id``.
.. [#] Required when ``systemType`` is "air-to-air" or "ground-to-air".
.. [#] ``systemType`` choices are "mini-split", "air-to-air", and "ground-to-air".
.. [#] ``compressorType`` choices are "single stage", "two stage", and "variable speed".
.. [#] ``heatEfficiencyUnits`` choices are "HSPF" or "HSPF2".
.. [#] ``coolEfficiencyUnits`` choices are "SEER" or "SEER2".

``backupSystem`` schema for HVAC Heat Pumps:

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``systemType``                     string                               see [#]_        no        integrated  
``heatingSwitchoverTemp``          float    F                                           no                            determined by modeling engine if undefined
``fuel``                           string                               see [#]_        no        electricity         only applicable if backupSystem.systemType = "integrated"
``heatEfficiency``                 float    see ``heatEfficiencyUnit``  0-1             no        1                   only applicable if backupSystem.systemType = "integrated"
``heatEfficiencyUnits``            string                               see [#]_        no        fraction            only applicable if backupSystem.systemType = "integrated"
``heatCapacityBtuPerHour``         float    Btu/hr                      >=0             no                            autosized by modeling engine if undefined, only applicable if backupSystem.systemType = "integrated"
``backupHvacId``                   idref                                see [#]_        see [#]_  `PSC`_
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 

.. [#] ``systemType`` choices are "integrated" and "separate".
.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".
.. [#] ``heatEfficiencyUnits`` choices are "AFUE" and "fraction".
.. [#] Must reference an defined ``hvacHeatingSystem.id``
.. [#] Required when ``backupSystem.systemType`` is "separate".

.. _air_distribution_systems:

Air Distribution Systems
^^^^^^^^^^^^^^^^^^^^^^^^

Each separate air distribution system can be entered in ``...building.systems.hvac.hvacDistributionSystems.airDistributionSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/AirDistributionSystem.json5
   :language: javascript

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``id``                             id                                   Must be unique  yes       `PSC`_    
``systemType``                     string                               see [#]_        yes       `PSC`_  
``numberOfReturnRegisters``        integer  count                       >=0             no        `PSC`_
``conditionedFloorAreaServed``     float    ft2                         >0              no        `PSC`_
``ducts``                          object                                               yes
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 

.. [#] ``systemType`` choices are "regular velocity" and "gravity".

``ducts`` object uses the following schema:

=================================  =======  ==========================  ==============  ========  ======================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default                 Notes
=================================  =======  ==========================  ==============  ========  ======================  ==============================================       
``id``                             id                                   Must be unique  yes       `PSC`_
``systemType``                     string                               see [#]_        yes       "supply" and "return"   both supply and return must be defined
``insulationRValue``               float    F-ft2-hr/Btu                >=0             no        0   
``leakageValue``                   float    see ``leakageUnits``        >=0             no        `BSA`_    
``leakageUnits``                   string                               see [#]_        no        fraction 
``location``                       string                               see [#]_        no        see notes [#]_
=================================  =======  ==========================  ==============  ========  ======================  ============================================== 

.. [#] ``systemType`` choices are "supply" and "return".
.. [#] ``leakageUnits`` choices are "CFM25", "CFM50", and "fraction".
.. [#] ``location`` choices are "living space", "basement conditioned", "basement unconditioned", "crawlspace unvented", "crawlspace vented", "attic unvented", "attic vented", "garage", "outside", "exterior wall", "under slab", "roof deck", "other heated space", and "other non-freezing space".
.. [#] If ``location`` not provided, defaults to the first present space type: "basement conditioned", "basement unconditioned", "crawlspace vented", "crawlspace unvented", "attic vented", "attic unvented", "garage", or "living space".

.. _hydronic_distribution_systems:

Hydronic Distribution Systems
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each separate hydronic distribution system can be entered in ``...building.systems.hvac.hvacDistributionSystems.hydronicDistributionSystems``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/HydronicDistributionSystem.json5
   :language: javascript

=================================  =======  ==========================  ==============  ========  ==================  ============================================== 
Property                           Type     Units                       Constraints     Required  Default             Notes
=================================  =======  ==========================  ==============  ========  ==================  ==============================================       
``id``                             id                                   Must be unique  yes       `PSC`_    
``systemType``                     string                                               yes       
``conditionedFloorAreaServed``     float    ft2                         >0              no        `PSC`_
=================================  =======  ==========================  ==============  ========  ==================  ============================================== 

.. _hvac_control_systems:

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

.. _water_heating_systems:

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
``efficency``                                float    fraction                    >0              no        `BSA`_
``efficiencyUnits``                          string                               see [#]_        no        `BSA`_
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
.. [#] ``efficiencyUnits`` choices are "EF" and "UEF".

.. _electrical_panels:

Electrical Panels
*****************

Each electrical panel is entered in ``...building.systems.electricalPanels``. Currently, this array is limited to a maximum size of 1.
See note about `objects and arrays`_ for more information.

.. literalinclude:: schemas/request/ElectricalPanel.json5
   :language: javascript

.. _photovoltaics:

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

.. _appliances:

Appliances
**********

Appliances can be entered in ``...building.appliances``.

.. literalinclude:: schemas/request/Appliances.json5
   :language: javascript

.. _clothes_dryers:

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
``fuel``                           string                     see [#]_        no        `BSA`_  
``combinedEnergyFactor``           float    lb/kWh            >0              no        3.73    
``isVented``                       boolean                                    no        true    
``ventedFlowRate``                 float    ft3/min                           no        150 
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".

.. _cooking_ranges:

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
``fuel``                           string                     see [#]_        no        `PSC`_, `BSA`_   
``isInduction``                    boolean                                    no        false   
=================================  =======  ================  ==============  ========  ==================  ============================================== 

.. [#] ``fuel`` choices are "electricity", "natural gas", "fuel oil", "propane", "coal", "wood", and "wood pellets".

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
``effectiveUsefulLifeDays``        integer  days                >0              see [#LT]_   `BSA`_
``installedDate``                  date                         in the past     see [#LT]_   `PSC`_, `BSA`_
=================================  =======  ==================  ==============  ==========  ==================  ============================================== 

.. [#] Required to run status quo timeline.
.. [#LT] Two of these three properties (``endOfLifeDate``, ``effectiveUsefulLifeDays``, ``installedDate``) are required to be included in the status quo timeline.


Timelines
---------

.. literalinclude:: schemas/request/Timeline.json5
   :language: javascript

Models
******

.. literalinclude:: schemas/request/Model.json5
   :language: javascript

.. _model_controls:

Model Controls
^^^^^^^^^^^^^^

Model controls (``timelines.models.modelControls``) and default model controls (``defaultModelControls``) are optional schemas. If the default model controls object is provided, and no model controls object is present
in a given model, values in this object are taken by default, affecting such model.

.. literalinclude:: schemas/request/ModelControls.json5
   :language: javascript

.. _energy_costs_rates:

Energy Costs Rates
~~~~~~~~~~~~~~~~~~

.. literalinclude:: schemas/request/EnergyCostsRates.json5
   :language: javascript

.. _weather:

Weather
~~~~~~~

.. literalinclude:: schemas/request/Weather.json5
   :language: javascript

.. _loans:

Loans
^^^^^

.. literalinclude:: schemas/request/Loan.json5
   :language: javascript

.. _incentives:

Incentives
^^^^^^^^^^

.. literalinclude:: schemas/request/Incentive.json5
   :language: javascript

.. _automated_measures:

Automated Measures
^^^^^^^^^^^^^^^^^^

Automated measures allow users to apply improvements to the base building without fully defining either the base or improved building definition.

.. literalinclude:: schemas/request/AutomatedMeasures.json5
   :language: javascript

Operate on existing systems
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remove or adjust relevant values of existing systems (e.g. heating, cooling, heat pump, water heater or
distribution systems). The defaulting engine determines whether a house contains one or more of these systems. Existing
systems are harder to refer to in improved building definitions and assumptions change from house to house. For that
reason, these automated measures help modify the base building definition without much knowledge of said
characteristics.

.. _existing_hvac_heating_system:

Existing HVAC Heating System
""""""""""""""""""""""""""""

Operations to the existing HVAC heating system are entered in ``automatedMeasures.existingHvacHeatingSystem``. This object covers the singular predefined system providing heating in the base building, including a heat pump.

.. literalinclude:: schemas/request/ExistingHVACHeatingSystem.json5
   :language: javascript

  ==========  ====================  ===========  ========  ============================
  Property    Type                  Constraints  Required  Description
  ==========  ====================  ===========  ========  ============================
  ``action``  String                See [#]_     Yes       Operation on existing heating system
  ``adjust``  Object                             No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`               No [#]_   Implied costs of measure
  ==========  ====================  ===========  ========  ============================

  .. [#] ``action`` choices are "keep", "remove", or "adjust".

     **"keep"** maintains existing heating system as is, including load fraction. Using this action indicates that there are no changes to heating at all. This action will override anything in ``adjust`` object.

     **"remove"** indicates the existing heating system is completely removed. New system(s) must cover 100% of load or specify ``heatLoadGapFraction``. This action will override anything in ``adjust`` object.

  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] Defaults to ``[]`` if not provided.

.. note::

  If ``action`` = "remove" and the existing water heating system is dependent on the removed heating system (i.e. indirect water heater),
  then a new standalone heat pump water heating system will automatically be added to the ``improvedBuilding``.

.. note::

  If ``action`` = "remove" and the existing heating system is a heat pump, then the existing cooling system will also be removed.
  This is based on the assumption that the ``newHeatPump`` will serve both loads.

``adjust`` schema for existing HVAC heating system:

  ======================  =======  ===========  =========  ==============================================
  Property                Type     Constraints  Required   Description
  ======================  =======  ===========  =========  ==============================================
  ``backup``              Boolean               See [#a]_  Indicates the existing heating system is being kept, but switched to be a backup system
  ``heatLoadFraction``    Double   0 - 1        See [#a]_  Heat load for the existing heating system
  ======================  =======  ===========  =========  ==============================================

.. [#a] Either ``backup`` or ``heatLoadFraction`` can be defined. If ``backup`` is true, then ``heatLoadFraction`` must be 0 or undefined. If ``backup`` is false, then ``heatLoadFraction must be > 0.

.. _existing_hvac_cooling_system:

Existing HVAC Cooling System
""""""""""""""""""""""""""""

Operations on the existing cooling systems are entered in ``automatedMeasures.existingHvacCoolingSystem``. This object covers the singular predefined system providing cooling in the base building, including a heat pump.

.. literalinclude:: schemas/request/ExistingHVACCoolingSystem.json5
   :language: javascript

  ==========  ====================  ===========  ========  ============================
  Property    Type                  Constraints  Required  Description
  ==========  ====================  ===========  ========  ============================
  ``action``  String                See [#]_     Yes       Operation on existing cooling system
  ``adjust``  Object                             No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`               No [#]_   Implied costs of measure
  ==========  ====================  ===========  ========  ============================

   .. [#] ``action`` choices are "keep", "remove", or "adjust".

     **"keep"** maintains existing cooling system as is, including load fraction. Using this action indicates that there are no changes to cooling at all. This action will override anything in ``adjust`` object.

     **"remove"** indicates the existing cooling system is completely removed. New system(s) must cover 100% of load or specify ``coolLoadGapFraction``. This action will override anything in ``adjust`` object.


  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] Defaults to ``[]`` if not provided.

.. note::

  If ``action`` = "remove" and the existing cooling system is a heat pump, then the existing heating system will also be removed. This is based on the assumption that the ``newHeatPump`` will serve both loads.

``adjust`` schema for existing HVAC cooling system:

  ======================  =======  ===================  ========  =========================================
  Property                Type     Constraints          Required  Description
  ======================  =======  ===================  ========  =========================================
  ``coolLoadFraction``    Double   0 - 1                Yes       Cool load for the existing cooling system
  ======================  =======  ===================  ========  =========================================

.. _existing_hvac_distribution_system:

Existing HVAC Distribution System
"""""""""""""""""""""""""""""""""

Operations on the existing distribution systems are entered in ``automatedMeasures.existingHvacDistributionSystem``. This object covers the predefined distribution system(s), either air and/or hydronic,
connected to the base building's heating and cooling systems.

.. literalinclude:: schemas/request/ExistingHVACDistributionSystem.json5
   :language: javascript

  ==========  ====================  ===========  ========  ============================
  Property    Type                  Constraints  Required  Description
  ==========  ====================  ===========  ========  ============================
  ``action``  String                See [#]_     Yes       Operation on existing distribution system
  ``adjust``  Object                             No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`               No [#]_   Implied costs of measure
  ==========  ====================  ===========  ========  ============================

  .. [#] ``action`` choices are "keep", "remove", or "adjust".

     **"keep"** maintains existing distribution system as is. Using this action indicates that there are no changes to the distribution system at all. This action will override anything in ``adjust`` object.

     **"remove"** indicates the existing distribution system is completely removed. This action will override anything in ``adjust`` object.

  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for existing HVAC distribution system:

  ====================  =======  ===========  ========================================================
  Property              Type     Constraints  Description
  ====================  =======  ===========  ========================================================
  ``leakageUnits``      String   See [#]_     Duct leakage units must be the same as ``baseBuilding``.
  ``leakageValue``      Double   >= 0.0       Duct leakage value
  ``insulationRValue``  Double   >= 0.0
  ====================  =======  ===========  ========================================================

  Values can be defined and will only be applied if applicable. For example, if there isn't ``airDistribution``, then ``leakageValue`` won't be applied.
  
  .. [#] Units choices are "CFM25", "CFM50", or "fraction".

.. _existing_water_heating_system:

Existing Water Heating System
"""""""""""""""""""""""""""""

Operations on the existing water heating system can be entered in ``automatedMeasures.existingWaterHeatingSystem``. This object covers the predefined water heating system in the base building.

.. literalinclude:: schemas/request/ExistingWaterHeatingSystem.json5
   :language: javascript

  ==========  ====================  ===========  ========  ============================
  Property    Type                  Constraints  Required  Description
  ==========  ====================  ===========  ========  ============================
  ``action``  String                See [#]_     Yes       Operation on existing water heating system
  ``adjust``  Object                             No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`               No [#]_   Implied costs of measure
  ==========  ====================  ===========  ========  ============================

  .. [#] ``action`` choices are "keep", "remove", or "adjust".

     **"keep"** maintains existing water heating system as is, including load fraction. Using this action indicates that there are no changes to the water heating system at all. This action will override anything in ``adjust`` object.

     **"remove"** indicates the existing water heating system is completely removed. This action will override anything in ``adjust`` object.

  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for existing water heating system:

  =====================  =======  ===========  ==============================================
  Property               Type     Constraints  Description
  =====================  =======  ===========  ==============================================
  ``dhwLoadFraction``    Double   0 - 1        Domestic hot water load for the existing water heating system
  =====================  =======  ===========  ==============================================

Add new systems with minimal configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adding a new system may require knowledge of the current house, possibly not available at request time. For that reason,
simpler instructions are made available to let the user add a system with minimal configuration (e.g. ENERGY STAR
compliant heat pump).

.. _new_heat_pump:

New Heat Pump
"""""""""""""

Characteristics of a new heat pump system can be entered in ``automatedMeasures.newHeatPump``.

.. literalinclude:: schemas/request/NewHeatPump.json5
   :language: javascript

  =========================  ====================  ===========  ========  =======  ===================================
  Property                   Type                  Constraints  Required  Default  Description
  =========================  ====================  ===========  ========  =======  ===================================
  ``systemType``             String                See [#]_     Yes                Type of heat pump
  ``performanceClass``       String                See [#]_     Yes
  ``heatLoadFraction``       Double                0 - 1        Yes                Heat load for the new heat pump
  ``heatLoadGapFraction``    Double                0 - 1        No        0.0      Heat load for the new heat pump
  ``coolLoadFraction``       Double                0 - 1        Yes                Cool load for the new heat pump
  ``coolLoadGapFraction``    Double                0 - 1        No        0.0      Cool load for the new heat pump
  ``costs``                  Array of :ref:`cost`               No        ``[]``   Implied costs of measure
  =========================  ====================  ===========  ========  =======  ===================================

  .. [#] ``systemType`` choices are "heat pump", "mini-split" or "air-to-air"

     **"heat pump"** is a generic air source heat pump that will be automatically determined based on the existing conditions
     in the building. If the existing building contains ducts, a central ducted ASHP will be defined. If no ducts exist, a ductless mini-split will be defined.

  .. [#] ``performanceClass`` choices are "federal minimally compliant" or "energy star compliant"

Assumptions for ``efficiencyClass``:
  ===========================  ===========================  =====================
  Type                         Federal Minimally Compliant  ENERGY STAR Compliant
  ===========================  ===========================  =====================
  mini-split (ductless)        SEER: 14, HSPF: 8.8          SEER: 17.8, HSPF: 10
  air-to-air (ducted/central)  SEER: 14, HSPF: 8.8          SEER: 15.7, HSPF: 9.2
  ===========================  ===========================  =====================

.. _new_water_heating_system:

New Water Heating System
""""""""""""""""""""""""

Characteristics of a new water heating system can be entered in ``automatedMeasures.newWaterHeatingSystem``.

.. literalinclude:: schemas/request/NewWaterHeatingSystem.json5
   :language: javascript

  =====================  ====================  ===========  ========  =======  ===================================
  Property               Type                  Constraints  Required  Default  Description
  =====================  ====================  ===========  ========  =======  ===================================
  ``systemType``         String                See [#]_     Yes                Type of water heating system. fuelType assumed as base heating fuel for "storage water heater" and "instantaneous water heater".
  ``efficiencyClass``    String                See [#]_     Yes
  ``dhwLoadFraction``    Double                0 - 1 [#]_   No        1.0      DHW load for the new water heating system
  ``costs``              Array of :ref:`cost`               No        ``[]``   Implied costs of measure
  =====================  ====================  ===========  ========  =======  ===================================

  .. [#] ``systemType`` choices are "storage water heater", "instantaneous water heater", and "heat pump water heater"
  .. [#] ``efficiencyClass`` choices are "standard" or "premium"
  .. [#] The sum of ``dhwLoadFraction`` across all water heating systems must be <= 1.


Assumptions for ``efficiencyClass``:
  ==========================  ===========  ========  =======
  Type                        Fuel         Standard  Premium
  ==========================  ===========  ========  =======
  heat pump water heater      electricity  N/A       3.5
  storage water heater        electricity  0.92      0.95
  storage water heater        natural gas  0.59      0.67
  storage water heater        fuel oil     0.62      0.68
  storage water heater        propane      0.59      0.67
  storage water heater        other        0.59      N/A
  instantaneous water heater  electricity  0.99      N/A
  instantaneous water heater  natural gas  0.82      N/A
  instantaneous water heater  fuel oil     N/A       N/A
  instantaneous water heater  propane      0.82      N/A
  instantaneous water heater  other        N/A       N/A
  ==========================  ===========  ========  =======

Adjust global aspects of the building
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use these special measures to adjust global aspect of the building. At the moment, the supported measures modify the
thermostat, attic insulation and air sealing.

.. _adjust_air_sealing:

Air Sealing
"""""""""""

Adjustments to the building air leakage rates can be entered in ``automatedMeasures.airSealing``.

.. literalinclude:: schemas/request/AirSealing.json5
   :language: javascript

  ==========  ===================================================  ========  ===========================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ===========================
  ``adjust``  Object                                               Yes       Aspect properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ===========================

  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for air sealing:

  ===================  ======  ===========  =======  =======================================
  Property             Type    Constraints  Default  Description
  ===================  ======  ===========  =======  =======================================
  ``rateUnit``         String  See [#]_     ACH      Units of air leakage rate
  ``rate``             Double  > 0.0        7.0      Value of air leakage rate
  ``housePressurePa``  Double  > 0.0        50.0     House pressure in Pa with respect to outside
  ===================  ======  ===========  =======  =======================================
  
  .. [#] rateUnit choices are ACH or CFM.

.. _adjust_attic_insulation:

Attic Insulation
""""""""""""""""

Adjustments to existing attic insulation can be entered in ``automatedMeasures.atticInsulation``.

.. literalinclude:: schemas/request/AtticInsulation.json5
   :language: javascript

  ==========  ===================================================  ========  ===========================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ===========================
  ``adjust``  Object                                               Yes       Aspect properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ===========================

  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for attic insulation:

  ================================  ======  ============  ===========  =======  =======================================
  Property                          Type    Units         Constraints  Default  Description
  ================================  ======  ============  ===========  =======  =======================================
  ``floorAssemblyEffectiveRValue``  Double  F-ft2-hr/Btu  > 0.0        50.6     Effective R-value of attic floor assembly
  ================================  ======  ============  ===========  =======  =======================================

.. _adjust_thermostat:

Thermostat
""""""""""

Adjustments to thermostat settings can be entered in ``automatedMeasures.thermostat``.

.. literalinclude:: schemas/request/Thermostat.json5
   :language: javascript

  ==========  ===================================================  ========  ===========================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ===========================
  ``adjust``  Object                                               Yes       Aspect properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ===========================

  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for thermostat:

  =================  ======  ==============================================
  Property           Type    Description
  =================  ======  ==============================================
  ``heatingSeason``  Object  Thermostat settings for heating season
  ``coolingSeason``  Object  Thermostat settings for cooling season
  =================  ======  ==============================================

``heatingSeason`` and ``coolingSeason`` objects share the following schema:

  ===========================  =======  ========  ===========  ========================  ===========
  Property                     Type     Units     Constraints  Default                   Description
  ===========================  =======  ========  ===========  ========================  ===========
  ``setpoint``                 Integer  F         > 0          Heating: 67, Cooling: 78  Season setpoint temperature
  ``setback``                  Integer  F         > 0          Heating: 64, Cooling: 72  Season setback temperature [#]_
  ``setbackStartHour``         Integer            0 - 23       Heating: 23, Cooling: 9   Start hour for daily setback period
  ``totalWeeklySetbackHours``  Integer  hrs/week  > 0          Heating: 49, Cooling: 42  Hours per week of temperature setback
  ===========================  =======  ========  ===========  ========================  ===========

  .. [#] Also referred to as setup temperature for the cooling season.

.. _cost:

Cost
^^^^

.. literalinclude:: schemas/request/Cost.json5
   :language: javascript

.. _custom_measures:

Custom Measures
^^^^^^^^^^^^^^^

.. literalinclude:: schemas/request/CustomMeasure.json5
   :language: javascript

.. _improved_building:

Improved Building
^^^^^^^^^^^^^^^^^

Custom-defined improved buildings are not currently supported. The associated schemas will be added here in a future version.


Global Controls
---------------

Global controls is an optional schema and contains a variety of customization settings that affect all models contained within the request.

.. literalinclude:: schemas/request/GlobalControls.json5
   :language: javascript

.. _units:

Units
*****

Units is an optional schema that allows the user to specify what unit(s) are included in the response.

.. literalinclude:: schemas/request/Units.json5
   :language: javascript

The following list contains the conversion factors used within the modeling engine.
- 1 MBTU= 1,000,000 BTU
- 1 BTU = 0.000293071 kWh
- 1 BTU = 1.055056 × 10^-5 therms
- 1 kWh = 3412.142 BTU
- 1 kWh = 0.003412142 MBTU
- 1 metric ton of coal =  20.9 MBTU
- 1 short ton of coal = 19.6 MBTU
- 1 gallon fueloil = 0.139 MBTU
- 1 gallon propane = 0.09154 MBTU
- 1 ccf natural gas = 1.034129 MBTU
- 1 therm natural gas = 0.000010015 MBTU

.. _escalation_rates:

Escalation Rates
****************

.. literalinclude:: schemas/request/EscalationRates.json5
   :language: javascript
