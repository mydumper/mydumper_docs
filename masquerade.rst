.. include:: header.rst

.. _mydumper_masquerade:

Masquerade
==========

In today's data-driven world, the need for data privacy and security has never been more critical. Organizations are tasked with safeguarding sensitive information while also ensuring compliance with various regulations such as GDPR and HIPAA. As developers and data engineers, we must find effective ways to obfuscate data without compromising its utility for testing, development, and analytics. This is where masquerade backups come into play, allowing teams to create realistic yet anonymized datasets that maintain the integrity of the original data while protecting personally identifiable information (PII).


Basic Functions
---------------

The nexts functions doesn't receive any parameter and replaces the content of the column.

random_string / random_string_with_mem
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It replaces the characters in the string. It doesn't alter the size of the string.
When random_string_with_mem is used, it keeps in memory to replace with same contents, for instance, if it has replaced "AAA" with "BBB", on other rows with "AAA", it will replace it with "BBB". 

random_int / random_int_with_mem
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It replaces the digits on the numeric value. It doesn't alter the length of the number but could be 0 at the begining.
When random_int_with_mem is used, it keeps in memory to replace with same contents, for instance, if it has replaced 111 with 222, on other rows with 111, it will replace it with 222.

random_uuid / random_uuid_with_mem
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If MyDumper has been compiled with GLIB version higher than 2.52, it will use `g_uuid_string_random <https://docs.gtk.org/glib/func.uuid_string_random.html>`_ on other cases it will replaces with alphanumeric values the character that are not equal to '-'.
When random_uuid_with_mem is used, it keeps in memory to replace with same UUID.

Advance
-------

The nexts functions receives parameter that needs to be parsed.

random_format
^^^^^^^^^^^^^

The parameter of this funtion is parsed. It expected 3 kinds of tags: file, string and number. You can also use other character between the tags, for example:

.. code-block::  bash

  `pad`=random_format <file words_alpha.txt.100>-<file words_alpha.txt.100>-<file words_alpha.txt.100>-<file words_alpha.txt.100>-<file words_alpha.txt.100>
  `pad`=random_format <number 9>-<number 9>-<number 9>-<number 9>-<number 9>  

The size of the number or the string is specified after the tag name.

apply
^^^^^

This functions is used when you want to apply a function to a column which value will be shown. This can not be considered a data masking function, as the content of the column will be written anyways. This is an examples that you can test with sakila.film table:

.. code-block::  bash

  `title`=apply 'concat(' '," - part 2")'
  `last_update`=apply 'date_add(' ', interval 1 year)'


constant
^^^^^^^^

The parameter passed to this function will replace the content of the column. This is useful if you need to calculate values based on other columns, such as:

.. code-block::  bash

  `email`=constant concat(first_name,concat(".",concat(last_name,"@sakilacustomer.org")))
  `tax`=constant price*21/100


