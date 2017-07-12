.. figure:: images/white-logo.png
   :alt: SlamData Logo

.. raw:: html

    <embed>
    <script>
      !function(){var analytics=window.analytics=window.analytics||[];if(!analytics.initialize)if(analytics.invoked)window.console&&console.error&&console.error("Segment snippet included twice.");else{analytics.invoked=!0;analytics.methods=["trackSubmit","trackClick","trackLink","trackForm","pageview","identify","reset","group","track","ready","alias","debug","page","once","off","on"];analytics.factory=function(t){return function(){var e=Array.prototype.slice.call(arguments);e.unshift(t);analytics.push(e);return analytics}};for(var t=0;t<analytics.methods.length;t++){var e=analytics.methods[t];analytics[e]=analytics.factory(e)}analytics.load=function(t){var e=document.createElement("script");e.type="text/javascript";e.async=!0;e.src=("https:"===document.location.protocol?"https://":"http://")+"cdn.segment.com/analytics.js/v1/"+t+"/analytics.min.js";var n=document.getElementsByTagName("script")[0];n.parentNode.insertBefore(e,n)};analytics.SNIPPET_VERSION="4.0.0";
      analytics.load("ZVWTrB6ijnF00L6ZENqtGzMCjfIauXvo");
      analytics.page();
      }}();
    </script>
    </embed>
    
Reference - SlamDown
====================

This SlamDown Reference can assist with the correct formatting of
SlamDown code to produce static and interactive forms within SlamData.


Section 1 - Introduction
------------------------

SlamData contains its own markup language called SlamDown, that is
useful for creating reports and forms. SlamDown is a subset of
`CommonMark <http://commonmark.org/>`__, a specification for a highly
compatible implementation of
`Markdown <https://en.wikipedia.org/wiki/Markdown>`__.

In addition, SlamDown also includes two extensions to CommonMark:
`form fields <#form-elements>`__ and
`evaluated SQL² queries <#evaluated-sql-query>`__.


Section 2 - Block Elements
--------------------------

The following SlamDown elements create blocks of content.


2.1 Horizontal Rules
~~~~~~~~~~~~~~~~~~~~

Three dashes or more create a horizontal line. Put a blank line above
and below the dashes.

Example:

::

    Text here

    ---

    More text here

This results in the following output:

Text here

--------------

More text here


2.2 Headers
~~~~~~~~~~~

