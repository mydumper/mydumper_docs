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
