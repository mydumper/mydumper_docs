
Connection Options
------------------

.. option:: -h, --host

  The host to connect to

.. option:: -u, --user

  Username with the necessary privileges

.. option:: -p, --password

  User password

.. option:: --default-connection-database

  Set the database name to connect to. Default: INFORMATION_SCHEMA

.. option:: -a, --ask-password

  Prompt For User password

.. option:: -P, --port

  TCP/IP port to connect to

.. option:: -S, --socket

  UNIX domain socket file to use for connection

.. option:: --protocol

  The protocol to use for connection (tcp, socket)

.. option:: -C, --compress-protocol

  Use compression on the MySQL connection

.. option:: --ssl

  Connect using SSL

.. option:: --ssl-mode

  Desired security state of the connection to the server: DISABLED, PREFERRED, REQUIRED, VERIFY_CA, VERIFY_IDENTITY

.. option:: --key

  The path name to the key file

.. option:: --cert

  The path name to the certificate file

.. option:: --ca

  The path name to the certificate authority file

.. option:: --capath

  The path name to a directory that contains trusted SSL CA certificates in PEM format

.. option:: --cipher

  A list of permissible ciphers to use for SSL encryption

.. option:: --tls-version

  Which protocols the server permits for encrypted connections

Filter Options
--------------

.. option:: -x, --regex

  Regular expression for 'db.table' matching

.. option:: -s, --source-db

  Database to restore

.. option:: --skip-triggers

  Do not import triggers. By default, it imports triggers

.. option:: --skip-post

  Do not import events, stored procedures and functions. By default, it imports events, stored procedures or functions

.. option:: --skip-constraints

  Do not import constraints. By default, it imports constraints

.. option:: --skip-indexes

  Do not import secondary indexes on InnoDB tables. By default, it import the indexes

.. option:: --no-data

  Do not dump or import table data

.. option:: -O, --omit-from-file

  File containing a list of database.table entries to skip, one per line (skips before applying regex option)

.. option:: -T, --tables-list

  Comma delimited table list to dump (does not exclude regex option). Table name must include database name. For instance: test.t1,test.t2

PMM Options
-----------

.. option:: --pmm-path

  which default value will be /usr/local/percona/pmm2/collectors/textfile-collector/high-resolution

.. option:: --pmm-resolution

  which default will be high

Execution Options
-----------------

.. option:: -e, --enable-binlog

  This option is discouraged. Use [myloader_session_variables] in the --defaults-file or --defaults-extra-file instead

.. option:: --innodb-optimize-keys

  Option --innodb-optimize-keys is deprecated use --optimize-keys instead

.. option:: --optimize-keys

  Creates the table without the indexes unless SKIP is selected. It will add the indexes right after completing the table restoration by default or after importing all the tables. Options: AFTER_IMPORT_PER_TABLE, AFTER_IMPORT_ALL_TABLES and SKIP. Default: AFTER_IMPORT_PER_TABLE

.. option:: --no-schema

  Do not import table schemas and triggers

.. option:: --purge-mode

  Option --purge-mode is deprecated use -o/--drop-table instead

.. option:: --disable-redo-log

  Disables the REDO_LOG and enables it after, doesn't check initial status

.. option:: --checksum

  Treat checksums: skip, fail(default), warn.

.. option:: --drop-database

  Executes a DROP DATABASE if the schema database file is found.

.. option:: -o, --drop-table

  Executes or simulates a DROP TABLE if the table already exists. The drop modes can be: FAIL, NONE, DROP, TRUNCATE and DELETE. This option accepts no parameter which set default to: DROP. If this option is not used, the default is set to: FAIL

.. option:: --overwrite-tables

  Option --overwrite-tables has been deprecated. User -o/--drop-table instead.

.. option:: --overwrite-unsafe

  Same as --overwrite-tables but starts data load as soon as possible. May cause InnoDB deadlocks for foreign keys.

.. option:: --retry-count

  Lock wait timeout exceeded retry count, default 10 (currently only for DROP TABLE)

.. option:: --serialized-table-creation

  Table recreation will be executed in series, one thread at a time. This means --max-threads-for-schema-creation=1. This option will be removed in future releases

.. option:: --stream

  It will receive the stream from STDIN and create the file in the disk before start processing. Since v0.12.7-1, accepts NO_DELETE, NO_STREAM_AND_NO_DELETE and TRADITIONAL which is the default value and used if no parameter is given and also NO_STREAM since v0.16.3-1

