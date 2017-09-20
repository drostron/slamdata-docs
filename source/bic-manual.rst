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

SlamData BI Connector Manual
============================


Section 1 - Introduction
------------------------

This README is for the SlamData BI Connector only. It does not apply to
the SlamData Standard or SlamData Advanced editions.

The Quasar Analytics engine, also written by SlamData, provides native
analytics for MongoDB and other NoSQL databases. It is also used in the
SlamData Standard and SlamData Advanced analytics products. These two
solutions provide a graphical front end to Quasar, which in turn
provides data access to multiple target databases, both NoSQL and
relational. These SlamData flagship products allow users to embed
beautiful charts with enterprise-grade security into any app, while
using SQL against both NoSQL and relational data sources. Find out more
at www.slamdata.com

More information about Quasar Analytics can be found at:
https://github.com/quasar-analytics/quasar/blob/master/README.md


1.1 Advantages
~~~~~~~~~~~~~~

The advantages of using the SlamData BI Connector over alternatives are:

-  Takes full advantage of computational "push down". This allows
   maximum processing in the database before returning results to the
   client. Other solutions will pull back a much larger data set and
   then process it within PostgreSQL instead.

-  Written in C for maximum speed.

-  Full commercial support from the SlamData team.


1.2 Requirements
~~~~~~~~~~~~~~~~

-  PostgreSQL version 9.4.x (does not run on 9.3 or 9.5)

-  MongoDB version 3.2 or newer

-  Python 2.7 (for use with the gen\_schema.py script)

-  Mac OS X 10.10 or newer, Windows 7 or newer, Ubuntu Linux 16.04 or
   newer

-  Read/write access to MongoDB. Required when MongoDB runs large
   queries and must use temporary writing space (see 'allowDiskUse'
   MongoDB term)

-  Read/write file system access to system that the SlamData BI
   Connector is installed.


1.3 Data Flow
~~~~~~~~~~~~~

The SlamData BI Connector allows communication from a client to
PostgreSQL, from PostgreSQL to the Quasar analytics engine, and from
Quasar to MongoDB.

User Flow: User -> PostgreSQL -> Quasar -> MongoDB

Data Flow: User <- PostgreSQL <- Quasar <- MongoDB


Section 2 - Installation
------------------------


2.1 All environments
~~~~~~~~~~~~~~~~~~~~

-  Ensure MongoDB is installed properly prior to install of the SlamData
   BI Connector

-  Ensure PostgreSQL 9.4.x is installed properly prior to install of the
   SlamData BI Connector

-  Verify appropriate credentials for MongoDB. User should have both
   read and write capabilities for the databases and collections that
   will be queried.

-  Verify firewalls allow appropriate ports between relevant systems.
   Default values are below.

   -  MongoDB: 27017

   -  SlamData BI Connector: 8080

   -  PostgreSQL: 5432


2.2 OS X
~~~~~~~~

-  Mount the ``.dmg`` file image

-  Execute the SlamData BI Connector installer

-  Proceed to the Configuration section below


2.3 Linux
~~~~~~~~~

-  User executing the install script **must** have sudo privileges to
   write to system directories.

-  Extract the sdbic.tar.gz file:

::

    tar xvfz ./sdbic.tar.gz


2.4 Windows
~~~~~~~~~~~

-  Unzip the provided installation package

-  Execute the SlamData BI Connector installer

-  Proceed to Configuration section below


Section 3 - Configuration
-------------------------

To successfully configure the SlamData BI Connector an administrator
must understand JSON formatting for MongoDB and schemas in PostgreSQL.

After the installation steps in the previous section have been
completed, it is time to configure the SlamData BI Connector.


3.1 Assisted Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``sd_install`` is a bash shell script will allow administrators to
create a basic BI Connector configuration. If you are on a Windows
platform, you can either install a Bash shell executor on your system,
or you may choose to manually configure the product by following steps
in the next section.

-  Run the script:

   ::

       cd sdbic
       sudo ./sd_install

-  Enter ``install`` at the prompt

-  Provide appropriate values to prompted questions.

The ``install`` module of the script will copy platform-specific
libraries to appropriate directories based on your operating system. It
will also copy the .jar files to /opt/slamdata/bic. Finally it places a
``quasar-config.json`` file in that directory.

