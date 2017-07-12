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

Developer's Guide
=================

This Developer's Guide will assist the developer who is unfamiliar with
SlamData to install, configure, customize and embed a complete solution
from start to finish.

For information on how to use SlamData from an administrator's perspective
see the `SlamData Administrator's Guide <administration-guide.html>`__.

For information on how to use SlamData from a user's perspective
see the `SlamData User's Guide <users-guide.html>`__.


Section 1 - Installing and Running SlamData
-------------------------------------------

1.1 Purpose
~~~~~~~~~~~

The purpose of this Developer's Guide is to walk a software developer
through SlamData from installation through to a completed project.  The goal
is to provide a step-by-step process that a developer can follow,
including sample data, that is repeatable with other data sets and
environments.


1.2 Introduction
~~~~~~~~~~~~~~~~

SlamData is both an Open Source Software project and a commercially
available Visual Analytics platform for multidimensional data (including
two-dimensional RDBMS data).  SlamData provides the ability to query
all of your data, in any form, in any location with a single solution.
This is achieved with some of the following features of SlamData:

- Patented multidimensional relational technology, allowing SlamData to
  communicate with any data source in any data format. This includes not
  only historical two-dimensional data such as RDBMS in rows and columns,
  but also deeply nested, semi-structured data such as JSON and XML.

- Ability to understand schemas dynamically, resulting in absolutely no
  requirement to map field types from one technology to another.  This also allows
  SlamData to use both field values **and** the schema as data.  This is
  not possible with other NoSQL -> relational solutions.

- A fully generalized database backend technology, providing a reliable
  and ANSI compatible superset of SQL called SQL² that runs on top of any
  supported data source.  There is no need to learn yet another proprietary
  query language.

- Fully embeddable solution that merges seamlessly with your own applications
  providing a consistent look and feel while providing significant and
  immediate value out of the box.

- Easy to use search capabilities for non-technical users.  Search for a
  key word, value or any other data type without knowing where it is or
  in which format.

- Visually appealing charts (eCharts_ from Baidu) that can be customized
  and natively understand nested data.

- Ability to secure data in a multi-tenant environment through OpenID Connect
  and OAuth 2.0.


1.3 Assumptions
~~~~~~~~~~~~~~~

This guide was written with the following assumptions in mind.  The reader
is a developer that:

- Has a basic to moderate understanding of SQL.
- Has a basic to moderate understanding of JSON.
- Has a basic to moderate understanding of HTML web applications.
- Can perform basic navigation of a data source, such as a database system.
- Has appropriate permissions to install relevant software.


1.4 Requirements
~~~~~~~~~~~~~~~~

For SlamData to run in an optimal environment see the
`Minimum System Requirements <administration-guide.html#minimum-system-requirements>`__
section.

.. attention:: **Windows Developers**

  This Developer's Guide includes example code in several sections in addition to
  shell scripts or command line utilities.  While this guide can be followed
  by most Mac OS and Linux developers, Microsoft Windows developers will have to
  implement similar functionality through other means such as DOS shell scripts.


1.5 Installation
~~~~~~~~~~~~~~~~

Instructions for installing SlamData can be found
`here <administration-guide.html#obtaining-slamdata>`__.


1.6 Starting SlamData
~~~~~~~~~~~~~~~~~~~~~

Instructions for starting SlamData can be found
`here <administration-guide.html#starting-slamdata>`__.

Once SlamData is running then continue to Section 2.


Section 2 - Exploring Data
--------------------------

By the end of this Developer's Guide the reader will have a fully working
SlamData environment that is securely embedded with user authentication,
interactive forms and dynamic charts.  To start, however, the basics of
the user interface will need to be covered.  The guide will then move
on to more complex topics focused on importing data, exploring that data
and searching it with keywords and eventually using SlamData's SQL² dialect
to perform SQL queries on the data.


2.1 Interface Navigation
~~~~~~~~~~~~~~~~~~~~~~~~

The image below shows the Home screen after starting SlamData.  Note the numbers
and their descriptions following the image.

|Home-Annotated|


