.. _System:

System
======

The System section of the web interface contains these entries:

* :ref:`General` configures general settings such as HTTPS access, the
  language, and the timezone

* :ref:`NTP Servers` adds, edits, and deletes Network Time Protocol
  servers

* :ref:`Boot` creates, renames, and deletes boot
  environments. It also shows the condition of the Boot Pool.

* :ref:`Advanced` configures advanced settings such as the serial
  console, swap space, and console messages


* :ref:`Email` configures the email address to receive notifications

* :ref:`System Dataset` configures the location where logs and
  reporting graphs are stored

* :ref:`Alert Services` configures services used to notify the
  administrator about system events.

* :ref:`Alert Settings` lists the available :ref:`Alert` conditions and
  provides configuration of the notification frequency for each alert.

* :ref:`Cloud Credentials` is used to enter connection credentials for
  remote cloud service providers

* :ref:`SSH Connections` manages connecting to a remote system with SSH.

* :ref:`SSH Keypairs` manages all private and public SSH key pairs.

* :ref:`Tunables` provides a front-end for tuning in real-time and to
  load additional kernel modules at boot time

* :ref:`Update` performs upgrades and checks for system
  updates

* :ref:`CAs`: import or create internal or intermediate CAs
  (Certificate Authorities)

* :ref:`Certificates`: import existing certificates, create
  self-signed certificates, or configure ACME certificates.

* :ref:`ACME DNS`: automate domain authentication for compatible CAs and
  certificates.


* :ref:`Support`: report a bug or request a new feature.


Each of these is described in more detail in this section.


.. _General:

General
-------

:menuselection:`System --> General`
contains options for configuring the web interface and other basic system
settings.

.. _system_general_fig:
.. figure:: images/system-general.png

   General System Options


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.25\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.12\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.63\linewidth-2\tabcolsep}|

.. _system_general_tab:

.. table:: General Configuration Settings
   :class: longtable

   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | Setting              | Value          | Description                                                                                                              |
   |                      |                |                                                                                                                          |
   +======================+================+==========================================================================================================================+
   | GUI SSL Certificate  | drop-down menu | The system uses a self-signed :ref:`certificate <Certificates>` to enable encrypted web interface connections. To change      |
   |                      |                | the default certificate, select a different created or imported certificate.                                             |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | WebGUI IPv4 Address  | drop-down menu | Choose a recent IP addresses to limit the usage when accessing the web interface. The                                         |
   |                      |                | built-in HTTP server binds to the wildcard address of *0.0.0.0* (any address) and issues an                              |
   |                      |                | alert if the specified address becomes unavailable.                                                                      |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | WebGUI IPv6 Address  | drop-down menu | Choose a recent IPv6 addresses to limit the usage when accessing the web interface. The                                       |
   |                      |                | built-in HTTP server binds to the wildcard address of *0.0.0.0* (any address) and issues an alert                        |
   |                      |                | if the specified address becomes unavailable.                                                                            |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | WebGUI HTTP Port     | integer        | Allow configuring a non-standard port for accessing the web interface over HTTP. Changing this setting                        |
   |                      |                | might require changing a                                                                                                 |
   |                      |                | `Firefox configuration setting                                                                                           |
   |                      |                | <https://www.redbrick.dcu.ie/~d_fens/articles/Firefox:_This_Address_is_Restricted>`__.                                   |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | WebGUI HTTPS Port    | integer        | Allow configuring a non-standard port to access the web interface over HTTPS.                                                 |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | WebGUI HTTP ->       | checkbox       | Redirect *HTTP* connections to *HTTPS*. A :guilabel:`GUI SSL Certificate` is required for *HTTPS*. Activating this also  |
   | HTTPS Redirect       |                | sets the `HTTP Strict Transport Security (HSTS) <https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security>`__        |
   |                      |                | maximum age to *31536000* seconds (one year). This means that after a browser connects to the FreeNAS\ :sup:`®`          |
   |                      |                | web interface for the first time, the browser continues to use HTTPS and renews this setting every year.                      |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | Language             | combo box      | Select a language from the drop-down menu. The list can be sorted by :guilabel:`Name` or                                 |
   |                      |                | `Language code <https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes>`__.                                               |
   |                      |                | View the translated status of a language in the                                                                          |
   |                      |                | `webui GitHub repository <https://github.com/freenas/webui/tree/master/src/assets/i18n>`__.                              |
   |                      |                | Refer to :ref:`Contributing to FreeNAS\ :sup:`®`` for more information about assisting with translations.                |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | Console Keyboard Map | drop-down menu | Select a keyboard layout.                                                                                                |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | Timezone             | drop-down menu | Select a timezone.                                                                                                       |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | Syslog level         | drop-down menu | When :guilabel:`Syslog server` is defined, only logs matching this level are sent.                                       |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | Syslog server        | string         | Remote syslog server DNS hostname or IP address. Nonstandard port numbers can be used by adding a colon and              |
   |                      |                | the port number to the hostname, like :samp:`mysyslogserver:1928`. Log entries are written to local logs                 |
   |                      |                | and sent to the remote syslog server.                                                                                    |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | Crash reporting      | checkbox       | Send failed HTTP request data which can include client and server IP addresses, failed method call tracebacks, and       |
   |                      |                | middleware log file contents to iXsystems.                                                                               |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+
   | Usage Collection     | checkbox       | Enable sending anonymous usage statistics to iXsystems.                                                                  |
   +----------------------+----------------+--------------------------------------------------------------------------------------------------------------------------+

After making any changes, click :guilabel:`SAVE`. Changes to
any of the :guilabel:`GUI` fields can interrupt web interface connectivity while the
new settings are applied.

This screen also contains these buttons:

.. _saveconfig:

* :guilabel:`SAVE CONFIG`: save a backup copy of the current configuration
  database in the format *hostname-version-architecture* to the computer
  accessing the web interface. Saving the configuration after
  making any configuration changes is highly recommended. FreeNAS\ :sup:`®`
  automatically backs up the configuration database to the system
  dataset every morning at 3:45. However, this backup does not occur if
  the system is shut down at that time. If the system dataset is stored
  on the boot pool and the boot pool becomes unavailable, the backup
  will also not be available. The location of the system dataset can be
  viewed or set using
  :menuselection:`System --> System Dataset`.

  .. note:: :ref:`SSH` keys are not stored in the configuration database
     and must be backed up separately. System host keys are files with
     names beginning with :file:`ssh_host_` in :file:`/usr/local/etc/ssh/`.
     The root user keys are stored in :file:`/root/.ssh`.


  There are two types of passwords. User account passwords for the base
  operating system are stored as hashed values, do not need to be
  encrypted to be secure, and are saved in the system configuration
  backup. Other passwords, like iSCSI CHAP passwords, Active Directory
  bind credentials, and cloud credentials are stored in an encrypted form
  to prevent them from being visible as plain text in the saved system
  configuration. The key or *seed* for this encryption is normally stored
  only on the operating system device. When :guilabel:`Save Config` is chosen, a
  dialog gives two options. :guilabel:`Export Password Secret Seed`
  includes passwords in the configuration file which allows the
  configuration file to be restored to a different operating system device where the
  decryption seed is not already present. Configuration backups
  containing the seed must be physically secured to prevent decryption
  of passwords and unauthorized access.

  .. warning:: The :guilabel:`Export Password Secret Seed` option is off
     by default and should only be used when making a configuration
     backup that will be stored securely. After moving a configuration
     to new hardware, media containing a configuration backup with a
     decryption seed should be securely erased before reuse.

  :guilabel:`Export Pool Encryption Keys` includes the encryption keys of
  encrypted pools in the configuration file. The encyrption keys are
  restored if the configuration file is uploaded to the system using
  :guilabel:`UPLOAD CONFIG`.

* :guilabel:`UPLOAD CONFIG`: allows browsing to the location of a
  previously saved configuration file to restore that configuration.

* :guilabel:`RESET CONFIG`: reset the configuration database
  to the default base version. This does not delete user SSH keys or any
  other data stored in a user home directory. Since configuration
  changes stored in the configuration database are erased, this option
  is useful when a mistake has been made or to return a test system to
  the original configuration.


.. index:: NTP Servers,
.. _NTP Servers:

NTP Servers
-----------

The network time protocol (NTP) is used to synchronize the time on the
computers in a network. Accurate time is necessary for the successful
operation of time sensitive applications such as Active Directory or
other directory services. By default, FreeNAS\ :sup:`®` is pre-configured to use
three public NTP servers. If the network is using a directory service,
ensure that the FreeNAS\ :sup:`®` system and the server running the directory
service have been configured to use the same NTP servers.

Available NTP servers can be found at
`<https://support.ntp.org/bin/view/Servers/NTPPoolServers>`__.
For time accuracy, choose NTP servers that are geographically close to
the physical location of the FreeNAS\ :sup:`®` system.

Click :menuselection:`System --> NTP Servers` and :guilabel:`ADD`
to add an NTP server. :numref:`Figure %s <ntp_server_fig>` shows the
configuration options.
:numref:`Table %s <ntp_server_conf_opts_tab>`
summarizes the options available when adding or editing an NTP server.
`ntp.conf(5) <https://www.freebsd.org/cgi/man.cgi?query=ntp.conf>`__
explains these options in more detail.


.. _ntp_server_fig:

.. figure:: images/system-ntp-servers-add.png

   Add an NTP Server


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.25\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.12\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.63\linewidth-2\tabcolsep}|

.. _ntp_server_conf_opts_tab:

.. table:: NTP Servers Configuration Options
   :class: longtable

   +-------------+-----------+----------------------------------------------------------------------------------------------------+
   | Setting     | Value     | Description                                                                                        |
   |             |           |                                                                                                    |
   |             |           |                                                                                                    |
   +=============+===========+====================================================================================================+
   | Address     | string    | Enter the hostname or IP address of the NTP server.                                                |
   +-------------+-----------+----------------------------------------------------------------------------------------------------+
   | Burst       | checkbox  | Recommended when :guilabel:`Max. Poll` is greater than *10*. Only use on personal servers.         |
   |             |           | **Do not** use with a public NTP server.                                                           |
   +-------------+-----------+----------------------------------------------------------------------------------------------------+
   | IBurst      | checkbox  | Speed up the initial synchronization, taking seconds rather than minutes.                          |
   +-------------+-----------+----------------------------------------------------------------------------------------------------+
   | Prefer      | checkbox  | This option is only recommended for highly accurate NTP servers, such as those with                |
   |             |           | time monitoring hardware.                                                                          |
   +-------------+-----------+----------------------------------------------------------------------------------------------------+
   | Min Poll    | integer   | The minimum polling interval, in seconds, as a power of 2. For example, *6* means 2^6,             |
   |             |           | or 64 seconds. The default is *6*, minimum value is *4*.                                           |
   +-------------+-----------+----------------------------------------------------------------------------------------------------+
   | Max Poll    | integer   | The maximum polling interval, in seconds, as a power of 2. For example, *10* means 2^10,           |
   |             |           | or 1,024 seconds. The default is *10*, maximum value is *17*.                                      |
   +-------------+-----------+----------------------------------------------------------------------------------------------------+
   | Force       | checkbox  | Force the addition of the NTP server, even if it is currently unreachable.                         |
   +-------------+-----------+----------------------------------------------------------------------------------------------------+


.. index:: Boot Environments, Multiple Boot Environments, Boot
.. _Boot:

Boot
----

FreeNAS\ :sup:`®` supports a ZFS feature known as multiple boot environments.
With multiple boot environments, the process of updating the operating
system becomes a low-risk operation. The updater automatically creates
a snapshot of the current boot environment and adds it to the boot
menu before applying the update.

If an update fails, reboot the system and select the previous boot
environment, using the instructions in :ref:`If Something Goes Wrong`,
to instruct the system to go back to that system state.

.. note:: Boot environments are separate from the configuration
   database. Boot environments are a snapshot of the
   *operating system* at a specified time. When a FreeNAS\ :sup:`®` system
   boots, it loads the specified boot environment, or operating
   system, then reads the configuration database to load the
   current configuration values. If the intent is to make
   configuration changes rather than operating system changes, make a
   backup of the configuration database first using the instructions in
   :ref:`System --> General <General>`.


The example shown in :numref:`Figure %s <view_boot_env_fig>`, includes
the two boot environments that are created when FreeNAS\ :sup:`®` is installed.
The *Initial-Install* boot environment can be booted into if the system
needs to be returned to a non-configured version of the installation.

.. _view_boot_env_fig:
.. figure:: images/system-boot-environments.png

   Viewing Boot Environments


