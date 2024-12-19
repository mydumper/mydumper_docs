.. MyDumper Project master file
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. include:: header.rst

Welcome to MyDumper Project
===========================

MyDumper was born out of necessity. mysqldump remains the norm for MySQL logical backups but to this day it remains a single-threaded process. 

For large databases, the only feasible way to take a logical backup - and restore it - in a reasonable amount of time is by parallelizing the process and executing it using multiple threads, which is the core value of MyDumper.

MyDumper is a MySQL Logical Backup Tool with 2 programs:

* :program:`mydumper` is responsible to export a consistent backup of MySQL databases

* :program:`myloader` reads the backup from :program:`mydumper`, connects to the destination database and imports the backup.

MyDumper is Open Source and maintained by the community, it is not a Percona, MariaDB or MySQL product.

The tools developed in this Project are based on C, GLib and support the client libraries of Percona, MariaDB and MySQL.

.. toctree::
   :maxdepth: 2
   :hidden:

   installing
   compiling
   usage
   exec_per_thread
   files
   stream
   stages
   locks
   daemon
   examples
   authors