+--------+------------------------------------------------------------------------------+
| Number | Description                                                                  |
+========+==============================================================================+
|     1  |  Server or Mount names that have been configured.                            |
+--------+------------------------------------------------------------------------------+
|     2  |  The current path you are viewing. In this example it is the Home path (/).  |
+--------+------------------------------------------------------------------------------+
|     3  |  The wrench icon configures a mount.                                         |
+--------+------------------------------------------------------------------------------+
|     4  |  The eye icon toggles visibility of the trash can icon.                      |
+--------+------------------------------------------------------------------------------+
|     5  |  Download all data starting from this path.                                  |
+--------+------------------------------------------------------------------------------+
|     6  |  Mount a new data source.                                                    |
+--------+------------------------------------------------------------------------------+
|     7  |  Create a new folder in the datasource virtual file system.                  |
+--------+------------------------------------------------------------------------------+
|     8  |  Upload a data file.                                                         |
+--------+------------------------------------------------------------------------------+
|     9  |  Create a new workspace.                                                     |
+--------+------------------------------------------------------------------------------+


2.2 Workspaces, Decks and Cards
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before we start looking at our data we need to discuss how to interact with
it.  This is done through the use of a **Workspace**.  A Workspace is the
primary method that users interact with data within SlamData.  A
Workspace in turn is comprised of cards, and decks of cards.

* **Root Deck** - Each Workspace must have a Root Deck in which all other unit types
  are stored. A Root Deck is always present in a Workspace but never visible.

* **Deck** - Each deck contains at least one or more cards that each perform a
  specific action and build upon each other.  Decks can be mirrored which allows
  easy creation of a new target deck that starts with the same functionality as
  the origin deck.  Changes in each deck, up to the point where they were
  mirrored, will impact each other.

* **Draftboard Card** - A special card type that creates a visual area to arrange
  multiple decks.

* **Card** - A unit that performs a distinct action. Examples include:

    * Query Card.
    * Search Card.
    * Preview Table Card.
    * and more ...

+-----------------+---------------------------------------------------------------+
| Unit Type       | May Contain:                                                  |
+=================+===============================================================+
| Root Deck       | Either a single **Draftboard Card** or multiple normal cards. |
+-----------------+---------------------------------------------------------------+
| Deck            | One or more cards, including one **Draftboard Card**.         |
+-----------------+---------------------------------------------------------------+
| Draftboard Card | One or more decks.                                            |
+-----------------+---------------------------------------------------------------+
| Card            | N/A                                                           |
+-----------------+---------------------------------------------------------------+

A visual example of the allowable nesting follows:

|SD-Nesting|

Don't worry!  You won't need to know any of this until section 3, and by then we
will take you through it step-by-step.


2.3 Creating a New Mount
~~~~~~~~~~~~~~~~~~~~~~~~

In this guide the MongoDB database will be used in the examples. As such,
the reader should download and run the latest stable version of MongoDB.

Default MongoDB installations run on port **27017** and have no user
authentication enabled.  This guide assumes this configuration in the following
instructions.

Click the New Mount Icon.  |Icon-Mount|

A dialog will appear requesting the name and Mount type.

|Mount-Dialog|

Enter the values below and the dialog will expand.

+------------+-----------+
| Parameter  | Value     |
+============+===========+
| Name       |  devguide |
+------------+-----------+
| Mount Type |  MongoDB  |
+------------+-----------+

In the expanded dialog enter the values below and click **Mount**.
If a parameter in the table below has no value, leave that
field empty in the interface.

+----------------+-----------+
| Parameter      | Value     |
+================+===========+
| Host           | localhost |
+----------------+-----------+
| Port           |  27017    |
+----------------+-----------+
| Username       |           |
+----------------+-----------+
| Password       |           |
+----------------+-----------+
| Database       |           |
+----------------+-----------+
| Other Settings |           |
+----------------+-----------+


|Mount-Dialog-Complete|


2.4 Creating a Database
~~~~~~~~~~~~~~~~~~~~~~~

* Click on the newly created server named **devguide**.  The interface now
  shows the databases that reside within the database system. A new database
  will need to be created to follow along with the guide.

* Click on the Create Folder icon.  |Create-Folder|

  A new folder will appear titled **Untitled Folder**.

