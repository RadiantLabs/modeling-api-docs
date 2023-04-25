.. _automated_measures:

Automated Measures
==================

High-level operations applied to the base building, without requiring a full improved building definition.

Operate on existing systems
---------------------------

Keep, remove or adjust relevant values of existing systems (e.g. heating, cooling, heat pump, water heater or
distribution systems). The defaulting engine determines whether a house contains one or more these systems. Existing
systems are harder to refer to in improved building definitions and assumptions change from house to house. For that
reason, these automated measures help modify the base building definition without much knowledge of the status quo said
characteristics.

Existing HVAC Heating System
****************************

Property: ``existingHvacHeatingSystem``

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
