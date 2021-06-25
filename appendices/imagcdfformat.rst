IMAGCDFV1.2 INTERMAGNET EXCHANGE FORMAT
=======================================

.. include:: ../shared/variables.rst


This document describes how NASA's Common Data Format ( CDF - |cdf_format| )
will be used to store geomagnetic data.
This format is called ImagCDF. Version 1.2 of ImagCDF is described
here.

.. rubric:: Design details and CDF concepts
    :name: design-details-and-cdf-concepts

.. rubric:: General design details
    :name: general-design-details

Geomagnetic data is held in CDF variables, one variable per
geomagnetic element. Additional variables hold time stamp data,
one time stamp variable being used for the timing of the vector
magnetometer data, a second variable for the scalar magnetometer
data, since data from these two instruments may be recorded at
different sample rates. Each variable has one or more records, an
individual record holding a single sample value. The index numbers
of the records in the vector geomagnetic variable correspond with
the index numbers in the vector time stamp variable's records,
likewise for the scalar data. A typical file might contain 6
variables: 3 geomagnetic elements (such as HDZ or XYZ); one scalar
element (F); and two timing variables, one for the vector data,
the other for the scalar. The format does not mandate that these
variables are present – there may be fewer (for example only
scalar data) or more (for example additional temperature data).

Metadata is held in CDF attributes in two ways: global attribute
entries concern all the data in a file; variable attributes have
entries that concern a single variable (e.g. the 'H' variable). An
entry holds an individual item of metadata. An attribute name
(whether global or variable) must be unique, so when an attribute
needs to be used more than once (e.g. the element type for a
geomagnetic variable must be used once for each geomagnetic
element), then multiple entries are created in a single attribute.
Global attributes in ImagCDF will have only one entry (with two
exceptions). Variable attributes may have an entry for each of the
relevant variables in the file. For variable attributes, the
multiple entries are indexed using the variable's numeric
identifier, so that the metadata 'belongs' to the variable. For
example, in an ImagCDF file holding HDZF data, the attribute Units
will have four entries (in this order): "nT"; "Degrees of arc";
"nT"; "nT".

.. rubric:: Data types used for variables and attributes
    :name: data-types-used-for-variables-and-attributes

.. rubric:: Real Numbers
    :name: real-numbers

ImagCDF uses double precision CDF_DOUBLE (8-byte) floating point
numbers (to the IEEE 754 standard) for all numeric values. These
numbers provide about 14.5 (decimal) digits of precision -
|floating_point_arithmetic|.
In order to use floating point numbers successfully, the dynamic
range of the quantity being represented (ratio of smallest to
largest value) must be smaller than 14.5 digits. For geomagnetic
field strengths, assume that the ratio of smallest difference to
largest value that we need to represent in geomagnetic field data
is 0.1pT in 80,000nT. This equates to 1 part in 80,000 x 1,000 x
10, or 1 part in 800,000,000, or 9 digits of precision, so is
within the dynamic range available.

.. rubric:: Dates / times
 :name: dates-times

All date / time values in ImagCDF are held as CDF TT2000 dates,
which are based on 8-byte integers. TT2000 uses an epoch (midday
on 1st January 2000) to store dates and times, has a precision of
1 nanosecond which gives a range in excess of ±280 years from the
epoch date. The TT2000 type can correctly handle leap seconds.

.. rubric:: Strings
    :name: strings

Text data is held using CDF_CHAR variables in ImagCDF.

.. rubric:: Compression
    :name: compression

The CDF library allows the user to specify whether all, part or
none of a CDF file is compressed at the time it is written. Once
this choice has been made, the CDF library handles compression and
decompression of the data in the file automatically (reading and
any further writing to the file will decompress or compress as
required). Choosing whether or not to compress a file is simply a
matter of specifying which (if any) compression method to use when
the file is created. ImagCDF allows any of the compressions
provided by CDF.

Significant compression is achieved using the CDF compression
option. A day file of four element minute data can occupy under
15Kb.

.. rubric:: Where to obtain the CDF software
      :name: where-to-obtain-the-cdf-software

Before you can use any of NASA or INTERMAGNET's tools for working
with ImagCDF, you will need to download and install the CDF
software from NASA: |cdf_software| . Software that has
been written to work with CDF is likely to need the libraries that
are installed. For details of other software that may be useful
see section `Tools to look at CDF data (todo)`__.

