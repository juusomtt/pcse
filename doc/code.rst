*****************
API documentation
*****************

The API documentation provides a description of the interface and internals of 
all SimulationObjects, AncillaryObjects and utility routines available in the
PCSE source distribution. All SimulationObjects and AncillaryObjects are
described using the same structure:
    
    1. A short description of the object
    2. The positional parameters and keywords specified in the interface.
    3. A table specifying the simulation parameters needed for the simulation
    4. A table specifying the state variables of the SimulationObject
    5. A table specifying the rate variables of the SimulationObject
    6. Signals sent or received by the SimulationObject
    7. External dependencies on state/rate variables of other SimulationObjects.
    8. The exceptions that are raised under which conditions.
    
One or more of these sections may be excluded when they are not appropriate
for the SimulationObject that is described.

The table specifying the simulation parameters has the following columns:
    1. The name of the parameter.
    2. A description of the parameter.
    3. The type of the parameter. This is provided as a three-character code
       with the following interpretation. The first character indicates of the
       parameter is a scalar **(S)** or table **(T)** parameter. The second and
       third
       character indicate whether this parameter should be present in the
       timerdata '**Ti**', cropdata '**Cr**', soildata '**So**' or
       sitedata '**Si**' dictionary.
    4. The unit of the parameter.

The tables specifying state/rate variables have the following columns:
    1. The name of the variable.
    2. A description of the variable.
    3. Whether the variable is published in the kiosk or not: Y|N
    4. The unit of the variable.
    
Finally, all public methods of all object are described as well.

Engines and models
==================

 .. autoclass:: pcse.engine.Engine
    :members:

.. autoclass:: pcse.models.Wofost71_PP
    :members:

.. autoclass:: pcse.models.Wofost71_WLP_FD
    :members:

.. _Agromanagement:

Agromanagement
==============

.. autoclass:: pcse.agromanagement.AgroManagementSingleCrop
    :members:

The Timer
=========

.. autoclass:: pcse.timer.Timer
    :members:

The waterbalance
================

The PyWOFOST distribution provides several waterbalance modules:
    1. WaterbalancePP which is used for simulation under non-waterlimited
       production
    2. WaterbalanceFD which is used for simulation of water-limited production
       under conditions of freely draining soils
    3. A multi-layer waterbalance implementing simulations for potential
       conditions, water-limited free drainage conditions and
       water-limited groundwater conditions (in case of shallow ground
       water tables.)
    4. A model for simulation the build-up and melting of the snow cover.
       
.. autoclass:: pcse.soil.WaterbalancePP
    :members:

.. autoclass:: pcse.soil.WaterbalanceFD
    :members:

.. autoclass:: pcse.soil.snowmaus.SnowMaus
    :members:

Crop simulation processes
=========================


Integration of crop simulation processes
----------------------------------------

.. autoclass:: pcse.cropsimulation.CropSimulation
    :members:


Phenology
---------

.. autoclass:: pcse.crop.phenology.DVS_Phenology
    :members:

.. autoclass:: pcse.crop.phenology.Vernalisation
    :members:

Partitioning
------------

.. autoclass:: pcse.crop.partitioning.DVS_Partitioning
    :members:


|CO2| Assimilation
------------------

.. autoclass:: pcse.crop.assimilation.WOFOST_Assimilation
    :members:
    
Respiration
-----------
.. autoclass:: pcse.crop.respiration.WOFOSTMaintenanceRespiration
    :members:
    
Evapotranspiration
------------------
.. autoclass:: pcse.crop.evapotranspiration.Evapotranspiration
    :members:

.. autofunction:: pcse.crop.evapotranspiration.SWEAF

    
Leaf dynamics
-------------
.. autoclass:: pcse.crop.leaf_dynamics.WOFOST_Leaf_Dynamics
    :members:

Root dynamics
-------------
.. autoclass:: pcse.crop.root_dynamics.WOFOST_Root_Dynamics
    :members:

Stem dynamics
-------------
.. autoclass:: pcse.crop.stem_dynamics.WOFOST_Stem_Dynamics
    :members:

Storage organ dynamics
----------------------
.. autoclass:: pcse.crop.storage_organ_dynamics.WOFOST_Storage_Organ_Dynamics
    :members:

Abiotic damage
--------------
.. autoclass:: pcse.crop.abioticdamage.FROSTOL
    :members:

.. autoclass:: pcse.crop.abioticdamage.CrownTemperature
    :members:

.. autoclass:: pcse.crop.abioticdamage.CERES_WinterKill
    :members:

Base classes
============

The base classes define much of the functionality which is used "under the
hood" in PyWOFOST. Except for the `VariableKiosk` and the `WeatherDataContainer`
all classes are not to be called directly but should be subclassed instead.


VariableKiosk
-------------
.. autoclass:: pcse.base_classes.VariableKiosk
    :members:


Base classes for parameters, rates and states
---------------------------------------------

