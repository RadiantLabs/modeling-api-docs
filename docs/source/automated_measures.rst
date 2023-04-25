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

Existing HVAC Heating System
****************************

This object covers the singular predefined system providing heating in the base building, including a heat pump.

Property: ``existingHvacHeatingSystem``

Schema:

  ==========  ===================================================  ========  =================================
  Property    Type                                                 Required  Description
  ==========  ===================================================  ========  =================================
  ``action``  One of ``keep`` [#]_, ``remove`` [#]_ or ``adjust``  Yes       Operation on existing system
  ``adjust``  Object                                               No [#]_   System properties to adjust
  ``costs``   Array of :ref:`cost`                                 No [#]_   Implied costs of measure
  ==========  ===================================================  ========  =================================

  .. [#] Keeps existing heating system as is, including load percentage. Using this flag indicates that there are no changes to heating at all.
  .. [#] Completely removes existing heating system and new system(s) must cover 100% of load or specify ``loadGapPercentage``. This property will override anything in adjust object.
  .. [#] The ``adjust`` object is required if ``action`` is set to ``adjust``.
  .. [#] No costs array will translate into an empty array of costs (i.e. ``"costs": []``).

``adjust`` schema:

  ======================  =======  ===================  ==============================================
  Property                Type     Constraints          Description
  ======================  =======  ===================  ==============================================
  ``backup``              Boolean                       Indicates the existing heating system is being kept, but switched to be a backup system. Either this OR ``heatLoadPercentage`` can be defined.
  ``heatLoadPercentage``  Double   In [0.0, 1.0] range  Set heat load for the existing heating system.
  ======================  =======  ===================  ==============================================

Existing HVAC Cooling System
****************************

Property: ``existingHvacCoolingSystem``

Existing Hvac Distribution System
*********************************

Property: ``existingHvacDistributionSystem``

Existing Water Heating System
*****************************

Property: ``existingWaterHeatingSystem``

Add new systems with minimal configuration
------------------------------------------

Adding a new system may require knowledge of the current house, possibly not available at request time. For that reason,
simpler instructions are made available to let the user add a system with minimal configuration (e.g. ENERGY STAR
compliant heat pump).

New Heat Pump
*************

Property: ``newHeatPump``

New Water Heating System
************************

Property: ``newWaterHeatingSystem``

Adjust global aspects of the building
-------------------------------------

Use these special measures to adjust global aspect of the building. At the moment, the supported measures modify the
thermostat, attic insulation and air sealing.

Air Sealing
***********

Property: ``airSealing``

Attic Insulation
****************

Property: ``atticInsulation``

Thermostat
**********

Property: ``thermostat``
