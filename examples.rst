Examples
========

Simple Usage
------------
Just running :program:`mydumper` without any options will load the configuration
from :option:`--defaults-file <mydumper --defaults-file>` and try to connect to
a server using the default connection string. It will then dump the tables from 
all databases using 4 worker threads.

Restoring a dump
----------------
MyDumper now include myloader which is a multi-threaded restoration tool.  To
use myloader with a mydumper dump you simply need to pass it the directory of
the dump along with a user capable of restoring the schemas and data.  As an
example the following will restore a dump overwriting any existing tables:

.. code-block:: bash

  # myloader --directory=export-20110614-094953 --overwrite-tables --user=root

If your backup is encrypted following the example above, you will need to reverse
the process with myloader. The backup should contain the encrypted key used during 
the encryption process, or otherwise be located via whatever storage means used in 
your environment. Decrypt the backup key first, then use myloader to restore the backup 
as usual.

.. code-block:: bash

  # myloader \
	--defaults-extra-file=.my.cnf \
	--directory dump/ \
	--exec-per-thread "openssl enc -d -aes-256-cbc -pbkdf2 -pass file:/tmp/myspecial.key" \
	--exec-per-thread-extension=.enc

Daemon mode
-----------
Mydumper has a daemon mode which will snapshot the dump data every so often
whilst continuously retreiving the binary log files.  This gives a continuous
consistent backup right up to the point where the database server fails.  To use
this you simply need to use the :option:`--daemon <mydumper --daemon>` option.

In the following example mydumper will use daemon mode, creating a snapshot
every half an hour and log to an output file:

.. code-block:: bash

  # mydumper --daemon --snapshot-interval=30 --logfile=dump.log
