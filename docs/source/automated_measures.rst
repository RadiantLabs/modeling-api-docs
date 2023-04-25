.. _automated_measures:

Automated Measures
==================

High-level operations applied to the base building, without requiring a full improved building definition.

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

This object covers the singular predefined system providing heating in the base building, including a heat pump.

Property: ``existingHvacHeatingSystem``

Schema:

  ==========  ===================================================  ========  ============================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ============================
  ``action``  One of ``keep`` [#]_, ``remove`` [#]_ or ``adjust``  Yes       Operation on existing system
  ``adjust``  Object                                               No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ============================

  .. [#] Keeps existing heating system as is, including load percentage. Using this action indicates that there are no changes to heating at all.
  .. [#] Completely removes existing heating system and new system(s) must cover 100% of load or specify ``loadGapPercentage``. This action will override anything in ``adjust`` object.
  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for existing HVAC heating system:

  ======================  =======  ===================  ==============================================
  Property                Type     Constraints          Description
  ======================  =======  ===================  ==============================================
  ``backup``              Boolean                       Indicates the existing heating system is being kept, but switched to be a backup system. Either this OR ``heatLoadPercentage`` can be defined.
  ``heatLoadPercentage``  Double   In [0.0, 1.0] range  Heat load for the existing heating system.
  ======================  =======  ===================  ==============================================

.. _existing_hvac_cooling_system:

Existing HVAC Cooling System
****************************

Property: ``existingHvacCoolingSystem``

Schema:

  ==========  ===================================================  ========  ============================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ============================
  ``action``  One of ``keep`` [#]_, ``remove`` [#]_ or ``adjust``  Yes       Operation on existing system
  ``adjust``  Object                                               No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ============================

  .. [#] Keeps existing cooling system as is, including load percentage. Using this action indicates that there are no changes to cooling at all.
  .. [#] Completely removes existing cooling system and new system(s) must cover 100% of load or specify ``loadGapPercentage``. This action will override anything in ``adjust`` object.
  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for existing HVAC cooling system:

  ======================  =======  ===================  =========================================
  Property                Type     Constraints          Description
  ======================  =======  ===================  =========================================
  ``coolLoadPercentage``  Double   In [0.0, 1.0] range  Cool load for the existing cooling system
  ======================  =======  ===================  =========================================

.. _existing_hvac_distribution_system:

Existing Hvac Distribution System
*********************************

Property: ``existingHvacDistributionSystem``

Schema:

  ==========  ===================================================  ========  ============================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ============================
  ``action``  One of ``keep`` [#]_, ``remove`` [#]_ or ``adjust``  Yes       Operation on existing system
  ``adjust``  Object                                               No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ============================

  .. [#] Keeps existing distribution system as is. Using this action indicates that there are no changes to distribution systems at all.
  .. [#] Completely removes existing distribution system. This action will override anything in ``adjust`` object.
  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for existing HVAC distribution system:

  ====================  =======  ===========  ==============================================
  Property              Type     Constraints  Description
  ====================  =======  ===========  ==============================================
  ``leakageValue``      Double   >= 0.0
  ``insulationRValue``  Double   >= 0.0
  ====================  =======  ===========  ==============================================

  Values can be defined and will only be applied if applicable. For example, if there isn't ``airDistribution``, then ``leakageValue`` won't be applied.

.. _existing_water_heating_system:

Existing Water Heating System
*****************************

Property: ``existingWaterHeatingSystem``

Schema:

  ==========  ===================================================  ========  ============================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ============================
  ``action``  One of ``keep`` [#]_, ``remove`` [#]_ or ``adjust``  Yes       Operation on existing system
  ``adjust``  Object                                               No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ============================

  .. [#] Keeps existing water heating system as is, including load percentage. Using this action indicates that there are no changes to water heating systems at all.
  .. [#] Completely removes existing water heating system and new system(s) must cover 100% of load. This action will override anything in ``adjust`` object.
  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for existing water heating system:

  =====================  =======  ===================  ==============================================
  Property               Type     Constraints          Description
  =====================  =======  ===================  ==============================================
  ``dhwLoadPercentage``  Double   In [0.0, 1.0] range  Domestic hot water load for the existing water heating system
  =====================  =======  ===================  ==============================================

Add new systems with minimal configuration
------------------------------------------

Adding a new system may require knowledge of the current house, possibly not available at request time. For that reason,
simpler instructions are made available to let the user add a system with minimal configuration (e.g. ENERGY STAR
compliant heat pump).

.. _new_heat_pump:

New Heat Pump
*************

Property: ``newHeatPump``

Schema:

  ======================  ===================================================================  ========  ===================================
  Property                Type                                                                 Required  Description
  ======================  ===================================================================  ========  ===================================
  ``systemType``          One of ``heat-pump`` [#]_, ``mini-split`` or ``air-to-air``          Yes       Type of heat pump
  ``performanceClass``    One of ``federal-minimally-compliant`` or ``energy-star-compliant``  Yes
  ``heatLoadPercentage``  Double                                                               No [#]_   Heat load for the new heat pump
  ``coolLoadPercentage``  Double                                                               No [#]_   Cool load for the new heat pump
  ``costs``               Array of :ref:`cost`                                                 No [#]_   Implied costs of measure
  ======================  ===================================================================  ========  ===================================

  .. [#] ``heat-pump`` is a generic air source heat pump that will be automatically determined based on the existing conditions in the building.
  .. [#] Defaults to ``1.0`` if not provided.
  .. [#] Defaults to ``1.0`` if not provided.
  .. [#] Defaults to ``[]`` if not provided.

.. _new_water_heating_system:

New Water Heating System
************************

Property: ``newWaterHeatingSystem``

Schema:

  =====================  =============================================================================================  ========  ===================================
  Property               Type                                                                                           Required  Description
  =====================  =============================================================================================  ========  ===================================
  ``systemType``         One of ``storage-water-heater``, ``instantaneous-water-heater`` or ``heat-pump-water-heater``  Yes       Type of water heating system
  ``efficiencyClass``    One of ``standard`` or ``high``                                                                Yes
  ``dhwLoadPercentage``  Double                                                                                         No [#]_   DHW load for the new water heating system
  ``costs``              Array of :ref:`cost`                                                                           No [#]_   Implied costs of measure
  =====================  =============================================================================================  ========  ===================================

  .. [#] Defaults to ``1.0`` if not provided.
  .. [#] Defaults to ``[]`` if not provided.

Adjust global aspects of the building
-------------------------------------

Use these special measures to adjust global aspect of the building. At the moment, the supported measures modify the
thermostat, attic insulation and air sealing.

.. _adjust_air_sealing:

Air Sealing
***********

Property: ``airSealing``

Schema:

  ==========  ===================================================  ========  ===========================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ===========================
  ``adjust``  Object                                               Yes       Aspect properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ===========================

  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for air sealing:

  ========  ======  ===========  =======================================
  Property  Type    Constraints  Description
  ========  ======  ===========  =======================================
  ``rate``  Double  > 0.0        If undefined, system default is applied
  ========  ======  ===========  =======================================

.. _adjust_attic_insulation:

Attic Insulation
****************

Property: ``atticInsulation``

Schema:

  ==========  ===================================================  ========  ===========================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ===========================
  ``adjust``  Object                                               Yes       Aspect properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ===========================

  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for attic insulation:

  ================================  ======  ===========  =======================================
  Property                          Type    Constraints  Description
  ================================  ======  ===========  =======================================
  ``floorAssemblyEffectiveRValue``  Double  > 0.0        If undefined, system default is applied
  ================================  ======  ===========  =======================================

.. _adjust_thermostat:

Thermostat
**********

Property: ``thermostat``

Schema:

  ==========  ===================================================  ========  ===========================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  ===========================
  ``adjust``  Object                                               Yes       Aspect properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  ===========================

  .. [#] Defaults to ``[]`` if not provided.

``adjust`` schema for air sealing:

  =================  ======  ==============================================
  Property           Type    Description
  =================  ======  ==============================================
  ``heatingSeason``  Object  Thermostat settings for heating season
  ``coolingSeason``  Object  Thermostat settings for cooling season
  =================  ======  ==============================================

``heatingSeason`` and ``coolingSeason`` objects share the following schema:

  ===========================  =======  ===========  ===========
  Property                     Type     Constraints  Description
  ===========================  =======  ===========  ===========
  ``setpoint``                 Integer  > 0
  ``setback``                  Integer  > 0
  ``setbackStartHour``         Integer  > 0
  ``totalWeeklySetbackHours``  Integer  > 0
  ===========================  =======  ===========  ===========
