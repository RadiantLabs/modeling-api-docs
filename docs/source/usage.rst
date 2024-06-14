.. _usage_instructions:

Usage Instructions
==================

.. setup_and_authentication:

Setup & Authentication
----------------------

To test the API ad-hoc, visit our `Interactive Modeling Documentation <https://radiantlabs-core-modeling-api.redoc.ly/>`__.

All other requests to the Modeling API must include authentication credentials.

API access tokens are applied to your requests to protect sensitive, personally-identifiable information and are compliant with common privacy and security standards.

A valid token must be included as part of the HTTP ``Authorization`` header, using the `bearer` HTTP authorization scheme. The value of the header will be ``Bearer <token>``, where you replace <token> with a valid token. Valid tokens will be provided to established customers of the API. It is planned to add the ability to rotate tokens and set expiration dates in future releases.

.. code-block::

  Authorization: Bearer <token>


Here is an example using cURL:

.. code-block:: shell

  curl -v 'https://api.radiantlabs.co/v1/timelines' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer <token>' \
    --data-raw '{"addressFull":"501 RANDALL RD, BALLSTON SPA, NY 12020"}' \
    --compressed

.. _simple_request:

Simple Request
--------------

Radiant Labs understands that API users may not always know all of the details about the building being modeled. We have built a logical, statistics-based
system to define default building characteristics. This defaulting engine uses tax assessor data, building permits, regional building stock characteristics, and detailed hourly building simulations.

This endpoint is built with flexibility and ease of use in mind. It requires as little information as an address to build an energy model, such as the following
payload. In this case, all characteristics of the ``baseBuilding`` would be populated using the Defaulting Engine. The response will include the ``appliedBaseBuilding``, which details what characteristics the defaulting engine used.

.. literalinclude:: examples/request/timelines/post/simple_bare_bones.json
   :language: json

See :ref:`address-only-response` for the response for this example.

.. _advanced_request:

Advanced Request
----------------

To specify certain known building characteristics and leave other characteristics up to the Defaulting Engine, there are two methods:

1. Define known properties and skip unknown properties. Any missing keys will be defaulted.
  a. For example, in this ``appliances`` payload, ``clothesDryers`` is missing and would be defined using the Defaulting Engine.

    .. code-block:: json
    
      "baseBuilding": {
        "appliances": {
          "cookingRanges": [
            {
              "id": "CookingRange1",
              "fuel": "natural gas",
              "isInduction": false
            }
          ]
        }
      }

2. Define known properties and indicate unknown properties as ``null``.
  a. For example, in this ``appliances`` payload, ``clothesDryer`` is ``null`` and would be defined using the Defaulting Engine.

    .. code-block:: json
      :emphasize-lines: 3
    
      "baseBuilding": {
        "appliances": {
          "clothesDryers": null,
          "cookingRanges": [
            {
              "id": "CookingRange1",
              "fuel": "natural gas",
              "isInduction": false
            }
          ]
        }
      }

To specify that certain property does not exist in the house and thus, the Defaulting Engine should :strong:`not` be used, an array should be left blank.

a. For example, in this ``appliances`` payload, ``clothesDryers`` is a blank array, which indicates that no clothes dryers exist in this building.

  .. code-block:: json
    :emphasize-lines: 3
  
    "baseBuilding": {
      "appliances": {
        "clothesDryers": [],
        "cookingRanges": [
          {
            "id": "CookingRange1",
            "fuel": "natural gas",
            "isInduction": false
          }
        ]
      }
    }

See :ref:`extensive_inputs_and_outputs` for an example of the extensive properties available in the API to fully define a building.
