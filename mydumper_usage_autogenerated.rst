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

.. option:: -B, --database

  Comma delimited list of databases to dump

.. option:: -i, --ignore-engines

  Comma delimited list of storage engines to ignore

.. option:: --where

  Dump only selected records.

.. option:: -U, --updated-since

  Use Update_time to dump only tables updated in the last U days

.. option:: --partition-regex

  Regex to filter by partition name.

.. option:: -O, --omit-from-file

  File containing a list of database.table entries to skip, one per line (skips before applying regex option)

.. option:: -T, --tables-list

  Comma delimited table list to dump (does not exclude regex option). Table name must include database name. For instance: test.t1,test.t2

Lock Options
------------
.. option:: -z, --tidb-snapshot

  Snapshot to use for TiDB

.. option:: -k, --no-locks

  This option is deprecated use --sync-thread-lock-mode instead

.. option:: --lock-all-tables

  This option is deprecated use --sync-thread-lock-mode instead

.. option:: --sync-thread-lock-mode

  There are 3 modes that can be use to sync: FTWRL, LOCK_ALL and GTID. If you don't need a consistent backup, use: NO_LOCK. More info https://mydumper.github.io/mydumper/docs/html/locks.html. Default: AUTO which uses the best option depending on the database vendor

.. option:: --use-savepoints

  Use savepoints to reduce metadata locking issues, needs SUPER privilege

.. option:: --no-backup-locks

  Do not use Percona backup locks

.. option:: --less-locking

  This option is deprecated and its behaviour is the default which is useful if you don't have transaction tables. Use --trx-tables otherwise

.. option:: --trx-consistency-only

  This option is deprecated use --trx-tables instead

.. option:: --trx-tables

  The backup process changes, if we know that we are exporting transactional tables only

.. option:: --skip-ddl-locks

  Do not send DDL locks when possible

PMM Options
-----------
.. option:: --pmm-path

  which default value will be /usr/local/percona/pmm2/collectors/textfile-collector/high-resolution

.. option:: --pmm-resolution

  which default will be high

Exec Options
------------
.. option:: --exec-threads

  Amount of threads to use with --exec

.. option:: --exec

  Command to execute using the file as parameter

.. option:: --exec-per-thread

  Set the command that will receive by STDIN and write in the STDOUT into the output file

.. option:: --exec-per-thread-extension

  Set the extension for the STDOUT file when --exec-per-thread is used

.. option:: --long-query-retries

  Retry checking for long queries, default 0 (do not retry)

.. option:: --long-query-retry-interval

  Time to wait before retrying the long query check in seconds, default 60

.. option:: -l, --long-query-guard

  Set long query timer in seconds, default 60

.. option:: -K, --kill-long-queries

  Kill long running queries (instead of aborting)

Job Options
-----------
.. option:: --max-time-per-select

  Maximum amount of seconds that a select should take. Default: 2

.. option:: --max-threads-per-table

  Maximum number of threads per table to use

.. option:: --use-single-column

  It will ignore if the table has multiple columns and use only the first column to split the table

.. option:: -r, --rows

  Splitting tables into chunks of this many rows. It can be MIN:START_AT:MAX. MAX can be 0 which means that there is no limit. It will double the chunk size if query takes less than 1 second and half of the size if it is more than 2 seconds

.. option:: --rows-hard

  This set the MIN and MAX limit when even if --rows is 0

.. option:: --split-partitions

  Dump partitions into separate files. This option overrides the --rows option for partitioned tables.

Checksum Options
----------------
.. option:: -M, --checksum-all

  Dump checksums for all elements

.. option:: --data-checksums

  Dump table checksums with the data

.. option:: --schema-checksums

  Dump schema table and view creation checksums

.. option:: --routine-checksums

  Dump triggers, functions and routines checksums

Objects Options
---------------
.. option:: -m, --no-schemas

  Do not dump table schemas with the data and triggers

.. option:: -Y, --all-tablespaces

  Dump all the tablespaces.

.. option:: -d, --no-data

  Do not dump table data

.. option:: -G, --triggers

  Dump triggers. By default, it do not dump triggers

.. option:: -E, --events

  Dump events. By default, it do not dump events

.. option:: -R, --routines

  Dump stored procedures and functions. By default, it does not dump stored procedures nor functions

.. option:: --skip-constraints

  Remove the constraints from the CREATE TABLE statement. By default, the statement is not modified

.. option:: --skip-indexes

  Remove the indexes from the CREATE TABLE statement. By default, the statement is not modified

.. option:: --views-as-tables

  Export VIEWs as they were tables

.. option:: -W, --no-views

  Do not dump VIEWs

Statement Options
-----------------
.. option:: --load-data

  Instead of creating INSERT INTO statements, it creates LOAD DATA statements and .dat files. This option will be deprecated on future releases use --format

