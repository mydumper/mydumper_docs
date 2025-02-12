.. include:: header.rst

.. _mydumper_configure:

Configuration
=============

We kept the main configuration of MyDumper in /etc/mydumper.cnf by default.

--defaults-file
---------------

We kept relevant configuration in it as you can find sections like:

.. code-block::  bash

  [mydumper]
  [mydumper_session_variables]
  [myloader]
  [myloader_session_variables]

Where we kept the defaults when mydumper and myloader is executed and the default session level variables.
In case of `[myloader_session_variables]` we have this defaults:

.. code-block::  bash

  [myloader_session_variables]
  SQL_MODE='NO_AUTO_VALUE_ON_ZERO' /*!40101
  UNIQUE_CHECKS=0 /*!40114
  FOREIGN_KEY_CHECKS=0 /*!40114

Which is replaced when you connect to MariaDB:

.. code-block::  bash

  [myloader_session_variables_mariadb]
  # This setting replaces the default in the section [myloader_session_variables]. More details in #987
  SQL_MODE='NO_AUTO_VALUE_ON_ZERO' /*!40101
  UNIQUE_CHECKS=1 /*!40114
  FOREIGN_KEY_CHECKS=0 /*!40114



--defaults-extra-file
---------------------

We recommend you to use for :code:`[\`database\`.\`table\`]` sections, in which you can add this options:

* where: receives a comparasion compatible with the table definition.
* limit: which receives a number and forces the select statement to do not read more reads if it reaches the limit.
* num_threads: Defines the amount of threads that will be used for the table.
* object_to_export: It receives a comma delimited list with this options: SCHEMA, DATA, TRIGGER, ALL and NONE. ALL is equal to SCHEMA,DATA,TRIGGER.
* columns_on_select: the list of columns in the SELECT statement will be replace by the content of this parameter.
* columns_on_insert: The columns in the INSERT statemnt will be replace by the content of this parameter.
* object_to_export: It receives a comma delimited list with this options: SCHEMA, DATA, TRIGGER, ALL and NONE. ALL is equal to SCHEMA,DATA,TRIGGER.
* partition_regex: Defines a regular expression to filter the partitions to export 

For example:

.. code-block::  bash

  where = column > 20
  limit = 1000
  object_to_export = SCHEMA
  columns_on_select = qty,price+20
  columns_on_insert = qty,price

.. toctree::
   :hidden:

   exec_per_thread
   replication
   stream
   locks
   masquerade
