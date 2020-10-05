

.. index:: Plugin
.. _Plugins:

Plugins
=======

FreeNAS\ :sup:`®` provides the ability to extend the built-in NAS
services by providing two methods for installing additional software.

:ref:`Plugins` allow the user to browse, install, and configure
pre-packaged software from the web interface. This method is easy to use,
but provides a limited amount of available software. Each plugin is
automatically installed into its own limited
`FreeBSD jail <https://en.wikipedia.org/wiki/Freebsd_jail>`__ that
cannot install additional software.

:ref:`Jails` provide more control over software installation, but
requires working from the command line and a good understanding of
networking basics and software installation on FreeBSD-based systems.

Look through the :ref:`Plugins` and :ref:`Jails` sections to become
familiar with the features and limitations of each. Choose the method
that best meets the needs of the application.

.. note:: :ref:`Jail Storage` must be configured before plugins are
   available on FreeNAS\ :sup:`®`. This means having a suitable
   :ref:`pool <Creating Pools>` created to store plugins.


.. _Installing Plugins:

Installing Plugins
-----------------------

A plugin is a self-contained application installer designed to
integrate into the FreeNAS\ :sup:`®` web interface. A plugin offers several
advantages:

* the FreeNAS\ :sup:`®` web interface provides a browser for viewing the list of
  available plugins

* the FreeNAS\ :sup:`®` web interface provides buttons for installing, starting,
  managing, and uninstalling plugins

* if the plugin has configuration options, a management screen is
  added to the FreeNAS\ :sup:`®` web interface for these options to be configured

View available plugins by clicking
:menuselection:`Plugins`.

:numref:`Figure %s <view_list_plugins_fig>` shows some of the
available plugins.


.. _view_list_plugins_fig:

.. figure:: images/plugins-available.png

   Viewing the List of Available Plugins


.. note:: If the list of available plugins is not displayed, open
   :ref:`Shell` and verify that the FreeNAS\ :sup:`®` system can :command:`ping`
   an address on the Internet. If it cannot, add a default gateway
   address and DNS server address in
   :menuselection:`Network --> Global Configuration`.

Click :guilabel:`Browse a Collection` to toggle the plugins list
between
`iXsystems plugins <https://www.freenas.org/plugins/>`__,
which receive updates every few weeks, and
`Community plugins <https://github.com/ix-plugin-hub/iocage-plugin-index>`__.

Click :guilabel:`REFRESH INDEX` to refresh the current list
of plugins.

Click a plugin icon to see the description, whether it is an Official
or Community plugin, the version available, and the number of
installed instances.

To install the selected plugin, click :guilabel:`INSTALL`.

.. _installing_plugin_fig:

.. figure:: images/plugins-install-example.png

   Installing the Plex Plugin


.. note:: A warning will display when an unofficial plugin is selected for installation.

Enter a :guilabel:`Jail Name`. A unique name is required, since
multiple installations of the same plugin are supported. Names can
contain letters, numbers, periods (:literal:`.`), dashes (:literal:`-`),
and underscores (:literal:`_`).

Most plugins default to :guilabel:`NAT`. This setting is recommended
as it does not require manual configuration of multiple available IP
addresses and prevents addressing conflicts on the network.

Some plugins default to :guilabel:`DHCP` as their management utility
conflicts with :guilabel:`NAT`. Keep these plugins set to
:guilabel:`DHCP` unless manually configuring an IP address is
preferred.

If both :guilabel:`NAT` and :guilabel:`DHCP` are unset, an IPv4 or
IPv6 address can be manually entered. If desired, an IPv4 or IPv6 interface can
be selected. If no interface is selected the jail IP address uses the current
active interface. The IPv4 or IPv6 address must be in the range of the local
network.

Click :guilabel:`ADVANCED PLUGIN INSTALLATION` to show all options for
the plugin jail. The options are described in
:ref:`Advanced Jail Creation`.

To start the installation, click :guilabel:`SAVE`.