Use hash marks (``#``) for `ATX
headers <http://spec.commonmark.org/0.22/#atx-header>`__, with one
hash mark for each level.

Example:

::

    # Top level  
    ## Second level
    ### Third level  

This results in a first, second, and third level heading, as follows:

|Headers|


2.3 Code Blocks
~~~~~~~~~~~~~~~

You can create blocks of code (that is, literal content in monospace
font) in two ways:

**1. Indented code blocks**

Indent by four spaces.

Example:

.. raw:: html

   <pre>
       for (int i = 0; i < 10; i++)
           sum += myArray[i];
   </pre>

**2. Fenced code blocks**

Start and end with three or more backtick (\`) characters.

Example:

::

    ```
    for (int i = 0; i < 10; i++)
        sum += myArray[i];
    ```

Both **Indented Code Blocks** and **Fenced Code Blocks** result in the following output:

::

    for (int i = 0; i < 10; i++)
        sum += myArray[i];


2.4 Paragraphs
~~~~~~~~~~~~~~

Paragraphs are separated by a blank line.

Example:

::

    This is paragraph 1.

    This is paragraph 2.

This results in the following output:

This is paragraph 1.

This is paragraph 2.


2.5 Block quotes
~~~~~~~~~~~~~~~~

Start with a greater than sign (``>``) to create a block quote.

Example:

::

    > This is a block quote.

This results in the following output:

    This is a block quote.


2.6 Lists
~~~~~~~~~

Ordrered lists start with numbers followed by a full stop (``.``). The actual
numbers in the SlamDown do not matter, as the list will be
displayed with ascending indices.

Example:

::

    1. First item
    2. Second item
    3. Third item

This results in the following output:

1. First item
2. Second item
3. Third item

Unordered lists start with either an asterisk (``*``), dash (``-``), or
a plus sign (``+``). All three are interchangeable.

Example:

::

    - First item
    - Second item
    - Third item

This results in the following output:

-  First item
-  Second item
-  Third item


Section 3 - Inline Elements
---------------------------

The following inline elements are supported in SlamDown. In addition to
standard Markdown elements, there is also the ability to `evaluate an SQL
query <#evaluated-sql-query>`__ and put the result into the content.


3.1 Emphasis and Strong Emphasis
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Surround content with asterisks (``*``) for emphasis and surround it
with double asterisks (``**``) for strong emphasis.

Example:

::

    This is *important*. This is **more important**.

This results in the following output:

This is *important*. This is **more important**.


3.2 Links
~~~~~~~~~

Links contain the link title in square brackets (``[]``) and the link
destination in parentheses (``()``).

Example:

::

    [SlamData](http://slamdata.com)

This results in the following output:

`SlamData <http://slamdata.com>`__

If the link title and destination are the same, an autolink can be used,
where the URI is contained in angled brackets (``<>``).

Example:

::

    <http://slamdata.com>

This results in the following output:

http://slamdata.com


3.3 Images
~~~~~~~~~~

Images start with an explanation mark (``!``), followed by the image
description in square brackets (``[]``) and the image URI in parentheses
(``()``).

Example:

::

    ![SlamData Logo](https://media.licdn.com/media/p/6/005/088/002/039b9f8.png)

This results in the following output:

|LogoLink|

.. |LogoLink| image:: https://media.licdn.com/media/p/6/005/088/002/039b9f8.png


3.4 Inline code formatting
~~~~~~~~~~~~~~~~~~~~~~~~~~

To add code formatting (literal content with monospace font) inline, put
the content between backtick (\`) characters.

Example:

::

    Start SQL statements with `SELECT * FROM`

This results in the following output:

Start SQL statements with ``SELECT * FROM``


Section 4 - Evaluated SQL² Queries
----------------------------------

SlamDown extends Markdown by allowing you to evaluate an SQL² query and
insert the results into the rendered content, including the form
elements listed in Section 5 below. Start the query with an
exclamation point and then contain the SQL² query between double backtick
(``````) characters.

.. hint:: **Backticks**

	Notice how the path to the query below has a space between the
	backtick that ends the path (`````) and the double backticks (``````)
	that end the query.
	This is a necessary space because three backticks in a row start a
	Fenced Code Block as stated above.

In the following example, there are 20 documents in the ``/col`` file.

::

    There are !``SELECT COUNT(*) FROM `/col` `` documents inside the collection.

This results in the following output:

There are ``20`` documents inside the collection.

SQL² queries are always surrounded by double backticks (``````) and
preceded with an exclamation point (``!``).  Additionally, they
may be surrounded by parentheses (``()``) for radio buttons,
braces (``{}``) for dropdowns, and brackets (``[]``) for check boxes
as seen in later sections.


Section 5 - Form Elements
-------------------------

Form elements provide interactive forms for user's with text fields,
date pickers, check boxes, and so on.

First define a variable name in Slamdown and then define the 
element type based on the formatting in the sections below.

Example:

::

	name = ____

This defines the variable ``name`` and creates a simple text
entry field in the browser.  This variable can then be used
in a **Query Card**.

Example:

.. code-block:: sql

	SELECT address, phone_number, city, state
	FROM `/mydb/mytable`
	WHERE fullname = :name

Note that the variable name needs to be preceded by a colon (``:``) when
referencing it as a variable inside a **Query Card**.


5.1 Text Field
~~~~~~~~~~~~~~

Use one or more underscores (``_``) to create a text input field where a
user can add text.

The following code creates an input file for a user's interests.
The value can then be referred to as ``:interests``.

Example:

::

    interests = ________

Optionally, the input field can be pre-filled with a default value by
having it after the underscores in parentheses. The following
code creates an input field called ``interests`` with a default value of "SlamData".
The value can then be referred to as ``:interests``.

Example:

::

    interests = ________ (SlamData)


5.2 Numeric Field
~~~~~~~~~~~~~~~~~

By default, input fields are evaluated as string types. To enforce a
numeric type, prefix the underscores with the (``#``) symbol.
A default value can also be provided.

Example:

::

    year = #________  (1999)


5.3 Radio Buttons
~~~~~~~~~~~~~~~~~

A set of radio buttons has only one button selected at a time.  Radio buttons
can be populated with static content or populated by a query.


5.3.1 Static Radio Buttons
''''''''''''''''''''''''''

Use parentheses followed by text to indicate radio buttons.  Indicate which
button is selected by putting an ``x`` in the parentheses.

This following code creates a set of radio buttons with the values
"car", "bus", and "bike", where "bus" is marked as the default. The
result is stored in the string variable named ``commute`` for later use.

Example:

::

    commute = () car (x) bus () bike

This results in the following output:

|Radio-Buttons-Static|

Note that the default selection became the first selection when the radio
buttons are rendered.


5.3.2 Dynamic Radio Buttons
'''''''''''''''''''''''''''

As with all other form elements, radio buttons may be populated by
means of an evaluated SQL² query.

The following code creates a set of radio buttons that
list the unique color values in a database.

Example:

.. code-block:: sql

	mycolor =
	(!``SELECT DISTINCT(color) FROM `/devguide/devdb/colors` ORDER BY color ASC LIMIT 1``)
	!``SELECT DISTINCT(color) FROM `/devguide/devdb/colors` ORDER BY color ASC``

First, note how the field is defined on multiple lines.

Second, there are now two queries instead of one.  The first query defines which value
is selected by default, the second query defines the remaining values.

This results in the following output:

|Radio-Buttons-Dynamic|


5.4 Checkboxes
~~~~~~~~~~~~~~

Use brackets (``[]``) followed by text to indicate checkboxes.
In a set of checkboxes each checkbox operates independently.

A checkbox array variable can be used in a query whether it was
defined statically in SlamDown or dynamically through an evaluated
SQL² query.  An example query within a **Query Card** would look
as follows.

Example:

::

	SELECT *
        FROM `/mydb/mytable`
        WHERE phone IN :phones


5.4.1 Static Check Boxes
''''''''''''''''''''''''

Use an ``x`` in the square brackets to indicate that the checkbox
should be checked by default. The string value returned will be an
array of strings in brackets.

The following code creates a set of checkboxes with the values
"Android", "iPhone", and "Blackberry". The result is stored in the
string variable named ``phones`` for later use.

Example:

::

	phones = [x] iPhone [] Blackberry [x] Android 

This results in the following output:

|Check-Boxes-Static|

Similar to the behavior of radio buttons, the fields pre-selected with an ``x``
are rendered first.

The selections above would result in the ``phones`` variable array containing
the following values:  [``"iPhone"``, ``"Android"``]


5.4.2 Dynamic Check Boxes
'''''''''''''''''''''''''

As with all other form elements, checkboxes may be populated by
means of an evaluated SQL² query.

The following code creates a set of checkboxes that
list the phone types within a database.

Example:

.. code-block:: sql

	myphone =
	[!``SELECT DISTINCT(phone) FROM `/mydb/mytable` ORDER BY phone ASC LIMIT 1``]
	!``SELECT DISTINCT(phone) FROM `/mydb/mytable` ORDER BY phone ASC``

This results in the following output:

|Check-Boxes-Dynamic|

The first query defines which value is selected by default, the second query
populates the remaining checkboxes.


5.5 Dropdowns
~~~~~~~~~~~~~

Dropdowns allow user's to select one (and only one) value from a list
of options, similar to radio buttons.  Unlike radio buttons, however,
dropdown elements typically take up less space in the browser and
are more suitable for longer lists of values.

Use a comma-separated list in braces (``{}``) to indicate a dropdown
element.

A dropdown array variable can be used in a query whether it was
defined statically in SlamDown or dynamically through an evaluated
SQL² query.  An example query within a **Query Card** would look
as follows.

Example:

::

	SELECT *
        FROM `/mydb/mytable`
        WHERE city IN :mycity


5.5.1 Static Dropdown
'''''''''''''''''''''

Define a static dropdown element by placing the values of array
elements within braces (``{}``).

The following code creates a dropdown element with BOS, SFO, and NYC
entries. The result is stored in an array variable named ``city`` for
later use.

Example:

::

    city = {BOS, SFO, NYC}

This results in the following output:

|Dropdown-Static|

Optionally, include a default value by listing it in parentheses at the
end. In the following example, NYC is set as the default.

Example:

::

    city = {BOS, SFO, NYC} (NYC)


5.5.2 Dynamic Dropdown
''''''''''''''''''''''

As with all other form elements, dropdown elements may be populated by
means of an evaluated SQL² query.

The following code creates a dropdown that contains the
names of cities within a database.

Example:

.. code-block:: sql

	mycity = {!``SELECT DISTINCT(city) FROM `/mydb/mytable` ORDER BY city ASC``}


5.6 Dates and Times
~~~~~~~~~~~~~~~~~~~

Provide a date, time or both date and time selector by
implementing the following syntax.


5.6.1 Date
''''''''''

The following code creates a date selector element and
stores the value in a variable called ``start``.

Example:

::

	start = ____-__-__ (2016-04-19)

This results in the following output:

|Date-Only|


5.6.2 Time
''''''''''

The following code creates a time selector element.

Example:

::

	start = __:__ (02:30 PM)

This results in the following output:

|Time-Only|


5.6.3 Date & Time (TIMESTAMP)
'''''''''''''''''''''''''''''

The following code creates both a date and time selector element.

Example:

::

	start = ____-__-__ __:__ (2016-04-19 14:00)

This results in the following output:

|Date-And-Time|


Section 6 - Slamdown Variables in Queries
-----------------------------------------

SlamData has the ability to use values selected in SlamDown form elements
to be used in a query.  For more information and examples, see
`Section 11 <sql-squared-reference.html#section-11-variables-and-sql2>`__ of
the SQL² Reference Guide.



.. |Headers| image:: images/SD4/screenshots/fake-levels.png

.. |Radio-Buttons-Static| image:: images/SD4/screenshots/radio-buttons-static.png

.. |Radio-Buttons-Dynamic| image:: images/SD4/screenshots/radio-buttons-dynamic.png

.. |Check-Boxes-Static| image:: images/SD4/screenshots/check-boxes-static.png

.. |Check-Boxes-Dynamic| image:: images/SD4/screenshots/check-boxes-dynamic.png

.. |Dropdown-Static| image:: images/SD4/screenshots/dropdown-static.png

.. |Date-Only| image:: images/SD4/screenshots/date-only.png

.. |Time-Only| image:: images/SD4/screenshots/time-only.png

.. |Date-And-Time| image:: images/SD4/screenshots/date-and-time.png

.. raw:: html

    <embed>
    <script type="text/javascript" id="hs-script-loader" async defer src="//js.hs-scripts.com/2389041.js"></script>
    </embed>

