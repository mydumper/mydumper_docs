Installing
==========

Built pacakges
^^^^^^^^^^^^^^

We build pacakges for multiple distributions that can be found in the `release section <https://github.com/mydumper/mydumper/releases>`_ of MyDumper

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