Depending on the size of the application, the installation can take
several minutes to download and install. A confirmation message is
shown when the installation completes, along with any
post-installation notes.

Installed plugins appear on the :menuselection:`Plugins`
page as shown in :numref:`Figure %s <view_installed_plugins_fig>`.

.. note:: Plugins are also added to
   :menuselection:`Jails`
   as a *pluginv2* jail. This type of jail is editable like a
   standard jail, but the *UUID* cannot be altered.
   See :ref:`Managing Jails` for more details about modifying
   jails.


.. _view_installed_plugins_fig:

.. figure:: images/plugins-available.png

   Viewing Installed Plugins


Plugins are immediately started after installation. By default, all
plugins are started when the system boots. Unsetting :guilabel:`Boot`
means the plugin will not start when the system boots and must be
started manually.

In addition to the :guilabel:`Jail` name, the :guilabel:`Columns`
menu can be used to display more information about installed
Plugins.

More information such as *RELEASE* and
*VERSION* is shown by clicking ui-chevron-right. Options to
:guilabel:`RESTART`, :guilabel:`STOP`, :guilabel:`UPDATE`,
:guilabel:`MANAGE`, and :guilabel:`UNINSTALL` the plugin are also
displayed. If an installed plugin has notes, the notes can be viewed by
clicking :guilabel:`POST INSTALL NOTES`.

Plugins with additional documentation also have a
:guilabel:`DOCUMENTATION` button which opens the
README in the plugin repository.

The plugin must be started before the installed application is
available. Click ui-chevron-right and :guilabel:`START`. The plugin
:guilabel:`Status` changes to :literal:`up` when it starts successfully.

Stop and immediately start an :literal:`up` plugin by clicking
ui-chevron-right and :guilabel:`RESTART`.

Click ui-chevron-right and :guilabel:`MANAGE` to open a management
or configuration screen for the application. Plugins with a management
interface show the IP address and port to that page in the *Admin Portal* column.

.. note:: Not all plugins have a functional management option. See
   :ref:`Managing Jails` for more instructions about interacting with
   a plugin jail with the shell.


Some plugins have options that need to be set before their
service will successfully start. Check the website of the application to see what
documentation is available. If there are any difficulties using a plugin, refer to the
official documentation for that application.

If the application requires access to the data stored on the FreeNAS\ :sup:`®`
system, click the entry for the associated jail in the
:menuselection:`Jails` page and add storage as described in
:ref:`Additional Storage`.

Click ui-options and :guilabel:`Shell` for the plugin jail in the
:menuselection:`Jails` page. This will give access to the shell of the
jail containing the application to complete or test the configuration.

If a plugin jail fails to start, open the plugin jail shell from the
:menuselection:`Jail` page and type :command:`tail /var/log/messages` to
see if any errors were logged.


.. _Updating Plugins:

Updating Plugins
----------------

When a newer version of a plugin or release becomes available in the
official repository, click ui-chevron-right and :guilabel:`UPDATE`.
Updating a plugin updates the operating system and version of the
plugin.

.. _updating_installed_plugin_fig:

.. figure:: images/plugins-update.png

   Updating a Plugin


Updating a plugin also restarts that plugin. To update or upgrade the
plugin jail operating system, see :ref:`Jail Updates and Upgrades`.


.. _Uninstalling Plugins:

Uninstalling Plugins
----------------------------

Installing a plugin creates an associated jail. Uninstalling a plugin
deletes the jail because it is no longer required. This
means all **datasets or snapshots that are associated with the plugin
are also deleted.** Make sure to back up any important data from the
plugin **before** uninstalling it.

:numref:`Figure %s <deleting_installed_plugin_fig>` shows an example of
uninstalling a plugin by expanding the plugin's entry and clicking
:guilabel:`UNINSTALL`. A two-step dialog opens to
confirm the action. **This is the only warning.** Enter the
plugin name, set the :guilabel:`Confirm` checkbox, and click
:guilabel:`DELETE` to remove the plugin and the associated jail,
dataset, and snapshots.

