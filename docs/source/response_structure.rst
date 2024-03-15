Response Structure
==================

The following sections provide the specifications and syntax for each API response schema.

.. literalinclude:: schemas/response/TopLevelResponsePayload.json5
   :language: javascript
   :caption: Top-level response payload schema

Address Found
-------------

Address Found Full
******************

.. literalinclude:: schemas/response/AddressFoundFull.json5
   :language: json

Address Found Components
************************

Refer to the :ref:`address_components` schema.

Timelines
---------

.. literalinclude:: schemas/response/ResponseTimeline.json5
   :language: javascript

Models
******

.. literalinclude:: schemas/response/ResponseModel.json5
   :language: javascript

Provided Base Building
~~~~~~~~~~~~~~~~~~~~~~

This object reflects the ``baseBuilding`` from the request. Refer to the :ref:`base_building` schema.

Applied Base Building
~~~~~~~~~~~~~~~~~~~~~

This object reflects the modeled base building, including all characteristics populated by Radiant Labs using our common schema and defaulting engine. Refer to the :ref:`base_building` schema.

Requested Improved Building
~~~~~~~~~~~~~~~~~~~~~~~~~~~

This feature is not yet supported, but will reflect the ``improvedBuilding`` from the request. Refer to the :ref:`improved_building` schema.

Applied Improved Building
~~~~~~~~~~~~~~~~~~~~~

This feature is not yet supported, but will reflect the modeled improved building, including all characteristics populated by Radiant Labs using our common schema and defaulting engine. Refer to the :ref:`improved_building` schema.

Model Costs
~~~~~~~~~~~



Energy Totals
~~~~~~~~~~~~~

.. literalinclude:: schemas/response/EnergyTotals.json5
   :language: javascript

Energy Totals per Fuel Time Series
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: schemas/response/EnergyTotalsPerFuelTimeSeries.json5
   :language: javascript

Energy Costs
~~~~~~~~~~~~

.. literalinclude:: schemas/response/EnergyCosts.json5
   :language: javascript

Energy Costs per Fuel Time Series
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: schemas/response/EnergyCostsPerFuelTimeSeries.json5
   :language: javascript

Emissions Totals
~~~~~~~~~~~~~~~~

.. literalinclude:: schemas/response/EmissionTotals.json5
   :language: javascript

Total Emissions per Fuel Time Series
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: schemas/response/EmissionTotalsPerFuelTimeSeries.json5
   :language: javascript

Typical
-------

The ``typical`` object, if requested, is an independently generated version of a typical, comparable ``baseBuilding``. The ``typicalBuilding`` has the same ``addressFull`` (i.e. physical location for regional properties, such as weather), ``conditionedFloorArea``,  ``yearBuilt``,  and ``storiesCount``. Any remaining characteristics of the ``baseBuilding`` are removed and based on our defaulting engine. The schema of this object follows the same schema as Model.

