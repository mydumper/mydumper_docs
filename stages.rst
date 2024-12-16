.. _mydumper_usage:

.. image:: ../images/horizontal/color-dark.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-dark

.. image:: ../images/horizontal/color-light.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-light

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

At this point we are able to start to create jobs 

Start the dump threads
----------------------

As the locking mechanisim is in please we are allow to create the working threads to start processing the jobs. 

Release lockings
----------------

The threads will be on sync allowing the main connection to release some lockings.

Wait backup to complete
-----------------------

The workers will be processing the jobs and exporting the schema and data. The main thread will be waiting until they finish.

Releasing remaining lockings
----------------------------

Depending on the locking mechanisim that you configure, we might need to release some locks.