.. option:: --metadata-refresh-interval

  Every this amount of tables the internal metadata will be refreshed. If the amount of tables you have in your metadata file is high, then you should increase this value. Default: 100

.. option:: --skip-table-sorting

  Starting with largest table is better, but this can be ignored due performance impact when you have high amount of tables

.. option:: --set-gtid-purged

  After import, it will execute the SET GLOBAL gtid_purged with the value found on source section of the metadata file

Threads Options
---------------

.. option:: --max-threads-per-table

  Maximum number of threads per table to use, defaults to --threads

.. option:: --max-threads-for-index-creation

  Maximum number of threads for index creation, default 4

.. option:: --max-threads-for-post-actions

  Maximum number of threads for post action like: constraints, procedure, views and triggers, default 1

.. option:: --max-threads-for-schema-creation

  Maximum number of threads for schema creation. When this is set to 1, is the same than --serialized-table-creation, default 4

.. option:: --exec-per-thread

  Set the command that will receive by STDIN from the input file and write in the STDOUT

.. option:: --exec-per-thread-extension

  Set the input file extension when --exec-per-thread is used. Otherwise it will be ignored

Statement Options
-----------------

.. option:: -r, --rows

  Split the INSERT statement into this many rows.

.. option:: -q, --queries-per-transaction

  Number of queries per transaction, default 1000

.. option:: --append-if-not-exist

  Appends IF NOT EXISTS to the create table statements. This will be removed when https://bugs.mysql.com/bug.php?id=103791 has been implemented

.. option:: --set-names

  Sets the names, use it at your own risk, default binary

.. option:: --skip-definer

  Removes DEFINER from the CREATE statement. By default, statements are not modified

.. option:: --ignore-set

  List of variables that will be ignored from the header of SET

Load from metadata Options
--------------------------

.. option:: -Q, --quote-character

  Identifier quote character used in INSERT statements. Possible values are: BACKTICK, bt, ` for backtick and DOUBLE_QUOTE, dt, " for double quote. Default: detect from metadata file if possible, otherwise BACKTICK

.. option:: --local-infile

  Enables the ability to use the 'LOAD DATA LOCAL INFILE' statementDefault: detect from metadata file if possible, otherwise is disabled

Application Options:
--------------------

.. option:: -?, --help

  Show help options

.. option:: -d, --directory

  Directory of the dump to import

.. option:: -L, --logfile

  Log file name to use, by default stdout is used

.. option:: --fifodir

  Directory where the FIFO files will be created when needed. Default: Same as backup

.. option:: -B, --database

  An alternative database to restore into

.. option:: --show-warnings

  If enabled, during INSERT IGNORE the warnings will be printed

.. option:: --resume

  Expect to find resume file in backup dir and will only process those files

.. option:: -k, --kill-at-once

  When Ctrl+c is pressed it immediately terminates the process

.. option:: --mysqldump

  It expect a mysqldump format when stream is used

.. option:: -t, --threads

  Number of threads to use, 0 means to use number of CPUs. Default: 4, Minimum: 2

.. option:: -V, --version

  Show the program version and exit

.. option:: -v, --verbose

  Verbosity of output, 0 = silent, 1 = errors, 2 = warnings, 3 = info, default 2

.. option:: --debug

  Turn on debugging output (automatically sets verbosity to 3)

.. option:: --ignore-errors

  Not increment error count and Warning instead of Critical in case of any of the comma-separated error number list

.. option:: --defaults-file

  Use a specific defaults file. Default: /etc/mydumper.cnf

.. option:: --defaults-extra-file

  Use an additional defaults file. This is loaded after --defaults-file, replacing previous defined values

.. option:: --source-control-command

  Instruct the proper commands to execute depending where are configuring the replication. Options: TRADITIONAL, AWS

.. option:: --optimize-keys-engines

  List of engines that will be used to split the create table statement into multiple stages if possible. Default: InnoDB,ROCKSDB

.. option:: --server-version

  Set the server version avoid automatic detection

.. option:: --source-data

  It will include the options in the metadata file, to allow myloader to establish replication

.. option:: --throttle

  Expects a string like Threads_running=10. It will check the SHOW GLOBAL STATUS and if it is higher, it will increase the sleep time between SELECT. If option is used without parameters it will use Threads_running and the amount of threads