Each boot environment entry contains this information:

* **Name:** the name of the boot entry as it will appear in the boot
  menu. Alphanumeric characters, dashes (*-*), underscores (*_*),
  and periods (*.*) are allowed.

* **Active:** indicates which entry will boot by default if the user
  does not select another entry in the boot menu.

* **Created:** indicates the date and time the boot entry was created.

* **Space:** displays the size of the boot environment.

* **Keep:** indicates whether or not this boot environment can be
  pruned if an update does not have enough space to proceed. Click
  ui-options and :guilabel:`Keep` for an entry if that boot
  environment should not be automatically pruned.

Click ui-options on an entry to access actions specific to that entry:

* **Activate:** only appears on entries which are not currently set to
  :guilabel:`Active`. Changes the selected entry to the default boot
  entry on next boot. The status changes to :guilabel:`Reboot` and
  the current :guilabel:`Active` entry changes from
  :guilabel:`Now/Reboot` to :guilabel:`Now`, indicating that it
  was used on the last boot but will not be used on the next boot.

* **Clone:** makes a new boot environment from the selected boot
  environment. When prompted for the name of the clone, alphanumeric characters,
  dashes (*-*), underscores (*_*), and periods (*.*) are allowed.

* **Rename:** used to change the name of the boot environment. Alphanumeric
  characters, dashes (*-*), underscores (*_*), and periods (*.*) are allowed.

* **Delete:** used to delete the highlighted entry, which also removes
  that entry from the boot menu. Since an activated entry cannot be
  deleted, this button does not appear for the active boot environment.
  To delete an entry that is currently activated, first activate another
  entry. Note that this button does not appear for the *default* boot
  environment as this entry is needed to return the system to the original
  installation state.

* **Keep:** used to toggle whether or not the updater can prune
  (automatically delete) this boot environment if there is not enough
  space to proceed with the update.

Click :guilabel:`ACTIONS` to:

* **Add:** make a new boot environment from the active environment. The
  active boot environment contains the text :literal:`Now/Reboot` in the
  :guilabel:`Active` column. Only alphanumeric characters, underscores,
  and dashes are allowed in the :guilabel:`Name`.

* **Stats/Settings:** display statistics for the operating system device: condition,
  total and used size, and date and time of the last scrub. By
  default, the operating system device is scrubbed every 7 days.  To change the
  default, input a different number in the
  :guilabel:`Automatic scrub interval (in days)` field and click
  :guilabel:`UPDATE INTERVAL`.

* **Boot Pool Status:** display the status of each device in the operating system device,
  including any read, write, or checksum errors.

* **Scrub Boot Pool:** perform a manual scrub of the operating system device.

.. index:: Mirroring the operating system device
.. _Mirroring the operating system device:

operating system device Mirroring
~~~~~~~~~~~~~~~~~~~~~

:menuselection:`System --> Boot --> Boot Pool Status` is used to manage
the devices comprising the operating system device. An example is seen in
:numref:`Figure %s <status_boot_dev_fig>`.

.. _status_boot_dev_fig:
.. figure:: images/system-boot-environments-status.png

   Viewing the Status of the operating system device

FreeNAS\ :sup:`®` supports 2-device mirrors for the operating system device. In a mirrored
configuration, a failed device can be detached and replaced.

An additional device can be attached to an existing one-device operating system device,
with these caveats:

* The new device must have at least the same capacity as the existing
  device. Larger capacity devices can be added, but the mirror will only
  have the capacity of the smallest device. Different models of devices
  which advertise the same nominal size are not necessarily the same
  actual size. For this reason, adding another device of the same model
  of is recommended.

* It is **strongly recommended** to use SSDs rather than USB devices when
  creating a mirrored operating system device.

Click ui-options on a device entry to access actions specific to that
device:

* **Attach:** use to add a second device to create a mirrored operating system device.
  If another device is available, it appears in the
  :guilabel:`Member disk` drop-down menu. Select the desired device. The
  :guilabel:`Use all disk space` option controls the capacity made
  available to the operating system device. By default, the new device is partitioned
  to the same size as the existing device. When
  :guilabel:`Use all disk space` is enabled, the entire capacity of the
  new device is used. If the original operating system device fails and is
  detached, the boot mirror will consist of just the newer drive, and
  will grow to whatever capacity it provides. However, new devices added
  to this mirror must now be as large as the new capacity. Click
  :guilabel:`SAVE` to attach the new disk to the mirror.

* **Detach:** remove the failed device from the mirror so that it can be
  replaced.

* **Replace:** once the failed device has been detached, select the new
  replacement device from the :guilabel:`Member disk` drop-down menu to
  rebuild the mirror.


.. _Advanced:

Advanced
--------

:menuselection:`System --> Advanced`
is shown in
:numref:`Figure %s <system_adv_fig>`.
The configurable settings are summarized in
:numref:`Table %s <adv_config_tab>`.


.. _system_adv_fig:
.. figure:: images/system-advanced.png

   Advanced Screen


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.25\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.12\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.63\linewidth-2\tabcolsep}|

.. _adv_config_tab:

.. table:: Advanced Configuration Settings
   :class: longtable

   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Setting                                  | Value              | Description                                                                                      |
   |                                          |                    |                                                                                                  |
   +==========================================+====================+==================================================================================================+
   | Show Text Console without Password       | checkbox           | Set for the text console to be available without entering a password.                            |
   | Prompt                                   |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Enable Serial Console                    | checkbox           | **Do not** enable this option if the serial port is disabled. Adds the *Serial Port* and         |
   |                                          |                    | *Serial Speed* fields.                                                                           |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Serial Port                              | string             | Select the serial port address in hex.                                                           |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Serial Speed                             | drop-down menu     | Select the speed in bps used by the serial port.                                                 |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Swap size in GiB                         | non-zero number    | By default, all data disks are created with this amount of swap. This setting does not affect    |
   |                                          |                    | log or cache devices as they are created without swap. Setting to *0* disables swap creation     |
   |                                          |                    | completely. This is *strongly* discouraged.                                                      |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Enable autotune                          | checkbox           | Enable the :ref:`autotune` script which attempts to optimize the system based on                 |
   |                                          |                    | the installed  hardware. *Warning*: Autotuning is only used as a temporary measure and is        |
   |                                          |                    | not a permanent fix for system hardware issues.                                                  |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Enable Debug Kernel                      | checkbox           | Use a debug version of the kernel on the next boot.                                              |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Show console messages                    | checkbox           | Display console messages from :file:`/var/log/console.log` in real time at bottom of browser     |
   |                                          |                    | window. Click the console to bring up a scrollable screen. Set the :guilabel:`Stop refresh`      |
   |                                          |                    | option in the scrollable screen to pause updates. Unset to continue watching messages as they    |
   |                                          |                    | occur. When this option is set, a button to show the console log appears on busy spinner dialogs.|
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | MOTD banner                              | string             | This message is shown when a user logs in with SSH.                                              |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Show advanced fields by default          | checkbox           | Show :guilabel:`Advanced Mode` fields by default.                                                |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Use FQDN for logging                     | checkbox           | Include the Fully-Qualified Domain Name (FQDN) in logs to precisely identify systems             |
   |                                          |                    | with similar hostnames.                                                                          |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | ATA Security User                        | drop-down menu     | User passed to :command:`camcontrol security -u` for unlocking SEDs. Values are                  |
   |                                          |                    | *User* or *Master*.                                                                              |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | SED Password                             | string             | Global password used to unlock :ref:`Self-Encrypting Drives`.                                    |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+
   | Reset SED Password                       | checkbox           | Select to clear the :guilabel:`Password for SED` column of                                       |
   |                                          |                    | :menuselection:`Storage --> Disks`.                                                              |
   |                                          |                    |                                                                                                  |
   +------------------------------------------+--------------------+--------------------------------------------------------------------------------------------------+


Click the :guilabel:`SAVE` button after making any changes.

This tab also contains this button:

:guilabel:`SAVE DEBUG`: used to generate text files that contain diagnostic
information. After the debug data is collected, the system prompts for
a location to save the compressed :file:`.tar` file.


.. index:: Autotune
.. _Autotune:

Autotune
~~~~~~~~

FreeNAS\ :sup:`®` provides an autotune script which optimizes the system
depending on the installed hardware. For example, if a pool exists on
a system with limited RAM, the autotune script automatically adjusts
some ZFS sysctl values in an attempt to minimize memory starvation
issues. It should only be used as a temporary measure on a system that
hangs until the underlying hardware issue is addressed by adding more
RAM. Autotune will always slow such a system, as it caps the ARC.

The :guilabel:`Enable autotune` option in
:menuselection:`System --> Advanced`
is off by default. Enable this option to run the autotuner at boot.
To run the script immediately, reboot the system.

If the autotune script adjusts any settings, the changed values appear
in
:menuselection:`System --> Tunables`. Note that deleting
tunables that were created by autotune only affects the current
session, as autotune-set tunables are recreated at boot. This means that
any autotune-set value that is manually changed will revert back to the
value set by autotune on reboot. To permanently change a value set by
autotune, change the description of the tunable. For example, changing
the description to *manual override* prevents autotune from reverting
that tunable back to the autotune default value.

When attempting to increase the performance of the FreeNAS\ :sup:`®` system, and
particularly when the current hardware may be limiting performance,
try enabling autotune.

For those who wish to see which checks are performed, the autotune
script is located in :file:`/usr/local/bin/autotune`.


.. index:: Self-Encrypting Drives
.. _Self-Encrypting Drives:

Self-Encrypting Drives
~~~~~~~~~~~~~~~~~~~~~~

FreeNAS\ :sup:`®` version 11.1-U5 introduced Self-Encrypting Drive (SED) support.

These SED specifications are supported:

* Legacy interface for older ATA devices. **Not recommended for
  security-critical environments**

* `TCG Opal 1 <https://trustedcomputinggroup.org/wp-content/uploads/Opal_SSC_1.00_rev3.00-Final.pdf>`_
  legacy specification

* `TCG OPAL 2 <https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-Opal_SSC_v2.01_rev1.00.pdf>`__
  standard for newer consumer-grade devices

* `TCG Opalite <https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-Opalite_SSC_FAQ.pdf>`__
  is a reduced form of OPAL 2

* TCG Pyrite
  `Version 1 <https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-Pyrite_SSC_v1.00_r1.00.pdf>`__
  and
  `Version 2 <https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-Pyrite_SSC_v2.00_r1.00_PUB.pdf>`__
  are similar to Opalite, but hardware encryption is removed. Pyrite
  provides a logical equivalent of the legacy ATA security for non-ATA
  devices. Only the drive firmware is used to protect the device.

  .. danger:: Pyrite Version 1 SEDs do not have PSID support and **can
     become unusable if the password is lost.**


* `TCG Enterprise <https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-SSC_Enterprise-v1.01_r1.00.pdf>`__
  is designed for systems with many data disks. These SEDs do not have
  the functionality to be unlocked before the operating system boots.

See this
Trusted Computing Group\ :sup:`®` and NVM Express\ :sup:`®`
`joint white paper <https://nvmexpress.org/wp-content/uploads/TCGandNVMe_Joint_White_Paper-TCG_Storage_Opal_and_NVMe_FINAL.pdf>`__
for more details about these specifications.

FreeNAS\ :sup:`®` implements the security capabilities of
`camcontrol <https://www.freebsd.org/cgi/man.cgi?query=camcontrol>`__
for legacy devices and
`sedutil-cli <https://www.mankier.com/8/sedutil-cli>`__
for TCG devices. When managing a SED from the command line, it is
recommended to use the :command:`sedhelper` wrapper script for
:command:`sedutil-cli` to ease SED administration and unlock the full
capabilities of the device. Examples of using these commands to identify
and deploy SEDs are provided below.

A SED can be configured before or after assigning the device to a
:ref:`pool <Pools>`.

By default, SEDs are not locked until the administrator takes ownership
of them. Ownership is taken by explicitly configuring a global or
per-device password in the FreeNAS\ :sup:`®` web interface and adding the password to
the SEDs. Adding SED passwords to FreeNAS\ :sup:`®` also allows FreeNAS\ :sup:`®` to
automatically unlock SEDs.

A password-protected SED protects the data stored on the device
when the device is physically removed from the FreeNAS\ :sup:`®` system. This
allows secure disposal of the device without having to first wipe the
contents. Repurposing a SED on another system requires the SED password.


.. _Deploying SEDs:

Deploying SEDs
^^^^^^^^^^^^^^

