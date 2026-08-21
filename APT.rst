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

