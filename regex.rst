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