* Hover the mouse over the new **Untitled Folder** folder.

* Click the **Move / rename** icon that appears to the right.  |Move-Rename|

* Change the name from **Untitled Folder** to ``devdb`` and click **Rename**.

* Click on the newly renamed **devdb** folder.

The interface should now look like this:

|In-Devdb|

So far in this guide you've installed SlamData, mounted a database and
created and renamed a folder.  Good progress.  Let's now get some data into
the database and start exploring.

2.5 Importing Example Data
~~~~~~~~~~~~~~~~~~~~~~~~~~

This guide uses a data set of fictitious patient information that was
randomly generated.  The reader can use any data set they wish, but
the examples in the remaining sections will assume the patients data
set is being used.

You can download a data set with 10,000 documents by following these
instructions:

* Right click `this link <https://github.com/damonLL/tutorial_files/raw/master/patients>`__
  and save the file as ``patients``.  This is a 9 MB JSON file.

* If your operating system named the file something other than
  **patients** you can either rename it or you can rename it
  inside of SlamData once it has been uploaded.

* Ensure that the SlamData UI is in devdb, and click
  the Upload icon.  |Upload|

* In the file dialog find the patients file and submit it.

* After successful upload a new collection should appear in the UI
  as follows:

|After-Upload|

As you can see, it is easy to quickly import JSON data into SlamData.
Other formats, such as CSV, can also be quickly imported.


