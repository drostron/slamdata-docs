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

Reference - SQL²
================


Section 1 - Introduction
------------------------

SQL² is a subset of ANSI SQL. SQL² is designed for queries on NoSQL database systems.

SQL² has support for every major SQL SELECT clause, such as ``AS``,
``WHERE``, ``JOIN``, ``GROUP BY``, ``HAVING``, ``LIMIT``, ``OFFSET``,
``CROSS``, and so on. It follows PostgreSQL where SQL dialects diverge.


1.1 Data Types
~~~~~~~~~~~~~~

The following data types are used by SQL².

.. note::

  Some data types are not natively supported by all database systems.
  Instead, they are emulated by SlamData, meaning that you can use them as
  if they were supported by the database system.

+----------+-----------------------------------+---------------------------------------+
| Type     | Description                       | Examples                              |
+==========+===================================+=======================================+
| Null     | Indicates missing information.    | ``null``                              |
+----------+-----------------------------------+---------------------------------------+
| Boolean  | true or false                     | ``true``, ``false``                   |
+----------+-----------------------------------+---------------------------------------+
| Integer  | Whole numbers (no fractional      | ``1``, ``-2``                         |
|          | component)                        |                                       |
+----------+-----------------------------------+---------------------------------------+
| Decimal  | Decimal numbers (optional         | ``1.0``, ``-2.19743``                 |
|          | fractional components)            |                                       |
+----------+-----------------------------------+---------------------------------------+
| String   | Text                              | ``"221B Baker Street"``               |
+----------+-----------------------------------+---------------------------------------+
| DateTime | Date and time, in ISO8601 format  | ``TIMESTAMP("2004-10-19T10:23:54Z")`` |
+----------+-----------------------------------+---------------------------------------+
| Time     | Time in the format HH:MM:SS.      | ``TIME("10:23:54")``                  |
+----------+-----------------------------------+---------------------------------------+
| Date     | Date in the format YYYY-MM-DD     | ``DATE("2004-10-19")``                |
+----------+-----------------------------------+---------------------------------------+
| Interval | Time interval, in ISO8601 format  | ``INTERVAL("P3DT4H5M6S")``            |
+----------+-----------------------------------+---------------------------------------+
| Object ID| Unique object identifier.         | ``OID("507f1f77bcf86cd799439011")``   |
+----------+-----------------------------------+---------------------------------------+
| Ordered  | Ordered list with no duplicates   | ``(1, 2, 3)``                         |
| Set      | allowed                           |                                       |
+----------+-----------------------------------+---------------------------------------+
| Array    | Ordered list with duplicates      | ``[1, 2, 2]``                         |
|          | allowed                           |                                       |
+----------+-----------------------------------+---------------------------------------+


1.2 Clauses, Operators, and Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following clauses are supported:

+---------------+---------------------------------------------------------------------------------------+
| Type          | Clauses                                                                               |
+===============+=======================================================================================+
| Basic         | ``SELECT``, ``AS``, ``FROM``                                                          |
+---------------+---------------------------------------------------------------------------------------+
| Joins         | ``LEFT OUTER JOIN``, ``RIGHT OUTER JOIN``, ``INNER JOIN``, ``FULL JOIN``, ``CROSS``   |
+---------------+---------------------------------------------------------------------------------------+
| Filtering     | ``WHERE``                                                                             |
+---------------+---------------------------------------------------------------------------------------+
| Grouping      | ``GROUP BY``, ``HAVING``, ``ARBITRARY``                                               |
+---------------+---------------------------------------------------------------------------------------+
| Conditional   | ``CASE`` , ``WHEN``, ``DEFAULT``                                                      |
+---------------+---------------------------------------------------------------------------------------+
| Paging        | ``LIMIT``, ``OFFSET``                                                                 |
+---------------+---------------------------------------------------------------------------------------+
| Sorting       | ``ORDER BY`` , ``DESC``, ``ASC``                                                      |
+---------------+---------------------------------------------------------------------------------------+