.. _deleting_installed_plugin_fig:

.. figure:: images/plugins-delete-example.png

   Uninstalling a Plugin and its Associated Jail and Dataset

.. _Creating Plugins:

Create a Plugin
---------------


If an application is not available as a plugin, it is possible to
create a new plugin for FreeNAS\ :sup:`®` in a few steps. This requires an
existing `GitHub <https://github.com>`__ account.

**Create a new artifact repository on** `GitHub <https://github.com>`__.

Refer to :numref:`table %s <plugin-artifact-files>` for the files to add
to the artifact repository.


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.33\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.67\linewidth-2\tabcolsep}|

.. _plugin-artifact-files:

.. table:: FreeNAS\ :sup:`®` Plugin Artifact Files
   :class: longtable

   +-------------------------+----------------------------------------------------------------------+
   | Directory/File          | Description                                                          |
   +=========================+======================================================================+
   | :file:`post_install.sh` | This script is run *inside* the jail after it is created and any     |
   |                         | packages installed. Enable services in :file:`/etc/rc.conf` that     |
   |                         | need to start with the jail and apply any configuration              |
   |                         | customizations with this this script.                                |
   |                         |                                                                      |
   +-------------------------+----------------------------------------------------------------------+
   | :file:`ui.json`         | JSON file that accepts the key or value options. For example:        |
   |                         |                                                                      |
   |                         | :samp:`adminportal: "http://%%IP%%/"`                                |
   |                         |                                                                      |
   |                         | designates the web-interface of the plugin.                          |
   |                         |                                                                      |
   +-------------------------+----------------------------------------------------------------------+
   | :file:`overlay/`        | Directory of files overlaid on the jail after install.               |
   |                         | For example, :file:`usr/local/bin/myfile` is placed in the           |
   |                         | :file:`/usr/local/bin/myfile` location of the jail. Can be used to   |
   |                         | supply custom files and configuration data, scripts, and             |
   |                         | any other type of customized files to the plugin jail.               |
   +-------------------------+----------------------------------------------------------------------+
   | :file:`settings.json`   | JSON file that manages the settings interface of the plugin.         |
   |                         | Required fields include:                                             |
   |                         |                                                                      |
   |                         | * :samp:`"servicerestart" : "service foo restart"`                   |
   |                         |                                                                      |
   |                         | Command to run when restarting the plugin service after              |
   |                         | changing settings.                                                   |
   |                         |                                                                      |
   |                         | * :samp:`"serviceget" : "/usr/local/bin/myget"`                      |
   |                         |                                                                      |
   |                         | Command used to get values for plugin configuration.                 |
   |                         | Provided by the plugin creator. The command accepts                  |
   |                         | two arguments for key or value pair.                                 |
   |                         |                                                                      |
   |                         | * :samp:`"options" : { }`                                            |
   |                         |                                                                      |
   |                         | This subsection contains arrays of elements, starting with the "key" |
   |                         | name and required arguments for that particular type of setting.     |
   |                         |                                                                      |
   |                         | See :ref:`options subsection example <plugin-json-options>`          |
   |                         | below.                                                               |
   |                         |                                                                      |
   +-------------------------+----------------------------------------------------------------------+


This example :file:`settings.json` file is used for the
:guilabel:`Quasselcore` plugin. It is also available online in the
`iocage-plugin-quassel artifact repository
<https://github.com/freenas/iocage-plugin-quassel/blob/master/settings.json>`__.


.. _plugin-json-options:

