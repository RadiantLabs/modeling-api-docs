Request Inputs
==============

The following sections provide the specifications and syntax for each API request schema.

.. literalinclude:: schemas/request/TopLevelRequestPayload.json5
   :language: json
   :caption: Top-level request payload schema

.. note::

   _`Objects and arrays` are used as values throughout the API. The difference is:
      - Object: This represents a single item that is not inherently a collection. An example of this is `Air Infiltration`_, which is characteristic of the entire building and thus can only be defined once.
      - Array: This represents an item that is inherently a collection, even if we don’t yet support more than one item. Often it will be an array of objects, where each object defines an item in the collection. `HVAC`_ and `Walls`_ are examples of this.

Address
-------

An address can be specified using either ``addressFull`` *or* ``addressComponents``.

.. _address_full:

Address Full
******************

.. literalinclude:: schemas/request/AddressFull.json5
   :language: json

.. _address_components:

Address Components
******************

.. literalinclude:: schemas/request/AddressComponents.json5
   :language: json


Base Building
-------------

  
Timelines
---------


Global Controls
---------------


Default Model Controls
----------------------