The following operators are supported:

+--------------+------------------------------------------------------------------+
| Type         | Operators                                                        |
+==============+==================================================================+
| Numeric      | ``+``, ``-``, ``*``, ``/``, ``%``                                |
+--------------+------------------------------------------------------------------+
| String       | ``~`` , ``~*``, ``!~``, ``!~*``, ``LIKE``, ``||``                |
+--------------+------------------------------------------------------------------+
| Array        | ``||``, ``[ ... ]``                                              |
+--------------+------------------------------------------------------------------+
| Relational   | ``=``, ``>=``, ``<=``, ``<>``, ``BETWEEN``, ``IN``, ``NOT IN``   |
+--------------+------------------------------------------------------------------+
| Boolean      | ``AND``, ``OR``, ``NOT``                                         |
+--------------+------------------------------------------------------------------+
| Projection   | ``foo.bar``, ``foo[2]``, ``foo{*}``, ``foo[*]``                  |
+--------------+------------------------------------------------------------------+
| Date/Time    | ``TIMESTAMP``, ``DATE``, ``INTERVAL``, ``TIME``                  |
+--------------+------------------------------------------------------------------+
| Identity     | ``OID``                                                          |
+--------------+------------------------------------------------------------------+

.. note::

  ``~`` , ``~*``, ``!~``, and ``!~*`` are regular expression
  operators. ``~*``, ``!~``, and ``!~*`` are preliminary and may not
  work in the current release.

.. note::

  The ``||`` operator for strings will concatenate two
  strings. For example, you can create a full name from a first and last
  name property: \ ``c.firstName || ' ' || c.lastName``. The ``||``
  operator for arrays will concatenate two arrays; for example, if ``xy``
  is an array with two values, then ``c.xy || [0]`` will create an array
  with three values, where the third value is zero.

The following functions are supported:

+---------------+---------------------------------------------------------------------------+
| Type          | Functions                                                                 |
+===============+===========================================================================+
| String        | ``CONCAT``, ``LOWER``, ``UPPER``, ``SUBSTRING``, ``LENGTH``, ``SEARCH``   |
+---------------+---------------------------------------------------------------------------+
| DateTime      | ``DATE_PART``, ``TO_TIMESTAMP``                                           |
+---------------+---------------------------------------------------------------------------+
| Nulls         | ``COALESCE``                                                              |
+---------------+---------------------------------------------------------------------------+
| Arrays        | ``ARRAY_LENGTH``, ``FLATTEN_ARRAY``                                       |
+---------------+---------------------------------------------------------------------------+
| Objects       | ``FLATTEN_OBJECT``                                                        |
+---------------+---------------------------------------------------------------------------+
| Set-Level     | ``DISTINCT``, ``DISTINCT_BY``                                             |
+---------------+---------------------------------------------------------------------------+
| Aggregation   | ``COUNT``, ``SUM``, ``MIN``, ``MAX``, ``AVG``                             |
+---------------+---------------------------------------------------------------------------+
| Identity      | ``SQUASH``                                                                |
+---------------+---------------------------------------------------------------------------+


Section 2 - Basic Selection
---------------------------

The ``SELECT`` statement returns a result set of records from one or
more tables.


2.1 Select all values from a path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To select all values from a path, use the asterisk (``*``).

Example:

.. code-block:: sql

    SELECT *
    FROM `/users`


2.2 Select specific fields from a path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To select specific fields from a path, use the field names, separated by
commas.

Example:

.. code-block:: sql

    SELECT name, age
    FROM `/users`


2.3 Path Aliases
~~~~~~~~~~~~~~~~

Follow the path name with an ``AS`` and an alias name, and then you can
use the alias name when specifying the fields. This is especially useful
when you have data from more than one source.

Example:

.. code-block:: sql

    SELECT c.name, c.age
    FROM `/users` AS c


