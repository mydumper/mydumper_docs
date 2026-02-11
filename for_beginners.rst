.. include:: header.rst

For beginners
=============

mydumper
--------

You need to use :option:`--outputdir <mydumper --outputdir>`  to set the directory of the backup or a backup dir will be created.

What do you want to backup? a single database? multiple databases? a few tables?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can use 

* :option:`-B <mydumper -B>` with a list of databases
* :option:`-T <mydumper -T>` with a list of tables
* multiple instances of :option:`--regex <mydumper --regex>` are allowed


What kind of lockings can be used?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

mydumper needs to sync the threads and it provides different options based on your capabilities https://mydumper.github.io/mydumper/docs/html/locks.html#synchronizing-threads

What about DDL locks?
^^^^^^^^^^^^^^^^^^^^^

https://mydumper.github.io/mydumper/docs/html/locks.html#ddl-locks

Do you have non transactional tables?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you do, you must set --trx-tables=0 or --no-trx-tables (if available)

Do you want to compress the backup?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

fyi: zstd takes the same amount of time than raw files and reduce it like gzip compression

https://mydumper.github.io/mydumper/docs/html/exec_per_thread.html

Do you want a single backup file?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use :option:`--stream <mydumper --stream>`

Do you need the replica position?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use :option:`--replica-data <mydumper --replica-data>`


Do you want to check the consistency?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Use Use :option:`-M <mydumper -M>`

If you are not sure follow this guidance:

- if it is a migration, you should do it
- if it is just for testings, you need it fast and it is a large backup, you can avoid it.
- if it is a daily backup you can avoid it to reduce the backup timings
- if it is a daily backup that you are going to test, then you must use it



myloader
--------

Are you going to replace the tables? -o/--drop-tables?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use :option:`-o, --drop-table <myloader -o, --drop-table>`

