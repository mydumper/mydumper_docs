.. include:: header.rst

.. _mydumper_requirements:

Requirements
============

To create a backup user in MySQL for MyDumper. Here’s the SQL script to create a backup user:

.. code-block::  bash
  
  CREATE USER 'backup_user'@'localhost' IDENTIFIED BY 'StrongPassword';

Privileges on mydumper
----------------------

.. code-block::  bash

  
  PROCESS ON *.*: Grants access to process list (SHOW PROCESSLIST).
  REPLICATION CLIENT ON *.*: Required for reading binary and replication log positions.
  BACKUP_ADMIN: required to DDL locks when LOCK INSTANCE FOR BACKUP is executed.
  FLUSH_TABLES or RELOAD: Required for FLUSH operations.
  LOCK TABLES: Needed when --sync-thread-lock-mode=LOCK_ALL is used, it can be ignored otherwise.
  SHOW VIEW: Allows viewing database views.
  SHOW_ROUTINE: Allows viewing database store procedure and functions.
  TRIGGER: Allows backing up triggers.
  EVENT: Grants access to backup database events.
  SELECT ON *.*: Allows reading all databases, tables and routines


You need to grant the necessary GLOBAL privileges to allow the user to perform backups efficiently:

.. code-block::  bash
  
  GRANT FLUSH_TABLES, PROCESS, REPLICATION CLIENT ON *.* TO 'backup_user'@'localhost';

The simplest privilege setting, is to grant privileges to ALL:

.. code-block::  bash
  
  GRANT SHOW VIEW, EVENT, TRIGGER, SELECT ON *.* TO 'backup_user'@'localhost';

When the user is only allow to SELECT to a particular database for instance `sakila`, we need to add SHOW_ROUTINE:

.. code-block::  bash
  
  GRANT SHOW VIEW, EVENT, TRIGGER, SELECT ON `sakila`.* TO 'backup_user'@'localhost';
  GRANT SHOW_ROUTINE ON *.* TO 'backup_user'@'localhost';


When --sync-thread-lock-mode=LOCK_ALL is used you must grant LOCK TABLES:

.. code-block::  bash
  
  GRANT LOCK TABLES ON `sakila`.* TO 'backup_user'@'localhost';

Privileges on myloader
----------------------

.. code-block::  bash

  SESSION_VARIABLES_ADMIN ON *.*: It is required due the disabling of SQL_LOG_BIN
  CREATE: Allows to create databases, tables and indexes
  DROP: Allows to drop databases, tables or views which is required if you need to overwrite
  CREATE VIEW: Only required if you use VIEWs
  TRIGGER: Only required to create the triggers
  ALTER TABLE: Required to add the secondary indexes and constraints.
  REFERENCES: Only required if you use Foreign Keys.
  ALTER ROUTINE: Only required if you are creating store procedure or functions
  INSERT: Mandatory as you will be inserting data.

The only necessary GLOBAL privilege is SESSION_VARIABLES_ADMIN:

.. code-block::  bash
  
  GRANT SESSION_VARIABLES_ADMIN ON *.* TO 'backup_user'@'localhost';

You will need this priveleges to successfuly import the database into `new_sakila`:

.. code-block::  bash
  
  GRANT INSERT, CREATE, DROP, REFERENCES, ALTER, CREATE VIEW, CREATE ROUTINE, ALTER ROUTINE ON `new_sakila`.* TO 'backup_user'@'localhost';

Take into account that when using VIEWs you might need SELECT on the destination database.

.. code-block::  bash
  
  GRANT SELECT ON `new_sakila`.* TO 'backup_user'@'localhost';

When you create store procedures, functions, views and triggers, and the DEFINER is not the current user, you will need to use --skip-definer or this privileges:

.. code-block::  bash
  
  GRANT SET_USER_ID, SYSTEM_USER ON *.* TO 'backup_user'@'localhost';