.. option:: --csv

  Automatically enables --load-data and set variables to export in CSV format. This option will be deprecated on future releases use --format

.. option:: --format

  Set the output format which can be INSERT, LOAD_DATA, CSV or CLICKHOUSE. Default: INSERT

.. option:: --include-header

  When --load-data or --csv is used, it will include the header with the column name

.. option:: --fields-terminated-by

  Defines the character that is written between fields

.. option:: --fields-enclosed-by

  Defines the character to enclose fields. Default: "

.. option:: --fields-escaped-by

  Single character that is going to be used to escape characters in theLOAD DATA statement, default: ''

.. option:: --lines-starting-by

  Adds the string at the beginning of each row. When --load-data is used it is added to the LOAD DATA statement. It affects INSERT INTO statements also when it is used.

.. option:: --lines-terminated-by

  Adds the string at the end of each row. When --load-data is used it is added to the LOAD DATA statement. It affects INSERT INTO statements also when it is used.

.. option:: --statement-terminated-by

  This might never be used, unless you know what are you doing

.. option:: -N, --insert-ignore

  Dump rows with INSERT IGNORE

.. option:: --replace

  Dump rows with REPLACE

.. option:: --complete-insert

  Use complete INSERT statements that include column names

.. option:: --hex-blob

  Dump binary columns using hexadecimal notation

.. option:: --skip-definer

  Removes DEFINER from the CREATE statement. By default, statements are not modified

.. option:: -s, --statement-size

  Attempted size of INSERT statement in bytes, default 1000000

.. option:: --tz-utc

  SET TIME_ZONE='+00:00' at top of dump to allow dumping of TIMESTAMP data when a server has data in different time zones or data is being moved between servers with different time zones, defaults to on use --skip-tz-utc to disable.

.. option:: --skip-tz-utc

  Doesn't add SET TIMEZONE on the backup files

.. option:: --set-names

  Sets the names, use it at your own risk, default binary

.. option:: --table-engine-for-view-dependency

  Table engine to be used for the CREATE TABLE statement for temporary tables when using views

Extra Options
-------------
.. option:: -F, --chunk-filesize

  Split data files into pieces of this size in MB. Useful for myloader multi-threading.

.. option:: --exit-if-broken-table-found

  Exits if a broken table has been found

.. option:: --success-on-1146

  This option is deprecated use --ignore-errors instead

.. option:: -e, --build-empty-files

  Build dump files even if no data available from table

.. option:: --no-check-generated-fields

  Queries related to generated fields are not going to be executed.It will lead to restoration issues if you have generated columns

.. option:: --order-by-primary

  Sort the data by Primary Key or Unique key if no primary key exists

.. option:: --compact

  Give less verbose output. Disables header/footer constructs.

.. option:: -c, --compress

  Compress output files using: gzip and zstd. Options: gzip and zstd. Default: gzip. On future releases the default will be zstd

.. option:: --use-defer

  Use defer integer sharding until all non-integer PK tables processed (saves RSS for huge quantities of tables)

.. option:: --check-row-count

  Run SELECT COUNT(*) and fail mydumper if dumped row count is different

Daemon Options
--------------
.. option:: -D, --daemon

  Enable daemon mode

.. option:: -I, --snapshot-interval

  Interval between each dump snapshot (in minutes), requires --daemon, default 60

.. option:: -X, --snapshot-count

  number of snapshots, default 2

Application Options:
--------------------
.. option:: -?, --help

  Show help options

.. option:: -o, --outputdir

  Directory to output files to

.. option:: --clear

  Clear output directory before dumping

.. option:: --dirty

  Overwrite output directory without clearing (beware of leftower chunks)

.. option:: --merge

  Merge the metadata with previous backup and overwrite output directory without clearing (beware of leftower chunks)

.. option:: --stream

  It will stream over STDOUT once the files has been written. Since v0.12.7-1, accepts NO_DELETE, NO_STREAM_AND_NO_DELETE and TRADITIONAL which is the default value and used if no parameter is given and also NO_STREAM since v0.16.3-1

.. option:: -L, --logfile

  Log file name to use, by default stdout is used

.. option:: --disk-limits

  Set the limit to pause and resume if determines there is no enough disk space.Accepts values like: '<resume>:<pause>' in MB.For instance: 100:500 will pause when there is only 100MB free and will resume if 500MB are available

.. option:: --masquerade-filename

  Masquerades the filenames

.. option:: --ftwrl-max-wait-time

  Sets the max time that we are going to wait before kill the FLUSH TABLES related commands. Default: 60

.. option:: --ftwrl-timeout-retries

  Sets the amount of retries before give up acquiring FLUSH TABLES. Default: 0, never gives up.

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