.. code-block:: json

   {
	   "servicerestart":"service quasselcore restart",
	   "serviceget": "/usr/local/bin/quasselget",
	   "serviceset": "/usr/local/bin/quasselset",
	   "options": {
		   "adduser": {
			   "type": "add",
			   "name": "Add User",
			   "description": "Add new quasselcore user",
			   "requiredargs": {
				   "username": {
					   "type": "string",
					   "description": "Quassel Client Username"
				   },
				   "password": {
					   "type": "password",
					   "description": "Quassel Client Password"
				   },
				   "fullname": {
					   "type": "string",
					   "description": "Quassel Client Full Name"
				   }
			   },
			   "optionalargs": {
				   "adminuser": {
					   "type": "bool",
					   "description": "Can this user administrate quasselcore?"
				   }
			   }
		   },
		   "port": {
			   "type": "int",
			   "name": "Quassel Core Port",
			   "description": "Port for incoming quassel connections",
			   "range": "1024-32000",
			   "default": "4242",
			   "requirerestart": true
		   },
		   "sslmode": {
			   "type": "bool",
			   "name": "SSL Only",
			   "description": "Only accept SSL connections",
			   "default": true,
			   "requirerestart": true

		   },
		   "ssloption": {
			   "type": "combo",
			   "name": "SSL Options",
			   "description": "SSL Connection Options",
			   "requirerestart": true,
			   "default": "tlsallow",
			   "options": {
					   "tlsrequire": "Require TLS",
					   "tlsallow": "Allow TLS",
					   "tlsdisable": "Disable TLS"
			   }
		   },
		   "deluser": {
			   "type": "delete",
			   "name": "Delete User",
			   "description": "Remove a quasselcore user"
		   }

	   }
   }


**Create and submit a new JSON file for the plugin:**

Clone the
`iocage-plugin-index <https://github.com/ix-plugin-hub/iocage-plugin-index>`__
GitHub repository.


.. tip:: Full tutorials and documentation for GitHub and :command:`git`
   commands are available on
   `GitHub Guides <https://guides.github.com/>`__.


On the local copy of :file:`iocage-plugin-index`, create a new JSON file
for the FreeNAS\ :sup:`®` plugin. The JSON file describes the plugin, the
packages it requires for operation, and other installation details.
This file is named :samp:`{pluginname}.json`. For example, the
`Madsonic <https://github.com/ix-plugin-hub/iocage-plugin-index/blob/master/madsonic.json>`__
plugin is named :file:`madsonic.json`.

The fields of the file are described in
:numref:`table %s <plugins-plugin-jsonfile-contents>`.


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.33\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.67\linewidth-2\tabcolsep}|

.. _plugins-plugin-jsonfile-contents:

.. table:: Plugin JSON File Contents
   :class: longtable

   +------------------------------+-------------------------------------------------------------------------------+
   | Data Field                   | Description                                                                   |
   +==============================+===============================================================================+
   | :literal:`"name":`           | Name of the plugin.                                                           |
   |                              |                                                                               |
   +------------------------------+-------------------------------------------------------------------------------+
   | :literal:`"plugin_schema":`  | Optional. Enter *2* if simplified post-install information has                |
   |                              | been supplied in :file:`post_install.sh`. After specifying *2*,               |
   |                              | echo the information to be presented to the user in                           |
   |                              | :file:`/root/PLUGIN_INFO` inside the                                          |
   |                              | :file:`post_install.sh` file.                                                 |
   |                              | See the :ref:`rslsync.json <rslsync-plugin-schema>` and                       |
   |                              | :ref:`rslsync post_install.sh <rslsync-post-install>` examples.               |
   |                              |                                                                               |
   +------------------------------+-------------------------------------------------------------------------------+
   | :literal:`"release":`        | FreeBSD RELEASE to use for the plugin jail.                                   |
   |                              |                                                                               |
   +------------------------------+-------------------------------------------------------------------------------+
   | :literal:`"artifact":`       | URL of the plugin artifact repository.                                        |
   |                              |                                                                               |
   +------------------------------+-------------------------------------------------------------------------------+
   | :literal:`"pkgs":`           | The FreeBSD packages required by the plugin.                                  |
   |                              |                                                                               |
   +------------------------------+-------------------------------------------------------------------------------+
   | :literal:`"packagesite":`    | Content Delivery Network (CDN) used by the plugin jail. Default for           |
   |                              | the TrueOS CDN is :literal:`http://pkg.cdn.trueos.org/iocage`.                |
   |                              |                                                                               |
   +------------------------------+-------------------------------------------------------------------------------+
   | :literal:`"fingerprints":`   | :literal:`"function":`                                                        |
   |                              |                                                                               |
   |                              | Default is                                                                    |
   |                              | :literal:`sha256`.                                                            |
   |                              |                                                                               |
   |                              | :literal:`"fingerprint":`                                                     |
   |                              |                                                                               |
   |                              | The pkg fingerprint for the artifact repository. Default is                   |
   |                              | :literal:`226efd3a126fb86e71d60a37353d17f57af816d1c7ecad0623c21f0bf73eb0c7`   |
   |                              |                                                                               |
   +------------------------------+-------------------------------------------------------------------------------+
   | :literal:`"official":`       | Define whether this is an official iXsystems-supported plugin.                |
   |                              | Enter :literal:`true` or :literal:`false`.                                    |
   |                              |                                                                               |
   +------------------------------+-------------------------------------------------------------------------------+