.. autoclass:: pcse.base_classes.StatesTemplate
    :members:

.. autoclass:: pcse.base_classes.RatesTemplate
    :members:

.. autoclass:: pcse.base_classes.ParamTemplate
    :members:

Base and utility classes for weather data
-----------------------------------------

.. autoclass:: pcse.base_classes.WeatherDataProvider
    :members:

.. autoclass:: pcse.base_classes.WeatherDataContainer
    :members:

Signals defined
===============
.. automodule:: pcse.signals
    :members:
    

Utilities
=========

The utilities section deals with tools for reading weather data and parameter
values from files or databases.

.. _TheCABOtools:

The CABO tools
--------------

The CABO tools contain two classes: the `CABOFileReader` for reading in
parameter files in the CABO format and the `CABOWeatherDataProvider` for reading
files from the CABO weather system.

.. autoclass:: pcse.cabo.CABOFileReader
    :members:

.. autoclass:: pcse.cabo.CABOWeatherDataProvider
    :members:

The PW tools
------------

the PW tools


The database tools
------------------

The database tools contain functions and classes for retrieving parameters
and weather database from (modified) CGMS database.

.. autofunction:: pcse.db_input.fetch_cropdata
.. autofunction:: pcse.db_input.fetch_soildata
.. autofunction:: pcse.db_input.fetch_timerdata
.. autofunction:: pcse.db_input.fetch_sitedata
.. autoclass:: pcse.db_input.GridWeatherDataProvider
    :members:
.. autoclass:: pcse.db_input.EnsembleGridWeatherDataProvider
    :members:
    
Convenience routines
--------------------

These routines are there for conveniently starting a PyWOFOST session. They
are mainly used in the tutorial and examples but can be used to further
elaborating on when writing your own scripts.

.. autofunction:: pcse.start_pywofost.start_pywofost

.. autofunction:: pcse.run_pywofost.run_pywofost


Miscelaneous utilities
----------------------

Many miscelaneous for a variety of purposes such as the Arbitrary Function
Generator (AFGEN) and functions for calculating Penman evapotranspiration,
the Angstrom equation and astronomical calculations such as day length.

.. autofunction:: pcse.util.penman
.. autofunction:: pcse.util.check_angstAB
.. autofunction:: pcse.util.wind10to2
.. autofunction:: pcse.util.angstrom
.. autofunction:: pcse.util.doy
.. autofunction:: pcse.util.limit
.. autofunction:: pcse.util.daylength
.. autofunction:: pcse.util.astro
.. autofunction:: pcse.util.merge_dict
.. autoclass:: pcse.util.Afgen
    :members:



.. |CO2| replace:: CO\ :sub:`2`\
.. |d| replace:: day
.. |C| replace:: :math:`^{\circ}C`
.. |hPa| replace:: :math:`hPa`
.. |msec-1| replace:: :math:`m sec^{-1}`
.. |cmday-1| replace:: :math:`cm day^{-1}`
.. |Jm-2day-1| replace:: :math:`J m^{-2} day^{-1}`

.. |C day-1| replace:: :math:`^{\circ}C day^{-1}`
.. |C-1day-1| replace:: :math:`^{\circ}C^{-1} day^{-1}`
.. |C-3day-1| replace:: :math:`^{\circ}C^{-3} day^{-1}`

.. |kg ha-1| replace:: kg ha\ :sup:`-1`\
.. |ha kg-1| replace:: ha kg\ :sup:`-1`\
.. |kg ha-1 d-1| replace:: kg ha\ :sup:`-1`\ day\ :sup:`-1`\
.. |kg kg-1 d-1| replace:: kg kg\ :sup:`-1`\ day\ :sup:`-1`\
.. |ha ha-1 d-1| replace:: ha ha\ :sup:`-1`\ day\ :sup:`-1`\
.. |kg ha-1 hr-1| replace:: kg ha\ :sup:`-1`\ hr\ :sup:`-1`\
.. |kg ha-1 hr-1 /(J m-2 s-1)| replace:: kg ha\ :sup:`-1`\ hr\ :sup:`-1`\ /(J m\ :sup:`-2`\ sec\ :sup:`-1`\)

.. |day-1| replace:: day\ :sup:`-1`\
.. |cm day-1| replace:: cm day\ :sup:`-1`\
.. |cmC-1day-1| replace:: :math:`cm^{\circ}C^{-1} day^{-1}`
.. |Cday| replace:: :math:`^{\circ}C day`
.. |kg CH2O kg-1 d-1| replace:: kg CH\ :sub:`2`\O kg\ :sup:`-1`\ d\ :sup:`-1`\
.. |kg CH2O ha-1| replace:: kg CH\ :sub:`2`\O ha\ :sup:`-1`\
.. |kg CH2O ha-1 d-1| replace:: kg CH\ :sub:`2`\O ha\ :sup:`-1`\ day\ :sup:`-1`\

