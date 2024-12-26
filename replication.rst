.. _mydumper_usage:

.. include:: header.rst

Replication Options
===================

The metadata file is a way to share information between mydumper and myloader.

There are 3 options on the metadata file to control which replication commands will execute myloader.

The option is :option:`--source-data <mydumper --source-data>`, which accepts a value between 0 and 7 following this table:

=============  =========================  ===========================  =========================
--source-data  myloader_exec_start_slave  myloader_exec_change_master  myloader_exec_reset_slave
=============  =========================  ===========================  =========================
0              0                          0                            0
1              0                          0                            1
2              0                          1                            0
3              0                          1                            1
4              1                          0                            0
5              1                          0                            1
6              1                          1                            0
7              1                          1                            1
=============  =========================  ===========================  =========================

Where 0 means that it will not be executed and 1 means that it will be executed.

