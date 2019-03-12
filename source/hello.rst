hello,world
=============
This is a paragraph.  It's quite
short.

   This paragraph will result in an indented block of
   text, typically used for quoting other text.

This is another one.

*emphasis*

**strong emphasis**

``printf("%s","hello world")``

thisis\ *one*\ word

* This is a bulleted list.
* It has two items, the second
  item uses two lines.

1. This is a numbered list.
2. It has two items too.

#. This is a numbered list.
#. It has two items too.

* this is
* a list

  * with a nested list
  * and some subitems

* and here the parent list continues

term
   Definition of the term, which must be indented

   and can even consist of multiple paragraphs

next term
   Description.

| These lines are
| broken exactly like in
| the source file.

:fieldname: Field content

This is a typical paragraph.  An indented literal block follows.

::

    for a in [5,4,3,2,1]:   # this is program code, shown as-is
        print a
    print "it's..."
    # a literal block continues until the indentation ends

This text has returned to the indentation of the first paragraph,
is outside of the literal block, and is therefore treated as an
ordinary paragraph.

+------------------------------+
| paragraph                    |
| (ends with "::")             |
+------------------------------+
   +---------------------------+
   | indented literal block    |
   +---------------------------+

John Doe wrote::

>> Great idea!
>
> Why didn't I think of that?

You just did!  ;-)

This is a normal text paragraph. The next paragraph is a code sample::

   It is not processed in any way, except
   that the indentation is removed.

   It can span multiple lines.

This is a normal text paragraph again.

>>> 1 + 1
2

+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | ...        | ...      |          |
+------------------------+------------+----------+----------+

=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======

.. math::

   (a + b)^2 = a^2 + 2ab + b^2

   (a - b)^2 = a^2 - 2ab + b^2

.. math::

   (a + b)^2  &=  (a + b)(a + b) \\
              &=  a^2 + 2ab + b^2

.. math:: (a + b)^2 = a^2 + 2ab + b^2

=================
This is a heading
=================


=====
hello
=====


#####
what
#####

*****
are
*****

=====
you
=====

-----
dong
-----

[#]_ is a reference to footnote 1, and [#]_ is a reference to
footnote 2.

.. [#] This is footnote 1.
.. [#] This is footnote 2.
.. [#] This is footnote 3.

[#]_ is a reference to footnote 3.

.. DANGER::
   Beware killer rabbits!

.. ATTENTION::
  Beware killer rabbits!

.. CAUTION::
  Beware killer rabbits!

.. ERROR::
  Beware killer rabbits!

.. HINT::
  Beware killer rabbits!

.. IMPORTANT::
  Beware killer rabbits!

.. NOTE::
  Beware killer rabbits!

.. TIP::
  Beware killer rabbits!

.. WARNING::
  Beware killer rabbits!

.. note:: This is a note admonition.
   This is the second line of the first paragraph.

   - The note contains all indented body elements
     following.
   - It includes this bullet list.

.. csv-table:: Frozen Delights!(A csv table)
   :header: "Treat", "Quantity", "Description"
   :widths: 15, 10, 30

   "Albatross", 2.99, "On a stick!"
   "Crunchy Frog", 1.49, "If we took the bones out, it wouldn't be
   crunchy, now would it?"
   "Gannet Ripple", 1.99, "On a stick!"

.. list-table:: Frozen Delights!(A list table)
   :widths: 15 10 30
   :header-rows: 1

   * - Treat
     - Quantity
     - Description
   * - Albatross
     - 2.99
     - On a stick!
   * - Crunchy Frog
     - 1.49
     - If we took the bones out, it wouldn't be
       crunchy, now would it?
   * - Gannet Ripple
     - 1.99
     - On a stick!

.. class:: multiple

   First paragraph.

   Second paragraph.

.. role:: custom
An example of using :custom:`interpreted text`

.. role:: custom(emphasis)

:custom:`text`
