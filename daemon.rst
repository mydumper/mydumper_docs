Daemon
------
MyDumper has a daemon mode which will snapshot the dump data every so often
whilst continuously retreiving the binary log files.  This gives a continuous
consistent backup right up to the point where the database server fails.  To use
this you simply need to use the :option:`--daemon <mydumper --daemon>` option.

Daemon mode does things a little differently.  There are the directories ``0``
and ``1`` inside the dump directory.  These alternate when dumping so that if
mydumper fails for any reason there is still a good snapshot.  When a snapshot
dump is complete the ``last_dump`` symlink is updated to point to that dump.

In the following example mydumper will use daemon mode, creating a snapshot
every half an hour and log to an output file:

.. code-block:: bash

  # mydumper --daemon --snapshot-interval=30 --logfile=dump.log
