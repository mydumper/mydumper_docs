.. include:: header.rst

.. _mydumper_requirements:

Requirements
============

To create a backup user in MySQL for MyDumper, you need to grant the necessary privileges to allow the user to perform backups efficiently. Here’s the SQL script to create a backup user:

.. code-block::  bash
  
  CREATE USER 'backup_user'@'localhost' IDENTIFIED BY 'StrongPassword';
  GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT, SHOW VIEW, EVENT, TRIGGER, SELECT ON *.* TO 'backup_user'@'localhost';

Explanation of Privileges:

.. code-block::  bash

  SELECT ON *.*: Allows reading all databases and tables.
  RELOAD: Required for FLUSH operations (used in mydumper).
  LOCK TABLES: Needed to use FLUSH TABLES WITH READ LOCK for consistent backups.
  PROCESS: Grants access to process list (SHOW PROCESSLIST).
  REPLICATION CLIENT: Required for reading binary log positions.
  SHOW VIEW: Allows viewing database views.
  EVENT: Grants access to backup database events.
  TRIGGER: Allows backing up triggers.