.. _rslsync-plugin-schema:

.. code-block:: json
   :caption: rslsync.json
   :linenos:
   :emphasize-lines: 3

   {
     "name": "rslsync",
     "plugin_schema": "2",
     "release": "11.2-RELEASE",
     "artifact": "https://github.com/freenas/iocage-plugin-btsync.git",
     "pkgs": [
       "net-p2p/rslsync"
     ],
     "packagesite": "http://pkg.cdn.trueos.org/iocage/unstable",
     "fingerprints": {
	     "iocage-plugins": [
		     {
		     "function": "sha256",
		     "fingerprint": "226efd3a126fb86e71d60a37353d17f57af816d1c7ecad0623c21f0bf73eb0c7"
	     }
	     ]
     },
     "official": true
   }

.. _rslsync-post-install:

.. code-block:: sh
   :caption: post_install.sh
   :name: rslsync-post_install
   :linenos:
   :emphasize-lines: 9

   #!/bin/sh -x

   # Enable the service
   sysrc -f /etc/rc.conf rslsync_enable="YES"
   # Start the service
   service rslsync start 2>/dev/null

   echo "rslsync now installed" > /root/PLUGIN_INFO
   echo "foo" >> /root/PLUGIN_INFO

Here is :file:`quasselcore.json` reproduced as an example:

.. code-block:: json

   {
     "name": "Quasselcore",
     "release": "11.1-RELEASE",
     "artifact": "https://github.com/freenas/iocage-plugin-quassel.git",
     "pkgs": [
       "irc/quassel-core"
     ],
     "packagesite": "http://pkg.cdn.trueos.org/iocage",
     "fingerprints": {
             "iocage-plugins": [
                     {
                     "function": "sha256",
                     "fingerprint": "226efd3a126fb86e71d60a37353d17f57af816d1c7ecad0623c21f0bf73eb0c7"
             }
             ]
     },
     "official": true
   }


The correct directory and package name of the plugin application must be
used for the :samp:`"pkgs":` value. Find the package name and directory
by searching `FreshPorts <https://www.freshports.org/>`__ and checking
the "To install the port:" line. For example, the *Quasselcore* plugin
uses the directory and package name :file:`/irc/quassel-core`.

Now edit :file:`iocage-plugin-index/INDEX`. Add an entry for the new
plugin that includes these fields:

* :literal:`"MANIFEST":` Add the name of the newly created
  :file:`plugin.json` file here.

* :literal:`"name":` Use the same name used within the :file:`.json`
  file.

* :literal:`"icon":` Most plugins will have a specific icon. Search the
  web and save the icon to the :file:`iocage-plugin-index/icons/`
  directory as a :file:`.png`. The naming convention is
  :file:`pluginname.png`. For example, the
  :guilabel:`Madsonic` plugin has the icon file
  :file:`madsonic.png`.

* :literal:`"description":` Describe the plugin in a single sentence.

* :literal:`"official":` Specify if the plugin is supported by
  iXsystems. Enter :literal:`false`.

