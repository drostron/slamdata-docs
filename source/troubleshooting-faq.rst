.. figure:: images/white-logo.png
   :alt: SlamData Logo


Troubleshooting FAQ
===================


Section 1 - Configuration
-------------------------


1.1 Configuration File Locations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Upon initial launch, SlamData will not have a configuration file.
However, once a valid database mount has been configured, a file
will be created and used to store mount points.
Unless specified on the command line, SlamData will look for its
configuration file in the following locations by default:

+-------------------------+-------------------------------------------------------------+
| Operating System        | File Location                                               |
+=========================+=============================================================+
| Mac OS                  | $HOME/Library/Application Support/quasar/quasar-config.json |
+-------------------------+-------------------------------------------------------------+
| Microsoft Windows       | %HOMEDIR%\\AppData\\Local\\quasar\\quasar-config.json       |
+-------------------------+-------------------------------------------------------------+
| Linux (various vendors) | $HOME/.config/quasar/quasar-config.json                     |
+-------------------------+-------------------------------------------------------------+

.. warning:: **Modifying the configuration file**

  If the configuration file needs to be modified by hand, a backup copy should be created
  first. Furthermore, if the file is modified while SlamData is running, any changes may
  be overwritten.


1.1.1 Configuration File Differences
''''''''''''''''''''''''''''''''''''

SlamData Community Edition relies on the **quasar-config.json**
configuration file to store all metadata for the product, including
server configurations, mount points, views, and so on.

SlamData Analyst and Advanced Editions rely upon a PostgreSQL or
Java H2 database to store metadata. Depending upon the edition,
additional information will be stored such as security information for users,
groups, permissions, actions and tokens.

If there is no metadata source when SlamData Analyst or Advanced Edition
start, the **quasar-config.json** file will be used.


1.2 Log File Locations
~~~~~~~~~~~~~~~~~~~~~~

SlamData has a single log file whose location depends upon the Operating System.
Replace ``version`` in the table below with the actual version number that you are
running.

+-------------------------+---------------------------------------------------------------------------------+
| Operating System        | File Location                                                                   |
+=========================+=================================================================================+
| Mac OS                  | /Applications/SlamData <version>.app/Contents/java/app/slamdata-<version>.log   |
+-------------------------+---------------------------------------------------------------------------------+
| Microsoft Windows       | C:\\Program Files (x86)\\slamdata <version>\\slamdata-<version>.log             |
+-------------------------+---------------------------------------------------------------------------------+
| Linux (various vendors) | $HOME/slamdata<version>/slamdata-<version>.log                                  |
+-------------------------+---------------------------------------------------------------------------------+


Section 2 - Running SlamData
----------------------------


2.1 SlamData Won't Start
~~~~~~~~~~~~~~~~~~~~~~~~

Follow the steps below to ensure all known issues have been addressed.

1. If an older version of SlamData is installed in a Virtual Machine (VM),
   it may require more than one CPU core before it will launch. If you are
   experiencing problems running an older version of SlamData in a VM, try
   increasing the number of cores and restarting.

2. In older versions of SlamData, an invalid database mount may prevent SlamData
   from starting.  An invalid database mount could be a database that was
   previously available but is no longer available, credentials may have changed, port
   number changed, or any other configuration change that does not allow
   previously validated configurations to successfully connect.


2.2 Accessing SlamData
~~~~~~~~~~~~~~~~~~~~~~

The default SlamData URL is ``http://<servername>:20223``

Example: ``http://localhost:20223``


2.3 How do I see which version I'm running?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

SlamData's version will be displayed in the browser title bar or
tab title.

The version of the Quasar analytics backend engine can be obtained
by browsing to ``http://<servername>:20223/server/info``

Example: ``http://localhost:20223/server/info``


2.4 Running SlamData in the Cloud
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When running SlamData with a hosting provider, such as Amazon EC2, the
most common error encountered is a security policy misconfiguration.
SlamData will need to connect to a data source over the same port as a
standard database client.

A data source or database server and the SlamData server do not
need to run on the same system.

Use the following checklist to ensure network problems are minimized.

1. Verify the security policy for the data source or database server is:

-  Accepting incoming connections from the SlamData server IP address.
-  Accepting incoming connections on the correct port.

2. If you are still unable to connect to your hosted data source or database system:

-  Verify that you can connect with a standard database client from any system.
-  Connect with a standard database client from the same system SlamData is running on.
