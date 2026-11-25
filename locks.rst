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

Snapshot cloning
----------------

On Percona Server 5.7 and newer, :program:`mydumper` can open one transaction with `START TRANSACTION WITH CONSISTENT SNAPSHOT` and clone it into every thread with `START TRANSACTION WITH CONSISTENT SNAPSHOT FROM SESSION`. All the threads see exactly the same snapshot and no global read lock is needed. The binlog position and GTID written in the metadata file are taken from the `Binlog_snapshot_file`, `Binlog_snapshot_position` and `Binlog_snapshot_gtid_executed` status variables. On Percona Server 5.7, `LOCK BINLOG FOR BACKUP` is held while the snapshot is opened, so the position matches the snapshot.

This is the mode that `AUTO` uses on Percona Server.

.. code-block::  bash

  --sync-thread-lock-mode=SNAP_CLONE

Non-transactional tables, which are only exported with :option:`--trx-tables=0 <mydumper --trx-tables>`, are not covered by the snapshot. On Percona Server 5.7 they are protected by `LOCK TABLES FOR BACKUP`, which also blocks writes to them. On Percona Server 8.0 and newer, `LOCK INSTANCE FOR BACKUP` does not block those writes, so :program:`mydumper` warns for each non-transactional table that it might not be consistent with the snapshot.

Transactional tables
^^^^^^^^^^^^^^^^^^^^

By default, :program:`mydumper` assumes that all the tables are transactional (:option:`--trx-tables <mydumper --trx-tables>`), which reduces the amount of time the global lock is held. If a non-transactional table is found, the backup stops. If you need to export non-transactional tables, use :option:`--trx-tables=0 <mydumper --trx-tables>` or :option:`--no-trx-tables <mydumper --no-trx-tables>`.

Non-transactional tables
^^^^^^^^^^^^^^^^^^^^^^^^

When :option:`--trx-tables=0 <mydumper --trx-tables>` or :option:`--no-trx-tables <mydumper --no-trx-tables>` is used, :program:`mydumper` locks the non-transactional tables with `LOCK TABLES` while the global lock is held. As `LOCK TABLES` commits the current transaction, these tables are locked and dumped by a dedicated set of threads, and the threads that dump transactional tables keep their consistent snapshot. Once the non-transactional threads have taken their locks, the global lock is released.

The non-transactional threads are only created when there are non-transactional tables to dump. The transactional tables are dumped after the non-transactional tables.

The amount of threads of each set can be set with :option:`--threads <mydumper --threads>` using `<non-transactional>:<transactional>`. A single number is used for both sets, and `0` means the number of CPUs:

.. code-block::  bash

  # 2 threads for non-transactional tables and 8 for transactional tables
  --threads=2:8

  # 4 threads for each set
  --threads=4

When the backup is paused, with Ctrl+C or because of the disk space check, both sets of threads are paused.

DDL Locks
^^^^^^^^^

There are scenarios where a backup is cancel due DDL operations over tables that we are exporting. In order to avoid cancelling the backup, :program:`mydumper` send DDL locks like `LOCK INSTANCE FOR BACKUP` or `BACKUP STAGE START` when possible.

Disable DDL locks
-----------------

However, if you are tacking a partial backup or when you know that the operations that are being blocked are safe, you can disable this useing :option:`--skip-ddl-locks <mydumper --skip-ddl-locks>`

Disable Percona Backup Locks
----------------------------

On Percona Server 5.7, we have the option `LOCK TABLES FOR BACKUP` which has been replace by `LOCK INSTANCE FOR BACKUP` on Percona Server 8.0. We can disable this DDL locking mechanisim with :option:`--no-backup-locks <mydumper --no-backup-locks>`

On Percona Server 5.7 with `FTWRL`, `LOCK TABLES FOR BACKUP` is taken after FLUSH TABLE WITH READ LOCK, as it blocks writes to non-transactional tables, and the FLUSH TABLES would wait for them.

Savepoints
----------

It is possible to use savepoints to reduce metadata locking issues.