Section 3 - Filtering a Result Set
----------------------------------

You can filter a result set using the WHERE clause. The following
operators are supported:

-  Relational: ``-``, ``=``, ``>=``, ``<=``, ``<>``, ``BETWEEN``,
   ``IN``, ``NOT IN``
-  Boolean: ``AND``, ``OR``, ``NOT``


3.1 Filtering using a numeric value
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example:

.. code-block:: sql

    SELECT c.name
    FROM `/users` AS c
    WHERE c.age > 40


3.2 Filtering using a string value
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example:

.. code-block:: sql

    SELECT c.name
    FROM `/users` AS c
    WHERE c.name = "Sherlock Holmes"


3.3 Filtering using multiple Boolean predicates
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example:

.. code-block:: sql

    SELECT
      c.name FROM `/users` AS c
    WHERE
      c.name = "Sherlock Holmes" AND
      c.street = "Baker Street"


Section 4 - Numeric and String Operations
-----------------------------------------

You can use any of the operators or functions listed in the `Clauses,
Operators, and Functions <#clauses-operators-and-functions>`__ section on
numbers and strings. Some common string operators and functions include:

+------------------------+----------------------------+
| Operator or Function   | Description                |
+========================+============================+
| ``||``                 | Concatenates               |
+------------------------+----------------------------+
| ``LOWER``              | Converts to lowercase      |
+------------------------+----------------------------+
| ``UPPER``              | Converts to uppercase      |
+------------------------+----------------------------+
| ``SUBSTRING``          | Returns a substring        |
+------------------------+----------------------------+
| ``LENGTH``             | Returns length of string   |
+------------------------+----------------------------+

4.1 - Examples
~~~~~~~~~~~~~~

Using mathematical operations:

.. code-block:: sql

    SELECT c.age + 2 * 1 / 4 % 2
    FROM `/users` AS c

Concatenating strings:

.. code-block:: sql

    SELECT c.firstName || ' ' || c.lastName AS name
    FROM `/users` AS c

Filtering by fuzzy string comparison using the ``LIKE`` operator:

.. code-block:: sql

    SELECT * FROM `/users` AS c
    WHERE c.firstName LIKE "%Joan%"

Filtering by regular expression:

.. code-block:: sql

    SELECT * FROM `/users` AS c
    WHERE c.firstName ~ "[sS]h+""


Section 5 - Dates and Times
---------------------------

Filter by dates and times using the ``TIMESTAMP``, ``TIME``, and
``DATE`` operators. The ``DATE_PART`` operator can also be used
to select part of a date, such as the day.

.. note::

  Some database systems will automatically convert strings into dates
  or date/times. SlamData does not perform this conversion, since the
  underlying database system has no schema and no fixed type for any field. As a
  result, an expression like ``WHERE ts > "2015-02-10"`` compares
  string-valued ``ts`` fields with the string ``"2015-02-10"`` instead of
  a date comparison.

If you want to embed literal dates, timestamps, etc. into your SQL
queries, you should use the time conversion operators, which accept
a string and return value of the appropriate type. For example, the
above snippet could be converted to
``WHERE ts > DATE("2015-02-10")``, which looks for date-valued
``ts`` fields and compares them with the date ``2015-02-10``.

.. note:: **MongoDB Users**

  If your MongoDB data does not use MongoDB's native date/time type,
  and instead, you store your timestamps as epoch milliseconds in a
  numeric value, then you should either compare numbers or use the
  ``TO_TIMESTAMP`` function.


5.1 Filter based on a timestamp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``TIMESTAMP`` operator to convert a string into a date and time.
The string should have the format ``YYYY-MM-DDTHH:MM:SSZ``.

Example:

.. code-block:: sql

    SELECT *
    FROM `/log/events` AS c
    WHERE c.ts > TIMESTAMP("2015-04-29T15:16:55Z")


5.2 Filter based on a time
~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``TIME`` operator to convert a string into a time. The string
should have the format ``HH:MM:SS``.

