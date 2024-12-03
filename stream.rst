.. image:: ../images/horizontal/color-dark.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-dark

.. image:: ../images/horizontal/color-light.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-light

Stream
======

When you take a backup with :program:`mydumper`, it will connect to the database,
export the data and save into files in :option:`backup directory <mydumper --outputdir>`.
It is also possible to :option:`--stream <mydumper --stream>` the backup to STDOUT
to save the backup in a single file or to send it over the network.
:program:`myloader` has also the availity to read a stream from the STDIN and 
restore it.

Streaming backup
----------------

Even when :program:`mydumper` uses :option:`--stream <mydumper --stream>`, it will 
connect to the database and stores the backup in the :option:`directory <mydumper --outputdir>`.
After each time that a backup file is close, it will stream the header of the file
to STDOUT, and depending the option that we set, it will be write the content of the
file and delete from the directory.
The options that :option:`--stream <mydumper --stream>` allows are:

TRADITIONAL
^^^^^^^^^^^

It will write the content of the file to STDOUT and delete the file after.
It is useful when you want to keep the backup in a single file, which is easier to
transfer later.

NO_DELETE
^^^^^^^^^

It will write the content of the file to STDOUT and will not delete the file.
This option is useful for debuging as you can review the files that are being sent
to myloader.

NO_STREAM_AND_NO_DELETE
^^^^^^^^^^^^^^^^^^^^^^^

It will not write the content and it will not delete the file.
In this case, only the header is being send to STDOUT, but the file is kept on the 
directory which is useful as :program:`myloader` is able to read the file from it
and delete it after restore it.

NO_STREAM
^^^^^^^^^

It will not write the content and it will delete the files.
This is mostly for compatibility with :program:`myloader`.

Restore Stream
--------------

By default, :program:`myloader` reads the files from the :option:`directory <myloader --directory>`,
but when :option:`--stream <myloader --stream>` is used, it will be a bit more 
complex.
It will create a thread that will read from STDIN the headers of the file and it 
will into the :option:`directory <myloader --directory>` the content depending what
we set on :option:`--stream <myloader --stream>`:


TRADITIONAL
^^^^^^^^^^^

It will read the content from STDIN, write the files in the directory for processing 
and delete the file after.
It is the common way to restore backups from a single file.

NO_DELETE
^^^^^^^^^

It will read the content from STDIN, write the files in the directory for processing
and it will not delete the files after.
This option is useful for debuging as you can review the files that are being restored.

NO_STREAM_AND_NO_DELETE
^^^^^^^^^^^^^^^^^^^^^^^

It will read the header from STDIN but it will not expect the content of the file, then
it will process and it will not delete the files after.
This is useful when :program:`mydumper` shares the backup directory with :program:`myloader`
but you want to keep with the backup files.

NO_STREAM
^^^^^^^^^

It will read the header from STDIN but it will not expect the content of the file, then
it will process and it will delete the files after.
This is useful when :program:`mydumper` shares the backup directory with :program:`myloader`.