.. rubric:: ImagCDF data
    :name: imagcdf-data

All variables holding geomagnetic data have the following
features:

- Units used must be nT for geomagnetic field values, degrees for
  angles or celsius for temperatures.
- Lengths of time series are arbitrary (e.g. a file may be used
  to store an entire day of data or a small fragment of a day
  down to a single sample).

Geomagnetic data is held in variables called GeomagneticField <E>
where <E> represents the code for the geomagnetic element recorded
- see section `Valid codes for elements recorded (todo)`__
for a list of valid codes. The variable has 0 dimensions,
each consecutive record holding individual consecutive data samples in CDF double data type,
starting at record 1. Missing data values are represented by a
data sample that contains the same number as is present in the
FILLVAL metadata attribute. The FILLVAL must exceed any valid
geomagnetic field strength or angle. Typically this would be
99999.0. The variable attribute FIELDNAM must be set to
"Geomagnetic Field Element <E>".

An ImagCDF file must include a set of geomagnetic field variables
that describe the vector field in a recognised orientation (such
as 'HDZ', 'XYZ' or 'DIF). All vector variables must have the same
number of records. An ImagCDF file may also include an additional
field element from an independent scalar instrument, with an
element code of 'G' or 'S'.

Temperature data is optional (unless otherwise stated in the data
standard that the data conforms to, for example, temperature is
mandatory for data that conforms to the INTERMAGNET 1-second data
standard). The first temperature variable is called Temperature1,
then Temperature2 and so on. The FIELDNAM attribute describes, in
free text, for each temperature variable, the location at which
the temperature was recorded.

Time stamps for the data are held in separate variables. Time
stamps must represent a regular time series with no missing values
in the series. Time stamps for all the geomagnetic field vector
variables are held in a single variable called
GeomagneticVectorTimes, for the scalar data in a variable called
GeomagneticScalarTimes. Time stamps for temperature data are held
in variables called Temperature1Times, Temperature2Times and so
on. These variables have 0 dimensions and must have the same
number of records as the data variables that they apply to. Each
record in a time stamp variable holds a CDF TT2000 epoch time.
Time stamps must always refer to the start of each sample period
(e.g. for minute data, the seconds and milliseconds will always be
set to zero).

Recommended names for time stamp variables are:

.. tabularcolumns:: |p{6cm}|p{8.5cm}|

.. table::
    :widths: auto
    :align: center

    +----------------------------------+----------------------------------+
    | Situation                        | Names                            |
    +==================================+==================================+
    | The same time stamps can be used |  DataTimes                       |
    | for all data in the file (i.e.   |                                  |
    | there is a single time stamp     |                                  |
    | variable in the file)            |                                  |
    +----------------------------------+----------------------------------+
    | Different time stamps for        | *GeomagneticVectorTimes,         |
    | vector, scalar and temperature   | GeomagneticScalarTimes,          |
    |                                  | Temperature1Times,               |
    |                                  | Temperature2Times, …*            |
    +----------------------------------+----------------------------------+

Additional variables and metadata may be carried in an ImagCDF
(e.g. it may be convenient to include meteorological data in the
same file). The format of these variables and metadata is left to
the user to define. The CDF system means that these variables can
be included without causing problems to software reading ImagCDF
files. Software writing ImagCDF files should preserve additional
variables and attributes read from an ImagCDF file.

.. rubric:: ImagCDF "global" attributes
    :name: imagcdf-global-attributes

The following attributes apply to all the data in an ImagCDF file.
The "Entries" column shows whether the attribute has:

- A single mandatory entry (number of entries is exactly 1)
- A single optional entry (number of entries may be 0 or 1)
- Multiple mandatory entries (number of entries is between 1 and
  N\ :sup:`1`)
- Optional mandatory entries (number of entries is between 1 and
  N\ :sup:`2`)

Superscript numbers following the attribute name show:

#. That the attribute is a recommended attribute for use with
   NASA's CDF tools
#. That the attribute is part of the ISTP/IACG guidelines -
   |istp_guide|

.. rubric:: Attributes that describe the data format
   :name: index_data_format_1

These are 'constant' values that will be the same for all ImagCDF
files. They allow 'generic' CDF programs to understand and process
the data correctly.

.. tabularcolumns:: |p{3cm}|p{1.5cm}|>{\centering\arraybackslash}p{1.5cm}|p{7cm}|

.. table::
    :widths: auto
    :align: center

    ================= ====== ======= ================================================
    Attribute Name    Type   Entries Description
    ================= ====== ======= ================================================
    FormatDescription String 1       Always set to "INTERMAGNET CDF Format"
    FormatVersion     String 1       Set to the current version of the format – "1.2"
    Title1            String 1       Always set to "Geomagnetic time series data"
    ================= ====== ======= ================================================

.. rubric:: Attributes that uniquely identify the data
     :name: item_attributes_uniquely_id_data

The attributes in this section are sufficient, along with the
start date and duration of the time series, to uniquely identify a
piece of geomagnetic data.

.. tabularcolumns:: |p{3cm}|p{1.5cm}|>{\centering\arraybackslash}p{1.5cm}|p{8cm}|

.. table::
    :widths: auto
    :align: center

    +-----------------------+------------+------------+------------------------------------------------+
    | Attribute Name        | Type       | Entries    | Description                                    |
    +=======================+============+============+================================================+
    | IagaCode              | String     | 1          | The IAGA code for the observatory              |
    +-----------------------+------------+------------+------------------------------------------------+
    | ElementsRecorded      | String     | 1          | A string consisting of single character codes, |
    |                       |            |            | each describing one of the geomagnetic field   |
    |                       |            |            | elements that is recorded in this data file.   |
    |                       |            |            | This might typically be a three or four digit  |
    |                       |            |            | code such as HDZ, XYZG or DIFG. The codes in   |
    |                       |            |            | this attribute determine the names of the data |
    |                       |            |            | variables (see the section on geomagnetic      |
    |                       |            |            | data). Valid codes are defined in section      |
    |                       |            |            | `Valid codes for elements                      |
    |                       |            |            | recorded (todo)`__                             |
    +-----------------------+------------+------------+------------------------------------------------+
    | PublicationLevel      | String     | 1          | Choose one of the following codes to describe  |
    |                       |            |            | the level that the data has been processed to: |
    |                       |            |            |                                                |
    |                       |            |            | -  *1*: The data is unprocessed and as         |
    |                       |            |            |    recorded at the observatory with no changes |
    |                       |            |            |    made.                                       |
    |                       |            |            | -  *2*: Some edits have been made such as gap  |
    |                       |            |            |    filling and spike removal and possibly a    |
    |                       |            |            |    preliminary baseline added.                 |
    |                       |            |            | -  *3*: The data is at the level required for  |
    |                       |            |            |    production of an initial bulletin or for    |
    |                       |            |            |    quasi-definitive publication.               |
    |                       |            |            | -  *4*: The data has been finalised and no     |
    |                       |            |            |    further changes are intended                |
    |                       |            |            |                                                |
    |                       |            |            | Only these values are allowed.                 |
    |                       |            |            |                                                |
    |                       |            |            | This field provides a quick description of the |
    |                       |            |            | point the data has reached in the publication  |
    |                       |            |            | process. For detailed information on the       |
    |                       |            |            | standards that the data conforms to see        |
    |                       |            |            | section `Attributes that relate to data_       |
    |                       |            |            | standards and                                  |
    |                       |            |            | quality (todo)`__.                             |
    +-----------------------+------------+------------+------------------------------------------------+
    | PublicationDate       | Date/time  | 1          | Date and time on which the data was published. |
    |                       |            |            | This attribute is used to distinguish multiple |
    |                       |            |            | publications of the same data.                 |
    +-----------------------+------------+------------+------------------------------------------------+

.. rubric:: Attributes that describe the observatory
     :name: item_attributes_describe_observatory

These attributes are available from other metadata systems (given
an IAGA code), but are included for convenience of the user.

.. tabularcolumns:: |p{3cm}|p{1.5cm}|>{\centering\arraybackslash}p{1.5cm}|p{8cm}|

.. table::
    :widths: auto
    :align: center

    +------------------+--------+-----------+-------------------------------------------------------+
    | Attribute Name   | Type   | Entries   | Description                                           |
    +==================+========+===========+=======================================================+
    | ObservatoryName  | String | 1         | The full name for the observatory                     |
    +------------------+--------+-----------+-------------------------------------------------------+
    | Latitude         | Double | 1         | The latitude of the observing position in degrees, to |
    |                  |        |           | WGS-84 datum                                          |
    +------------------+--------+-----------+-------------------------------------------------------+
    | Longitude        | Double | 1         | The longitude of the observing position in degrees,   |
    |                  |        |           | to WGS-84 datum                                       |
    +------------------+--------+-----------+-------------------------------------------------------+
    | Elevation        | Double | 1         | The height of the observing position in metres above  |
    |                  |        |           | sea level, to WGS-84 datum. Set to 99999.0 if not     |
    |                  |        |           | known.                                                |
    +------------------+--------+-----------+-------------------------------------------------------+
    | Institution      | String | 1 or more | The name of the responsible institute.                |
    +------------------+--------+-----------+-------------------------------------------------------+
    | VectorSensOrient | String | 0 - 1     | The orientation code of the sensor at the original    |
    |                  |        |           | recording of the vector data. A string consisting of  |
    |                  |        |           | single character codes, each describing one of the    |
    |                  |        |           | geomagnetic field elements that was recorded by the   |
    |                  |        |           | vector instrument. Valid codes are the same as for    |
    |                  |        |           | the ElementsRecorded attribute.                       |
    +------------------+--------+-----------+-------------------------------------------------------+

.. rubric:: Attributes that relate to data standards and quality
    :name: item_attributes_data_standards_and_quality

These attributes describe the standards, if any, that the data
meets.


.. tabularcolumns:: |p{3cm}|p{1.5cm}|>{\centering\arraybackslash}p{1.5cm}|p{8cm}|

.. table::
    :widths: auto
    :align: center

    +-------------------------+-----------+---------+------------------------------------------------+
    | Attribute Name          | Type      | Entries | Description                                    |
    +=========================+===========+=========+================================================+
    | StandardLevel           | String    | 1       | Describe whether the data conforms to a        |
    |                         |           |         | standard. Choose from one of the following     |
    |                         |           |         | codes:                                         |
    |                         |           |         |                                                |
    |                         |           |         | -  *None*: The data does not conform to any    |
    |                         |           |         |    standards. When using this, the             |
    |                         |           |         |    StandardName attribute does not need to be  |
    |                         |           |         |    set.                                        |
    |                         |           |         | -  *Partial*: The data partially conforms to   |
    |                         |           |         |    the relevant standard for this data         |
    |                         |           |         |    product.                                    |
    |                         |           |         | -  *Full*: The data fully conforms to the      |
    |                         |           |         |    relevant standard for this data product.    |
    |                         |           |         |                                                |
    |                         |           |         | Only these values are allowed.                 |
    |                         |           |         |                                                |
    |                         |           |         | If *StandardsLevel* is set to *Partial*, then  |
    |                         |           |         | the *PartialStandDesc* attribute must also be  |
    |                         |           |         | set.                                           |
    +-------------------------+-----------+---------+------------------------------------------------+
    | StandardName            | String    | 0 - 1   | The name of the relevant standard. See section |
    |                         |           |         | `Relevant data                                 |
    |                         |           |         | standards(todo)`__                             |
    |                         |           |         | for a description of how to use this           |
    |                         |           |         | attribute.                                     |
    +-------------------------+-----------+---------+------------------------------------------------+
    | StandardVersion         | String    | 0 – 1   | If the standard has a version, put its version |
    |                         |           |         | number in this attribute.                      |
    +-------------------------+-----------+---------+------------------------------------------------+
    | PartialStandDesc        | String    | 0 - 1   | See section `Relevant data                     |
    |                         |           |         | standards (todo)`__                            |
    |                         |           |         | for a description of how to use this           |
    |                         |           |         | attribute.                                     |
    +-------------------------+-----------+---------+------------------------------------------------+

.. rubric:: Attributes that relate to publication of the data
   :name: attributes-that-relate-to-publication-of-the-data

These attributes are needed when that data is published.

.. tabularcolumns:: |p{3cm}|p{2cm}|>{\centering\arraybackslash}p{1.5cm}|p{8cm}|

.. table::
    :widths: auto
    :align: center

    +-------------------+------------+-----------+--------------------------------------------------+
    | Attribute Name    | Type       | Entries   | Description                                      |
    +-------------------+------------+-----------+--------------------------------------------------+
    | Source            | String     | 1         | Set to one of: "institute" (if the named         |
    |                   |            |           | institution provided the data- see section       |
    |                   |            |           | `Attributes that describe the                    |
    |                   |            |           | observ                                           |
    |                   |            |           | atory (todo)`__                                  |
    |                   |            |           | for the institution); "INTERMAGNET" (if the data |
    |                   |            |           | file has been created by INTERMAGNET from        |
    |                   |            |           | another data source); "WDC" (if the World Data   |
    |                   |            |           | Centre has created the file from another data    |
    |                   |            |           | source)                                          |
    +-------------------+------------+-----------+--------------------------------------------------+
    | TermsOfUse        | String     | 0 – 1     | The terms of use for the data. This could be     |
    |                   |            |           | text describing the terms of use or a link to a  |
    |                   |            |           | web page. INTERMAGNET has a recommended wording  |
    |                   |            |           | for data provided through INTERMAGNET.           |
    +-------------------+------------+-----------+--------------------------------------------------+
    | UniqueIdentifier  | String     | 0 – 1     | A string that can be used to uniquely identify   |
    |                   |            |           | this data. This could be a Digital object        |
    |                   |            |           | identifier or could be an identifier created     |
    |                   |            |           | according to local rules. Note this is optional  |
    |                   |            |           | and will not be present if this data is not      |
    |                   |            |           | covered by an identifier.                        |
    +-------------------+------------+-----------+--------------------------------------------------+
    | ParentIdentifiers | String     | 0 or more | The unique identifiers of the parent data sets   |
    |                   |            |           | (if any), one identifier per entry. The parent   |
    |                   |            |           | data set is the data set from which this data    |
    |                   |            |           | set's values have been derived. E.g. if a one    |
    |                   |            |           | minute data set has been created from a one      |
    |                   |            |           | second data set, the one second data set is the  |
    |                   |            |           | parent. Another example is where definitive data |
    |                   |            |           | is created from provisional data.                |
    +-------------------+------------+-----------+--------------------------------------------------+
    | ReferenceLinks    | String/URL | 0 or more | URLs pointing to (e.g.) information about the    |
    |                   |            |           | data creator, information about the data         |
    |                   |            |           | repository… One URL per entry.                   |
    +-------------------+------------+-----------+--------------------------------------------------+

.. rubric:: Valid codes for elements recorded
     :name: item_valid_codes

- 'X', 'Y', or 'Z' indicate that the variable holds the strength
  of the magnetic field vector in the standard geographic
  coordinates in nT.
- 'H' indicates that the variable holds the strength of the
  magnetic field vector in the horizontal plane along the
  magnetic meridian in nT.
- 'D' indicates that the variable holds the angle between the
  magnetic vector and true north, in degrees of arc, positive
  east.
- 'E' indicates that the variable holds a field strength in the
  horizontal plane perpendicular to 'H' in nT. 'E' is only valid
  for data that is not baseline corrected.
- 'V' indicates that the variable holds the field strength along
  the direction of the inclination.
- 'I' indicates that the variable holds the angle between the
  magnetic vector and the horizontal plane, in degrees of arc,
  positive below the horizontal.
- 'F' indicates that the variable holds the geomagnetic field
  strength in nT, calculated from and consistent with XYZ or HDZ
  field elements.
- 'S' indicates that the variable holds the geomagnetic field
  strength in nT, measured by an independent scalar instrument
- 'G' indicates that the variable holds delta- F values, defined
  as F(vector) –S(scalar) in nT. When calculating values for the
  G element, if F(vector) is missing, G is set to –S (scalar)

Other codes are allowed, but may lead to data not being
understood.

.. rubric:: Relevant data standards
   :name: relevant-data-standards

Different geomagnetic data products have different standards
associated with them. This table shows what standards are being
referred to in the StandardsLevel attribute and describes what to
put into the StandardName attribute in the case where
StandardsLevel is set to Partial or Full. The table also shows
what to put in the PartialStandDesc attribute in the case where
the StandardsLevel attribute is set to Partial.