Run :command:`sedutil-cli --scan` in the :ref:`Shell` to detect and list
devices. The second column of the results identifies the drive type:

* **no** indicates a non-SED device
* **1** indicates a legacy TCG OPAL 1 device
* **2** indicates a modern TCG OPAL 2 device
* **L** indicates a TCG Opalite device
* **p** indicates a TCG Pyrite 1 device
* **P** indicates a TCG Pyrite 2 device
* **E** indicates a TCG Enterprise device

Example:

.. code-block:: none

   root@truenas1:~ # sedutil-cli --scan
   Scanning for Opal compliant disks
   /dev/ada0  No  32GB SATA Flash Drive SFDK003L
   /dev/ada1  No  32GB SATA Flash Drive SFDK003L
   /dev/da0   No  HGST    HUS726020AL4210  A7J0
   /dev/da1   No  HGST    HUS726020AL4210  A7J0
   /dev/da10    E WDC     WUSTR1519ASS201  B925
   /dev/da11    E WDC     WUSTR1519ASS201  B925


FreeNAS\ :sup:`®` supports setting a global password for all detected SEDs or
setting individual passwords for each SED. Using a global password for
all SEDs is strongly recommended to simplify deployment and avoid
maintaining separate passwords for each SED.


.. _Setting a global password for SEDs:

Setting a global password for SEDs
..................................

Go to
:menuselection:`System --> Advanced --> SED Password`
and enter the password. **Record this password and store it in a safe
place!**

Now the SEDs must be configured with this password. Go to the
:ref:`Shell` and enter :samp:`sedhelper setup {password}`, where
*password* is the global password entered in
:menuselection:`System --> Advanced --> SED Password`.

:command:`sedhelper` ensures that all detected SEDs are properly
configured to use the provided password:

.. code-block:: none

   root@truenas1:~ # sedhelper setup abcd1234
   da9			[OK]
   da10			[OK]
   da11			[OK]


Rerun :samp:`sedhelper setup {password}` every time a new SED is placed
in the system to apply the global password to the new SED.


.. _Creating separate passwords for each SED:

Creating separate passwords for each SED
........................................

Go to
:menuselection:`Storage --> Disks`.
Click ui-options for the confirmed SED, then :guilabel:`Edit`.
Enter and confirm the password in the :guilabel:`SED Password` and
:guilabel:`Confirm SED Password` fields.

The
:menuselection:`Storage --> Disks`
screen shows which disks have a configured SED password. The
:guilabel:`SED Password` column shows a mark when the disk has a
password. Disks that are not a SED or are unlocked using the global
password are not marked in this column.

The SED must be configured to use the new password. Go to the
:ref:`Shell` and enter :samp:`sedhelper setup --disk {da1} {password}`,
where *da1* is the SED to configure and *password* is the created
password from
:menuselection:`Storage --> Disks --> Edit Disks --> SED Password`.

This process must be repeated for each SED and any SEDs added to the
system in the future.

.. danger:: Remember SED passwords! If the SED password is lost, SEDs
   cannot be unlocked and their data is unavailable. Always record SED
   passwords whenever they are configured or modified and store them
   in a secure place!


.. _Check SED Functionality:

Check SED Functionality
^^^^^^^^^^^^^^^^^^^^^^^

When SED devices are detected during system boot, FreeNAS\ :sup:`®` checks for
configured global and device-specific passwords.

Unlocking SEDs allows a pool to contain a mix of SED and non-SED
devices. Devices with individual passwords are unlocked with their
password. Devices without a device-specific password are unlocked using
the global password.

To verify SED locking is working correctly, go to the :ref:`Shell`.
Enter :samp:`sedutil-cli --listLockingRange 0 {password} dev/{da1}`,
where *da1* is the SED and *password* is the global or individual
password for that SED. The command returns :literal:`ReadLockEnabled: 1`,
:literal:`WriteLockEnabled: 1`, and :literal:`LockOnReset: 1` for drives
with locking enabled:

.. code-block:: none

   root@truenas1:~ # sedutil-cli --listLockingRange 0 abcd1234 /dev/da9
   Band[0]:
       Name:            Global_Range
       CommonName:      Locking
       RangeStart:      0
       RangeLength:     0
       ReadLockEnabled: 1
       WriteLockEnabled:1
       ReadLocked:      0
       WriteLocked:     0
       LockOnReset:     1


.. index:: SED, SED Password
.. _Managing SED Password and Data:

Managing SED Passwords and Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This section contains command line instructions to manage SED
passwords and data. The command used is
`sedutil-cli(8) <https://www.mankier.com/8/sedutil-cli>`__. Most
SEDs are TCG-E (Enterprise) or TCG-Opal
(`Opal v2.0 <https://trustedcomputinggroup.org/wp-content/uploads/TCG_Storage-Opal_SSC_v2.01_rev1.00.pdf>`__).
Commands are different for the different drive types, so the first
step is identifying which type is being used.

.. warning:: These commands can be destructive to data and passwords.
   Keep backups and use the commands with
   caution.

Check SED version on a single drive, :literal:`/dev/da0` in this example:

.. code-block:: none

   root@truenas:~ # sedutil-cli --isValidSED /dev/da0
   /dev/da0 SED --E--- Micron_5N/A U402


All connected disks can be checked at once:

.. code-block:: none

   root@truenas:~ # sedutil-cli --scan
   Scanning for Opal compliant disks
   /dev/ada0 No 32GB SATA Flash Drive SFDK003L
   /dev/ada1 No 32GB SATA Flash Drive SFDK003L
   /dev/da0 E Micron_5N/A U402
   /dev/da1 E Micron_5N/A U402
   /dev/da12 E SEAGATE XS3840TE70014 0103
   /dev/da13 E SEAGATE XS3840TE70014 0103
   /dev/da14 E SEAGATE XS3840TE70014 0103
   /dev/da2 E Micron_5N/A U402
   /dev/da3 E Micron_5N/A U402
   /dev/da4 E Micron_5N/A U402
   /dev/da5 E Micron_5N/A U402
   /dev/da6 E Micron_5N/A U402
   /dev/da9 E Micron_5N/A U402
   No more disks present ending scan
   root@truenas:~ #


.. _TCG-Opal Instructions:

TCG-Opal Instructions
.....................

Reset the password without losing data:
:samp:`sedutil-cli --revertNoErase {oldpassword} /dev/{device}`

Use **both** of these commands to change the password without
destroying data:

| :samp:`sedutil-cli --setSIDPassword {oldpassword} {newpassword} /dev/{device}`
| :samp:`sedutil-cli --setPassword {oldpassword} Admin1 {newpassword} /dev/{device}`

Wipe data and reset password to default MSID:
:samp:`sedutil-cli --revertPer {oldpassword} /dev/{device}`

Wipe data and reset password using the PSID:
:samp:`sedutil-cli --yesIreallywanttoERASEALLmydatausingthePSID {PSINODASHED} /dev/{device}`
where *PSINODASHED* is the PSID located on the pysical drive with no
dashes (:literal:`-`).


.. _TCG-E Instructions:

TCG-E Instructions
..................

Use **all** of these commands to reset the password without losing
data:

| :samp:`sedutil-cli --setSIDPassword {oldpassword} "" /dev/{device}`
| :samp:`sedutil-cli --setPassword {oldpassword} EraseMaster "" /dev/{device}`
| :samp:`sedutil-cli --setPassword {oldpassword} BandMaster0 "" /dev/{device}`
| :samp:`sedutil-cli --setPassword {oldpassword} BandMaster1 "" /dev/{device}`

Use **all** of these commands to change the password without destroying
data:

| :samp:`sedutil-cli --setSIDPassword {oldpassword} {newpassword} /dev/{device}`
| :samp:`sedutil-cli --setPassword {oldpassword} EraseMaster {newpassword} /dev/{device}`
| :samp:`sedutil-cli --setPassword {oldpassword} BandMaster0 {newpassword} /dev/{device}`
| :samp:`sedutil-cli --setPassword {oldpassword} BandMaster1 {newpassword} /dev/{device}`

Wipe data and reset password to default MSID:

| :samp:`sedutil-cli --eraseLockingRange 0 {password} /dev/<device>`
| :samp:`sedutil-cli --setSIDPassword {oldpassword} "" /dev/<device>`
| :samp:`sedutil-cli --setPassword {oldpassword} EraseMaster "" /dev/<device>`

Wipe data and reset password using the PSID:
:samp:`sedutil-cli --yesIreallywanttoERASEALLmydatausingthePSID {PSINODASHED} /dev/{device}`
where *PSINODASHED* is the PSID located on the pysical drive with no
dashes (:literal:`-`).




.. index:: Email
.. _Email:

Email
-----

An automatic script sends a nightly email to the *root* user account
containing important information such as the health of the disks.
:ref:`Alert` events are also emailed to the *root* user account.
Problems with :ref:`Scrub Tasks` are reported separately in an email
sent at 03:00AM.

.. note:: :ref:`S.M.A.R.T.` reports are mailed separately to the
   address configured in that service.


The administrator typically does not read email directly on
the FreeNAS\ :sup:`®` system. Instead, these emails are usually sent to an
external email address where they can be read more conveniently. It is
important to configure the system so it can send these emails to the
administrator's remote email account so they are aware of problems or
status changes.

The first step is to set the remote address where email will be sent.
Go to
:menuselection:`Accounts --> Users`,
click ui-options and :guilabel:`Edit` for the *root* user. In the
:guilabel:`Email` field, enter the email address on the remote system
where email is to be sent, like *admin@example.com*. Click
:guilabel:`SAVE` to save the settings.

Additional configuration is performed with
:menuselection:`System --> Email`,
shown in
:numref:`Figure %s <email_conf_fig>`.

.. _email_conf_fig:
.. figure:: images/system-email.png

   Email Screen


.. tabularcolumns:: |p{1.2in}|p{1.2in}|p{3.6in}|
.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.60\linewidth-2\tabcolsep}|

.. _email_conf_tab:

.. table:: Email Configuration Settings
   :class: longtable

   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Setting              | Value                | Description                                                                                     |
   |                      |                      |                                                                                                 |
   +======================+======================+=================================================================================================+
   | From E-mail          | string               | The envelope From address shown in the email. This can be set to make filtering mail            |
   |                      |                      | on the receiving system easier.                                                                 |
   |                      |                      |                                                                                                 |
   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | From Name            | string               | The friendly name to show in front of the sending email address.                                |
   |                      |                      |                                                                                                 |
   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Outgoing Mail Server | string or IP address | Hostname or IP address of SMTP server used for sending this email.                              |
   |                      |                      |                                                                                                 |
   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Mail Server Port     | integer              | SMTP port number. Typically *25*,                                                               |
   |                      |                      | *465* (secure SMTP), or                                                                         |
   |                      |                      | *587* (submission).                                                                             |
   |                      |                      |                                                                                                 |
   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Security             | drop-down menu       | Choose an encryption type. Choices are *Plain (No Encryption)*,                                 |
   |                      |                      | *SSL (Implicit TLS)*, or                                                                        |
   |                      |                      | *TLS (STARTTLS)*.                                                                               |
   |                      |                      |                                                                                                 |
   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | SMTP                 | checkbox             | Enable or disable                                                                               |
   | Authentication       |                      | `SMTP AUTH <https://en.wikipedia.org/wiki/SMTP_Authentication>`__                               |
   |                      |                      | using PLAIN SASL. Setting this enables the required :guilabel:`Username` and optional           |
   |                      |                      | :guilabel:`Password` fields.                                                                    |
   |                      |                      |                                                                                                 |
   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Username             | string               | Enter the SMTP username when the SMTP server requires authentication.                           |
   |                      |                      |                                                                                                 |
   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Password             | string               | Enter the SMTP account password if needed for authentication. Only plain text characters        |
   |                      |                      | (7-bit ASCII) are allowed in passwords. UTF or composed characters are not allowed.             |
   |                      |                      |                                                                                                 |
   +----------------------+----------------------+-------------------------------------------------------------------------------------------------+


Click the :guilabel:`SEND TEST MAIL` button to verify that the
configured email settings are working. If the test email fails,
double-check that the :guilabel:`Email` field of the *root* user is
correctly configured by clicking the :guilabel:`Edit` button for
the *root* account in :menuselection:`Accounts --> Users`.

Configuring email for TLS/SSL email providers is described in
`Are you having trouble getting FreeNAS to email you in Gmail?
<https://forums.freenas.org/index.php?threads/are-you-having-trouble-getting-freenas-to-email-you-in-gmail.22517/>`__.


