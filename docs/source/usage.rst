Usage
=====

.. _introduction:

Introduction
------------

This API is built with flexibility in mind to provide each user with as much or as little data they have available and develop the best representation of that house possible.

Use cases
---------

Address only request
********************

.. sidebar:: Address only request

   .. literalinclude:: examples/request/timelines/post/simple_bare_bones.json
      :language: json

The simplest request available is a simple address. Our system will pull all available data for the address and build an energy model for the condition of the current house, which is called the ``baseBuilding``.

Different modeling results resolution
*************************************

Energy consumption datapoints and derived calculations from any model can be requested in different resolutions: annual,
monthly and hourly.

.. See :ref:`modeling_results_resolution` for further details.

See :doc:`modeling_results_resolution` for further details.

Use automated measures
**********************

High-level operations applied to the base building, without requiring a full improved building definition.

.. See :ref:`automated_measures` for further details.

See :doc:`automated_measures` for further details.

Request Example
***************

Send a POST request to `/v1/timelines` with the following payload.

.. literalinclude:: examples/request/timelines/post/simple_bare_bones.json
   :language: json

Response Example
****************

And the response will have the following structure.

.. literalinclude:: examples/response/timelines/post/simple_bare_bones.json
   :language: json



Extensive inputs and outputs
****************************

..
   Lastly, see :ref:`extensive_inputs_and_outputs` for extensive request and response examples that showcase many
   properties available in the API.

Lastly, see :doc:`extensive_inputs_and_outputs` for extensive request and response examples that showcase many properties
available in the API.
