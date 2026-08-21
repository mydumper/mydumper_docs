Overwrite tables
----------------
myloader expects CREATE TABLE statements in `*-schema.sql` which will create the tables. However,
it is possible to instruct myloader to drop the tables in case you want to overwrite the table.

By default, myloader doesn't execute any overwrite command and it will FAIL if it is not succed
creating it, which can be ignored using the value NONE.

When you use :option:`-o/--drop-table <myloader --drop-table>`, it executes DROP TABLE by default, previously executing the
CREATE TABLE statement.

There are other ways to remove all the data from the table:

DELETE: myloader will execute a DELETE statement which is going to remove all the rows from the table.

TRUNCATE: myloader will execute a TRUNCATE TABLE statement and MySQL internally is going to drop and recreate the table.

Take into account that --skip-constraints and --skip-indexes should be used to avoid issue or will be
set by default on myloader in future releases.
