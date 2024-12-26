.. include:: header.rst

Installing
==========

Built packages
^^^^^^^^^^^^^^

We build packages for multiple distributions that can be found in the `release section <https://github.com/mydumper/mydumper/releases>`_ of MyDumper

APT
^^^

You need to import the key.

In releases older than Debian 12 and Ubuntu 22.04, /etc/apt/keyrings does not exist by default. It SHOULD be created with permissions 0755 if it is needed and does not already exist.

.. code-block::  bash

  wget -qO- 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x1D357EA7D10C9320371BDD0279EA15C0E82E34BA&exact=on' | sudo tee /etc/apt/keyrings/mydumper.asc

Ubuntu
------

Source file (/etc/apt/sources.list.d/mydumper.list) should be:

.. tab:: noble

    .. code-block::  bash

        deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu noble main
        #deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu noble testing


.. tab:: jammy 

    .. code-block::  bash

        deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu jammy main
        #deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu jammy testing


.. tab:: focal

    .. code-block::  bash

        deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu focal main
        #deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu focal testing

  
Debian
------

Source file (/etc/apt/sources.list.d/mydumper.list) should be:

.. tab:: bookworm

    .. code-block::  bash

 
        deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian bookworm main
        #deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian bookworm testing

.. tab:: bullseye

    .. code-block::  bash

        deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian bullseye main
        #deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian bullseye testing

.. tab:: buster

    .. code-block::  bash

        deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian buster main
        #deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian buster testing

YUM
^^^

Fedora/Redhat/CentOS
--------------------

We need to import the GPG key:

.. code-block::  bash

  wget -O GPG-KEY-MyDumper "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x79EA15C0E82E34BA"
  rpm --import GPG-KEY-MyDumper

On /etc/yum.repos.d/mydumper.repo

.. code-block::  bash

  [mydumper]
  name=MyDumper
  baseurl=https://mydumper.github.io/mydumper/repo/yum/$releasever
  enabled=1
  gpgcheck=1

  [mydumper-testing]
  name=MyDumper
  baseurl=https://mydumper.github.io/mydumper/repo/yum/$releasever/testing/
  enabled=0
  gpgcheck=1

FreeBSD
^^^^^^^

By using pkg

.. code-block::  bash

  pkg install mydumper

or from ports

.. code-block::  bash

  cd /usr/ports/databases/mydumper && make install

  
MacOS
^^^^^

By using `Homebrew <https://formulae.brew.sh/formula/mydumper>`_

.. code-block::  bash

  brew install mydumper

Take into account that the mydumper.cnf file is going to be located on /usr/local/etc or /opt/homebrew/etc. So, you might need to run mydumper/myloader with:

.. code-block::  bash

  mydumper --defaults-file=/opt/homebrew/etc/mydumper.cnf
  myloader --defaults-file=/opt/homebrew/etc/mydumper.cnf



