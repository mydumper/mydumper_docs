.. image:: ../images/horizontal/color-dark.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-dark

.. image:: ../images/horizontal/color-light.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-light

Compiling
=========

Requirements
------------

Development Tools
^^^^^^^^^^^^^^^^^

MyDumper requires the following development tools before it can be compiled:

Ubuntu/Debian
~~~~~~~~~~~~~

.. code-block::  bash

  apt-get install cmake g++ git

Fedora/Redhat/CentOS
~~~~~~~~~~~~~~~~~~~~

.. code-block::  bash

  yum install -y cmake gcc gcc-c++ git make


Mac OSX
~~~~~~~

for MacOS <= 10.13 (High Sierra) < 13 (Ventura) with MacPorts package manager:

.. code-block::  bash

  sudo port install cmake pkgconfig



Additionally the following packages are optional:

 * `python-sphinx <https://www.sphinx-doc.org/>`_ (for documentation)

Development Libraries
^^^^^^^^^^^^^^^^^^^^^

Ubuntu/Debian
~~~~~~~~~~~~~

.. code-block::  bash

   apt-get install libglib2.0-dev libpcre3-dev libssl-dev

Fedora/Redhat/CentOS
~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   yum install glib2-devel pcre-devel openssl-devel

OpenSUSE
~~~~~~~~

.. code-block:: bash

   zypper install glib2-devel pcre-devel

Mac OSX
~~~~~~~

.. code-block:: bash

   port install glib2 pcre

MySQL/Percona/MariaDB development libraries
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You need to select one vendor development library.


Ubuntu/Debian
~~~~~~~~~~~~~

.. code-block::  bash

  apt-get install libmysqlclient-dev
  apt-get install libperconaserverclient20-dev
  apt-get install libmariadbclient-dev


Fedora/Redhat/CentOS
~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

  yum install -y mysql-devel
  yum install -y Percona-Server-devel-57
  yum install -y mariadb-devel

OpenSUSE
~~~~~~~~

.. code-block:: bash

  zypper install libmysqlclient-devel

Mac OSX
~~~~~~~

MacOS <= 10.13 (High Sierra) < 13 (Ventura) with MacPorts package manager


.. code-block:: bash

  sudo port install mariadb-10.11
  sudo port select mysql

CMake
-----

CMake is used for MyDumper's build system and is executed as follows::

  cmake .
  make
  sudo make install


You can optionally provide parameters for CMake, the possible options are:

 * ``-DMYSQL_CONFIG=/path/to/mysql_config`` - The path and filename for the mysql_config executable
 * ``-DCMAKE_INSTALL_PREFIX=/install/path`` - The path where mydumper should be installed

One has to make sure, that pkg-config, mysql_config, pcre-config are all in $PATH

Binlog dump is disabled by default to compile with it you need to add -DWITH_BINLOG=ON to cmake options

To build against mysql libs < 5.7 you need to disable SSL adding -DWITH_SSL=OFF

Documentation
-------------

If you wish to just compile the documentation you can do so with::

  cmake .
  make doc_html

or for a man page output::

  cmake .
  make doc_man
