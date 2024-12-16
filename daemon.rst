.. image:: ../images/horizontal/color-dark.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-dark

.. image:: ../images/horizontal/color-light.svg
  :width: 50%
  :alt: MyDumper's logo
  :class: only-light

Daemon
------
Daemon mode does things a little differently.  There are the directories ``0``
and ``1`` inside the dump directory.  These alternate when dumping so that if
mydumper fails for any reason there is still a good snapshot.  When a snapshot
dump is complete the ``last_dump`` symlink is updated to point to that dump.