Example:

.. code-block:: sql

    SELECT *
    FROM `/log/events` AS c
    WHERE c.ts > TIME("15:16:55")


5.3 Filter based on a date
~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``DATE`` operator to convert a string into a date. The string
should have the format ``YYYY-MM-DD``.

Example:

.. code-block:: sql

    SELECT *
    FROM `/log/events` AS c
    WHERE c.ts > DATE("2015-04-29")


5.4 Filter based on part of a date
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``DATE_PART`` function to select part of a date. ``DATE_PART``
has two arguments: a string that indicates what part of the date or time
that you want and a timestamp field. Valid values for the first argument
are ``century``, ``day``, ``decade``, ``dow`` (day of week), ``doy`` (day of year),
``epoch``, ``hour``, ``isodow``, ``isoyear``,  ``microseconds``, ``millennium``,
``milliseconds``, ``minute``, ``month``, ``quarter``, ``second``, ``week`` and
``year``, although some values are not supported by all connectors.

Example:

.. code-block:: sql

    SELECT DATE_PART("day", c.ts)
    FROM `/log/events` AS c


5.5 Filter based on a Unix epoch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``TO_TIMESTAMP`` function to convert Unix epoch (milliseconds)
to a timestamp.

Example:

.. code-block:: sql

    SELECT *
    FROM `/log/events` AS c
    WHERE c.ts > TO_TIMESTAMP(1446335999)


Section 6 - Grouping
--------------------

SQL² allows you to group data by fields and by date parts.


6.1 Group based on a single field
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use ``GROUP BY`` to group results by a field.

Example:

.. code-block:: sql

    SELECT
        c.age,
        COUNT(*) AS cnt
    FROM `/users` AS c
    GROUP BY c.age


6.2 Group based on multiple fields
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can group by multiple fields with a comma-separated list of fields
after ``GROUP BY``.

Example:

.. code-block:: sql

    SELECT
        c.age,
        c.gender,
        COUNT(*) AS cnt
    FROM `/users` AS c
    GROUP BY c.age, c.gender


6.3 Group based on date part
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``DATE_PART`` function to group by a part of a date, such as the
month.

Example:

.. code-block:: sql

    SELECT
        DATE_PART("day", c.ts) AS day,
        COUNT(*) AS cnt
    FROM `/log/events` AS c
    GROUP BY DATE_PART("day", c.ts)


6.4 Filter within a group
~~~~~~~~~~~~~~~~~~~~~~~~~

Filter results within a group by adding a ``HAVING`` clause followed by
a Boolean predicate.

Example:

.. code-block:: sql

    SELECT
        DATE_PART("day", c.ts) AS day,
        COUNT(*) AS cnt
    FROM `/prod/purger/events` AS c
    GROUP BY DATE_PART("day", c.ts)
    HAVING c.gender = "female"


6.5 Filter with Arbitrary Value
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

``ARBITRARY`` returns an arbitrary value from a set.  Each target
data source may implement this differently but is intended to retrieve
a single value from a set in the cheapest way, and is not necessarily
deterministic.


6.6 Double grouping
~~~~~~~~~~~~~~~~~~~

Perform double-grouping operations by putting operators inside other
operators. The inside operator will be performed on each group created
by the ``GROUP BY`` clause, and the outside operator will be performed
on the results of the inside operator.

Example:

This query returns the average population of states. The outer
aggregation function (AVG) operates on the results of the inner
aggregation (``SUM``) and ``GROUP BY`` clause.

.. code-block:: sql

    SELECT AVG(SUM(pop))
    FROM `/population`
    GROUP BY state


Section 7 - Nested Data and Arrays
----------------------------------

Unlike a relational database system, many NoSQL database systems allow data to be
nested (that is, data can be objects) and to contain arrays.


7.1 Nesting
~~~~~~~~~~~

Nesting is represented by levels separated by a full stop (``.``).