See the
`INDEX <https://github.com/ix-plugin-hub/iocage-plugin-index/blob/master/INDEX>`__
for examples of :file:`INDEX` entries.

**Submit the plugin**

Open a pull request for the
`iocage-plugin-index repo <https://github.com/ix-plugin-hub/iocage-plugin-index>`__.
Make sure the pull request contains:

* the new :file:`plugin.json` file.

* the plugin icon :file:`.png` added to the
  :file:`iocage-plugin-index/icons/` directory.

* an update to the :file:`INDEX` file with an entry for the new plugin.

* a link to the artifact repository populated with all required plugin
  files.


.. _Test a plugin:

Test a Plugin
~~~~~~~~~~~~~

.. warning:: Installing experimental plugins is not recommended for
   general use of FreeNAS\ :sup:`®`. This feature is meant to help plugin creators
   test their work before it becomes generally available on FreeNAS\ :sup:`®`.


Plugin pull requests are merged into the :literal:`master` branch of the
`iocage-plugin-index <https://github.com/ix-plugin-hub/iocage-plugin-index>`__
repository. These plugins are not available in the web interface until they
are tested and added to a *RELEASE* branch of the repository. It is
possible to test an in-development plugin by using this
:command:`iocage` command:
:samp:`iocage fetch -P --name {PLUGIN} {IPADDRESS_PROPS} --branch 'master'`

This will install the plugin, configure it with any chosen properties,
and specifically use the :literal:`master` branch of the repository to
download the plugin.

Here is an example of downloading and configuring an experimental plugin
with the FreeNAS\ :sup:`®`
:menuselection:`Shell`:

.. code-block:: none

   [root@freenas ~]# iocage fetch -P --name mineos ip4_addr="em0|10.231.1.37/24" --branch 'master'
   Plugin: mineos
     Official Plugin: False
     Using RELEASE: 11.2-RELEASE
     Using Branch: master
     Post-install Artifact: https://github.com/jseqaert/iocage-plugin-mineos.git
     These pkgs will be installed:
   ...

   ...
   Running post_install.sh
   Command output:
   ...

   ...
   Admin Portal:
   http://10.231.1.37:8443
   [root@freenas ~]#


This plugin appears in the
:menuselection:`Jails` and
:menuselection:`Plugins`
screens as :literal:`mineos` and can be tested with the FreeNAS\ :sup:`®` system.

.. index:: Asigra Plugin
.. _Asigra Plugin:

Asigra Plugin
------------------

The Asigra plugin connects FreeNAS\ :sup:`®` to a third party service and is
subject to licensing. Please read the
`Asigra Software License Agreement <https://www.asigra.com/legal/software-license-agreement>`__
before using this plugin.

To begin using Asigra services after installing the plugin, open the
plugin options and click :guilabel:`Register`. A new browser tab opens
to
`register a user with Asigra <https://licenseportal.asigra.com/licenseportal/user-registration.do>`__.

The FreeNAS\ :sup:`®` system must have a public static IP address for Asigra
services to function.

Refer to the Asigra documentation for details about using the Asigra
platform:

* `DS-Operator Management Guide <https://s3.amazonaws.com/asigra-documentation/Help/v14.1/DS-System%20Help/index.html>`__:
  Using the :literal:`DS-Operator` interface to manage the plugin
  :literal:`DS-System` service. Click :guilabel:`Management` in the
  plugin options to open the :literal:`DS-Operator` interface.

* `DS-Client Installation Guide <https://s3.amazonaws.com/asigra-documentation/Guides/Cloud%20Backup/v14.1/Client_Software_Installation_Guide.pdf>`__:
  How to install the :literal:`DS-Client` system. :literal:`DS-Client`
  aggregates backup content from endpoints and transmits it to the
  :literal:`DS-System service`.

* `DS-Client Management Guide <https://s3.amazonaws.com/asigra-documentation/Help/v14.1/DS-Client%20Help/index.html>`__:
  Managing the :literal:`DS-Client` system after it has been
  successfully installed at one or more locations.
