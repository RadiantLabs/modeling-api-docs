.. _defaulting_engine:

Defaulting Engine
=================

Radiant Labs understands that API useres may not always know all of the details about the building being modeled. We have built a logical, statistics-based
system to define default building characterstics. This defaulting engine uses tax assessor data, building permits, and NREL's ResStock building stock data. 

This endpoint is built with flexibility and ease of use in mind. It requires as little information as an address to build an energy model, such as the following
payload. In this case, all characteristics of the ``baseBuilding`` would be populated using the Defaulting Engine. 

.. literalinclude:: examples/request/timelines/post/simple_bare_bones.json
   :language: json

To specify certain known building characteristics and leave other characteristics up to the Defaulting Engine, there are two methods:

1. Define known properties and skip unknown properties. Any missing keys will be defaulted.
  a. For example, in this payload for ``appliances``, ``clothesDryers`` is missing and would be defined using the Defaulting Engine.

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
  a. For example, in this payload for ``appliances``, ``clothesDryer`` is ``null`` and would be defined using the Defaulting Engine.

    .. code-block:: json
    
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

a. For example, in this payload for ``appliances``, ``clothesDryers`` is a blank array, which indicates that no clothes dryers exist in this building.

  .. code-block:: json
  
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
