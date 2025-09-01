.. include:: header.rst

Examples
========

Simple Usage
------------
Just running :program:`mydumper` without any options will load the configuration
from :option:`--defaults-file <mydumper --defaults-file>` and try to connect to
a server using the default connection string. It will then dump the tables from 
all databases using 4 worker threads.

Regex
-----
To use :program:`mydumper`'s regex feature simply use the
:option:`--regex <mydumper --regex>` option.  In the following example mydumper
will ignore the ``test`` and ``mysql`` databases

.. code-block::  bash

  # mydumper --regex '^(?!(mysql\.|test\.))'

Once can use --regex functionality, for example not to dump mysql, sys and test databases:

.. code-block::  bash

  # mydumper --regex '^(?!(mysql\.|sys\.|test\.))'


To dump only mysql and test databases:

.. code-block::  bash

  # mydumper --regex '^(mysql\.|test\.)'

To not dump all databases starting with test:

.. code-block::  bash

  # mydumper --regex '^(?!(test))'

To dump specific tables in different databases (Note: The name of tables should end with $:

.. code-block::  bash

  # mydumper --regex '^(db1\.table1$|db2\.table2$)'

If you want to dump a couple of databases but discard some tables, you can do:

.. code-block::  bash

  # mydumper --regex '^(?=(?:(db1\.|db2\.)))(?!(?:(db1\.table1$|db2\.table2$)))'

Which will dump all the tables in db1 and db2 but it will exclude db1.table1 and db2.table2

Of course, regex functionality can be used to describe pretty much any list of tables.

Encryption / Encrypted Backups
------------------------------
Encrypting database backups is an important, and typically required, step to
protect the contents of the backup. Thanks to mydumper's :option:`--exec-per-thread`
option, we can encrypt each table's backup file during the backup process.

Create a random key for this backup. A different random key is used for each
backup to ensure maximum security. Each backup key is encrypted with a static
public key. This public key could be managed by a KMS like OpenBao, or fetched
from AWS KMS.

.. code-block:: bash

  # NOW=`date +"%Y%m%d"`
  # ENCKEY=$(mktemp)
  # openssl rand -base64 32 > ${ENCKEY}

Run mydumper with your usual parameters, adding the two extra flags for
:option:`--exec-per-thread` and :option:`--exec-per-thread-extension`

.. code-block:: bash

  # mydumper \
	--defaults-extra-file=.my.cnf \
	--verbose 3 \
	--exec-per-thread "openssl enc -aes-256-cbc -salt -pbkdf2 -pass file:${ENCKEY}" \
	--exec-per-thread-extension=.enc \
	-o dump

Looking inside `dump/` directory, we see our backup files, each one is encrypted:

.. code-block:: bash

  # ls dump/
  metadata                               sbtest_proxysql.sbtest1.00001.sql.enc
  sbtest_direct-schema-create.sql.enc    sbtest_proxysql.sbtest1.00002.sql.enc
  sbtest_proxysql.sbtest1.00000.sql.enc  sbtest_proxysql.sbtest1-schema.sql.enc

  # file dump/sbtest_direct-schema-create.sql.enc
  dump/sbtest_direct-schema-create.sql.enc: openssl enc'd data with salted password

Now that the backup is complete, encrypt the random key used for this backup. In the
example below, `${PUBKEY}.bin` would be file containing your public key as fetched
from whatever source/storage you are using. 

.. code-block:: bash

  # openssl pkeyutl \
	-in ${ENCKEY} \
	-out backup_key_{$NOW}.enc \
	-inkey ${PUBKEY}.bin \
	-keyform DER \
	-pubin \
	-encrypt \
	-pkeyopt rsa_padding_mode:oaep \
	-pkeyopt rsa_oaep_md:sha256

In the case of a KMS like OpenBao, in which you cannot fetch/download the public key, 
you would instead call the API to encrypt the key; sending the backup key to the KMS 
which returns back the encrypted bytes.

From here, we can copy our encrypted backup key, and backup files to remote storage.
Be sure to include the key with the backup! Without this key, you cannot decrypt this
backup. Remember, each backup uses a different backup key. The KMS/public key is used
to encrypt each separate backup key.

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

Overwrite tables
----------------
myloader expects CREATE TABLE statements in *-schema.sql which will create the tables. However, 
it is possible to instruct myloader to drop the tables in case you want to overwrite the table.

By default, myloader doesn't execute any overwrite command and it will FAIL if it is not succed 
creating it.

When you use `-o`/`--drop-table`, it executes DROP TABLE by default, previously executing the 
CREATE TABLE statement. 

There are other ways to remove all the data from the table:

DELETE: myloader will execute a DELETE statement which is going to remove all the rows from the table.

TRUNCATE: myloader will execute a TRUNCATE TABLE statement and MySQL internally is going to drop and recreate the table.

Take into account that --skip-constraints and --skip-indexes should be used to avoid issue or will be 
set by default on myloader in future releases.


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
