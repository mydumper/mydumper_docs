.. MyDumper documentation master file
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. image:: ../images/horizontal/color-dark.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-dark

.. image:: ../images/horizontal/color-light.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-light

Welcome to MyDumper's documentation!
====================================

MyDumper is a MySQL Logical Backup Tool. It has 2 tools:

* mydumper which is responsible to export a consistent backup of MySQL databases

* myloader reads the backup from mydumper, connects to the destination database and imports the backup.

Both tools use multithreading capabilities.
MyDumper is Open Source and maintained by the community, it is not a Percona, MariaDB or MySQL product.


Contents:

.. toctree::
   :maxdepth: 2

   authors
   installing
   compiling
   mydumper_usage
   myloader_usage
   files
   examples

Indices and tables
==================

* :ref:`genindex`
* :ref:`search`

