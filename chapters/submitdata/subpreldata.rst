.. _sub_dat_prel_data:

Submission Of Preliminary Data
==============================

.. include:: ../../shared/variables.rst


An observatory sends preliminary (non-definitive) data to its
assigned GIN via e-mail, web upload or satellite transmission.
INTERMAGNET encourages observatories to send data as close to
real-time as possible. The best way to achieve this is using
the web interface to submit data in IAGA-2002 format.
Observatories are also encouraged to make regular submissions
of quasi-definitive data using the same methods for submitting
data.

.. _sub_dat_prel_upf:

Update Frequencies And Formats For Preliminary Data
---------------------------------------------------

GINs can receive data in IAGA-2002 and IMFV1.23 formats. These
formats are fully defined and described in Appendix E-3 and
Appendix E-5. GINs also support IMFV2.83 format, but only where
an observatory is transmitting via satellite. IMFV2.83 is
defined in Appendix E-1. Preliminary data cannot be accepted by
GINs in other formats (expect where a GIN makes arrangements to
receive data from its own observatories in a private format –
in these cases the data must be converted to IAGA-2002 before
forwarding to the INTERMAGNET web site).

GINs can receive ‘fragments’ of a day file – that is day files
that are incomplete – in any of these formats and recombine the
fragments into a day file in IAGA-2002 format. In this way
observatories can send multiple short data files that contain
only the data from the end of the previous transmission up to
what has been most recently recorded. This allows an
observatory to update the INTERMAGNET web site in near
real-time for the current day as the day progresses.

GINs will overwrite data if they receive data that has
previously been sent. This allows observatories to send data in
close to real-time, then correct that data at a later date
(e.g. after manual gap filling or spike removal).

IAGA-2002 is the preferred format for observatories to send
data to INTERMAGNET. The format is flexible enough to allow
very small fragments of day files to be transmitted easily,
whereas IMFV1.23 format requires a minimum of 1-hour of data to
be encoded in a file (though where a fragment is smaller than
an hour, the remainder of the 1-hour block could be coded as
missing values). Observatories sending data in IMFV1.23 should
be aware that the GIN will convert their data to IAGA-2002
format. INTERMAGNET intends to stop supporting the IMFV1.23
format at some point in the future. The smallest fragment of
data that should be encoded into an IAGA-2002 file is 2
samples, since some software using this format calculates the
sample rate from time difference between the first pair of
samples in the file.

There are a number of ways that observatories can combine the
different transmission methods that INTERMAGNET offers to
achieve reliable throughput of data in near real-time. Two of
the most successful have been:

#. To use the web interface to submit small data fragments to
   the GIN. The web interface returns a code indicating
   successful upload of data. If this code is not received, the
   observatory can flag that this data was not uploaded and
   attempt to upload it again (along with any new data) on a
   subsequent upload.
#. It is not always easy to keep track of whether data has been
   successfully uploaded (and it is likely to be only a small
   number of occasions where there is a problem). An
   alternative is to submit regular small fragments of a day
   file throughout the day via the web interface, without
   tracking their successful upload, then submit a complete day
   file via email shortly after the day has ended, which will
   have the effect of filling any gaps that occurred from
   unsuccessful transmission via the web interface.

Other combinations of data transmission are possible if they
suit and observatory’s working practice. As at 2016, the
fastest rate at which data is being sent using these techniques
is every 3 minutes (i.e. files of 1-minute data with 3 samples
per file).

.. _sub_dat_prel_types:

Types Of Preliminary Data
-------------------------

In the past INTERMAGNET has collected Reported (or Variometer)
data - preliminary data without a baseline. New observatories
are not allowed to send data of this type and existing
INTERMAGNET observatories are encouraged to stop sending this
data and instead send baseline corrected data using the
Adjusted (or Provisional) data type.

Observatories should send Adjusted (or Provisional ) data as
close to real-time as possible.

Observatories are encouraged to send quasi-definitive data to
the GIN as well as adjusted data, unless they are producing
quasi-definitive data in near real-time, in which case it is
sufficient to just send quasi-definitive data. Quasi-definitive
data need not be produced in near real-time – some methods of
producing this type of data do not allow for near real-time
production. However where possible observatories are encouraged
to send quasi-definitive data in near real-time.

.. _sub_dat_prel_ws:

Submission Via Web Service
--------------------------

Data can be submitted to INTERMAGNET GINs using a web service.
The web was chosen as a delivery medium because it provides the
best chance of avoiding issues with firewalls between two
institutes. The http: ‘file upload’ mechanism is used, as
described in RFC1867 "Form-based File Upload in HTML" - see
|rfc1867|. The web service responsible for receiving data
from INTERMAGNET observatories works as follows:

#. Observatories upload their data to the web service.
#. The web service indexes and caches the data, making it
   available for download by the GIN.
#. The web service provides a search mechanism by which a GIN
   can find out what data is in the cache.
#. The web service provides a download form which allows the
   GIN to download data files that observatories have uploaded
   to the cache.
#. The responses which the web service makes to these actions
   are standardised, making it easy to automate the processes
   of uploading and downloading data.

In this way, issues with firewalls are avoided, since both
observatories and the GIN access the web service using a
standard http: connection.

Observatories connect to the web service in one of three ways:

#. Manually using a standard web browser. Once connected the
   file upload is very simple, similar to the method used by
   online mail services to upload files as attachments when
   sending mail.
