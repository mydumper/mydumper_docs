.. image:: ../images/horizontal/color-dark.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-dark

.. image:: ../images/horizontal/color-light.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-light

Compression, Encryption and more
--------------------------------

It is possible to change what mydumper is writing and what myloader is receiving,
with :option:`--exec-per-thread <mydumper --exec-per-thread>` and :option:`--exec-per-thread-extension <mydumper --exec-per-thread-extension>`.

When you use :option:`mydumper -c/--compress<mydumper -c>`, :program:`mydumper` internally set::

  --exec-per-thread="/usr/bin/gzip -c"
  --exec-per-thread-extension=".gz"

On :program:`myloader` it will automatically detect the extension and use::

  --exec-per-thread="/usr/bin/gzip -c -d"
  --exec-per-thread="/usr/bin/zstd -c -d"

to decompress the files by default. However, if you need to change to a different location of the executable, parameters or use a different compression software, you need to set both options::

  --exec-per-thread
  --exec-per-thread-extension

Example
^^^^^^^

If you want to compress with bzip2, you need to run on :program:`mydumper` with this parameters, for example::

  mydumper -o data --clear -T sakila.film \
           --exec-per-thread="/usr/bin/bzip2" \
           --exec-per-thread-extension=".bz2"

You will get this backup files::

  -rw-r--r-- 1 circleci circleci   520 Jul 16 13:59 metadata
  -rw-r----- 1 circleci circleci 11500 Jul 16 13:59 sakila.film.00000.sql.bz2
  -rw-r----- 1 circleci circleci 11360 Jul 16 13:59 sakila.film.00001.sql.bz2
  -rw-r----- 1 circleci circleci   758 Jul 16 13:59 sakila.film-schema.sql.bz2
  -rw-r----- 1 circleci circleci   322 Jul 16 13:59 sakila-schema-create.sql.bz2

On :program:`myloader` then you can use this options::

  myloader -d data -o \
           --exec-per-thread="/usr/bin/bzip2 -d" \
           --exec-per-thread-extension=".bz2"
