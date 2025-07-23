.. include:: header.rst

Output Files
============

:program:`mydumper` generates 3 main types of files during the generation of the dump:

* Metadata
* Schema
* Data


Metadata
--------
When a dump is executed, a file called ``metadata.partial`` is created in the output
directory and, if mydumper finishes without error, it gets renamed to ``metadata`` .  
This contains the start and end time of the dump as well as the
master binary log positions if applicable.

Since version 0.14.1-1, format has been changed to::

  # Started dump at: 2023-06-09 11:47:18
  [master]
  # Channel_Name = '' # It can be use to setup replication FOR CHANNEL
  File = mydumper1-bin.000017
  Position = 241149225
  Executed_Gtid_Set = 7b166a41-65a2-11ed-9de3-0800275ff74d:1-147115,7b166a41-65a2-11ed-9de3-0800275ff74e:1-61558

  [`sakila`.`store`]
  Rows = 2
  data_checksum = 3119812626
  schema_checksum = B7B99B4C
  indexes_checksum = B4D31E3

  [`sakila`]
  schema_checksum = FDF2173B
  post_checksum = 42085F07
  # Finished dump at: 2023-06-09 11:47:18

This is an example of the content of this file for older versions::

  Started dump at: 2011-05-05 13:57:17
  SHOW MASTER STATUS:
    Log: linuxjedi-laptop-bin.000001
    Pos: 106

  Finished dump at: 2011-05-05 13:57:17

Schemas
-------
As long as the :option:`--no-schemas <mydumper --no-schemas>` option is not specified, :program:`mydumper` will
create a schema file per database, per table, per view, per trigger.
The files for databases are in the following format::

  database-schema.sql

The files for tables are in the following format::

  database.table-schema.sql

If :option:`--triggers <mydumper --triggers>` is specified, :program:`mydumper` will export the trigger.
Depending on the filter options that you selected you can get a single file for all the trigger::

  database-schema-triggers.sql

Or a file per table::

  database.table-schema-triggers.sql

If :option:`--events <mydumper --events>` and/or :option:`--routines <mydumper --routines>` are specified, :program:`mydumper` will export the Events, Functions and Store Procedures in a single file following format::

  database-schema-post.sql

With :option:`--all-tablespaces <mydumper --all-tablespaces>`, it will export the tablespaces definition in a single file with this name::

  all-schema-create-tablespace.sql


Data
----
The data from every table is written into a separate file. If the
:option:`--rows <mydumper --rows>` option is used, each chunk of table will
be in a separate file.  The file names for this are in the format::

  database.table.sql

or if chunked::

  database.table.chunk.sql

Where 'chunk' is a number padded with up to 5 zeros or::

  database.table.chunk.chunk2.sql

Where 'chunk2' is a number padded with up to 5 zeros.