2.5.1 Indexing Your Database
''''''''''''''''''''''''''''

.. attention:: **Indexing Your Database**

  While this step is not necessary, any database without
  indexes is going to perform slowly.  In SlamData this can be
  seen as a delay in displaying results.  If you choose to skip
  this step, be prepared to wait several seconds while the database
  system performs your searches.


The following commands are specific to MongoDB and must be executed
from the ``mongo`` shell console.

::

    use devdb
    db.patients.createIndex({first_name:1})
    db.patients.createIndex({middle_name:1})
    db.patients.createIndex({last_name:1})
    db.patients.createIndex({city:1})
    db.patients.createIndex({county:1})
    db.patients.createIndex({state:1})
    db.patients.createIndex({zip_code:1})
    db.patients.createIndex({street_address:1})
    db.patients.createIndex({height:1})
    db.patients.createIndex({weight:1})
    db.patients.createIndex({age:1})
    db.patients.createIndex({gender:1})
    db.patients.createIndex({last_visit:1})
    db.patients.createIndex({previous_visits:1})
    db.patients.createIndex({previous_addresses:1})
    db.patients.createIndex({codes:1})
    db.patients.createIndex({"codes.code":1})
    db.patients.createIndex({"codes.desc":1})


Congratulations!  There is now a usable dataset in your database
that is full of complex, nested data that you can explore.  Let's
start!


2.6 Exploring Data
~~~~~~~~~~~~~~~~~~

To simply look around and explore data, you can click on any file
(collection) that you see.  Start by clicking on the **patients**
file.

You'll be prompted to provide a name for a new Workspace.  A
Workspace is how users interact with the actual data within the
database.  Let's start by calling this ``My First Test`` and
clicking **Explore**.

|Name-Workspace|

Once you click Explore, the following screen should appear:

|First-Explore-Annotated|

+--------+---------------------------------------------------------------------------------------+
| Number | Description                                                                           |
+========+=======================================================================================+
|     1  |  Zoom icon takes user out of the Workspace and back to the database screen.           |
+--------+---------------------------------------------------------------------------------------+
|     2  |  Flip the card over for more options.                                                 |
+--------+---------------------------------------------------------------------------------------+
|     3  |  Card grips.  Slide these left or right to see the previous card or create a new one. |
+--------+---------------------------------------------------------------------------------------+
|     4  |  Browse controls for the current card.                                                |
+--------+---------------------------------------------------------------------------------------+
|     5  |  Your position within the deck. Gray circle indicates your place, white circles are   |
|        |  available to view.                                                                   |
+--------+---------------------------------------------------------------------------------------+

Feel free to click around on the browse arrows at the bottom to flip through the pages of
data.  It's easy to get an idea of the schema of this data set by looking at the top row.
In this case you can also see that the **codes** field is not actually a simple field but
an array of other documents!  Each of those documents in turn have a **code** and **desc**
field.

.. hint:: **Workspace Usage**

  You may not know it, but you actually just created a Workspace and a Root Deck,
  which contains an **Open Card** and a **Preview Table Card**!  SlamData did this
  automatically to save you time.

Any changes made within a Workspace are saved automatically.
At any time the user may zoom out of the current window.


2.7 Searching Data
~~~~~~~~~~~~~~~~~~

Viewing and browsing the data is helpful but data becomes less useful if you can't
find what you're looking for.  SlamData has two very powerful ways of finding
the data you need.  One is the **Search Card** and the other is the
**Query Card**.   We'll start with the **Search Card**.

* Click the **Flip Card** Icon (#2 in the previous image).

You'll see the following options on the back of that card:

|Card-Back|

* Click on **Delete card**.

The UI will now show the only remaining card in the deck which is the
**Open Card**.  This card allows you to select which collection you wish
to operate on with subsequent cards.  Let's leave this card in place.

* Click and drag the right-hand grip and slide it to the left.

You'll be presented with the following card types to choose from:

|Card-Choices-1|

Notice how the cards are different colors.  Blue cards
are those that can be created directly after the **Open Card**.  Light
gray cards are those cards that cannot be used following the previous
card.

* Select the **Search Card**.

A new **Search Card** will appear in the UI.  The search string appears
simple but has some very powerful search features within.

* Type the word ``Austin`` and either drag the right grip bar
  to the left, or simply click on the right grip bar.

* Select the **Preview Table Card**.

Depending on the performance of your system and database it may take
several seconds before the results are displayed.  Keep in mind that
SlamData is searching the patients collection that we imported into
the database system, and that indexes can significantly boost performance
for searches.

Once the results appear, you can browse them
with the controls in the bottom left of the
interface.

Did you notice that in the search string earlier we did not specify
which field we wanted to search?  That is part of the power of SlamData.
Relatively non-technical users can use SlamData to search all of
their data sources with little (or even no) knowledge in advance of the data
stored within.

Of course when searching all available fields for the search string
it is going to take longer than if we were to explicitly define which field.
Let's go back to the search card by dragging the current card
to the right again, or single-click on the left grip.

Let's search for any patients currently living in the city of Dallas.

* Type the string ``city:Dallas`` and either drag the right grip bar
  to the left, or simply click on the right grip bar.

* View the results in the **Preview Table Card** again.

The results should have appeared much faster than the previous search
because we told SlamData to only look at the **city** field.

We can also search on non-string values such as numbers.  Let's find
all of the patients who are between the ages of 45 and 50:

* Go back to the **Search Card**.

* Enter the string ``age:>=45 age:<=50``.

* View the results in the **Preview Table Card** again.

As one last example let's see how we can mix and match different types.
We want to know how many males over the age of 50 used to live in California.

* Go back to the **Search Card**.

* Enter the string ``previous_addresses:"[*]":state:CA age:>50 gender:=male``.

* View the results.

See the table below for some helpful query examples:


+---------------------------+---------------------------------------------------------------+
| Example                   | Description                                                   |
+===========================+===============================================================+
| ``colorado``              | Searches for the **substring** ``colorado`` in **all fields**.|
+---------------------------+---------------------------------------------------------------+
| ``=colorado``             | Searches for the **full word** ``colorado`` in **all fields**.|
+---------------------------+---------------------------------------------------------------+
| ``age:=50``               | Searches the field **age** for a value of 50.                 |
+---------------------------+---------------------------------------------------------------+
| ``age:>=50``              | Searches the field **age** for any value greater              |
|                           | than or equal to 50.                                          |
+---------------------------+---------------------------------------------------------------+
| ``age:>=50 age:<=60``     | Searches the field **age** for values between or equal to     |
|                           | 50 and 60.                                                    |
+---------------------------+---------------------------------------------------------------+
| ``codes:"[*]":desc:flu``  | Performs a deep search through the **codes** array and        |
|                           | examines each subdocument's **desc** field for the            |
|                           | **substring** ``flu``.                                        |
+---------------------------+---------------------------------------------------------------+

As you can see even users with no knowledge of SQL² can perform powerful
searches within SlamData!  


2.8 Querying Data with SQL²
~~~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to the **Search Card**, SlamData provides a **Query Card** that
allows users to execute ANSI-compatible SQL queries on top of any data source,
including NoSQL databases!  This is accomplished by using SlamData's SQL²
dialect, which is a superset of SQL that allows dynamic modeling and querying
of deeply nested, semi-structured data.

Using the same dataset we are going to perform queries, moving from basic
queries to more advanced queries.  Let's start off by cleaning up our
Workspace.

* Go to the **Preview Table Card**.

* Flip it over.

* Click on **Delete card**.

This should take you to the **Search Card**.

* Flip it over.

* Click on **Delete card**.

This should take you to the **Open Card**.  We will be using full
path names in the queries we will write, and **Query Cards** do not
use the **Open Card** so let's delete that one as well.

* Flip it over.

* Click on **Delete card**.

* Create a new **Query Card**.

The UI now presents the **Query Card**.  Within this card users can
enter simple or very long and complex SQL² queries against one,
two or more collections.

* Type in the following query:

.. code-block:: sql

    SELECT *
    FROM `/devguide/devdb/patients`

Notice how the path to the dataset is surrounded by
back-ticks (`````) not apostrophes (``'``)

* Select **Run Query** in the bottom right.

* Click the right grip.

* Select the **Preview Table Card** to see the results.

* Slide back to the **Query Card**.

* Type in or paste the following query:

.. code-block:: sql

    SELECT
        first_name,
        last_name
    FROM `/devguide/devdb/patients`
    WHERE
        state="TX" AND
        city="DALLAS"

Note that the query can span multiple lines, and that strings
are surrounded by quotation marks (``"``) on both ends.  This
is a requirement for all string data types.

* Select **Run Query** in the bottom right.

* Slide back to the **Preview Table Card** to see the results.

* Slide back to the **Query Card**.

Let's now create a query that formats the results a little better.

* Type in or paste the following query:

.. code-block:: sql

    SELECT
        last_name || ',' || first_name AS Name,
        city AS City,
        zip_code AS Zip
    FROM `/devguide/devdb/patients`
    WHERE
        state="TX"
    ORDER BY zip_code ASC

* Select **Run Query** in the bottom right.

* Slide back to the **Preview Table Card** to see the results.

Notice in this query we are concatenating the **last_name** and
**first_name** fields together, separated by a comma.  The comma
itself is surrounded by apostrophes (``'``) because it is a single
character.  If it was more than one character it would be a string
and would require full quotation marks around it.

We have also given the results some aliases to display rather
than the actual field names.

Finally, we are ordering (**ORDER BY**) the results in ascending (**ASC**)
order based on the **zip_code** field.

The results table should now look similar to the following image:

|Zip-Results|

Up to this point we have been using SQL² to query simple *top-level* fields,
or those fields which are not nested.  We know from previous examples
that this data set stores nested data in the **codes** array, but 
it also contains **previous_addresses** and **previous_visits** arrays.

Let's find out the total number of male and female patients
from each state that have an illness related to an ulcer. This will
require using the flattening operator (``[*]``) so SlamData
can examine all of the documents in the **codes** array.

* Slide to the **Query Card**.

* Type or paste the following query:

.. code-block:: sql

    SELECT
        state AS State,
        gender AS Gender,
        COUNT(*) AS Count
    FROM `/devguide/devdb/patients`
    WHERE
        codes[*].desc LIKE "%ulcer%"
    GROUP BY state, gender
    ORDER BY COUNT(*) DESC
    LIMIT 20

* Select **Run Query** in the bottom right.

* Slide to the **Preview Table Card** to see the results.

SQL² allows for very complex queries.  You can find out more by
reviewing the `SQL² Reference <sql-squared-reference.html>`__.
Additional features include using the **JOIN** command to combine data
from two or more tables, utilizing variables within queries
(as explained in Section 3), using standard math operations,
retrieving not only field values but also field names
dynamically, and much more.

Now that you have a good idea of what can be accomplished with
SQL² queries, let's create some forms that your users can
interact with.  These forms can drive the results of the charts
we'll use for visualization, which makes it easy for your users
to find, report and chart complex data without understanding
the mechanics behind it!


Section 3 - Interactive Forms and Visualizations
------------------------------------------------

SlamData provides everything you need to create an interactive
visual analytics environment for your users.

From this point on in the guide we will assume that we
are creating an environment for medical facilities to search
through patient data for various reasons.  The Workspaces we
create will be used by medical staff for this purpose.


3.1 Static Markdown Forms
~~~~~~~~~~~~~~~~~~~~~~~~~

We will start this section with a new Workspace.  You can leave
the existing Workspace alone or you can delete it if you wish.

To (optionally) delete the existing Workspace:

* If you are still in the Workspace, click on the zoom-out
  icon. |Zoom-Out|

* Locate the **My First Test** Workspace and hover your mouse over it.

* Click on the trash can icon that appears to the right. |Trash-Can|

We'll create a new Workspace and call it **Average Weight by City**.

* Click the Create Workspace icon in the upper right. |Create-Workspace|

* Select the **Setup Markdown Card**.

This step is necessary so that the Workspace is saved and we can go
back to rename it soon.

* Create a **Show Markdown** card directly after the **Setup Markdown Card**.

* Zoom back out to the database view.

Let's rename the Workspace now so it's obvious that we are working
with it.

* Hover over the new Workspace labeled **Untitled Workspace.slam**.

* Click the **Move / rename** icon to the right. |Move-Rename|

* Replace **Untitled Workspace** with ``Average Weight by City``
  and click **Rename**.

* Click on the **Average Weight by City.slam** Workspace again.

Ensure that you are in the **Setup Markdown Card**.

SlamData uses a specific form of `Markdown <https://daringfireball.net/projects/markdown/>`__ 
sometimes referred to
as SlamDown.  Markdown allows a user to format text with a few
simple syntax rules.  SlamData's version also allows UI elements
(such as drop downs, radio buttons and check boxes) to be dynamically
populated from the results of queries.

Let's first show some examples of what the Markdown forms can do.
Paste the following text into the card:

::

    # Heading 1

    ## Heading 2

    ### Text formatting

    * Here is an unnumbered list.
    * You can have _emphasized_ and **bold** text.

    1. Here is a numbered list.
    2. Here is the second entry with ```inline formatting```

    Paragraphs are separated by
    an empty line.

    This is another new paragraph.

    > You can also have some nice
    > block quote areas.

    You can also have fenced code blocks like this:

    ```
    SELECT * FROM `/devguide/devdb/patients`
    WHERE
      first_name = "Sue"
    ```

    ### Interactive Elements

    #### Input Fields

    name = ____ (Sue)

    numberOnly = #____ (1984)

    #### Selectors

    city = {Austin, Dallas, Houston}

    favoriteColor = (x) red () blue () green

    computers = [] PC [x] Mac [x] Linux

    beginDate = ____-__-__

    stopTime = __:__

    fullDateTime = ____-__-__ __:__

* Select **Run Query** in the bottom right.

* Click over to the **Show Markdown Card** to view the results.

Notice how much control you have over the presentation of
the information.  You can also include links and images inside
of Markdown as well.  For a full description of all fields
and their behavior see the `SlamDown Reference <slamdown-reference.html>`__.

* Click back to the **Setup Markdown Card**.

Replace the contents with something more useful and appropriate
to our use case:

::

    ## General Patient Information

    There are !`` SELECT COUNT(*) FROM `/devguide/devdb/patients` `` patients

    _Average_ age: !`` SELECT AVG(age) FROM `/devguide/devdb/patients` ``

    The *Heaviest* patient: !`` SELECT MAX(weight) FROM `/devguide/devdb/patients` `` pounds

    The **Shortest** patient: !`` SELECT MIN(height) FROM `/devguide/devdb/patients` `` inches

* Select **Run Query** in the bottom right.

* Click over to the **Show Markdown Card** to see the results.

Notice that we populated some of the text with actual results from the database.
Keep in mind that to print the results of a query in Markdown, the query must
begin with an exclamation point (``!``) and two back-ticks (``````) and end
with two more back-ticks (``````).

* Click back to the **Setup Markdown Card**.

We will use similar syntax to populate the elements of an interactive form
in the next section.


3.2 Interactive Markdown Forms
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here is where things get really fun for both you and your users.
Let's actually provide the functionality that we promise with the
title of **Average Weight by City**.

First we want the user to select the state to report on.  This will
then allow us to query the database for patients that reside in
cities within that state.

* Replace the contents of the current **Markdown Setup Card**
  with the following code.

::

    ### Select the state to report on

    state = {!``SELECT DISTINCT(state) FROM `/devguide/devdb/patients` ORDER BY state``}

* Select **Run Query** in the bottom right.

* Click over to the **Show Markdown Card** to see the results.

* Click on the dropdown next to **State** to see that the element
  was populated with the query we typed in.

* Flip the **Show Markdown Card** over by clicking the icon in the upper right. |Icon-Flip|

* Select **Wrap**.

Note that your interface should now look similar to the following:

|Wrapped-Deck|

You can click and drag the left and right hand grips just as before to see
the previous cards.

* Click on the deck to make it active.

* Flip the deck by clicking the icon. |Icon-Flip|

* Select **Mirror**.

Your interface should now look similar to the following:

|Mirrored-Deck|

We have just mirrored a deck.  This means that the second deck starts off
from where the first left off, but it also means any changes to the first
deck will immediately impact the second deck as well.  This is how
we chain events in a Workspace and allow the actions in one deck to
affect other decks.

* Click on the new second deck to make it active.

* Create a new card in this second deck, selecting the **Query Card**.

* Type in or paste the following query into the **Query Card**:

.. code-block:: sql

    SELECT
      city AS City,
      AVG(weight) AS AvgWeight
    FROM `/devguide/devdb/patients`
    WHERE
      state IN :state
    GROUP BY
      city
    ORDER BY AVG(weight) DESC

Whenever a variable from a Markdown form is used in a query it must be
preceded by a colon ( ``:`` ).

Also note that we can **ORDER BY** an aggregation value such as **AVG**.

* Select **Run Query** in the bottom right.

* Click on the right grip to create a new card and select the **Preview Table Card**.

|MD-and-Show-Decks|

* Select a different state in the first deck and watch the results
  table update automatically.

Viewing data in table form is useful but sometimes a graphical representation
makes all the difference.  To prepare for that, let's go back and change
the query and limit the results to 20 cities, so a bar chart doesn't appear crowded.

* Click the left grip to go back to the **Query Card**.

* Add the following line to the end of the query:

.. code-block:: sql

  LIMIT 20

* Select **Run Query** in the bottom right.

* Slide back over to the **Preview Table Card**.

Now we are ready to add some visualizations!


3.3 Creating a Chart
~~~~~~~~~~~~~~~~~~~~

Before creating an actual chart we need to set it up.  Remember earlier
that decks can build off one another.  We need to now mirror the
**Preview Table Card**:

* Click on second deck to make it active.

* Click on the flip icon to flip the deck over. |Icon-Flip|

* Select **Mirror**.

* Resize so that your interface looks similar to the following image:

|All-3-Decks|

* Select the new deck and click on the right grip and then select the **Setup Chart Card**.

* Select the **Bar Chart** icon.

The bar chart icon will change from gray to blue to show that it is active.

* For the **Category**, select **.City** as the axis source.

* Slide to the right to create a new card and select **Show Chart**.

Your interface should now look like the following image:

|All-3-With-Chart|

* Select a new state in the first deck and watch both of the other
  decks update dynamically.

* Try hovering your mouse over the individual bars in the chart and you can
  view the actual value.

Setting up interactive forms and charts is as simple as that!  In the next
section we'll go over how to share these charts with others.


Section 4 - Publishing and Simple Embedding
-------------------------------------------

4.1 - Publishing
~~~~~~~~~~~~~~~~

SlamData makes it easy to take all the work you've done up to this
point and publish it so that others can use it as well.

* Click the flip icon on the **Draftboard Card**.  Note that this
  is the card that contains all of the existing decks.  Just as
  each deck has a back to it, each card does as well, including
  the **Draftboard Card**.  Be sure not to flip any of the three
  decks we've created - click the icon in the white box border
  surrounding the other decks.

* Select **Publish deck**.

A URL will be presented to you that you can share with others.
The URL will only be accessible while SlamData is running.


.. _eCharts: https://ecomfe.github.io/echarts/index-en.html


.. |Murray| image:: images/SD4/murray.png

.. |Murray-Small| image:: images/SD4/murray-small.png

.. |Home-Annotated| image:: images/SD4/screenshots/home-annotated-with-numbers.png

.. |Icon-Mount| image:: images/SD4/icon-mount.png

.. |Mount-Dialog| image:: images/SD4/screenshots/mount-dialog.png

.. |Mount-Dialog-Complete| image:: images/SD4/screenshots/mount-dialog-complete.png

.. |Create-Folder| image:: images/SD4/icon-create-folder.png

.. |Move-Rename| image:: images/SD4/icon-move-rename.png

.. |Zoom-Out| image:: images/SD4/icon-zoom-out.png

.. |Create-Workspace| image:: images/SD4/icon-create-workspace.png

.. |Upload| image:: images/SD4/icon-upload.png

.. |Trash-Can| image:: images/SD4/icon-trash-can.png

.. |Icon-Flip| image:: images/SD4/icon-flip.png

.. |Icon-Gray-Bar-Chart| image:: images/SD4/icon-gray-bar.png

.. |In-Devdb| image:: images/SD4/screenshots/in-devdb-clean.png

.. |After-Upload| image:: images/SD4/screenshots/after-upload.png

.. |Name-Workspace| image:: images/SD4/screenshots/name-workspace.png

.. |First-Explore-Annotated| image:: images/SD4/screenshots/first-explore-annotated.png

.. |Wrapped-Deck| image:: images/SD4/screenshots/wrapped-deck.png

.. |Mirrored-Deck| image:: images/SD4/screenshots/mirrored-deck.png

.. |Card-Back| image:: images/SD4/screenshots/back-of-card.png

.. |Card-Choices-1| image:: images/SD4/screenshots/new-card-choices-1.png

.. |MD-and-Show-Decks| image:: images/SD4/screenshots/md-and-show-decks.png

.. |All-3-Decks| image:: images/SD4/screenshots/all-3-decks.png

.. |Zip-Results| image:: images/SD4/screenshots/zip-results.png

.. |All-3-With-Chart| image:: images/SD4/screenshots/all-3-with-chart.png

.. |SD-Nesting| image:: images/SD4/screenshots/sd-nesting.png

.. |Embed-Code-1| image:: images/SD4/screenshots/embed-code-1.png

.. |Embed-Code-2| image:: images/SD4/screenshots/embed-code-2.png

.. |Embed-Code-3| image:: images/SD4/screenshots/embed-code-3.png

.. |Sample-1-1-Full-Report| image:: images/SD4/screenshots/sample-1-1-full-report.png

.. |Report-2-Workspace| image:: images/SD4/screenshots/report-2-workspace.png

.. |Sample-1-2-Full-Report| image:: images/SD4/screenshots/sample-1-2-full-report.png

.. |Config-Example| image:: images/SD4/screenshots/config-example.png

.. |Header-Grip| image:: images/SD4/screenshots/header-grip.png

.. |Sign-In| image:: images/SD4/screenshots/sign-in.png

.. |Navigate| image:: images/SD4/screenshots/navigate.png

.. |Embed-Code-Secure-1| image:: images/SD4/screenshots/embed-code-secure-1.png

.. |Embed-Code-Secure-2| image:: images/SD4/screenshots/embed-code-secure-2.png

.. |Embed-Code-Secure-3| image:: images/SD4/screenshots/embed-code-secure-3.png

.. |Sample-2-1-Full-Report| image:: images/SD4/screenshots/sample-2-1-full-report.png

.. |Repo-Link| raw:: html

   <a href="https://github.com/slamdata/slamdata-dev-examples" target="_blank">repository link</a>

.. raw:: html

    <embed>
    <script type="text/javascript" id="hs-script-loader" async defer src="//js.hs-scripts.com/2389041.js"></script>
    </embed>

