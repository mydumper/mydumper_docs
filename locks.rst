.. _mydumper_locks:

Lock modes
==========

Synchronizing threads
^^^^^^^^^^^^^^^^^^^^^

When you take a consistent backup, you must set the point in time where the backup is going to be taken. If you use multiple threads to take the backup, all of them must to be in the same point in time. The option :option:`--sync-thread-lock-mode <mydumper --sync-thread-lock-mode>` sets the mode that mydumper can use to keep the threads in sync.


FLUSH TABLE WITH READ LOCK
--------------------------

By default, :program:`mydumper` uses the simplest way, which is send a FLUSH TABLE WITH READ LOCK, but there are other options:

.. code-block::  bash

  --sync-thread-lock-mode=FTWRL

Lock all Tables
---------------

In cases where FLUSH TABLE WITH READ LOCK is not available, we can use `LOCK_ALL` to lock all the tables that we are going to export.

.. code-block::  bash

  --sync-thread-lock-mode=LOCK_ALL

No Locks
--------

If you don't want to use FLUSH TABLE WITH READ LOCK and LOCK TABLE, you can simple disable it using :option:`--sync-thread-lock-mode=NO_LOCK <mydumper --sync-thread-lock-mode>` which might cause a inconsistent backup.
This option has been modified for Percona Server, as after sending START TRANSACTION WITH CONSISTENT SNAPSHOT, we are able to check if all the threads are in the same point in time and if it is not it will retry 3 more times. If it is not able to sync all the threads, it will continue informing that it will be an inconsistent backup.

.. code-block::  bash

  --sync-thread-lock-mode=NO_LOCK

Safe No Locks
-------------

This mode is very similar to NO_LOCK but it will stop the backup if detects that the are differences in the binlog position at the beginning of the backup and after syncing threads.

.. code-block::  bash

  --sync-thread-lock-mode=SAFE_NO_LOCK

Using GTID
----------

On Percona Server, we can check the status variable binlog_snapshot_gtid_executed when you execute `START TRANSACTION WITH CONSISTENT SNAPSHOT` to check the GTID position which mydumper uses to determine if all the threads are watching the same point in time of the database.

.. code-block::  bash

  --sync-thread-lock-mode=GTID

Transactional tables
^^^^^^^^^^^^^^^^^^^^

When you have transactional table only, you can use :option:`--trx-tables <mydumper --trx-tables>` which reduces the amount of time we held the global lockings.

DDL Locks
^^^^^^^^^

There are scenarios where a backup is cancel due DDL operations over tables that we are exporting. In order to avoid cancelling the backup, :program:`mydumper` send DDL locks like `LOCK INSTANCE FOR BACKUP` or `BACKUP STAGE START` when possible.

Disable DDL locks
-----------------

However, if you are tacking a partial backup or when you know that the operations that are being blocked are safe, you can disable this useing :option:`--skip-ddl-locks <mydumper --skip-ddl-locks>`

Disable Percona Backup Locks
----------------------------

On Percona Server 5.7, we have the option `LOCK TABLES FOR BACKUP` which has been replace by `LOCK INSTANCE FOR BACKUP` on Percona Server 8.0. We can disable this DDL locking mechanisim with :option:`--no-backup-locks <mydumper --no-backup-locks>`

Savepoints
----------

It is possible to use savepoints to reduce metadata locking issues.