.. index:: System Dataset
.. _System Dataset:

System Dataset
--------------

:menuselection:`System --> System Dataset`,
shown in
:numref:`Figure %s <system_dataset_fig>`,
is used to select the pool which contains the persistent system
dataset. The system dataset stores debugging core files,
:ref:`encryption keys <Encryption and Recovery Keys>` for encrypted
pools, and Samba4 metadata such as the user/group cache and share level
permissions.


.. _system_dataset_fig:
.. figure:: images/system-system-dataset.png

   System Dataset Screen


Use the :guilabel:`System Dataset Pool` drop-down menu to select the
volume (pool) to contain the system dataset. The system dataset can be
moved to unencrypted volumes (pools) or encrypted volumes which do not
have passphrases. If the system dataset is moved to an encrypted volume,
that volume is no longer allowed to be locked or have a passphrase set.

Moving the system dataset also requires
restarting the :ref:`SMB` service. A dialog warns that the SMB service
must be restarted, causing a temporary outage of any active SMB
connections.

System logs can also be stored on the system
dataset. Storing this information on the system dataset is recommended
when large amounts of data is being generated and the system has limited
memory or a limited capacity operating system device.

Set :guilabel:`Syslog` to store system logs on the system dataset. Leave
unset to store system logs in :file:`/var` on the operating system device.

Click :guilabel:`SAVE` to save changes.

If the pool storing the system dataset is changed at a later time,
FreeNAS\ :sup:`®` migrates the existing data in the system dataset to the new
location.

.. note:: Depending on configuration, the system dataset can occupy a
   large amount of space and receive frequent writes. Do not put the
   system dataset on a flash drive or other media with limited space
   or write life.


.. index:: Reporting, Reporting settings
.. _System Reporting:

Reporting
---------

This section contains settings to customize some of the reporting tools.
These settings are described in
:numref:`Table %s <reporting_options>`

.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.64\linewidth-2\tabcolsep}|

.. _reporting_options:

.. table:: Reporting Settings
   :class: longtable

   +-----------------------+-----------+-----------------------------------------------------+
   | Setting               | Value     | Description                                         |
   +=======================+===========+=====================================================+
   | Report CPU usage      | checkbox  | Report CPU usage in percent instead of units of     |
   | in percent            |           | kernel time.                                        |
   +-----------------------+-----------+-----------------------------------------------------+
   | Remote Graphite       | string    | Hostname or IP address of a remote                  |
   | Server Hostname       |           | `Graphite <http://graphiteapp.org/>`__ server.      |
   +-----------------------+-----------+-----------------------------------------------------+
   | Graph Age in Months   | integer   | Maximum time a graph is stored in months (allowed   |
   |                       |           | values are *1* - *60*). Changing this value causes  |
   |                       |           | the :guilabel:`Confirm RRD Destroy` dialog to       |
   |                       |           | appear. Changes do not take effect until the        |
   |                       |           | existing reporting database is destroyed.           |
   +-----------------------+-----------+-----------------------------------------------------+
   | Number of Graph Points| integer   | Number of points for each hourly, daily, weekly,    |
   |                       |           | monthly, or yearly graph (allowed values are *1*    |
   |                       |           | - *4096*). Changing this                            |
   |                       |           | value causes the :guilabel:`Confirm RRD Destroy`    |
   |                       |           | checkbox to appear. Changes do not take effect      |
   |                       |           | until the existing reporting database is destroyed. |
   +-----------------------+-----------+-----------------------------------------------------+

Changes to :ref:`Reporting settings <reporting_options>`
clear the report history. To keep history with the old settings,
cancel the warning dialog. Click :guilabel:`RESET TO DEFAULTS` to
restore the original settings.


.. index:: Alert Services
.. _Alert Services:

Alert Services
--------------

FreeNAS\ :sup:`®` can use a number of methods to notify the administrator of
system events that require attention. These events are system
:ref:`Alerts <Alert>`.

Available alert services:

* `AWS-SNS <https://aws.amazon.com/sns/>`__

* E-mail

* `InfluxDB <https://www.influxdata.com/>`__

* `Mattermost <https://about.mattermost.com/>`__

* `OpsGenie <https://www.opsgenie.com/>`__

* `PagerDuty <https://www.pagerduty.com/>`__

* `Slack <https://slack.com/>`__

* `SNMP Trap <http://www.dpstele.com/snmp/trap-basics.php>`__

* `VictorOps <https://victorops.com/>`__


.. warning:: These alert services might use a third party commercial
   vendor not directly affiliated with iXsystems. Please investigate
   and fully understand that vendor's pricing policies and services
   before using their alert service. iXsystems is not responsible for
   any charges incurred from the use of third party vendors with the
   Alert Services feature.


Select
:menuselection:`System --> Alert Services` to show the Alert Services
screen, :numref:`Figure %s <alert_services_fig>`.

.. _alert_services_fig:

.. figure:: images/system-alert-services.png

   Alert Services


Click :guilabel:`ADD` to display the :guilabel:`Add Alert Service` form,
:numref:`Figure %s <alert_service_add_fig>`.

.. _alert_service_add_fig:

.. figure:: images/system-alert-services-add.png

   Add Alert Service


Select the :guilabel:`Type` to choose an alert service to configure.

Alert services can be set for a particular severity :guilabel:`Level`.
All alerts of that level are then sent out with that alert service. For
example, if the *E-Mail* alert service :guilabel:`Level` is set to
*Info*, any *Info* level alerts are sent by that service. Multiple alert
services can be set to the same level. For instance, *Critical* alerts
can be sent both by email and PagerDuty by setting both alert services
to the *Critical* level.

The configurable fields and required information differ for each alert
service. Set :guilabel:`Enabled` to activate the service. Enter any
other required information and click :guilabel:`SAVE`.

Click :guilabel:`SEND TEST ALERT` to test the chosen alert service.

All saved alert services are displayed in
:menuselection:`System --> Alert Services`.
To delete an alert service, click ui-options and :guilabel:`Delete`.
To disable an alert service
temporarily, click ui-options and :guilabel:`Edit`, then unset the
:guilabel:`Enabled` option.


.. index:: Alert Settings

.. _Alert Settings:

Alert Settings
--------------

:menuselection:`System --> Alert Settings`
has options to configure each FreeNAS\ :sup:`®` :ref:`Alert`.

.. _alert_settings_fig:

.. figure:: images/system-alert-settings.png

   Alert Settings


Alerts are grouped by web interface feature or service monitor. To customize alert
importance, use the :guilabel:`Warning Level` drop-down. To adjust how often alert
notifications are sent, use the :guilabel:`Frequency` drop-down.
Setting the :guilabel:`Frequency` to *NEVER* prevents that alert from being added to
alert notifications, but the alert can still show in the web interface if it is triggered.

To configure where alert notifications are sent, use
:ref:`Alert Services`.


.. index:: Cloud Credentials
.. _Cloud Credentials:

Cloud Credentials
-----------------

FreeNAS\ :sup:`®` can use cloud services for features like :ref:`Cloud Sync Tasks`.
The `rclone <https://rclone.org/>`__ credentials to provide secure
connections with cloud services are entered here. Amazon S3, Backblaze
B2, Box, Dropbox, FTP, Google Cloud Storage, Google Drive, HTTP, hubiC,
Mega, Microsoft Azure Blob Storage, Microsoft OneDrive, pCloud, SFTP,
WebDAV, and Yandex are available.

.. note:: The hubiC cloud service has
	  `suspended creation of new accounts <https://www.ovh.co.uk/subscriptions-hubic-ended/>`__.


.. warning:: Cloud Credentials are stored in encrypted form. To be able
   to restore Cloud Credentials from a
   :ref:`saved configuration<General>`, "Export Password Secret Seed"
   must be set when saving that configuration.

Click
:menuselection:`System --> Cloud Credentials`
to see the screen shown in :numref:`Figure %s <cloud_creds_fig>`.

.. _cloud_creds_fig:

.. figure:: images/system-cloud-credentials.png

   Cloud Credentials List


The list shows the :guilabel:`Account Name` and :guilabel:`Provider`
for each credential. There are options to :guilabel:`Edit` and
:guilabel:`Delete` a credential after clicking ui-options for a
credential.

Click :guilabel:`ADD` to add a new cloud credential. Choose a
:guilabel:`Provider` to display any specific options for that
provider. :numref:`Figure %s <cloud_creds_add_fig>` shows an example
configuration:


.. _cloud_creds_add_fig:

.. figure:: images/system-cloud-credentials-add-example.png

   Add Amazon S3 Credential


Enter a descriptive and unique name for the cloud credential in the
:guilabel:`Name` field. The remaining options vary by
:guilabel:`Provider`, and are shown in
:numref:`Table %s <cloud_cred_tab>`. Clicking a provider name opens a
new browser tab to the
`rclone documentation <https://rclone.org/docs/>`__ for that provider.


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.64\linewidth-2\tabcolsep}|

.. _cloud_cred_tab:

