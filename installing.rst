.. include:: header.rst

Installing
==========

Built packages
^^^^^^^^^^^^^^

Packages for multiple distributions can be found in the `release section <https://github.com/mydumper/mydumper/releases>`_ of MyDumper

APT
^^^

In releases older than Debian 12 and Ubuntu 22.04, /etc/apt/keyrings does not exist by default. Hence, it must be created with permissions 0755. 

.. code-block::  bash

  wget -qO- 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x1D357EA7D10C9320371BDD0279EA15C0E82E34BA&exact=on' | sudo tee /etc/apt/keyrings/mydumper.asc

Ubuntu
------

Depending on the operating system, the source file (/etc/apt/sources.list.d/mydumper.list) should include:

.. tab:: noble

    .. code-block::  bash

        deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu noble main
        #deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu noble testing


.. tab:: jammy

    .. code-block::  bash

        deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu jammy main
        #deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu jammy testing


.. tab:: focal

    .. code-block::  bash

        deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu focal main
        #deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/ubuntu focal testing


Debian
------

Depending on the operating system, the source file (/etc/apt/sources.list.d/mydumper.list) should include:

.. tab:: trixie

    .. code-block::  bash


        deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian trixie main
        #deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian trixie testing

.. tab:: bookworm

    .. code-block::  bash


        deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian bookworm main
        #deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian bookworm testing

.. tab:: bullseye

    .. code-block::  bash

        deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian bullseye main
        #deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian bullseye testing

.. tab:: buster

    .. code-block::  bash

        deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian buster main
        #deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/debian buster testing

Ansible
-------

    .. code-block::  bash

        deb [signed-by=/etc/apt/keyrings/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/{{ ansible_distribution | lower }} {{ ansible_distribution_release }} main
        #deb [signed-by=/etc/apt/trusted.gpg.d/mydumper.asc] https://mydumper.github.io/mydumper/repo/apt/{{ ansible_distribution | lower }} {{ ansible_distribution_release }} testing

YUM
^^^

Fedora/Redhat/CentOS
--------------------

The GPG key needs to be imported:

.. code-block::  bash

  wget -O GPG-KEY-MyDumper "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x79EA15C0E82E34BA"
  rpm --import GPG-KEY-MyDumper

And the repository configuration:

.. code-block::  bash

  echo "[mydumper]
  name=MyDumper
  baseurl=https://mydumper.github.io/mydumper/repo/yum/$releasever
  enabled=1
  gpgcheck=1

  [mydumper-testing]
  name=MyDumper
  baseurl=https://mydumper.github.io/mydumper/repo/yum/$releasever/testing/
  enabled=0
  gpgcheck=1" > /etc/yum.repos.d/mydumper.repo

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