Subsequent runs of the ``sd_install`` script allow you to choose other
options to repeat any portion of the initial install, including
``create_config``, ``install_libs``, ``restart_postgres`` and
``install_quasar``.


3.2 Manual Configuration
~~~~~~~~~~~~~~~~~~~~~~~~

If you successfully used the ``install`` script from section 3.1 above
you may skip all of section 3.2.

If you are unable to run the ``sd_install`` script you may follow these
steps:

-  Create a directory located at ``/opt/slamdata/bic``

-  Copy the ``core_2.11-9.2.2-one-jar.jar`` and
   ``web_2.11-9.2.2-one-jar.jar`` files to ``/opt/slamdata/bic``

-  Create symbolic links ``/opt/slamdata/bic/quasar-repl.jar`` for the
   core jar file, and ``/opt/slamdata/bic/quasar-web.jar`` for the web
   jar file.

-  Ensure directory and file permissions are appropriate for your
   environment

**Note**: Users may choose a different directory, especially Windows
users. If an alternate directory is used, use that directory in any
subsequent steps.

-  Ensure MongoDB is running on a system you have access to.

-  Create a new ``quasar-config.json`` configuration file and place it
   in the ``/opt/slamdata/bic/`` directory. This is used by Quasar to
   connect to MongoDB.