.. table:: Cloud Credential Options
   :class: longtable

   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | Provider                                    | Setting              | Description                                                                                               |
   +=============================================+======================+===========================================================================================================+
   | `Amazon S3 <https://rclone.org/s3/>`__      | Access Key ID        | Enter the Amazon Web Services Key ID. This is found on `Amazon AWS <https://aws.amazon.com>`__ by going   |
   |                                             |                      | through *My Account --> Security Credentials --> Access Keys*. Must be alphanumeric and between 5 and     |
   |                                             |                      | 20 characters.                                                                                            |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Amazon S3 <https://rclone.org/s3/>`__      | Secret Access Key    | Enter the Amazon Web Services password. If the Secret Access Key cannot be found or remembered, go to     |
   |                                             |                      | *My Account --> Security Credentials --> Access Keys* and create a new key pair. Must be alphanumeric     |
   |                                             |                      | and between 8 and 40 characters.                                                                          |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Amazon S3 <https://rclone.org/s3/>`__      | Endpoint URL         | Set :guilabel:`Advanced Settings` to access this option. S3 API                                           |
   |                                             |                      | `endpoint URL <https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteEndpoints.html>`__. When using AWS, |
   |                                             |                      | the endpoint field can be empty to use the default endpoint for the region, and available buckets are     |
   |                                             |                      | automatically fetched. Refer to the AWS Documentation for a list of `Simple Storage Service Website       |
   |                                             |                      | Endpoints <https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_website_region_endpoints>`__.      |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Amazon S3 <https://rclone.org/s3/>`__      | Region               | `AWS resources in a geographic area <https://docs.aws.amazon.com/general/latest/gr/rande-manage.html>`__. |
   |                                             |                      | Leave empty to automatically detect the correct public region for the bucket. Entering a private region   |
   |                                             |                      | name allows interacting with Amazon buckets created in that region. For example, enter                    |
   |                                             |                      | :literal:`us-gov-east-1` to discover buckets created in the eastern                                       |
   |                                             |                      | `AWS GovCloud <https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/whatis.html>`__ region.           |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Amazon S3 <https://rclone.org/s3/>`__      | Disable Endpoint     | Set :guilabel:`Advanced Settings` to access this option. Skip automatic detection of the                  |
   |                                             | Region               | :guilabel:`Endpoint URL` region. Set this when configuring a custom :guilabel:`Endpoint URL`.             |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Amazon S3 <https://rclone.org/s3/>`__      | Use Signature        | Set :guilabel:`Advanced Settings` to access this option. Force using                                      |
   |                                             | Version 2            | `Signature Version 2 <https://docs.aws.amazon.com/general/latest/gr/signature-version-2.html>`__          |
   |                                             |                      | to sign API requests. Set this when configuring a custom :guilabel:`Endpoint URL`.                        |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Backblaze B2 <https://rclone.org/b2/>`__   | Key ID, Application  | Alphanumeric `Backblaze B2 <https://www.backblaze.com/b2/cloud-storage.html>`__ application keys. To      |
   |                                             | Key                  | generate a new application key, log in to the Backblaze account, go to the :guilabel:`App Keys` page, and |
   |                                             |                      | add a new application key. Copy the :literal:`keyID` and :literal:`applicationKey` strings into the       |
   |                                             |                      | FreeNAS\ :sup:`®` web interface fields.                                                                        |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Box <https://rclone.org/box/>`__           | Access Token         | Configured with :ref:`Open Authentication <OAuth Config>`.                                                |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Dropbox <https://rclone.org/dropbox/>`__   | Access Token         | Configured with :ref:`Open Authentication <OAuth Config>`. The access token can be manually created by    |
   |                                             |                      | going to the Dropbox `App Console <https://www.dropbox.com/developers/apps>`__. After creating an app, go |
   |                                             |                      | to *Settings* and click :guilabel:`Generate` under the Generated access token field.                      |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `FTP <https://rclone.org/ftp/>`__           | Host, Port           | Enter the FTP host and port.                                                                              |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `FTP <https://rclone.org/ftp/>`__           | Username, Password   | Enter the FTP username and password.                                                                      |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Google Cloud Storage                       | JSON Service Account | Upload a Google `Service Account credential file                                                          |
   | <https://rclone.org/googlecloudstorage/>`__ | Key                  | <https://rclone.org/googlecloudstorage/#service-account-support>`__. The file is created with the         |
   |                                             |                      | `Google Cloud Platform Console <https://console.cloud.google.com/apis/credentials>`__.                    |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Google Drive                               | Access Token,        | The :guilabel:`Access Token` is configured with :ref:`Open Authentication <OAuth Config>`.                |
   | <https://rclone.org/drive/>`__              | Team Drive ID        | :guilabel:`Team Drive ID` is only used when connecting to a `Team Drive                                   |
   |                                             |                      | <https://developers.google.com/drive/api/v3/reference/teamdrives>`__. The ID is also the ID of the top    |
   |                                             |                      | level folder of the Team Drive.                                                                           |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `HTTP <https://rclone.org/http/>`__         | URL                  | Enter the HTTP host URL.                                                                                  |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `hubiC <https://rclone.org/hubic/>`__       | Access Token         | Enter the access token. See the `Hubic guide <https://api.hubic.com/sandbox/>`__ for instructions to      |
   |                                             |                      | obtain an access token.                                                                                   |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Mega <https://rclone.org/mega/>`__         | Username, Password   | Enter the `Mega <https://mega.nz/>`__ username and password.                                              |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Microsoft Azure Blob Storage               | Account Name,        | Enter the Azure Blob Storage account name and key.                                                        |
   | <https://rclone.org/azureblob/>`__          | Account Key          |                                                                                                           |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Microsoft OneDrive                         | Access Token,        | The :guilabel:`Access Token` is configured with :ref:`Open Authentication <OAuth Config>`. Authenticating |
   | <https://rclone.org/onedrive/>`__           | Drives List,         | a Microsoft account adds the :guilabel:`Drives List` and selects the correct                              |
   |                                             | Drive Account Type,  | :guilabel:`Drive Account Type`.                                                                           |
   |                                             | Drive ID             |                                                                                                           |
   |                                             |                      | The :guilabel:`Drives List` shows all the drives and IDs registered to the Microsoft account. Selecting a |
   |                                             |                      | drive automatically fills the :guilabel:`Drive ID` field.                                                 |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `pCloud <https://rclone.org/pcloud/>`__     | Access Token         | Configured with :ref:`Open Authentication <OAuth Config>`.                                                |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `SFTP <https://rclone.org/sftp/>`__         | Host, Port,          | Enter the SFTP host and port. Enter an account user name that has SSH access to the host. Enter the       |
   |                                             | Username, Password,  | password for that account *or* import the private key from an existing :ref:`SSH keypair <SSH Keypairs>`. |
   |                                             | Private Key ID       | To create a new SSH key for this credential, open the :guilabel:`Private Key ID` drop-down and select     |
   |                                             |                      | *Generate New*.                                                                                           |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `WebDAV <https://rclone.org/webdav/>`__     | URL, WebDAV service  | Enter the URL and use the dropdown to select the WebDAV service.                                          |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `WebDAV <https://rclone.org/webdav/>`__     | Username, Password   | Enter the username and password.                                                                          |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+
   | `Yandex <https://rclone.org/yandex/>`__     | Access Token         | Configured with :ref:`Open Authentication <OAuth Config>`.                                                |
   +---------------------------------------------+----------------------+-----------------------------------------------------------------------------------------------------------+


For Amazon S3, :guilabel:`Access Key` and
:guilabel:`Secret Key` values are found on the Amazon AWS
website by clicking on the account name, then
:guilabel:`My Security Credentials` and
:guilabel:`Access Keys (Access Key ID and Secret Access Key)`.
Copy the Access Key value to the FreeNAS\ :sup:`®` Cloud Credential
:guilabel:`Access Key` field, then enter the :guilabel:`Secret Key`
value saved when the key pair was created. If the Secret Key value is
unknown, a new key pair can be created on the same Amazon screen.

.. _OAuth Config:

`Open Authentication (OAuth) <https://openauthentication.org/>`__
is used with some cloud providers. These providers have a
:guilabel:`LOGIN TO PROVIDER` button that opens a dialog to log in to
that provider and fill the :guilabel:`Access Token` field with
valid credentials.

Enter the information and click :guilabel:`VERIFY CREDENTIAL`.
:literal:`The Credential is valid.` displays when the credential
information is verified.

More details about individual :guilabel:`Provider` settings are
available in the `rclone documentation <https://rclone.org/about/>`__.


.. index:: SSH Connections
.. _SSH Connections:

SSH Connections
---------------

`Secure Socket Shell (SSH) <https://searchsecurity.techtarget.com/definition/Secure-Shell>`__
is a network protocol that provides a secure method to access and
transfer files between two hosts while using an unsecure network. SSH
can use user account credentials to establish secure connections, but
often uses key pairs shared between host systems for authentication.

FreeNAS\ :sup:`®` uses
:menuselection:`System --> SSH Connections`
to quickly create SSH connections and show any saved connections. These
connections are required when creating a new
:ref:`replication <Replication Tasks>` to back up dataset snapshots.

The remote system must be configured to allow SSH connections. Some
situations can also require allowing root account access to the remote
system. For FreeNAS\ :sup:`®` systems, go to
:menuselection:`Services`
and edit the :ref:`SSH` service to allow SSH connections and root
account access.

To add a new SSH connection, go to
:menuselection:`System --> SSH Connections`
and click :guilabel:`ADD`.

.. _system_ssh_connections_add_fig:

.. figure:: images/system-ssh-connections-add.png


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.64\linewidth-2\tabcolsep}|

.. _system_ssh_connections_tab:

.. table:: SSH Connection Options

   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Setting         | Value          | Description                                                                         |
   |                 |                |                                                                                     |
   +=================+================+=====================================================================================+
   | Name            | string         | Descriptive name of this SSH connection. SSH connection names must be unique.       |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Setup Method    | drop-down menu | How to configure the connection:                                                    |
   |                 |                |                                                                                     |
   |                 |                | *Manual* requires configuring authentication on the remote system. This can require |
   |                 |                | copying SSH keys and modifying the *root* user account on that system. See          |
   |                 |                | :ref:`Manual Setup`.                                                                |
   |                 |                |                                                                                     |
   |                 |                | *Semi-automatic* is only functional when configuring an SSH connection between      |
   |                 |                | FreeNAS\ :sup:`®` systems. After authenticating the connection, all remaining       |
   |                 |                | connection options are automatically configured. See :ref:`Semi-Automatic Setup`.   |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Host            | string         | Enter the hostname or IP address of the remote system. Only available with *Manual* |
   |                 |                | configurations.                                                                     |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Port            | integer        | Port number on the remote system to use for the SSH connection. Only available with |
   |                 |                | *Manual* configurations.                                                            |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | FreeNAS URL     | string         | Hostname or IP address of the remote FreeNAS\ :sup:`®` system. Only available       |
   |                 |                | with *Semi-automatic* configurations. A valid URL scheme is required. Example:      |
   |                 |                | :samp:`https://{10.231.3.76}`                                                       |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Username        | string         | User account name to use for logging in to the remote system                        |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Password        | string         | User account password used to log in to the FreeNAS\ :sup:`®` system. Only          |
   |                 |                | available with *Semi-automatic* configurations.                                     |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Private Key     | drop-down menu | Choose a saved :ref:`SSH Keypair <SSH Keypairs>` or select *Generate New* to create |
   |                 |                | a new keypair and apply it to this connection.                                      |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Remote Host Key | string         | Remote system SSH key for this system to authenticate the connection. Only          |
   |                 |                | available with *Manual* configurations. When all other fields are properly          |
   |                 |                | configured, click :guilabel:`DISCOVER REMOTE HOST KEY` to query the remote system   |
   |                 |                | and automatically populate this field.                                              |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Cipher          | drop-down menu | Connection security level:                                                          |
   |                 |                |                                                                                     |
   |                 |                | * *Standard* is most secure, but has the greatest impact on connection speed.       |
   |                 |                | * *Fast* is less secure than *Standard* but can give reasonable transfer rates for  |
   |                 |                |   devices with limited cryptographic speed.                                         |
   |                 |                | * *Disabled* removes all security in favor of maximizing connection speed.          |
   |                 |                |   Disabling the security should only be used within a secure, trusted network.      |
   |                 |                |                                                                                     |
   +-----------------+----------------+-------------------------------------------------------------------------------------+
   | Connect Timeout | integer        | Time (in seconds) before the system stops attempting to establish a connection with |
   |                 |                | the remote system.                                                                  |
   +-----------------+----------------+-------------------------------------------------------------------------------------+


Saved connections can be edited or deleted. Deleting an SSH connection
also deletes or disables paired :ref:`SSH Keypairs`,
:ref:`Replication Tasks`, and :ref:`Cloud Credentials`.


.. _Manual Setup:

Manual Setup
~~~~~~~~~~~~

Choosing to manually set up the SSH connection requires copying a public
encryption key from the local to remote system. This allows a secure
connection without a password prompt.

The examples here and in :ref:`Semi-Automatic Setup` refer to the
FreeNAS\ :sup:`®` system that is configuring a new connection in
:menuselection:`System --> SSH Connections`
as *Host 1*. The FreeNAS\ :sup:`®` system that is receiving the encryption key
is *Host 2*.

On *Host 1*, go to
:menuselection:`System --> SSH Keypairs`
and create a new :ref:`SSH Keypair <SSH Keypairs>`. Highlight the entire
:guilabel:`Public Key` text, right-click in the highlighted area, and
click :guilabel:`Copy`.

Log in to *Host 2* and go to
:menuselection:`Accounts --> Users`.
Click ui-options for the *root* account, then :guilabel:`Edit`.
Paste the copied key into the :guilabel:`SSH Public Key` field and click
:guilabel:`SAVE` as shown in
:numref:`Figure %s <zfs_paste_replication_key_fig>`.

.. _zfs_paste_replication_key_fig:

.. figure:: images/accounts-users-edit-ssh-key.png

   Paste the Replication Key


Switch back to *Host 1* and go to
:menuselection:`System --> SSH Connections`
and click :guilabel:`ADD`. Set the :guilabel:`Setup Method` to *Manual*, select
the previously created keypair as the :guilabel:`Private Key`, and fill
in the rest of the connection details for *Host 2*. Click
:guilabel:`DISCOVER REMOTE HOST KEY` to obtain the remote system key.
Click :guilabel:`SAVE` to store this SSH connection.


.. _Semi-Automatic Setup:

Semi-Automatic Setup
~~~~~~~~~~~~~~~~~~~~

FreeNAS\ :sup:`®` offers a semi-automatic setup mode that simplifies setting up an
SSH connection with another FreeNAS or TrueNAS system. When
administrator account credentials are known for *Host 2*,
semi-automatic setup allows configuring the SSH connection without
logging in to *Host 2* to transfer SSH keys.

In *Host 1*, go to
:menuselection:`System --> SSH Keypairs`
and create a new :ref:`SSH Keypair <SSH Keypairs>`.
Go to
:menuselection:`System --> SSH Connections`
and click :guilabel:`ADD`.

Choose *Semi-automatic* as the :guilabel:`Setup Method`. Enter the
*Host 2* URL in :guilabel:`FreeNAS URL` using the format
:samp:`http://{freenas.remote}`, where *freenas.remote* is the
*Host 2* hostname or IP address.

Enter credentials for an *Host 2* user account that can accept SSH
connection requests and modify *Host 2*. This is typically the
*root* account.

Select the SSH keypair that was just created for the
:guilabel:`Private Key`.

Fill in the remaining connection configuration fields and click
:guilabel:`SAVE`. *Host 1* can use this saved configuration to
establish a connection to *Host 2* and exchange the remaining
authentication keys.


.. index:: SSH Keypairs
.. _SSH Keypairs:

SSH Keypairs
------------

