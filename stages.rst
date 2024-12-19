.. include:: header.rst

Backup Stages
=============

When :program:`mydumper` start to take a backup needs to execute multiple task and reach milestone.

Determine backup strategy
-------------------------

There are multiple :option:`locking options <mydumper -z>` that you can instruct :program:`mydumper` to take backups. You should review which :ref:`locking mechanisim <mydumper_locks>` is the best for you.

Send locking statements
-----------------------

:program:`mydumper` establishes a primary main connection and in some cases a secondary main connection. We need them to synchronize the working threads.

Start jobs creation
-------------------

At this point we are able to start to create jobs depending the filters that has been used. For instance, if you used :option:`-B <mydumper -B>`, we need to export the schema definition but we also need to create the jobs to export the schema of the tables and the data in it.

Start the dump threads
----------------------

As the locking mechanisim is in please we are allow to create the working threads to start processing the jobs. 

Release lockings
----------------

Once the working threads are in sync, the main connection is allowed to release lockings related to the synchronization of the workers.

Wait backup to complete
-----------------------

The workers will be processing the jobs, exporting the schema and data. The main thread will be waiting until they finish.

Releasing remaining lockings
----------------------------

Depending on the locking mechanisim that you configure, we might need to release the remaining locks.
