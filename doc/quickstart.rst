*****************************
Getting started with PCSE
*****************************

This quickstart guide gives some examples to get you started with the Python
Crop Simulation Environment. All examples are currently focused on applying
the WOFOST crop simulation model, although other crop simulations may
become available within PCSE in the future.
All tests and examples were developed with the Enthought Python Distribution
(http://www.enthought.com/products/epd.php) under windows7 (EPD 32bit). This
product is now superseeded by Enthought Canopy.

Testing the PCSE package
========================
To guarantee its integrity, the PCSE package includes a number of self
tests that test individual components as well as the entire chain. These tests
verify that the output produced by the different components matches with the
expected outputs. Test data for the individual components can be found
in the `pcse.tests.test_data` package, while the test data for the entire chain
is stored in an SQLite database (pcse.db) that can be found under
`pcse.db.pcse`.

We assume here that PCSE is installed under 'D:\\USERDATA\\pylib\\' and
this location needs to be added to the search path of python::

    E:\temp>python
    Enthought Python Distribution -- www.enthought.com
    Version: 7.0-2 (32-bit)

    Python 2.7.1 |EPD 7.0-2 (32-bit)| (r271:86832, Dec  2 2010, 10:35:02) [MSC v.1500 32 bit (Intel)] on win32
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import sys
    >>> import sys.path.append(r"D:\USERDATA\pylib\")

Next, PCSE can be imported and the tests can be executed by calling
the `test()` function at the top of the package::

    >>> import pcse
    >>> pcse.test()
    runTest (pcse.tests.test_abioticdamage.Test_FROSTOL) ... ok
    runTest (pcse.tests.test_assimilation.Test_WOFOST_Assimilation) ... ok
    runTest (pcse.tests.test_partitioning.Test_DVS_Partitioning) ... ok
    runTest (pcse.tests.test_evapotranspiration.Test_PotentialEvapotranspiration) ... ok
    runTest (pcse.tests.test_evapotranspiration.Test_WaterLimitedEvapotranspiration1) ... ok
    runTest (pcse.tests.test_evapotranspiration.Test_WaterLimitedEvapotranspiration2) ... ok
    runTest (pcse.tests.test_respiration.Test_WOFOSTMaintenanceRespiration) ... ok
    runTest (pcse.tests.test_wofost.TestWaterlimitedPotato) ... ok
    runTest (pcse.tests.test_wofost.TestPotentialGrainMaize) ... ok
    runTest (pcse.tests.test_wofost.TestPotentialSpringBarley) ... ok
    runTest (pcse.tests.test_wofost.TestWaterlimitedWinterWheat) ... ok
    runTest (pcse.tests.test_wofost.TestPotentialSunflower) ... ok
    runTest (pcse.tests.test_wofost.TestWaterlimitedGrainMaize) ... ok
    runTest (pcse.tests.test_wofost.TestPotentialWinterRapeseed) ... ok
    runTest (pcse.tests.test_wofost.TestWaterlimitedWinterRapeseed) ... ok
    runTest (pcse.tests.test_wofost.TestPotentialPotato) ... ok
    runTest (pcse.tests.test_wofost.TestWaterlimitedSpringBarley) ... ok
    runTest (pcse.tests.test_wofost.TestWaterlimitedSunflower) ... ok
    runTest (pcse.tests.test_wofost.TestPotentialWinterWheat) ... ok

    ----------------------------------------------------------------------
    Ran 19 tests in 29.748s

    OK
    >>>

If the model output matches the expected output the test will report 'OK',
otherwise an error will be produced with a detailed traceback on where the
problem occurred.

An interactive PCSE/WOFOST session
==================================
The easiest way to demonstrate PCSE is to import WOFOST from PCSE and run it from
an interactive Python session. We will be using the built-in demo database which
contains meteorologic data, soil data and crop data for a grid location in South-Spain.

Initializing PCSE/WOFOST and advancing model state
--------------------------------------------------
Let's start a WOFOST object for modelling winter-wheat (crop=1) on a
location in South-Spain (grid 31031) for the year 2000 under water-limited
conditions for a freely draining soil (mode='wlp')::

    >>> wofost_object = pcse.start_wofost(grid=31031, crop=1, year=2000, mode='wlp')
    >>> type(wofost_object)
    <class 'pcse.models.Wofost71_WLP_FD'>

You have just successfully initialized a PCSE/WOFOST object in the python
interpreter, which is in its initial state and waiting to do some simulation. We
can now advance the model state for example with 1 day::

    >>> wofost_object.run()

Advancing the crop simulation with only 1 day, is often not so useful so the
number of days to simulate can be specified as well::

    >>> wofost_object.run(days=10)

Getting information about state and rate variables
--------------------------------------------------
Retrieving information about the calculated model states or rates 
can be done with the `get_variable()` method on a PCSE object.
For example, to retrieve the leaf area index value in the current
model state you can do::

    >>> wofost_object.get_variable('LAI')
    0.28708095263317146 
    >>> wofost_object.run(days=25)
    >>> wofost_object.get_variable('LAI')
    1.5281215808337203

Showing that after 11 days the LAI value is 0.287. When we increase time
with another 25 days, the LAI increases to 1.528. The `get_variable` method
can retrieve any state or rate variable that is defined somewhere in the
model. Finally, we can finish the crop season by simply specifying sufficient days
and store the results to a file 'myresults.csv'::

    >>> wofost_object.run(days=300)
    >>> wofost_object.store_to_file("myresults.txt")

Which should look like this :download:`myresults.txt`

Getting input data for PCSE/WOFOST
==================================

After running the examples you may be wondering where the data come
from that are used to run WOFOST. In fact, these data are retrieved from
an SQLite database `pcse.db` that is included with the source distribution
and can be found in the `pcse/db/pcse` folder.

For setting up PCSE/WOFOST with your
own data sources you should understand that WOFOST uses 5 different types of
inputs: `cropdata`, `soildata`, `timerdata`, `sitedata` and `driving variables`
(e.g. weather data). The fact that these names end with 'data' is a bit of
misnomer as they contain a mixture of parameter values, boundary conditions
and events rather than data, except for the driving variables which
can be considered as (observed) data. This terminology was inherited from the 
previous WOFOST versions and it was kept because changing it would
cause more confusion.

All the input `\*data` must be provided as python dictionaries
storing key/value pairs and several tools are available in the PCSE
distribution to read these from a file or a database. Moreover,
there are several tools available for reading weather data.

For the second example we will run a simulation for sugar beet in
Wageningen (Netherlands) and we will read the input data step by step from
several different sources instead of using the pre-configured `start_wofost()`
script. For the example we will assume that data files are in the directory
`D:\userdata\pcse_examples`. First we will import the necessary modules and
import set the data directory::

    >>> import os
    >>> import pcse
    >>> data_dir = r'D:\userdata\pcse_examples'

Cropdata
--------

Cropdata consist of parameter names (dictionary keys) and the
corresponding parameter values that are needed to parameterize the
components of the crop simulation model. These are
crop-specific values regarding phenology, assimilation, respiration,
biomass partitioning, etc. The parameter file for sugar beet can be
downloaded here: :download:`sug0601.crop` and is taken from the
crop files in the `WOFOST Control Centre`_.

.. _WOFOST Control Centre: http://www.wageningenur.nl/wofost

The crop parameter values for many models in
Wageningen are often provided in the CABO format that could be read
with the `TTUTIL <http://edepot.wur.nl/17847>`_ FORTRAN library. PCSE
tries to be backward compatible as much as possible and provides a
tool for reading parameter files in CABO format::

    >>> from pcse.fileinput import CABOFileReader
    >>> cropfile = os.path.join(data_dir, 'sug0601.crop')
    >>> cropdata = CABOFileReader(cropfile)
    >>> print cropdata

printing the cropdata dictionary gives you an listing of the header and
all parameters and their values.

Soildata
--------

The soildata dictionary must provide the parameter name/value pairs related
to the soil type and soil physical properties. The number of parameters is
variable depending on the soil water balance type that is used for the
simulation. For this example, we will use the water balance for freely
draining soils and use the soil file for medium fine sand: :download:`ec3.soil`.
This file is also taken from the soil files in the `WOFOST Control Centre`_ ::

    >>> soilfile = os.path.join(data_dir, 'ec3.soil')
    >>> soildata = CABOFileReader(soilfile)

Timerdata
---------

The timerdata dictionary provides the start date of the water balance,
the start date and type of the crop simulation, the end date and type of the crop
simulation and the maximum duration of the crop simulation. The latter is
included to avoid unrealistically long simulations for example as a results of
a too high temperature sum requirement. These values are used by the AgroManagement
unit of PCSE. Currently, there is only an AgroManagement unit for single cropping
seasons but will change in the future allowing for crop rotations. Therefore,
the approach for providing AgroManagement data (timerdata) will change.

The following list gives an overview of the parameter names, values and types that
need to be specified in the `timerdata` dictionary::

        CAMPAIGNYEAR: year of the agricultural campaign (e.g. harvest year)
          START_DATE: date of the start of the simulation
            END_DATE: date last possible day of the simulation
     CROP_START_TYPE: 'emergence' or 'sowing'
     CROP_START_DATE: date of the start of the crop simulation
       CROP_END_TYPE: 'maturity' | 'harvest' |'earliest'
       CROP_END_DATE: date of the end of the crop simulation in case of CROP_END_TYPE == 'harvest' | 'earliest'
        MAX_DURATION: maximum number of days of the crop simulation

The CABO format has no support for dates, therefore the PCSE file format was
developed that does allow to use dates. The crop calendar file for sugar beet
in Wageningen can be downloaded here: :download:`sugarbeet_calendar.pcse`::

    >>> from pcse.fileinput import PCSEFileReader
    >>> crop_calendar_file = os.path.join(data_dir, 'sugarbeet_calendar.pcse')
    >>> timerdata = PCSEFileReader(crop_calendar_file)
    >>> print timerdata
    PCSE parameter file contents loaded from:
    /home/allard/Sources/python/pcse/doc/sugarbeet_calendar.pcse

    CAMPAIGNYEAR: 2000 (<type 'int'>)
    CROP_START_DATE: 2000-04-05 (<type 'datetime.date'>)
    END_DATE: 2000-12-31 (<type 'datetime.date'>)
    MAX_DURATION: 300 (<type 'int'>)
    CROP_END_DATE: 2000-10-20 (<type 'datetime.date'>)
    CROP_START_TYPE: emergence (<type 'str'>)
    CROP_END_TYPE: harvest (<type 'str'>)
    START_DATE: 2000-01-01 (<type 'datetime.date'>)

Sitedata
--------

The sitedata dictionary provides ancillary parameters that are not related to
the crop, the soil or the agromanagement. Examples are the initial conditions of
the water balance such as the initial soil moisture content (WAV) and
the initial and maximum surface storage (SSI, SSMAX). For the moment, we will
define these parameters directly on the python commandline::

    >>> sitedata = {'SSMAX'  : 0.,
                    'IFUNRN' : 0,
                    'NOTINF' : 0,
                    'SSI'    : 0,
                    'WAV'    : 100,
                    'SMLIM'  : 0.03}

Driving variables (weather data)
--------------------------------

Daily weather variables are needed for running the simulation, see the section
on :ref:`DrivingVar` for reference. Currently, three options are available in
PCSE for storing and retrieving weather data:

    1. The database structure as provided by the Crop Growth Monitoring
       System. Weather data will be read from the GRID_WEATHER table which
       is implemented using `pcse.db.pcse.GridWeatherDataProvider`.
    2. The file structure as defined by the `CABO Weather System`_ which is
       implemented using `pcse.fileinput.CABOWeatherDataProvider`. For more
       details see :ref:`TheCABOtools`.
    3. The global weather data provided by the agroclimatology from the
       `NASA Power database`_ at a resolution of 1x1 degree. PCSE
       provides the `pcse.db.NASAPowerWeatherDataProvider' which retrieves
       the NASA Power data from the internet for a given latitude and
       longitude.

.. _CABO Weather System: http://edepot.wur.nl/43010
.. _NASA Power database: http://power.larc.nasa.gov

For this example we will use the weather data from the NASA Power database
for the location of Wageningen. Note that it can take around 30 seconds
to retrieve the weather data from the NASA Power server the first time::

    >>> from pcse.db import NASAPowerWeatherDataProvider
    >>> wdp = NASAPowerWeatherDataProvider(latitude=52, longitude=5)
    >>> print wdp
    Weather data provided by: NASAPowerWeatherDataProvider
    --------Description---------
    NASA/POWER Agroclimatology Daily Averaged Data
    Dates (month/day/year): 01/01/1984 through 05/10/2014
    Location: Latitude 52   Longitude 5
    Location clarification: Integer values may indicate the lower left (south and west)
    corner of the one degree lat/lon region that includes the requested locations
    Elevation (meters): Average for one degree lat/lon region = 5
    Methodology Documentation:
    *Vegetation type: "Airport": flat rough grass
    ----Site characteristics----
    Elevation:    5.0
    Latitude:  52.000
    Longitude:  5.000
    Data available for 1997-01-01 - 2014-01-31
    Number of missing days: 47

Importing, initializing and running a PCSE model
------------------------------------------------

Internally, PCSE uses a simulation `engine` to run a crop simulation. This
engine takes a configuration file that specifies the components for the crop,
the soil and the agromanagement that need to be used for the simulation.
So any PCSE model can be started by importing the `engine` and initializing
it with a given configuration file and the corresponding sitedata, cropdata,
soildata, timerdata and weather data.

However, as many users of PCSE only need a particular configuration (for
example the WOFOST model for potential production), preconfigured Engines
are provided in `pcse.models`. For the sugarbeet example we will import
the WOFOST model for water-limited simulation under freely draining soils::

    >>> from pcse.models import Wofost71_WLP_FD
    >>> Wofost71_WLP_FD
    pcse.models.Wofost71_WLP_FD