-  Configure the file (see
   https://github.com/quasar-analytics/quasar#configure)

-  Start Quasar to test it:

::

    java -jar /opt/slamdata/bic/quasar-repl.jar -c /opt/slamdata/bic/quasar-config.json

Only after Quasar is successfully communicating to MongoDB, and you can
run SQL queries with it, should you proceed to the next step. If you're
unable to run queries against Quasar and MongoDB, do not proceed as the
next steps rely on a working environment.

-  Stop PostgreSQL if it is running


3.2.1 Required Libraries
~~~~~~~~~~~~~~~~~~~~~~~~

-  Copy the PostgreSQL and YAJL library files to appropriate directories
   listed below:


3.2.1.1 Ubuntu Linux
''''''''''''''''''''

+---------------------------------------------------+-------------------------------------+
| Packaged file name and location                   | Copy to                             |
+===================================================+=====================================+
| platforms/all/libraries/quasar_fdw.control        | /usr/share/postgresql/9.4/extension |
+---------------------------------------------------+-------------------------------------+
| platforms/all/libraries/quasar_fdw--1.2.2.sql     | /usr/share/postgresql/9.4/extension |
+---------------------------------------------------+-------------------------------------+
| platforms/debian/libraries/quasar_fdw.so          | /usr/lib/postgresql/9.4/lib         |
+---------------------------------------------------+-------------------------------------+
| platforms/debian/libraries/yajl/libyajl.so        | /usr/lib/x86_64-linux-gnu           |
+---------------------------------------------------+-------------------------------------+
| platforms/debian/libraries/yajl/libyajl.so.2      | /usr/lib/x86_64-linux-gnu           |
+---------------------------------------------------+-------------------------------------+
| platforms/debian/libraries/yajl/libyajl.so.2.1.1  | /usr/lib/x86_64-linux-gnu           |
+---------------------------------------------------+-------------------------------------+
| platforms/debian/libraries/yajl/libyajl_s.a       | /usr/lib/x86_64-linux-gnu           |
+---------------------------------------------------+-------------------------------------+


3.2.1.2 Apple MacOS / OS X
''''''''''''''''''''''''''

The file destination will depend on how PostgreSQL was installed.  The example below
assumes that PostgreSQL 9.4.5_2 was installed via ``brew install postgres``

+---------------------------------------------------+-----------------------------------------------------+
| Packaged file name and location                   | Copy to                                             |
+===================================================+=====================================================+
| platforms/all/libraries/quasar_fdw.control        | /usr/share/postgresql/9.4/extension                 |
+---------------------------------------------------+-----------------------------------------------------+
| platforms/all/libraries/quasar_fdw--1.2.2.sql     | /usr/share/postgresql/9.4/extension                 |
+---------------------------------------------------+-----------------------------------------------------+
| platforms/osx/libraries/quasar_fdw.so             | /usr/local/Cellar/postgresql/9.4.5_2/lib/postgresql |
+---------------------------------------------------+-----------------------------------------------------+
| platforms/osx/libraries/yajl/libyajl.so           | /usr/local/Cellar/postgresql/9.4.5_2/\              |
|                                                   | share/postgresql/extension                          |
+---------------------------------------------------+-----------------------------------------------------+
| platforms/osx/libraries/yajl/libyajl.so.2         | /usr/local/Cellar/postgresql/9.4.5_2/\              |
|                                                   | share/postgresql/extension                          |
+---------------------------------------------------+-----------------------------------------------------+
| platforms/osx/libraries/yajl/libyajl.so.2.1.1     | /usr/local/Cellar/postgresql/9.4.5_2/\              |
|                                                   | share/postgresql/extension                          |
+---------------------------------------------------+-----------------------------------------------------+
| platforms/osx/libraries/yajl/libyajl_s.a          | /usr/local/Cellar/postgresql/9.4.5_2/\              |
|                                                   | share/postgresql/extension                          |
+---------------------------------------------------+-----------------------------------------------------+



- Restart PostgreSQL

- Load the Quasar Foreign Data Wrapper extension.  You should only need
  to execute this command once, unless it fails.


Section 4 - Initial Server Setup
--------------------------------

Once all of the files are installed or copied to their appropriate locations,
it is time to configure PostgreSQL to communicate with Quasar by registering
the ``quasar_fdw`` foreign data wrapper, and creating a remote/foreign server.


From the ``psql`` command line as user ``postgres``:

.. code-block:: sql

    CREATE EXTENSION quasar_fdw;

PostgreSQL should respond with an empty ``CREATE EXTENSION`` response.


- Create the Quasar foreign server object within PostgreSQL.

This step assumes that Quasar has already been successfully installed,
and configured with a MongoDB mount name of ``/target``, and that MongoDB has
a database called ``quasar``.

.. code-block:: sql

    DROP SERVER mybox CASCADE;
    CREATE SERVER mybox FOREIGN DATA WRAPPER quasar_fdw
           OPTIONS (server 'http://localhost:8080'
                   ,path '/target/quasar/'
                   ,timeout_ms '1000'
                   ,use_remote_estimate 'true'
                   ,fdw_startup_cost '10'
                   ,fdw_tuple_cost '0.01');


PostgreSQL should respond with an empty ``CREATE SERVER`` response.

The following parameters can be set on a Quasar foreign server object:

+-------------------------+------------------------------------------+---------------------------+
| Option                  | Description                              | Default Value             |
+=========================+==========================================+===========================+
| ``server``              | URL of remote Quasar Server.             | ``http://localhost:8080`` |
+-------------------------+------------------------------------------+---------------------------+
| ``path``                | Path to the data on remote Quasar.       | ``/test``                 |
+-------------------------+------------------------------------------+---------------------------+
| ``timeout_ms``          | Timeout in milliseconds of querying data | ``1000`` ms (1 sec)       |
|                         | from Quasar.                             |                           |
+-------------------------+------------------------------------------+---------------------------+
| ``use_remote_estimate`` | Boolean (``true`` or ``false``) to allow | ``true``                  |
|                         | quasar_fdw to contact Quasar with        |                           |
|                         | rowcounts to estimate cost of queries.   |                           |
+-------------------------+------------------------------------------+---------------------------+
| ``fdw_startup_cost``    | Cost (floating-point) of starting up a   | 100.0                     |
|                         | query to Quasar.                         |                           |
+-------------------------+------------------------------------------+---------------------------+
| ``fdw_tuple_cost``      | Cost (floating-point) of processing a    | ``0.01``                  |
|                         | tuple in quasar_fdw.                     |                           |
+-------------------------+------------------------------------------+---------------------------+



Section 5 - Table Setup
-----------------------

Before queries can be successfully executed through PostgreSQL to MongoDB,
there must be a mapping of PostgreSQL table columns to MongoDB collection fields.

Additionally, PostgreSQL does not understand the concept of nested data such as
arrays and subdocuments.  Due to these two factors, each collection that you wish
to query inside of MongoDB must have one or more PostgreSQL tables mapped to it.

This example assumes that a collection ``zips`` exists on the MongoDB server under
the ``quasar`` database mentioned in the previous step.  This example will create
a table with the name of ``myzips`` and map it to equivalent fields in the MongoDB
``zips`` collection, on the ``mybox`` Quasar server.

.. code-block:: sql

    CREATE FOREIGN TABLE zips(
            city varchar,
            pop integer,
            state char(2),
            loc float[2])
        SERVER mybox
        OPTIONS (table 'myzips');

The following parameters can be set on a Quasar **foreign table** object:

+-------------------------+----------------------------------+----------------+
| Option                  | Description                      | Default value  |
+=========================+==================================+================+
| ``table``               | Name of the Quasar table / mongo | N/A            |
|                         | collection to query.             |                |
+-------------------------+----------------------------------+----------------+       
| ``use_remote_estimate`` | Override the server-level option | Server value   |
+-------------------------+----------------------------------+----------------+       

At this point you have successfully setup a PostgreSQL < - > MongoDB mapping.

The example below assumes you have the ``patients`` JSON collection located
`here <https://github.com/damonLL/tutorial_files/raw/master/patients>`__

- Create a Quasar foreign table object using column mappings.

Note the use of the
`flattening operator <sql-squared-reference.html#flattening>`__ ``[*]`` from SQL² syntax.

.. code-block:: sql

    CREATE FOREIGN TABLE patients(
        _id VARCHAR,
        first_name VARCHAR,
        last_name VARCHAR,
        middle_name VARCHAR,
        street_address VARCHAR,
        city VARCHAR,
        state VARCHAR,
        zip_code BIGINT,
        county VARCHAR,
        ssn VARCHAR,
        age BIGINT,
        weight FLOAT,
        height FLOAT,
        loc FLOAT [],
        last_visit TIMESTAMP,
        gender CHAR(6),
        previous_visits TIMESTAMP [],
        i10_code VARCHAR OPTIONS (map 'codes[*].code'),
        i10_description VARCHAR OPTIONS (map 'codes[*].desc')
      SERVER mybox
        OPTIONS (table 'patients');


The following parameters can be set on a column in a Quasar **foreign
field**:

+----------------------------+--------------------------+-------------------------------------------+
| Option                     | Default Value            | Description                               |
+============================+==========================+===========================================+
| ``map``                    | The lower case name of   | Name of the column to query in            |
|                            | the column in PostgreSQL | PostgreSQL                                |
+----------------------------+--------------------------+-------------------------------------------+
| ``nopushdown``             | ``false``                | Boolean (``true`` or ``false``)           |
|                            |                          | value telling PostgreSQL not to           |
|                            |                          | push down any comparison clauses          |
|                            |                          | with this column in it. Used              |
|                            |                          | when underlying data is not               |
|                            |                          | stored as the correct type.               |
+----------------------------+--------------------------+-------------------------------------------+
| ``join_rowcount_esitmate`` | ``1``                    | Integer value representing the            |
|                            |                          | *distinctness* of a column's value in the |
|                            |                          | underlying data. This will be used to     |
|                            |                          | estimate the number of rows that might be |
|                            |                          | queried from a single value. For columsn  |
|                            |                          | with unique values, this should be ``1``. |
+----------------------------+--------------------------+-------------------------------------------+

Important notes regarding field mapping configuration:


- Postgres will downcase all field names, so if a field has a capital letter in it,
  you must use the map option: ``OPTIONS (map "camelCaseSensitive")``

-  The SlamData BI Connector will convert strings to other types, such as dates, times,
   timestamps, intervals, integers, and floats. However, if the
   underlying data is a string, we should *NOT* push down type-specific
   operations such as WHERE clauses to Quasar. Therefore, you should
   enforce a no pushdown restriction in the column options. Use the
   ``OPTIONS (nopushdown 'true')`` option to force no pushdown of any
   clause containing the column.


Section 6 - Queries
-------------------


6.1 Queries via PostgreSQL
~~~~~~~~~~~~~~~~~~~~~~~~~~

Once the appropriate server components are configured, and at least one
table and collection have been mapped, then PostgreSQL will act
as a proxy query server to MongoDB.  This essentially means that users
can either use the ``psql`` command line tool to query PostgreSQL, and
in turn MongoDB; but it also means that standard JDBC clients can
now query MongoDB through PostgreSQL as well.

The Quasar analytics engine has the advantage of pushing maximum
computation down to MongoDB.  This means that whatever complex aggregations
that may be submitted in a query will actually occur in MongoDB, rather
than inside PostgreSQL or the client.  With data sets ranging into
terabytes this is an important feature.

Example SQL queries:

.. code-block:: sql

    SELECT * FROM zips LIMIT 10;

    SELECT city, pop FROM zips WHERE pop % 2 = 1 LIMIT 10;

    SELECT * FROM zips ORDER BY pop DESC LIMIT 10;

    SELECT * FROM zips z1 INNER JOIN zips z2 ON z1.city = z2.city LIMIT 10;



6.2 Queries via Quasar
~~~~~~~~~~~~~~~~~~~~~~

After the SlamData BI Connector is fully installed, users have the
additional option of leveraging the REPL (read, evaluate, print, loop)
console.  This allows direct access to the MongoDB database, bypassing
PostgreSQL completely.  The primary benefit being that unstructured
databases such as MongoDB can be directly queried without any mapping
of fields.

Additionally users can leverage enhanced SQL² functionality that standard
JDBC and PostgreSQL drivers do not support, such as the flattening ``[*]``
operator to drill down into arrays, or dot-notation sub documents.

The SlamData BI Connector comes with two ``.jar`` files.  One is designed
to operator as a REST API for the PostgreSQL < - > communication pipeline.
The other is designed to be called independently and provides the interactive
REPL shell to the mounted MongoDB databases.

First, start the REPL console:

.. code-block:: bash

    java -jar /opt/slamdata/bic/quasar-repl.jar -c /opt/slamdata/bic/quasar-config.json


You'll be greeted with the Quasar console:

.. code-block:: bash

    💪 $


You can navigate the currently mounted databases very much like a Unix/Linux OS:

.. code-block:: bash

    💪 $ ls
    aws@ (mongodb)
    macbook@ (mongodb)
    💪 $ cd macbook
    💪 $ ls
    bp/
    charts/
    demo/
    devdb/
    local/
    numbers/
    quasar/
    💪 $ cd demo
    💪 $ ls
    dis
    💪 $ select * from dis
    MongoDB
    db.dis.find();


    Query time: 0.2s
     name   |
    --------|
     Abby   |
     David  |
     Tina   |
     Xavier |
    💪 $ 


The example above shows two mount points: ``aws`` and ``macbook``.  Inside
the ``macbook`` mount point there is a ``demo`` database, and within that
database the collection ``dis``.

Standard SQL can be executed within the REPL console, as well as enhanced
SQL² queries.  See the combination of both below.

Example SQL and SQL² queries:

.. code-block:: sql

    SELECT * FROM zips LIMIT 10;

    SELECT city, pop FROM zips WHERE pop % 2 = 1 LIMIT 10;

    SELECT loc[1] AS lat, loc[2] AS long FROM zips LIMIT 10;

    SELECT * FROM zips ORDER BY pop DESC LIMIT 10;

    SELECT * FROM zips z1 INNER JOIN zips z2 ON z1.city = z2.city LIMIT 10;


To view detailed information regarding the query plan for
a query, utilize the ``EXPLAIN`` function as follows.

To see the query that PostgreSQL sends to Quasar:

.. code-block:: sql

    EXPLAIN (COSTS off) SELECT * FROM zips LIMIT 10;


To see the query that Quasar sends to MongoDB:

.. code-block:: sql

    EXPLAIN (COSTS off, VERBOSE on) SELECT * FROM zips LIMIT 10;



+---------------+------------------------------------+
| General Type  | Specific Type                      |
+===============+====================================+
| String type   | ``char``, ``text``, ``varchar``,   |
|               | ``bpchar``, ``name``               |
+---------------+------------------------------------+
| Number type   | ``numeric``, ``int4``, ``int8``,   |
|               | ``int2``, ``float4``, ``float8``,  |
|               | ``oid``                            |
+---------------+------------------------------------+
| Time type     | ``time``, ``timestamp``, ``date``, |
|               | ``timestamptz``                    |
+---------------+------------------------------------+
| Boolean       |                                    |
+---------------+------------------------------------+
| Complex types | arrays, json, jsonb                |
+---------------+------------------------------------+


6.3 JOIN Query Functionality
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

JOINs can be executed in one of three ways, depending on the cost
estimation. This is why ``use_remote_estimate`` is so important. A
merge join is used for very large and similarly sized datasets. A
hash join is used for a large and a small dataset. A parameterized
join is used when one join condition is only going to return a very
small number of rows. This parameterized join is the best pushdown
that can be achieved with PostgreSQL 9.4's FDW interface.

.. raw:: html

    <embed>
    <script type="text/javascript" id="hs-script-loader" async defer src="//js.hs-scripts.com/2389041.js"></script>
    </embed>