Example:

.. code-block:: sql

    SELECT c.profile.address.street.number
    FROM `/users` AS c


7.2 Arrays
~~~~~~~~~~

Array elements are represented by the array index in square brackets
(``[n]``).

Example:

.. code-block:: sql

    SELECT c.profile.allAddress[0].street.number
    FROM `/users` AS c


7.2.1 Flattening
''''''''''''''''

You can extract all elements of an array or all field values
simultaneously, essentially removing levels and flattening the data. Use
the asterisk in square brackets (``[*]``) to extract all array elements.

Example:

.. code-block:: sql

    SELECT c.profile.allAddresses[*]
    FROM `/users` AS c

Use the asterisk in curly brackets (``{*}``) to extract all field
values.

Example:

.. code-block:: sql

    SELECT c.profile.{*}
    FROM `/users` AS c


7.2.2 Filtering using arrays
''''''''''''''''''''''''''''

You can filter using data in all array elements by using the asterisk in
square brackets (``[*]``) in a ``WHERE`` clause.

Example:

.. code-block:: sql

    SELECT DISTINCT *
    FROM `/users` AS c
    WHERE c.profile.allAddresses[*].street.number = "221B"


Section 8 - Pagination and Sorting
----------------------------------


8.1 Pagination
~~~~~~~~~~~~~~

Pagination is used to break large return results into smaller chunks.
Use the ``LIMIT`` operator to set the number of results to be returned
and the ``OFFSET`` operator to set the index at which the results should
start.

Example (Limit results to 20 entries):

.. code-block:: sql

    SELECT *
    FROM `/users`
    LIMIT 20

Example (Return the 100th to 119th entry):

.. code-block:: sql

    SELECT *
    FROM `/users`
    OFFSET 100
    LIMIT 20


8.2 Sorting
~~~~~~~~~~~

Use the ``ORDER BY`` clause to sort the results. You can specify one or
more fields for sorting, and you can use operators in the ``ORDER BY``
arguments. Use ``ASC`` for ascending sorting and ``DESC`` for descending
sorting.

Example (Sort users by ascending age):

.. code-block:: sql

    SELECT *
    FROM `/users`
    ORDER BY age ASC

Example (Sort users by last digit in age, descending, and full name,
ascending):

.. code-block:: sql

    SELECT *
    FROM `/users`
    ORDER BY age % 10 DESC, firstName + lastName ASC


Section 9 - Joining Collections
-------------------------------

Use the ``JOIN`` operator to join two or more collections.

There is no technical limitation to the number of collections or tables
that can be joined, but users are encouraged to consider the performance
impact based upon the dataset sizes.

For MongoDB ``JOIN`` s, see the database specific notes section about
`JOINs on MongoDB <sql-squared-reference.html#joins-on-mongodb>`__.


9.1 Examples
~~~~~~~~~~~~

This example returns the names of employees and the names of the
departments they belong to by matching up the employee department ID with
the department's ID, where both IDs are ObjectID types.

.. code-block:: sql

    SELECT
        emp.name,
        dept.name
    FROM `/employees` AS emp
    JOIN `/departments` AS dept ON dept._id = emp.departmentId

If one of the IDs is a string, then use the ``OID`` operator to convert
it to an ID.

.. code-block:: sql

    SELECT
        emp.name,
        dept.name
    FROM `/employees` AS emp
    JOIN `/departments` AS dept ON dept._id = OID(emp.departmentId)

9.2 Join Considerations
~~~~~~~~~~~~~~~~~~~~~~~

On ``JOIN``\ s with more than two collections or tables, the standard
rule of thumb is to place the tables in order from smallest to largest.
If the collections ``a``, ``b``, and ``c`` have ``4``, ``8``, and ``16``
documents respectively, then ordering ``FROM `/a`, `/b`, `/c``` is most
efficient with ``WHERE a._id = b._id``.