FreeNAS\ :sup:`®` generates and stores
`RSA-encrypted <https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29>`__
SSH public and private keypairs in
:menuselection:`System --> SSH Keypairs`.
These are generally used when configuring :ref:`SSH Connections` or
*SFTP* :ref:`Cloud Credentials`. Encrypted keypairs or keypairs with
passphrases are not supported.

To generate a new keypair, click :guilabel:`ADD`, enter a name, and click
:guilabel:`GENERATE KEYPAIR`. The :guilabel:`Private Key` and
:guilabel:`Public Key` fields fill with the key strings. SSH key pair
names must be unique.

.. _system_ssh_keypairs_add_fig:

.. figure:: images/system-ssh-keypairs-add.png

   Example Keypair


Click :guilabel:`SAVE` to store the new keypair. These saved keypairs
can be selected later in the web interface wihout having to manually copy
the key values.

Keys are viewed or modified by going to
:menuselection:`System --> SSH Keypairs`
and clicking ui-options and :guilabel:`Edit` for the keypair name.

Deleting an SSH Keypair also deletes any associated
:ref:`SSH Connections`. :ref:`Replication Tasks` or SFTP
:ref:`Cloud Credentials` that use this keypair are disabled but not
removed.


.. index:: Tunables
.. _Tunables:

Tunables
--------

:menuselection:`System --> Tunables`
can be used to manage:

#. **FreeBSD sysctls:** a
   `sysctl(8) <https://www.freebsd.org/cgi/man.cgi?query=sysctl>`__
   makes changes to the FreeBSD kernel running on a FreeNAS\ :sup:`®` system
   and can be used to tune the system.

#. **FreeBSD loaders:** a loader is only loaded when a FreeBSD-based
   system boots and can be used to pass a parameter to the kernel or
   to load an additional kernel module such as a FreeBSD hardware
   driver.

#. **FreeBSD rc.conf options:**
   `rc.conf(5) <https://www.freebsd.org/cgi/man.cgi?query=rc.conf>`__
   is used to pass system configuration options to the system startup
   scripts as the system boots. Since FreeNAS\ :sup:`®` has been optimized for
   storage, not all of the services mentioned in rc.conf(5) are
   available for configuration. Note that in FreeNAS\ :sup:`®`, customized
   rc.conf options are stored in
   :file:`/tmp/rc.conf.freenas`.


.. warning:: Adding a sysctl, loader, or :file:`rc.conf` option is an
   advanced feature. A sysctl immediately affects the kernel running
   the FreeNAS\ :sup:`®` system and a loader could adversely affect the ability
   of the FreeNAS\ :sup:`®` system to successfully boot.
   **Do not create a tunable on a production system before
   testing the ramifications of that change.**


Since sysctl, loader, and rc.conf values are specific to the kernel
parameter to be tuned, the driver to be loaded, or the service to
configure, descriptions and suggested values can be found in the man
page for the specific driver and in many sections of the
`FreeBSD Handbook
<https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/>`__.

To add a loader, sysctl, or :file:`rc.conf` option, go to
:menuselection:`System --> Tunables`
and click :guilabel:`ADD` to access the screen shown in
:numref:`Figure %s <add_tunable_fig>`.


.. _add_tunable_fig:

.. figure:: images/system-tunables-add.png

   Adding a Tunable


:numref:`Table %s <add_tunable_tab>`
summarizes the options when adding a tunable.

.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.64\linewidth-2\tabcolsep}|

.. _add_tunable_tab:

.. table:: Adding a Tunable
   :class: longtable

   +-------------+-------------------+-------------------------------------------------------------------------------------+
   | Setting     | Value             | Description                                                                         |
   |             |                   |                                                                                     |
   |             |                   |                                                                                     |
   +=============+===================+=====================================================================================+
   | Variable    | string            | The name of the sysctl or driver to load.                                           |
   |             |                   |                                                                                     |
   +-------------+-------------------+-------------------------------------------------------------------------------------+
   | Value       | integer or string | Set a value for the :guilabel:`Variable`. Refer to the man page for the specific    |
   |             |                   | driver or the                                                                       |
   |             |                   | `FreeBSD Handbook <https://www.freebsd.org/doc/en_US.ISO08859-1/books/handbook/>`__ |
   |             |                   | for suggested values.                                                               |
   |             |                   |                                                                                     |
   +-------------+-------------------+-------------------------------------------------------------------------------------+
   | Type        | drop-down menu    | Choices are *Loader*, *rc.conf*, and *Sysctl*.                                      |
   |             |                   |                                                                                     |
   +-------------+-------------------+-------------------------------------------------------------------------------------+
   | Description | string            | Optional. Enter a description of this tunable.                                      |
   |             |                   |                                                                                     |
   +-------------+-------------------+-------------------------------------------------------------------------------------+
   | Enabled     | checkbox          | Deselect this option to disable the tunable without deleting it.                    |
   |             |                   |                                                                                     |
   +-------------+-------------------+-------------------------------------------------------------------------------------+


.. note:: As soon as a *Sysctl* is added or edited, the running kernel
   changes that variable to the value specified. However, when a
   *Loader* or *rc.conf* value is changed, it does not take effect
   until the system is rebooted. Regardless of the type of tunable,
   changes persist at each boot and across upgrades unless the tunable
   is deleted or the :guilabel:`Enabled` option is deselected.


Existing tunables are listed in
:menuselection:`System --> Tunables`.
To change the value of an existing tunable, click ui-options and
:guilabel:`Edit`. To remove a tunable, click ui-options and
:guilabel:`Delete`.

Restarting the FreeNAS\ :sup:`®` system after making sysctl changes is
recommended. Some sysctls only take effect at system startup, and
restarting the system guarantees that the setting values correspond
with what is being used by the running system.

The web interface does not display the sysctls that are pre-set when FreeNAS\ :sup:`®` is
installed. FreeNAS\ :sup:`®` |release| ships with the sysctls set:

.. code-block:: none

   kern.corefile=/var/tmp/%N.core
   kern.metadelay=3
   kern.dirdelay=4
   kern.filedelay=5
   kern.coredump=1
   kern.sugid_coredump=1
   vfs.timestamp_precision=3
   net.link.lagg.lacp.default_strict_mode=0
   vfs.zfs.min_auto_ashift=12

**Do not add or edit these default sysctls** as doing so may render
the system unusable.

The web interface does not display the loaders that are pre-set when FreeNAS\ :sup:`®` is
installed. FreeNAS\ :sup:`®` |release| ships with these loaders set:

.. code-block:: none

   product="FreeNAS"
   autoboot_delay="5"
   loader_logo="FreeNAS"
   loader_menu_title="Welcome to FreeNAS"
   loader_brand="FreeNAS"
   loader_version=" "
   kern.cam.boot_delay="30000"
   debug.debugger_on_panic=1
   debug.ddb.textdump.pending=1
   hw.hptrr.attach_generic=0
   vfs.mountroot.timeout="30"
   ispfw_load="YES"
   ipmi_load="YES"
   freenas_sysctl_load="YES"
   hint.isp.0.role=2
   hint.isp.1.role=2
   hint.isp.2.role=2
   hint.isp.3.role=2
   module_path="/boot/kernel;/boot/modules;/usr/local/modules"
   net.inet6.ip6.auto_linklocal="0"
   vfs.zfs.vol.mode=2
   kern.geom.label.disk_ident.enable=0
   kern.geom.label.ufs.enable=0
   kern.geom.label.ufsid.enable=0
   kern.geom.label.reiserfs.enable=0
   kern.geom.label.ntfs.enable=0
   kern.geom.label.msdosfs.enable=0
   kern.geom.label.ext2fs.enable=0
   hint.ahciem.0.disabled="1"
   hint.ahciem.1.disabled="1"
   kern.msgbufsize="524288"
   hw.mfi.mrsas_enable="1"
   hw.usb.no_shutdown_wait=1
   vfs.nfsd.fha.write=0
   vfs.nfsd.fha.max_nfsds_per_fh=32
   vm.lowmem_period=0

**Do not add or edit the default tunables.** Changing the default
tunables can make the system unusable.

The ZFS version used in |release| deprecates these tunables:

.. code-block:: none

   kvfs.zfs.write_limit_override
   vfs.zfs.write_limit_inflated
   vfs.zfs.write_limit_max
   vfs.zfs.write_limit_min
   vfs.zfs.write_limit_shift
   vfs.zfs.no_write_throttle

After upgrading from an earlier version of FreeNAS\ :sup:`®`, these tunables are
automatically deleted. Please do not manually add them back.


.. _Update:

Update
------

FreeNAS\ :sup:`®` has an integrated update system to make it easy to keep up to
date.

.. _Preparing for Updates:

Preparing for Updates
~~~~~~~~~~~~~~~~~~~~~

It is best to perform updates at times the FreeNAS\ :sup:`®` system is idle,
with no clients connected and no scrubs or other disk activity going
on. Most updates require a system reboot. Plan updates around scheduled
maintenance times to avoid disrupting user activities.

The update process will not proceed unless there is enough free space
in the boot pool for the new update files. If a space warning is
shown, go to :ref:`Boot` to remove unneeded boot environments.



.. _Updates and Trains:

Updates and Trains
~~~~~~~~~~~~~~~~~~

Cryptographically signed update files are used to update FreeNAS\ :sup:`®`.
Update files provide flexibility in deciding when to upgrade the system.
Go to :ref:`Boot <If Something Goes Wrong>` to test an update.

FreeNAS\ :sup:`®` defines software branches, known as *trains*.
There are several trains available for updates, but the web interface only
displays trains that can be selected as an upgrade.

Update trains are labeled with a numeric version followed by a short
description. The current version receives regular bug fixes and new
features. Supported older versions of FreeNAS\ :sup:`®` only receive maintenance
updates. Several specific words are used to describe the type of train:

* **STABLE:** Bug fixes and new features are available from this train.
  Upgrades available from a *STABLE* train are tested and ready to apply
  to a production environment.

* **Nightlies:**  Experimental train used for testing future versions of
  FreeNAS\ :sup:`®`.

* **SDK:** Software Developer Kit train. This has additional tools for
  testing and debugging FreeNAS\ :sup:`®`.

.. warning:: The UI will warn if the currently selected train is not
   suited for production use. Before using a non-production train,
   be prepared to experience bugs or problems. Testers are encouraged to
   submit bug reports at
   `<https://bugs.ixsystems.com>`.


.. _Checking for Updates:

Checking for Updates
~~~~~~~~~~~~~~~~~~~~

:numref:`Figure %s <update_options_fig>`
shows an example of the
:menuselection:`System --> Update`
screen.


.. _update_options_fig:
.. figure:: images/system-update.png

   Update Options


The system checks daily for updates and downloads an update if one
is available. An alert is issued when a new update becomes
available. The automatic check and download of updates is disabled by
unsetting :guilabel:`Check for Updates Daily and Download if Available`.
Click ui-refresh to perform another check for updates.

To change the train, use the drop-down menu to make a different
selection.

.. note:: The train selector does not allow downgrades. For example,
   the STABLE train cannot be selected while booted into a Nightly
   boot environment, or a 9.10 train cannot be selected while booted
   into a 11 boot environment. To go back to an earlier version
   after testing or running a more recent version, reboot and select a
   boot environment for that earlier version. This screen can then be
   used to check for updates that train.


In the example shown in
:numref:`Figure %s <review_updates_fig>`, information about the update
is displayed along with a link to the :guilabel:`release notes`. It is
important to read the release notes before updating to determine if any
of the changes in that release impact the use of the system.

.. _review_updates_fig:

.. figure:: images/system-update-staged.png

   Reviewing Updates


.. _Saving_The_Configuration_File:

Saving the Configuration File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A dialog to save the system
:ref:`configuration file <saveconfig>` appears before installing
updates.

.. figure:: images/save-config.png

.. warning:: Keep the system configuration file secure after saving
   it. The security information in the configuration file could be
   used for unauthorized access to the FreeNAS\ :sup:`®` system.


Applying Updates
~~~~~~~~~~~~~~~~

Make sure the system is in a low-usage state as described above in
:ref:`Preparing for Updates`.

Click :guilabel:`DOWNLOAD UPDATES` to immediately download and install an
update.

The :ref:`Save Configuration <Saving_The_Configuration_File>` dialog
appears so the current configuration can be saved to external media.

A confirmation window appears before the update is installed. When
:guilabel:`Apply updates and reboot system after downloading` is
set and, clicking :guilabel:`CONTINUE` downloads, applies the
updates, and then automatically reboots the system.
The update can be downloaded for a later manual installation by
unsetting the
:guilabel:`Apply updates and reboot system after downloading` option.

