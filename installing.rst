
.. title:: Installing



Installing
==========

To simplify installation and keep your setup up to date, official YUM and APT repositories are available for supported Linux distributions. You can configure your package manager and install the latest packages using the links below:

.. toctree::
   :maxdepth: 2
   :titlesonly:

   APT
   YUM

Built packages
^^^^^^^^^^^^^^

Packages for multiple distributions can be found in the `release section <https://github.com/mydumper/mydumper/releases>`_ of MyDumper

FreeBSD
^^^^^^^

MyDumper can be installed in FreeBSD using: 

.. code-block::  bash

  pkg install mydumper

Or, if it is installled from ports;

.. code-block::  bash

  cd /usr/ports/databases/mydumper && make install
  
MacOS
^^^^^

MyDumper can be installed on MacOS using `Homebrew <https://formulae.brew.sh/formula/mydumper>`_: 

.. code-block::  bash

  brew install mydumper

Bear in mind that since the mydumper.cnf file is going to be located on /usr/local/etc or /opt/homebrew/etc, you might need to run mydumper/myloader with:

.. code-block::  bash

  mydumper --defaults-file=/opt/homebrew/etc/mydumper.cnf
  myloader --defaults-file=/opt/homebrew/etc/mydumper.cnf


Compilation requirements
^^^^^^^^^^^^^^^^^^^^^^^^

Development dependencies
------------------------

MyDumper requires the following development tools and libraries before it can be compiled:

.. tab:: Ubuntu/Debian

    .. code-block::  bash

        apt-get install cmake g++ git
        apt-get install libglib2.0-dev libpcre3-dev libssl-dev

.. tab:: Fedora/Redhat/CentOS

    .. code-block:: bash

        yum install -y cmake gcc gcc-c++ git make
        yum install glib2-devel pcre-devel openssl-devel

.. tab:: Mac OSX

    for MacOS <= 10.13 (High Sierra) < 13 (Ventura) with MacPorts package manager:

    .. code-block:: bash

        sudo port install cmake pkgconfig
        sudo port install glib2 pcre

.. tab:: OpenSUSE

    .. code-block:: bash

        zypper install glib2-devel pcre-devel

MySQL/Percona/MariaDB development libraries
-------------------------------------------

You need to select one vendor development library.

.. tab:: Ubuntu/Debian

    .. code-block::  bash

        apt-get install libmysqlclient-dev
        apt-get install libperconaserverclient20-dev
        apt-get install libmariadbclient-dev


.. tab:: Fedora/Redhat/CentOS

    .. code-block:: bash

        yum install -y mysql-devel
        yum install -y Percona-Server-devel-57
        yum install -y mariadb-devel

.. tab:: Mac OSX

    MacOS <= 10.13 (High Sierra) < 13 (Ventura) with MacPorts package manager

    .. code-block:: bash

        sudo port install mariadb-10.11
        sudo port select mysql

.. tab:: OpenSUSE

    .. code-block:: bash

        zypper install libmysqlclient-devel

CMake
^^^^^

CMake is used by MyDumper's build system and is executed as follows::

  cmake .
  make
  sudo make install

You can optionally provide parameters for CMake, the possible options are:

 * ``-DMYSQL_CONFIG=/path/to/mysql_config`` - The path and filename for the mysql_config executable
 * ``-DCMAKE_INSTALL_PREFIX=/install/path`` - The path where mydumper should be installed

You have to make sure that pkg-config, mysql_config and pcre-config are all in $PATH

Binlog dump is disabled by default, to compile with it you need to add -DWITH_BINLOG=ON to cmake options

To build against mysql libs < 5.7, you need to disable SSL adding -DWITH_SSL=OFF