If, however, the filter condition is ``WHERE b._id = c._id`` then the
appropriate ordering would be
``FROM `/b`, `/c`, `/a` WHERE b._id = c._id``. This is because without
the filter \|a ⨯ b\| = 32 which is less than \|b ⨯ c\| = 128, but with
the filter, \|b ⨯ c\| is limited to the number of documents in b, which
is 8 (and which is lower than the unconstrained \|a ⨯ b\|).


Section 10 - Conditionals and Nulls
-----------------------------------


10.1 Conditionals
~~~~~~~~~~~~~~~~~

Use the ``CASE`` expression to provide if-then-else logic to SQL². The
``CASE`` sytax is:

.. code-block:: sql

    SELECT (CASE <field>
        WHEN <value1> THEN <result1>
        WHEN <value2> THEN <result2>
        ...
        ELSE <elseResult>
        END)
    FROM `<path>`

Example:

The following example generates a code based on gender string values.

.. code-block:: sql

    SELECT (CASE c.gender
        WHEN "male" THEN 1
        WHEN "female" THEN 2
        ELSE 3
        END) AS genderCode
    FROM `/users` AS c

10.2 Nulls
~~~~~~~~~~

Use the ``COALESCE`` function to evaluate the arguments in order and
return the current value of the first expression that initially does not
evaluate to ``NULL``.

Example:

This example returns a full name, if not null, but returns the first
name if the full name is null.

.. code-block:: sql

    SELECT COALESCE(c.fullName, c.firstName) AS name
    FROM `/users` AS c


Section 11 - Data Type Conversion
---------------------------------


11.1 Converting to Boolean
~~~~~~~~~~~~~~~~~~~~~~~~~~

SQL² allows String data type fields with values of either ``"true"`` or
``"false"`` to be converted to their corresponding Boolean value.

Prefix the field name with the ``BOOLEAN`` function.

Example:

.. code-block:: sql

    SELECT BOOLEAN(survey_complete) AS Survey
    FROM `/users`


11.2 Converting to Strings
~~~~~~~~~~~~~~~~~~~~~~~~~~

SQL² allows most fields to be converted to String data types by prefixing
the field name with the ``TO_STRING`` function.

Example:

.. code-block:: sql

    SELECT TO_STRING(zip_code) AS ZipCode
    FROM `/users`


11.3 Converting to Integer
~~~~~~~~~~~~~~~~~~~~~~~~~~

SQL² allows string representations of valid integer values to be converted
to an actual integer number.  Prefix the field name with the
``INTEGER`` function.

If a field named ``myField`` had the value
of ``"1234"`` as a String, it could be converted to an integer with this example:

.. code-block:: sql

    SELECT INTEGER(myField) AS MyField
    FROM `/users`

If a field is not a valid string representation of an integer value then a
null value will be returned.


11.4 Converting to Decimal
~~~~~~~~~~~~~~~~~~~~~~~~~~

SQL² allows string representations of valid integer and decimal values to be converted
to an actual decimal number.  Prefix the field name with the
``DECIMAL`` function.

If a field named ``myField`` had the value
of ``"1.234"`` as a String, it could be converted to a decimal with this example:

.. code-block:: sql

    SELECT DECIMAL(myField) AS MyField
    FROM `/users`

If the field does not a contain a valid string representation of a numeric value,
such as ``"123"`` or ``"123.456"`` then a null value will be returned.


11.5 Converting to Dates and Times
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SQL² allows strings in a specific format to be converted
to date and time related data types. See
`Section 5 <sql-squared-reference.html#section-5-dates-and-times>`__
for examples of converting to date, time, and timestamp types.


Section 12 - Variables and SQL²
-------------------------------

SQL² has the ability to use variables in queries in addition to statically
typed content.  Variables can be generated through the use of a **Variables Card**
or through a combination of **Setup Markdown Card** / **Show Markdown Card**.  Both
scenarios require that the variables be defined before the **Query Card** is
executed.