:guilabel:`APPLY PENDING UPDATE` is visible when an update is
downloaded and ready to install. Click the button to see a
confirmation window. Setting :guilabel:`Confirm` and clicking
:guilabel:`CONTINUE` installs the update and reboots the system.

.. warning:: Each update creates a boot environment. If the update
   process needs more space, it attempts to remove old boot
   environments. Boot environments marked with the *Keep* attribute as
   shown in :ref:`Boot` are not removed. If space for
   a new boot environment is not available, the upgrade fails. Space
   on the operating system device can be manually freed using
   :menuselection:`System --> Boot`.
   Review the boot environments and remove the *Keep* attribute or
   delete any boot environments that are no longer needed.


Manual Updates
~~~~~~~~~~~~~~

Updates can also be manually downloaded and applied in
:menuselection:`System --> Update`.


.. note:: Manual updates cannot be used to upgrade from older major
   versions.


Go to
`<https://download.freenas.org/>`__
and find an update file of the desired version. Manual update file
names end with :file:`-manual-update-unsigned.tar`.

Download the file to a desktop or laptop computer. Connect to FreeNAS\ :sup:`®`
with a browser and go to
:menuselection:`System --> Update`.
Click :guilabel:`INSTALL MANUAL UPDATE FILE`.

The :ref:`Save Configuration <Saving_The_Configuration_File>` dialog
opens. This makes it possible to save a copy of the current
configuration to external media for backup in case of an update
problem.

After the dialog closes, the manual update screen is shown:


.. figure:: images/system-manualupdate.png


The current version of FreeNAS\ :sup:`®` is shown for verification.

Select the manual update file with the :guilabel:`Browse` button. Set
:guilabel:`Reboot After Update` to reboot the system after the update
has been installed. Click :guilabel:`APPLY UPDATE` to begin the
update.


.. _Update in Progress:

Update in Progress
~~~~~~~~~~~~~~~~~~~

Starting an update shows a progress dialog. When an update is in
progress, the web interface shows an ui-update icon in the top row. Dialogs
also appear in every active web interface session to warn that a system
update is in progress. **Do not** interrupt a system update.




.. index:: CA, Certificate Authority
.. _CAs:

CAs
---

FreeNAS\ :sup:`®` can act as a Certificate Authority (CA). When encrypting SSL
or TLS connections to the FreeNAS\ :sup:`®` system, either import an existing
certificate, or create a CA on the FreeNAS\ :sup:`®` system, then create a
certificate. This certificate will appear in the drop-down menus for
services that support SSL or TLS.

For secure LDAP, the public key of an existing CA can be imported with
:guilabel:`Import CA`, or a new CA created on the FreeNAS\ :sup:`®` system and
used on the LDAP server also.

:numref:`Figure %s <cas_fig>`
shows the screen after clicking
:menuselection:`System --> CAs`.

.. _cas_fig:
.. figure:: images/system-cas.png

   Initial CA Screen


If the organization already has a CA, the CA certificate and key
can be imported. Click :guilabel:`ADD` and set the :guilabel:`Type` to
*Import CA* to see the configuration options shown in
:numref:`Figure %s <import_ca_fig>`.
The configurable options are summarized in
:numref:`Table %s <import_ca_opts_tab>`.


.. _import_ca_fig:

.. figure:: images/system-cas-add-import-ca.png

   Importing a CA


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.64\linewidth-2\tabcolsep}|

.. _import_ca_opts_tab:

.. table:: Importing a CA Options
   :class: longtable

   +----------------------+--------------------+---------------------------------------------------------------------------------------------------+
   | Setting              | Value              | Description                                                                                       |
   |                      |                    |                                                                                                   |
   +======================+====================+===================================================================================================+
   | Identifier           | string             | Enter a descriptive name for the CA using only alphanumeric,                                      |
   |                      |                    | underscore (:literal:`_`), and dash (:literal:`-`) characters.                                    |
   |                      |                    |                                                                                                   |
   +----------------------+--------------------+---------------------------------------------------------------------------------------------------+
   | Type                 | drop-down menu     | Choose the type of CA. Choices are *Internal CA*, *Intermediate CA*, and *Import CA*.             |
   |                      |                    |                                                                                                   |
   +----------------------+--------------------+---------------------------------------------------------------------------------------------------+
   | Certificate          | string             | Mandatory. Paste in the certificate for the CA.                                                   |
   |                      |                    |                                                                                                   |
   +----------------------+--------------------+---------------------------------------------------------------------------------------------------+
   | Private Key          | string             | If there is a private key associated with the :guilabel:`Certificate`, paste it here.             |
   |                      |                    | Private keys must be at least 1024 bits long.                                                     |
   |                      |                    |                                                                                                   |
   +----------------------+--------------------+---------------------------------------------------------------------------------------------------+
   | Passphrase           | string             | If the :guilabel:`Private Key` is protected by a passphrase, enter it here and repeat             |
   |                      |                    | it in the "Confirm Passphrase" field.                                                             |
   |                      |                    |                                                                                                   |
   +----------------------+--------------------+---------------------------------------------------------------------------------------------------+


To create a new CA, first decide if it will be the only CA
which will sign certificates for internal use or if the CA will be
part of a
`certificate chain <https://en.wikipedia.org/wiki/Root_certificate>`__.

To create a CA for internal use only, click :guilabel:`ADD` and set the
:guilabel:`Type` to *Internal CA*. :numref:`Figure %s <create_ca_fig>`
shows the available options.


.. _create_ca_fig:

.. figure:: images/system-cas-add-internal-ca.png

   Creating an Internal CA


The configurable options are described in
:numref:`Table %s <internal_ca_opts_tab>`.
When completing the fields for the certificate authority, supply the
information for the organization.


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.64\linewidth-2\tabcolsep}|

.. _internal_ca_opts_tab:

.. table:: Internal CA Options
   :class: longtable

   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Setting                 | Value                | Description                                                                                     |
   |                         |                      |                                                                                                 |
   +=========================+======================+=================================================================================================+
   | Identifier              | string               | Enter a descriptive name for the CA using only alphanumeric,                                    |
   |                         |                      | underscore (:literal:`_`), and dash (:literal:`-`) characters.                                  |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Type                    | drop-down menu       | Choose the type of CA. Choices are *Internal CA*, *Intermediate CA*, and *Import CA*.           |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Key Type                | drop-down menu       | Cryptosystem for the certificate authority key. Choose between *RSA*                            |
   |                         |                      | (`Rivest-Shamir-Adleman <https://en.wikipedia.org/wiki/RSA_(cryptosystem)>`__) and *EC*         |
   |                         |                      | (`Elliptic-curve <https://en.wikipedia.org/wiki/Elliptic-curve_cryptography>`__) encryption.    |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | EC Curve                | drop-down menu       | Elliptic curve to apply to the certificate authority key. Choose from different *Brainpool* or  |
   |                         |                      | *SEC* curve parameters. See `RFC 5639 <https://tools.ietf.org/html/rfc5639>`__ and              |
   |                         |                      | `SEC 2 <http://www.secg.org/sec2-v2.pdf>`__ for more details. Applies to *EC* keys only.        |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Key Length              | drop-down menu       | For security reasons, a minimum of *2048* is recommended. Applies to *RSA* keys only.           |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Digest Algorithm        | drop-down menu       | The default is acceptable unless the organization requires a different algorithm.               |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Lifetime                | integer              | The lifetime of a CA is specified in days.                                                      |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Country                 | drop-down menu       | Select the country for the organization.                                                        |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | State                   | string               | Enter the state or province of the organization.                                                |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Locality                | string               | Enter the location of the organization.                                                         |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Organization            | string               | Enter the name of the company or organization.                                                  |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Organizational Unit     | string               | Organizational unit of the entity.                                                              |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Email                   | string               | Enter the email address for the person responsible for the CA.                                  |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Common Name             | string               | Enter the fully-qualified hostname (FQDN) of the system. The :guilabel:`Common Name`            |
   |                         |                      | **must** be unique within a certificate chain.                                                  |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+
   | Subject Alternate Names | string               | Multi-domain support. Enter additional space separated domain names.                            |
   |                         |                      |                                                                                                 |
   |                         |                      |                                                                                                 |
   +-------------------------+----------------------+-------------------------------------------------------------------------------------------------+


To create an intermediate CA which is part of a certificate
chain, set the :guilabel:`Type` to *Intermediate CA*. This
screen adds one more option to the screen shown in
:numref:`Figure %s <create_ca_fig>`:

* **Signing Certificate Authority:** this drop-down menu is used to
  specify the root CA in the certificate chain. This CA must first be
  imported or created.

Imported or created CAs are added as entries in
:menuselection:`System --> CAs`.
The columns in this screen indicate the name of the CA, whether it is
an internal CA, whether the issuer is self-signed, the CA lifetime (in
days), the common name of the CA, the date and time the CA was created,
and the date and time the CA expires.

Click ui-options on an existing CA to access these configuration
buttons:

* **View:** use this option to view the contents of an existing
  :guilabel:`Certificate`, :guilabel:`Private Key`, or to edit the
  :guilabel:`Identifier`.

* **Sign CSR:** used to sign internal Certificate Signing Requests
  created using
  :menuselection:`System --> Certificates --> Create CSR`.
  Signing a request adds a new certificate to
  :menuselection:`System --> Certificates`.

* **Export Certificate:** prompts to browse to the location to save a
  copy of the CA's X.509 certificate on the computer being used to
  access the FreeNAS\ :sup:`®` system.

* **Export Private Key:** prompts to browse to the location to save a
  copy of the CA's private key on the computer being used to access
  the FreeNAS\ :sup:`®` system. This option only appears if the CA has a private
  key.

* **Delete:** prompts for confirmation before deleting the CA.


.. index:: Certificates
.. _Certificates:

Certificates
------------

FreeNAS\ :sup:`®` can import existing certificates or certificate signing requests,
create new certificates, and issue certificate signing requests so that
created certificates can be signed by the CA which was previously
imported or created in :ref:`CAs`.

Go to
:menuselection:`System --> Certificates`
to add or view certificates.

.. _initial_cert_scr_fig:
.. figure:: images/system-certificates.png

   Certificates


FreeNAS\ :sup:`®` uses a self-signed certificate to enable encrypted access to the
web interface. This certificate is generated at boot and cannot be deleted
until a different certificate is chosen as the
:ref:`GUI SSL Certificate <system_general_tab>`.

To import an existing certificate, click :guilabel:`ADD` and set the
:guilabel:`Type` to *Import Certificate*.
:numref:`Figure %s <import_cert_fig>` shows the options.
When importing a certificate chain, paste the primary certificate,
followed by any intermediate certificates, followed by the root CA
certificate.

The configurable options are summarized in
:numref:`Table %s <cert_import_opt_tab>`.

.. _import_cert_fig:

.. figure:: images/system-certificates-add-import-certificate.png

   Importing a Certificate


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.64\linewidth-2\tabcolsep}|

.. _cert_import_opt_tab:

.. table:: Certificate Import Options
   :class: longtable

   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Setting              | Value          | Description                                                                                     |
   +======================+================+=================================================================================================+
   | Identifier           | string         | Enter a descriptive name for the certificate using only alphanumeric,                           |
   |                      |                | underscore (:literal:`_`), and dash (:literal:`-`) characters.                                  |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Type                 | drop-down menu | Choose the type of certificate. Choices are *Internal Certificate*,                             |
   |                      |                | *Certificate Signing Request*, *Import Certificate*, and *Import Certificate Signing Request*.  |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | CSR exists on this   | checkbox       | Set when the certificate being imported already has a Certificate Signing Request (CSR) on the  |
   | system               |                | system.                                                                                         |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Signing Certificate  | drop-down menu | Select a previously created or imported CA. Active when :guilabel:`CSR exists on this system`   |
   | Authority            |                | is set.                                                                                         |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Certificate          | string         | Paste the contents of the certificate.                                                          |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Private Key          | string         | Paste the private key associated with the certificate. Private keys must be at least 1024 bits  |
   |                      |                | long. Active when :guilabel:`CSR exists on this system` is unset.                               |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Passphrase           | string         | If the private key is protected by a passphrase, enter it here and repeat it in                 |
   |                      |                | the :guilabel:`Confirm Passphrase` field. Active when :guilabel:`CSR exists on this system`     |
   |                      |                | is unset.                                                                                       |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+


Importing a certificate signing request requires copying the contents of
the signing request and key files into the form. Having the signing
request :literal:`CERTIFICATE REQUEST` and :literal:`PRIVATE KEY`
strings visible in a separate window simplifies the import process.

