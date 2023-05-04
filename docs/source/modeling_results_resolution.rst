.. modeling_results_resolution:

Modeling Results Resolution
===========================

Simple bare bones example
-------------------------

Request
*******

Send a POST request to `/v1/timelines` with the following payload.

.. literalinclude:: examples/request/timelines/post/simple_bare_bones.json
   :language: json

Response
********

And the response will have the following structure.

.. literalinclude:: examples/response/timelines/post/simple_bare_bones.json
   :language: json


Simple example with monthly datapoints
--------------------------------------

Request
*******

Send a POST request to `/v1/timelines` with the following payload.

.. literalinclude:: examples/request/timelines/post/simple_with_monthly.json
   :language: json

Response
********

And the response will have the following structure.

.. literalinclude:: examples/response/timelines/post/simple_with_monthly.json
   :language: json


Simple example with monthly and hourly datapoints
-------------------------------------------------

Request
*******

Send a POST request to `/v1/timelines` with the following payload.

.. literalinclude:: examples/request/timelines/post/simple_with_monthly_and_hourly.json
   :language: json

Response
********

Unfortunately, the example with hourly datapoints is very big and difficult to process in the browser. We recommend to
download the compressed version, uncompress it locally and use an editor with JSON formatting capabilities.

The only difference, as with the monthly resolution, is that objects including datapoints will have an extra property
`hourly` with arrays of size 8760. There are 8760 hours in a standard year.

Download the response example with hourly datapoints here: :download:`simple_with_monthly_and_hourly.json.gz <examples/response/timelines/post/simple_with_monthly_and_hourly.json.gz>`.

.. note::

   In a future release, we'll automatically compress the response payload including hourly data and inform the client
   through the `Content-Encoding` HTTP response header.
