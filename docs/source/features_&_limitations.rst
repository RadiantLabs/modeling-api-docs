Features & Limitations
======================

Latest Version
**************

  .. Planning to move all versions implemented in reverse order beneath latest version header, then the next version(s) will be in ascending order after upcoming versions header 

v1.0.0
------

`baseBuilding <https://docs.radiantlabs.co/projects/modeling-api/en/latest/request_structure.html#base-building>`_
    Users may specify known building characteristics with this object, which override the Defaulting Engine.

`automatedMeasures <https://docs.radiantlabs.co/projects/modeling-api/en/latest/request_structure.html#automated-measures>`_
    Allows users to apply improvements to the base building without fully defining either the base or improved building definition.

Property Types
    The following **single-family residential** property types are currently supported for modeling.

    - "CABIN"
    - "DUPLEX"
    - "MANUFACTURED HOME"
    - "MOBILE HOME"
    - "MOBILE HOME PARK"
    - "RURAL HOMESITE"
    - "SFR"
    - "TOWNHOUSE/ROWHOUSE"
    - "Bandominium"
    - "Garden Home"
    - "Homestead"
    - "Recreational Vehicles"
    - "Single Family Residence w/ ADU"

Single Value Collections


Upcoming Versions
*****************

v2.0.0
------

- ``propertyUseType``
- ``energyEndUses``
- Bug fixes

Future Versions
---------------

- improvedBuilding
- Multiple Value Collections