#. Automatically using a command line based tool that supports
   file upload. One such tool is CURL |curl| .
   which is available for SUN Solaris, Linux and Windows.
#. Automatically using their own software. File upload to a web
   server is supported by Java and other languages.

The web service will accept data in the following formats:

#. One-second data in IAGA 2002 format.
#. One-minute data in IAGA 2002 format.
#. One-minute data in IMF V1.23.

Data files may contain a maximum of 24 hours data. There is no
minimum quantity of data, but an IMO may not make more than
1440 uploads per day. The same file may not be uploaded more
than once. These limits are imposed to help prevent Denial Of
Service (DOS) attacks.

The first operation that the web service performs when a file
is uploaded is to parse the file to check that the file name
and contents are correct and that the file conforms to the
maximum data size and maximum number of file upload rules. Once
a file has been verified it is assigned a unique ID number
(implemented as a 64-bit integer to ensure it cannot ‘run
out’). The ID number increments for each new file uploaded. The
ID number never gets smaller. ID numbers can be used to sort
files by their time of arrival on the server.

The web service will respond to an attempt to upload a file
using a formatted response in either HTML or plain text (the
format is under control of the user). The HTML response is
designed to be easy for an interactive user to understand, the
plain text response is designed to be easy to process from
software. The first line of the response indicates whether the
attempt was successful. It will contain one of the following
words:

.. table::
    :widths: auto
    :align: center

    +-------------+-------------------------------------------------------+
    | Word        | Description                                           |
    +=============+=======================================================+
    | Success     | The operation succeeded.                              |
    +-------------+-------------------------------------------------------+
    | Information | The operation succeeded and there is some additional  |
    |             | information.                                          |
    +-------------+-------------------------------------------------------+
    | Warning     | The operations succeeded, but there was a problem     |
    |             | with a peripheral part of the system.                 |
    +-------------+-------------------------------------------------------+
    | Error       | The operation failed.                                 |
    +-------------+-------------------------------------------------------+
    | Exception   | The operation failed with an unforeseen problem. A    |
    |             | full description of the software fault is included.   |
    +-------------+-------------------------------------------------------+
    | Fatal       | The entire web service is not working.                |
    +-------------+-------------------------------------------------------+

The second line of the response is a message further describing
what happened. The response may include subsequent lines with
further detail.

On successful upload, the following message will be returned:

Success

Data loaded OK, data file id = <ID>

Capturing the ID of the transaction from the response allows
you to confirm that the upload was successful and to download
and view the file that you uploaded at a later date. Note that
successful delivery of data to the web service does not mean
that the data has been loaded to the GIN. The GIN’s operating
software needs to download the data from the web service and
insert it into the GIN data store. This may take a few seconds
to minutes following the upload. In exceptional circumstances a
file may be successfully loaded to the web service, but fail to
load on the GIN – these occurrences are very rare.

An example of the web service (as implemented at the Edinburgh
GIN) can be seen here:

|gin_edin_up|

Detailed descriptions of the interface to the web service along
with some example software to upload data to the web service
are available here:

|gin_edin_up_help|

Note that this web site is password protected – for access
apply to the GIN manager.

For more detailed information see INTERMAGNET Technical Note 9:

|tn_9|

.. _sub_dat_prel_email:

Submission Via Email
--------------------

All GINs support receipt of data by email. An entire day of
data may be sent by email, or a smaller ‘fragment’ of data may
be sent (an hour, or even a few minutes) if an observatory is
sending updates more than once a day. In this case, the GIN is
responsible for reassembling the data fragments into a day
file.

Data may be sent by email in the following formats:

#. One-second data in IAGA 2002 format.
#. One-minute data in IAGA 2002 format.
#. One-minute data in IMF V1.23.

Whatever format is used, data may be submitted in one of two
ways:

#. The data may be put directly into the body of the email. In
   this case the ‘subject’ field for the mail message must
   contain the filename for the data, formatted according to
   the filename rules for the data format used. So, for
   example, if data is sent in the IMFV1.23 format for Boulder
   observatory on 15th March 1992, the mail subject line would
   be:

   Subject: MAR1592.BOU

   For 1-minute provisional Canberra data on 1st January 2002
   in IAGA2002 format the subject line would be:

   Subject: bou20020101pmin.min

   The body of the mail message should contain the data,
   formatted according to the rules of the format used. The
   body of the message should not contain anything else (e.g.
   automatic signatures) - there must be no additional text
   before or after the data. The data must be in ‘plain text’
   with no formatting. The requirement for plain text
   transmission is becoming increasingly problematic, as
   modern email clients don’t make it easy to guarantee that
   email is not formatted using HTML or Rich Text. If the
   email client you use makes it difficult to guarantee
   sending plain text emails, consider sending the data as an
   attachment instead.

#. The data may be attached to a mail message. The data must be
   contained in a file, correctly formatted for the chosen data
   format and attached to the mail message using the MIME
   (Multipurpose Internet Mail Extensions) standard. The
   attached file must be named according to the filename rules
   for the chosen data format. When data is attached to an
   email, the subject field and body of the mail message are
   ignored.

A list of GIN Internet addresses can be found in :numref:`sub_dat_intro_gin_addr`

.. _sub_dat_prel_sat:

Satellite Transmission
----------------------


