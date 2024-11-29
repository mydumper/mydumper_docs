.. image:: ../images/horizontal/color-dark.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-dark

.. image:: ../images/horizontal/color-light.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-light

Installing
==========

Built packages
^^^^^^^^^^^^^^

We build packages for multiple distributions that can be found in the `release section <https://github.com/mydumper/mydumper/releases>`_ of MyDumper

APT
^^^

You need to import the key.
For old versions:

.. code-block::  bash

  sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 79EA15C0E82E34BA

Recommended:

.. code-block::  bash

  wget -qO- 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x1D357EA7D10C9320371BDD0279EA15C0E82E34BA&exact=on' | sudo tee /etc/apt/trusted.gpg.d/mydumper.asc

Ubuntu
------

Source file (/etc/apt/sources.list.d/mydumper.list) should be:

.. code-block::  bash

  deb https://mydumper.github.io/mydumper/repo/apt/ubuntu ./
  #deb https://mydumper.github.io/mydumper/repo/apt/ubuntu/testing ./

Debian
------

Source file (/etc/apt/sources.list.d/mydumper.list) should be:

.. code-block::  bash

  deb https://mydumper.github.io/mydumper/repo/apt/debian ./
  #deb https://mydumper.github.io/mydumper/repo/apt/debian/testing ./

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
  baseurl=https://mydumper.github.io/mydumper/repo/yum/
  enabled=1
  gpgcheck=1

  [mydumper-testing]
  name=MyDumper
  baseurl=https://mydumper.github.io/mydumper/repo/yum/testing/
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



