.. include:: header.rst

.. _mydumper_configure:

Configuration
=============

The main configuration of MyDumper is kept in /etc/mydumper.cnf by default.

--defaults-file[=/etc/mydumper.cnf]
-----------------------------------

Said configuration is found in sections like: 

.. code-block::  bash

  [mydumper]
  [mydumper_session_variables]
  [myloader]
  [myloader_session_variables]

Where the defaults of mydumper, myloader and the session level variables are kept when they are executed. 
In the case of :code:`[myloader_session_variables]` , the default setting is: 

.. code-block::  bash

  [myloader_session_variables]
  SQL_MODE='NO_AUTO_VALUE_ON_ZERO' /*!40101
  UNIQUE_CHECKS=0 /*!40114
  FOREIGN_KEY_CHECKS=0 /*!40114

But, when you connect to MariaDB it gets replaced by: 

.. code-block::  bash

  [myloader_session_variables_mariadb]
  # This setting replaces the default in the section [myloader_session_variables]. More details in #987
  SQL_MODE='NO_AUTO_VALUE_ON_ZERO' /*!40101
  UNIQUE_CHECKS=1 /*!40114
  FOREIGN_KEY_CHECKS=0 /*!40114



--defaults-extra-file
---------------------

Using :code:`[\`database\`.\`table\`]` sections may be helpful, since you can add:

* where: It receives a comparison compatible with the table definition;
* limit: It receives a number and forces the SELECT statement to avoid reading more reads if it reaches the limit;
* num_threads: It defines the amount of threads that will be used for the table;
* object_to_export: It receives a comma delimited list with this options: SCHEMA, DATA, TRIGGER, ALL and NONE. ALL is equal to SCHEMA,DATA,TRIGGER;
* columns_on_select: The list of columns in the SELECT statement will be replaced by the content of this parameter;
* columns_on_insert: The columns in the INSERT statemnt will be replace by the content of this parameter;
* object_to_export: It receives a comma delimited list with this options: SCHEMA, DATA, TRIGGER, ALL and NONE. ALL is equal to SCHEMA,DATA,TRIGGER;
* partition_regex: It defines a regular expression to filter the partitions to export. 
* rows: It replaces the value of :option:`--rows <mydumper --rows>` or its default value.

For example:

.. code-block::  bash

  where = column > 20
  limit = 1000
  object_to_export = SCHEMA
  columns_on_select = qty,price+20
  columns_on_insert = qty,price
  rows = 1000000

.. toctree::
   :hidden:

   exec_per_thread
   replication
   stream
   locks
   masquerade
