.. _mydumper_usage:

.. image:: ../images/horizontal/color-dark.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-dark

.. image:: ../images/horizontal/color-light.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-light

Usage
=====

Synopsis
--------

:program:`mydumper` [:ref:`OPTIONS <mydumper-options-label>`]

:program:`myloader` :option:`--directory <myloader --directory>` = /path/to/mydumper/backup [:ref:`OPTIONS <myloader-options-label>`]

Description
-----------

:program:`mydumper` is a tool used for backing up MySQL database servers much
faster than the mysqldump tool distributed with MySQL. The advantages of :program:`mydumper` are:

  * Parallelism (hence, speed) and performance (avoids expensive character set conversion routines, efficient code overall)
  * Easier to manage output (separate files for tables, dump metadata, etc, easy to view/parse data)
  * Consistency - maintains snapshot across all threads, provides accurate master and slave log positions, etc
  * Manageability - supports PCRE for specifying database and tables inclusions and exclusions

.. toctree::
   :hidden:

   mydumper_usage
   myloader_usage