.. attention:: **SlamData Version**

  The syntax for using variables within SQL² was changed slightly
  in version 3.0.8.  This document assumes you are using a version
  no older than 3.0.8.


12.1 Single Values
~~~~~~~~~~~~~~~~~~

Single values are generated in Markdown through the following elements:

* String text field
* Numeric text field
* Calendar Picker
* Calendar / Time Picker
* Radio Boxes
* Drop Downs
  
For more information on Markdown / Slamdown and how to generate form
elements see the
`Form Elements Section <slamdown-reference.html#section-5-form-elements>`__
of the Slamdown Reference Guide.

Variables can be used in queries by prefixing the variable name with
a colon (``:``).

For example, if the following Markdown code was used:

.. code-block:: markdown

    ### Select year to report on

    year = {2011,2012,2013,2014,2015,2016}


The value selected by the user from the ``year`` dropdown can be referenced
like this:

.. code-block:: sql

    SELECT * FROM `/users`
    WHERE last_visit = :year


12.2 Multiple Values
~~~~~~~~~~~~~~~~~~~~

Multiple values are generated in Markdown only through the Check Boxes
UI element.

For example, if the following Markdown code was used:

.. code-block:: markdown

    ### Select years to report on

    years = [x] 2014 [] 2015 [] 2016 [] 2017


The values selected by the user from the ``years`` set of Check Boxes
should be referenced using the ``IN`` clause:

.. code-block:: sql

    SELECT * FROM `/users`
    WHERE last_visit IN :years


This example would find all users who have a ``last_visit`` that matched
one of the check boxes selected.



Section 13 - Database Specific Notes
------------------------------------


13.1 MongoDB
~~~~~~~~~~~~


13.1.1 The _id Field
''''''''''''''''''''

By default, the ``_id`` field will not appear in a result set. However,
you can specify it by selecting the ``_id`` field. For example:

.. code-block:: sql

    SELECT `_id` AS cust_id
    FROM `/users`

.. note::

  When using the ``_id`` field, it must be escaped in backtick characters or
  you will get an error. You must also give the ``_id`` an alias or it will
  not show up, even if you have it in your ``SELECT`` statement.

MongoDB has special rules about fields called ``_id``. For example, they
must remain unique, which means that some queries (such as
``SELECT myarray[*] FROM foo``) will introduce duplicates that MongoDB
won't allow. In addition, other queries change the value of ``_id``
(such as grouping). So SlamData manages ``_id`` and treats it as a
special field.

.. note::

  To filter on ``_id``, you must first convert a string to an
  object ID, by using the ``OID`` function, as shown in the
  example below.

.. code-block:: sql

    SELECT *
    FROM `/foo`
    WHERE `_id` = OID("abc123")


13.1.2 JOINs on MongoDB
'''''''''''''''''''''''

When executing a ``JOIN`` in SQL² against MongoDB, the analytics engine
will decide whether to use the mapreduce API, or the aggregation API along
with the ``$lookup`` operator.  This operator was introduced in MongoDB
version 3.2 and is the equivalent of a left outer equijoin.  You can
find out more `here <https://docs.mongodb.com/manual/reference/operator/aggregation/lookup>`__.

To leverage the ``$lookup`` operator, the query must satisfy the following
conditions that are imposed by MongoDB:

* Must be running MongoDB 3.2 or newer.
* One collection must use an indexed field.
* That collection must not be sharded.
* Both collections must be in the same database.
* Match must be an equijoin, based on equality only (``a.field = b.field`` is ok, ``a.field < b.field`` is not).

If ``$lookup`` cannot be used, SlamData will fall back to utilizing the
mapreduce API.  Utilizing mapreduce is usually slower but has a wider
range of use cases that it supports.

.. raw:: html

    <embed>
    <script type="text/javascript" id="hs-script-loader" async defer src="//js.hs-scripts.com/2389041.js"></script>
    </embed>

