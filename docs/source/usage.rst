Usage
=====

.. _introduction:

Introduction
------------

Request Example
***************

Send a POST request to `/v1/timelines` with the following payload.

.. literalinclude:: examples/request/timelines/post/simple_bare_bones.json
   :language: javascript

Response Example
****************

And the response will have the following structure.

.. literalinclude:: examples/response/timelines/post/simple_bare_bones.json
   :language: javascript

..
   Modeling
   --------

Use cases
---------

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

Extended inputs and outputs
***************************

.. Lastly, see :ref:`extended_inputs_and_outputs` for extensive request and response examples that showcase many properties
available in the API.

Lastly, see :doc:`extended_inputs_and_outputs` for extensive request and response examples that showcase many properties
available in the API.