.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.64\linewidth-2\tabcolsep}|

.. _csr_import_opt_tab:

.. table:: Certificate Signing Request Import Options
   :class: longtable

   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Setting              | Value          | Description                                                                                     |
   +======================+================+=================================================================================================+
   | Identifier           | string         | Enter a descriptive name for the certificate using only alphanumeric,                           |
   |                      |                | underscore (:literal:`_`), and dash (:literal:`-`) characters.                                  |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Type                 | drop-down menu | Choose the type of certificate. Choices are *Internal Certificate*,                             |
   |                      |                | *Certificate Signing Request*, *Import Certificate*, and *Import Certificate Signing Request*.  |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Signing Request      | drop-down menu | Paste the :literal:`CERTIFICATE REQUEST` string from the signing request.                       |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Private Key          | string         | Paste the private key associated with the certificate signing request. Private keys must be at  |
   |                      |                | least 1024 bits long.                                                                           |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Passphrase           | string         | If the private key is protected by a passphrase, enter it here and repeat it in                 |
   |                      |                | the :guilabel:`Confirm Passphrase` field.                                                       |
   +----------------------+----------------+-------------------------------------------------------------------------------------------------+


To create a new self-signed certificate, set the :guilabel:`Type` to
*Internal Certificate* to see the options shown in
:numref:`Figure %s <create_new_cert_fig>`. The configurable options are
summarized in :numref:`Table %s <cert_create_opts_tab>`. When completing
the fields for the certificate authority, use the information for the
organization. Since this is a self-signed certificate, use the CA that
was imported or created with :ref:`CAs` as the signing authority.


.. _create_new_cert_fig:

.. figure:: images/system-certificates-add-internal-certificate.png

   Creating a New Certificate


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.60\linewidth-2\tabcolsep}|

.. _cert_create_opts_tab:

.. table:: Certificate Creation Options
   :class: longtable

   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Setting                 | Value          | Description                                                                                     |
   +=========================+================+=================================================================================================+
   | Identifier              | string         | Enter a descriptive name for the certificate using only alphanumeric,                           |
   |                         |                | underscore (:literal:`_`), and dash (:literal:`-`) characters.                                  |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Type                    | drop-down menu | Choose the type of certificate. Choices are *Internal Certificate*,                             |
   |                         |                | *Certificate Signing Request*, and *Import Certificate*.                                        |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Signing Certificate     | drop-down menu | Select the CA which was previously imported or created using :ref:`CAs`.                        |
   | Authority               |                |                                                                                                 |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Key Type                | drop-down menu | Cryptosystem for the certificate key. Choose between *RSA*                                      |
   |                         |                | (`Rivest-Shamir-Adleman <https://en.wikipedia.org/wiki/RSA_(cryptosystem)>`__) and *EC*         |
   |                         |                | (`Elliptic-curve <https://en.wikipedia.org/wiki/Elliptic-curve_cryptography>`__) encryption.    |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | EC Curve                | drop-down menu | Elliptic curve to apply to the certificate key. Choose from different *Brainpool* or *SEC*      |
   |                         |                | curve parameters. See `RFC 5639 <https://tools.ietf.org/html/rfc5639>`__ and                    |
   |                         |                | `SEC 2 <http://www.secg.org/sec2-v2.pdf>`__ for more details. Applies to *EC* keys only.        |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Key Length              | drop-down menu | For security reasons, a minimum of *2048* is recommended. Applies to *RSA* keys only.           |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Digest Algorithm        | drop-down menu | The default is acceptable unless the organization requires a different algorithm.               |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Lifetime                | integer        | The lifetime of the certificate is specified in days.                                           |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Country                 | drop-down menu | Select the country for the organization.                                                        |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | State                   | string         | State or province of the organization.                                                          |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Locality                | string         | Location of the organization.                                                                   |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Organization            | string         | Name of the company or organization.                                                            |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Organizational Unit     | string         | Organizational unit of the entity.                                                              |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Email                   | string         | Enter the email address for the person responsible for the CA.                                  |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Common Name             | string         | Enter the fully-qualified hostname (FQDN) of the system. The :guilabel:`Common Name`            |
   |                         |                | **must** be unique within a certificate chain.                                                  |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+
   | Subject Alternate Names | string         | Multi-domain support. Enter additional domain names and separate them with a space.             |
   +-------------------------+----------------+-------------------------------------------------------------------------------------------------+


If the certificate is signed by an external CA,
such as Verisign, instead create a certificate signing request. To do
so, set the :guilabel:`Type` to *Certificate Signing Request*. The
options from :numref:`Figure %s <create_new_cert_fig>` display, but
without the :guilabel:`Signing Certificate Authority` and
:guilabel:`Lifetime` fields.

Certificates that are imported, self-signed, or for which a
certificate signing request is created are added as entries to
:menuselection:`System --> Certificates`.
In the example shown in
:numref:`Figure %s <manage_cert_fig>`,
a self-signed certificate and a certificate signing request have been
created for the fictional organization *My Company*. The self-signed
certificate was issued by the internal CA named *My Company* and the
administrator has not yet sent the certificate signing request to
Verisign so that it can be signed. Once that certificate is signed
and returned by the external CA, it should be imported with a new
certificate set to *Import Certificate*. This makes the certificate
available as a configurable option for encrypting connections.


.. _manage_cert_fig:

.. figure:: images/system-certificates-manage.png

   Managing Certificates


Clicking ui-options for an entry shows these configuration buttons:

* **View:** use this option to view the contents of an existing
  :guilabel:`Certificate`, :guilabel:`Private Key`, or to edit the
  :guilabel:`Identifier`.

* **Create ACME Certificate:** use an :ref:`ACME DNS` authenticator
  to verify, issue, and renew a certificate. Only visible with
  certificate signing requests.

* **Export Certificate** saves a copy of the certificate or
  certificate signing request to the system being used to access the
  FreeNAS\ :sup:`®` system. For a certificate signing request, send the
  exported certificate to the external signing authority so that it
  can be signed.

* **Export Private Key** saves a copy of the private key associated
  with the certificate or certificate signing request to the system
  being used to access the FreeNAS\ :sup:`®` system.

* **Delete** is used to delete a certificate or certificate signing
  request.



.. _ACME Certificates:

ACME Certificates
~~~~~~~~~~~~~~~~~

`Automatic Certificate Management Environment (ACME) <https://ietf-wg-acme.github.io/acme/draft-ietf-acme-acme.html>`__
is available for automating certificate issuing and renewal. The user
must verify ownership of the domain before certificate automation is
allowed.

ACME certificates can be created for existing certificate signing
requests. These certificates use an :ref:`ACME DNS` authenticator to
confirm domain ownership, then are automatically issued and renewed. To
create a new ACME certificate, go to
:menuselection:`System --> Certificates`,
click ui-options for an existing certificate signing request, and
click :guilabel:`Create ACME Certificate`.

.. _ACME_cert_fig:

.. figure:: images/system-acme-cert-add.png

   ACME Certificate Options


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.22\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.15\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.62\linewidth-2\tabcolsep}|

.. _ACME Certificate Options:

.. table:: ACME Certificate Options
   :class: longtable

   +--------------------------------------+----------------+-------------------------------------------------------------------------------------+
   | Setting                              | Value          | Description                                                                         |
   +======================================+================+=====================================================================================+
   | Identifier                           | string         | Internal identifier of the certificate. Only alphanumeric characters, dash          |
   |                                      |                | (:literal:`-`), and underline (:literal:`_`) are allowed.                           |
   +--------------------------------------+----------------+-------------------------------------------------------------------------------------+
   | Terms of Service                     | checkbox       | Please accept the terms of service for the given ACME Server.                       |
   +--------------------------------------+----------------+-------------------------------------------------------------------------------------+
   | Renew Certificate Day                | integer        | Number of days to renew certificate before expiring.                                |
   +--------------------------------------+----------------+-------------------------------------------------------------------------------------+
   | ACME Server Directory URI            | drop-down menu | URI of the ACME Server Directory. Choose a preconfigured URI or enter a custom URI. |
   +--------------------------------------+----------------+-------------------------------------------------------------------------------------+
   | Authenticator for {Domain Name}      | drop-down menu | Authenticator to validate the Domain. Choose a previously configured                |
   | ({Domain Name} dynamically changes)  |                | :ref:`ACME DNS` authenticator.                                                      |
   +--------------------------------------+----------------+-------------------------------------------------------------------------------------+


.. index:: ACME DNS
.. _ACME DNS:

ACME DNS
--------

Go to
:menuselection:`System --> ACME DNS`
and click :guilabel:`ADD` to show options to add a new DNS
authenticator to FreeNAS\ :sup:`®`. This is used to create
:ref:`ACME Certificates` that are automatically issued and renewed
after being validated.


.. _ACME_DNS_fig:

.. figure:: images/system-acmedns-add.png

   DNS Authenticator Options


Enter a name for the authenticator. This is only used to identify the
authenticator in the FreeNAS\ :sup:`®` web interface. Choose a DNS provider and
configure any required :guilabel:`Authenticator Attributes`:

* **Route 53:** Amazon DNS web service. Requires entering an Amazon
  account :guilabel:`Access ID Key` and :guilabel:`Secret Access Key`.
  See the
  `AWS documentation <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html>`__
  for more details about generating these keys.

Click :guilabel:`SAVE` to register the DNS Authenticator and add it to
the list of authenticator options for :ref:`ACME Certificates`.

.. index:: Support
.. _Support:

Support
-------

The FreeNAS\ :sup:`®` :guilabel:`Support` option, shown in
:numref:`Figure %s <support_fig>`, provides a built-in ticketing system
for generating bug reports and feature requests.

.. _support_fig:

.. figure:: images/system-support.png

   Support Menu


This screen provides a built-in interface to the FreeNAS\ :sup:`®` issue
tracker located at `<https://bugs.ixsystems.com>`.

An account is required to create tickets and receive notifications
as issues are addressed.

Log in to an existing account to enter an issue. If you do not have an
account yet, go to `<https://bugs.ixsystems.com>`, click :guilabel:`Register`, and
fill out the form. Reply to the registration email to validate the
account before logging in.

Before creating a bug report or feature request, ensure that an
existing report does not already exist at `<https://bugs.ixsystems.com>`. If a
similar issue is already present and has not been marked *Closed* or
*Resolved*, comment on that issue, adding new information to help solve
it. When similar issues are *Closed* or *Resolved*, create a new issue
and refer to the previous issue.

.. note:: Update the system to the latest version of STABLE
   and retest before reporting an issue. Newer versions of the software
   might have already fixed the problem.


To generate a report using the built-in :guilabel:`Support` screen,
complete these fields:

* **Username:** enter the login name created when registering at
  `<https://bugs.ixsystems.com>`.

* **Password:** enter the password associated with the registered
  login name.

* **Type:** select *Bug* when reporting an issue or *Feature* when
  requesting a new feature.

* **Category:** this drop-down menu is empty until a registered
  :guilabel:`Username` and :guilabel:`Password` are entered. The field
  remains empty if either value is incorrect. After the
  :guilabel:`Username` and :guilabel:`Password` are validated, possible
  categories are populated to the drop-down menu. Select the one that
  best describes the bug or feature being reported.

* **Attach Debug:** enabling this option is recommended so an
  overview of the system hardware, build string, and configuration is
  automatically generated and included with the ticket. Generating and
  attaching a debug to the ticket can take some time.

  Debug file attachments are limited to 20 MiB. If the debug file is
  too large to include, unset the option to generate the debug file
  and let the system create an issue ticket as shown below. Manually
  create a debug file by going to
  :menuselection:`System --> Advanced`
  and clicking :guilabel:`SAVE DEBUG`.

  Go to the ticket at `<https://bugs.ixsystems.com>` and add the debug file as a
  private attachment.

* **Subject:** enter a descriptive title for the ticket. A good
  *Subject* makes it easy to find similar reports.

* **Description:** enter a one- to three-paragraph summary of the
  issue that describes the problem, and if applicable, what steps can
  be taken to reproduce it.

* **Attach screenshots:** select screenshots on the client system to
  include with the report.

Click :guilabel:`SUBMIT` to automatically generate and upload the
report to the issue tracker (`<https://bugs.ixsystems.com>`). This process can
take several minutes while information is collected and sent. All
files included with the report are added to the issue tracker ticket
as private attachments and can only be accessed by the creator of the
ticket and iXsystems.

After the new ticket is created, the ticket URL is shown for viewing
or updating with more information.

