.. _automated_measures:

Automated Measures
==================

Automated measures allow users to apply improvements to the base building without fully defining either the base or improved building definition.

Operate on existing systems
---------------------------

Remove or adjust relevant values of existing systems (e.g. heating, cooling, heat pump, water heater or
distribution systems). The defaulting engine determines whether a house contains one or more of these systems. Existing
systems are harder to refer to in improved building definitions and assumptions change from house to house. For that
reason, these automated measures help modify the base building definition without much knowledge of said
characteristics.

.. _existing_hvac_heating_system:

Existing HVAC Heating System
****************************

Operations to the existing HVAC heating system are entered in ``automatedMeasures.existingHvacHeatingSystem``. This object covers the singular predefined system providing heating in the base building, including a heat pump.

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
****************************

Operations on the existing cooling systems are entered in ``automatedMeasures.existingHvacCoolingSystem``. This object covers the singular predefined system providing cooling in the base building, including a heat pump.

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
*********************************

Operations on the existing distribution systems are entered in ``automatedMeasures.existingHvacDistributionSystem``. This object covers the predefined distribution system(s), either air and/or hydronic,
connected to the base building's heating and cooling systems.

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

  ====================  =======  ===========  ==============================================
  Property              Type     Constraints  Description
  ====================  =======  ===========  ==============================================
  ``leakageUnits``      String   See [#]_     Duct leakage units
  ``leakageValue``      Double   >= 0.0       Duct leakage value
  ``insulationRValue``  Double   >= 0.0
  ====================  =======  ===========  ==============================================

  Values can be defined and will only be applied if applicable. For example, if there isn't ``airDistribution``, then ``leakageValue`` won't be applied.
  
  .. [#] Units choices are CFM25, CFM50, or Percent.

.. _existing_water_heating_system:

Existing Water Heating System
*****************************

Operations on the existing water heating system can be entered in ``automatedMeasures.existingWaterHeatingSystem``. This object covers the predefined water heating system in the base building.

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
------------------------------------------

Adding a new system may require knowledge of the current house, possibly not available at request time. For that reason,
simpler instructions are made available to let the user add a system with minimal configuration (e.g. ENERGY STAR
compliant heat pump).

.. _new_heat_pump:

New Heat Pump
*************

Characteristics of a new heat pump system can be entered in ``automatedMeasures.newHeatPump``.

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
************************

Characteristics of a new water heating system can be entered in ``automatedMeasures.newWaterHeatingSystem``.

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

Assumptions for ``tankVolume`` when ``systemType`` is "heat pump water heater":
  ===============  =============
  Resident Count   Volume (gal)
  ===============  =============
  2 or less        50
  3                65
  4 or more        80
  ===============  =============


Adjust global aspects of the building
-------------------------------------

Use these special measures to adjust global aspect of the building. At the moment, the supported measures modify the
thermostat, attic insulation and air sealing.

.. _adjust_air_sealing:

Air Sealing
***********

Adjustments to the building air leakage rates can be entered in ``automatedMeasures.airSealing``.

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

The air sealing measure is only applicable when the improved air sealing rate is less than the base rate. 

.. _adjust_attic_insulation:

Attic Insulation
****************

Adjustments to existing attic insulation can be entered in ``automatedMeasures.atticInsulation``.

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

Attic insulation measure applicability is determined based on the difference between the base and improved assembly effective R-value using the following table.

  =====================  =========================  ===========
  Base Assembly R-Value  Improved Assembly R-Value  Applicable?
  =====================  =========================  ===========
  < 8.7                  < 8.7                      false
  < 8.7                  >= 8.7                     true
  8.7 - 20.6             < 26.6                     false
  8.7 - 20.6             >= 26.6                    true
  > 20.6                 < 31.6                     false
  > 20.6                 >= 31.6                    true
  =====================  =========================  ===========

.. _adjust_thermostat:

Thermostat
**********

Adjustments to thermostat settings can be entered in ``automatedMeasures.thermostat``.

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


Automated measures example adding a new heat pump
-------------------------------------------------

Request
*******

Send a POST request to `/v1/timelines` with the following payload.

.. literalinclude:: examples/request/timelines/post/automated_measures_new_heat_pump.json
   :language: json

Response
********

And the response will have the following structure.

.. literalinclude:: examples/response/timelines/post/automated_measures_new_heat_pump.json
   :language: json
