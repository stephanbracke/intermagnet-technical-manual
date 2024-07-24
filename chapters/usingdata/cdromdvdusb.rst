.. _use_dat_cd:

INTERMAGNET Physical Media
==========================

.. include:: ../../shared/variables.rst

.. include:: ../../appendices/appendices.rst

[Under development] Convert to legacy data access???
Various types of data can be downloaded from the web site as
described in :numref:`use_dat_imag_web_down_data`.
This also applies to minute values with definitive status.
Note that the most recent definitive data are always available on the web site.
Downloading data from the web site is a good alternative in cases where a small
amount of data is necessary, e.g., a few months from several observatories.

If a larger amount of data is necessary then it is much more
efficient to use the data published on DVDs (before 2006 on
CD-ROMs, since 2014 on USB drives). One annual CD-ROM/DVD/USB
drive contains definitive minute values from all (more than
100) INTERMAGNET observatories for the whole given year.
Another advantage of the CD-ROM/DVD/USB drive is that it
provides metadata files, K indices, yearmean file, and baseline
file. These are not available on the INTERMAGNET website.
Additionally, the IMCDVIEW software can be found on the DVD
which enables an intuitive inspection of the data and metadata
on the DVD, as well as a data export to other common formats
(e.g., IAGA2002). This software is described in :numref:`use_dat_data_view`.

.. _use_dat_cd_content:

Content of the INTERMAGNET Physical Media (IPM)
-----------------------------------------------

The IPM's content is organized in a number of
subdirectories. Most of them are named with the IAGA codes,
they contain information regarding a particular observatory.
Further subdirectories provide country information about
national operators of magnetic observatories. A complete set
for one observatory consists of the following files (this
example is for the year 2010):

#. x:\\mag2010\\sss\\sss10mmm.bin (12 binary files in format IAF)
#. x:\\mag2010\\sss\\readme.sss (1 readme file)
#. x:\\mag2010\\sss\\sss2010.blv (1 baseline file)
#. x:\\mag2010\\sss\\yearmean.sss (1 annual means file)
#. x:\\mag2010\\sss\\sss10k.dka (1 text file of K-indices)
#. x:\\mag2010\\2010maps\\country_id.xxx (1 map for each country (xxx = png or pcx))
#. x:\\mag2010\\ctry_inf\\readme.country_id (1 readme-file for each country)
#. x:\\mag2010\\ctry_inf\\country_id+srn.xxx (1 about-screen for each country
   (xxx = png or pcx))

Where:

-  sss is the 3-letter observatory IAGA code
-  mmm is the month
-  country_id is the country code

#. A binary file in IAF format (described in detail in :numref:`sub_dat_1min_data`
   and |app_iaf|) includes minute values as well as
   other accompanying data such as: metadata headers W01-W16, K indices,
   hourly and daily mean values.
#. A text file readme.sss contains a lot of valuable
   information about the observatory, its location, the parent
   institute, personnel, used apparatus, etc.
#. A text file sssyyyy.blv includes results of absolute control
   of geomagnetic observations. The format of this file is
   described in |app_imag_ibf|. Baseline data are considered by
   many experts as an indicator of quality of the performed
   observation, especially with regard to observation of
   secular variations.
#. A text file yearmean.sss includes a listing of annual mean
   values for the observatory. The format of this file is
   described in |app_iyf|. Most observatories publish all
   annual means, i.e. since the beginning of their activity.
   Some observatories publish also annual means for quiet-days
   and disturbed-days.
#. A text file includes K-indices. DKA files are created by
   INTERMAGNET officers from the binary IAF files. There is no
   DKA file if there is a lack of K-indices in IAF files.
#. This graphic file shows the location of observatories of a
   given country.
#. The text file readme.country_id provides information about
   parent institute(s) in a given country.
#. This file provides information about parent institute(s)
   managing IMOs in a given country in graphical form.

Before 2006, corrected data were re-published on the next
available physical media if published definitive data were found to
contain errors. The 12 corrected monthly IAF bin files and
other metadata files were included, if necessary, in a
subdirectory with an observatory and year specific name
\\OBSYYYY (e.g. \\BEL1995).

From 2006 and onwards any corrections to one-minute definitive
data have been re-submitted to the INTERMAGNET web site. Only
time-series data are available from the web site so correction
of errors in metadata files are not captured after 2006.
Re-published IAF bin files contain the date (year and month) of
the re-publication in the daily header records.

IPMs contain a text file "\\errata\\errata" listing
the full history of corrected data and the year of the
IPM on which the correction was published. This file
remains unchanged since the 2006.

Now the INTERMAGNET web site should be regarded as the most
recent version of the definitive one-minute data.

.. _use_dat_cd_obtain:

How to Obtain INTERMAGNET Physical Media (IPM)
----------------------------------------------

Send your request to |contact_ipgp| indicating clearly the
Physical Medias required, your name, the name of your academic
institution, and your mailing address where the parcel should
be sent. INTERMAGNET Physical Medias are distributed by:

| INTERMAGNET Physical Media distribution office
| Observatoire Magnétique National
| Carrefour des 8 routes
| 45340 Chambon la Forêt
| FRANCE
| Tel: 33 (0) 2-38-33-95-01
| Email: |contact_ipgp|

The INTERMAGNET data are available without charge to
participating institutes and to bona fide scientists for
academic purposes only. By requesting one or more INTERMAGNET
Physical Medias you agree to abide by the Conditions of Use.

From 2015 onward, INTERMAGNET stopped the production of physical medias.
Definitive data is now published yearly with a Digital Object Identifier (DOI)
see :numref:`use_dat_imag_web_down_data_def`. Individual (monthly) IAF data files
are available from the INTERMAGNET web site.
