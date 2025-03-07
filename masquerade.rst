.. include:: header.rst

.. _mydumper_masquerade:

Masquerade
==========

In today's data-driven world, the need for data privacy and security has never been more critical. Organizations are tasked with safeguarding sensitive information while also ensuring compliance with various regulations such as GDPR and HIPAA. As developers and data engineers, we must find effective ways to obfuscate data without compromising its utility for testing, development, and analytics. This is where masquerade backups come into play, allowing teams to create realistic yet anonymized datasets that maintain the integrity of the original data while protecting personally identifiable information (PII).


Basic Functions
---------------

Since v0.18.1-1, the basic functions %_with_mem has been removed and the remaining random_string, random_int and random_uuid receives the sames parameters:

random_string
^^^^^^^^^^^^^

It replaces the characters in the string. 

random_int
^^^^^^^^^^

It replaces the digits on the numeric value. It doesn't alter the length of the number but could be 0 at the begining.

random_uuid
^^^^^^^^^^^

If MyDumper has been compiled with GLIB version higher than 2.52, it will use `g_uuid_string_random <https://docs.gtk.org/glib/func.uuid_string_random.html>`_ on other cases it will replaces with alphanumeric values the character that are not equal to '-'.

Options
^^^^^^^

**WITH_MEM**: It keeps in memory to replace with same contents, for instance, if it has replaced "AAA" with "BBB", on other rows with "AAA", it will replace it with "BBB".

**REPLACE_NULL**: When the column has a NULL value, it will return a value. If this option is not used, it will return NULL. It recieves an integer as parameter which will determine the size of the new value.

**UNIQUE**: It keeps record of the value that has been used and it will not repeat the same value twice, making it unique for each row.

**MAX_LENGTH**: This setting set the max length of the return value, when the value is not NULL.

Advance
-------

The nexts functions receives parameter that needs to be parsed.

random_format
^^^^^^^^^^^^^

The parameter of this funtion is parsed. It expected 4 kinds of tags: file, string, number and regex. You can also use other character between the tags, for example:

.. code-block::  bash

  `pad`=random_format <file words_alpha.txt.100>-<file words_alpha.txt.100>-<file words_alpha.txt.100>-<file words_alpha.txt.100>-<file words_alpha.txt.100>
  `pad`=random_format <number 9>-<number 9>-<number 9>-<number 9>-<number 9>
  `phone_number`=random_format <regex '.{4}$'><number 4>

The size of tags number or string is specified after the tag name.

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


