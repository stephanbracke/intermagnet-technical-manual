.. _use_dat_ftp:

FTP Server
==========

.. include:: ../../shared/variables.rst

The INTERMAGNET FTP offers the ability to download large
amounts of data without going through a step-by-step web
interface. Amongst many other advantages, the user can design
applications/scripts that request data directly from the FTP.

URL: |imag_ftp|

The INTERMAGNET FTP server hosts the following:

-  IAGA2002 and CDF minute variation
-  IAGA2002 and CDF second variation
-  IAGA2002 and CDF minute provisional
-  IAGA2002 and CDF second provisional
-  IAGA2002 and CDF minute quasi-definitive
-  IAGA2002 and CDF second quasi-definitive
-  IAGA2002, IAF and CDF minute definitive

And hosts the following publicly:

-  Yearly definitive data and metadata
-  Weekly data download statistics

The data can be found under the *intermagnet* directory and is
structured as follows::

    └── sample period
        └── data type
            └── data format


Where:

- sampling period = minute or second
- data type = variation, provisional, quasi-definitive, or
  definitive
- data format = IAGA2002, IAF, or CDF

Weekly download statistics are a JSON file located under the
*intermagnet stats* directory. The filename is in the form:

-  intermagnet.YYYYMMDD.json

Where:

-  YYYY = 4 digit year
-  MM = 2 digit month
-  DD = 2 digit day

The timestamp is the date when the logs are generated;
therefore, they represent the end of the weekly period. It
contains a series of counters per observatory, sampling period,
data types, and data format (keys of the objects).


