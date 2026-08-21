External commands
-----------------

It is possible to change what mydumper is writing and what myloader is receiving,
with :option:`--exec-per-thread <mydumper --exec-per-thread>` and :option:`--exec-per-thread-extension <mydumper --exec-per-thread-extension>`.

Compression
^^^^^^^^^^^
When you use :option:`mydumper -c/--compress<mydumper -c>`, :program:`mydumper` internally set::

  --exec-per-thread="/usr/bin/gzip -c"
  --exec-per-thread-extension=".gz"

On :program:`myloader` it will automatically detect the extension and use::

  --exec-per-thread="/usr/bin/gzip -c -d"
  --exec-per-thread="/usr/bin/zstd -c -d"

to decompress the files by default. However, if you need to change to a different location of the executable, parameters or use a different compression software, you need to set both options::

  --exec-per-thread
  --exec-per-thread-extension

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

Encryption / Encrypted Backups
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
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
  dump/sbtest_direct-schema-create.sql.enc: openssl enc\'d data with salted password

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
