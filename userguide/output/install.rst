.. _Installing and Upgrading:

Installing and Upgrading
========================

The FreeNAS\ :sup:`®` operating system has to be installed on a
separate device from the drives which hold the storage data. With only
one disk drive, the FreeNAS\ :sup:`®` |web-ui| is
available, but there is no place to store any data. And storing data
is, after all, the whole point of a NAS system. Home users
experimenting with FreeNAS\ :sup:`®` can install FreeNAS\ :sup:`®` on an inexpensive
|usb-stick| and use the computer disks for storage.

This section describes:

* :ref:`Getting FreeNAS\ :sup:`®``

* :ref:`Preparing the Media`

* :ref:`Performing the Installation`

* :ref:`Installation Troubleshooting`

* :ref:`Upgrading`

* :ref:`Virtualization`


.. index:: Getting FreeNAS\ :sup:`®`, Download
.. _Getting FreeNAS\ :sup:`®`:

Getting FreeNAS\ :sup:`®`
-------------------------

The latest STABLE version of FreeNAS\ :sup:`®` |release| is available for download
from `<https://www.freenas.org/download-freenas-release/>`__.

The download page has links to FreeNAS\ :sup:`®` release notes, :file:`.iso`
integrity checksums, and PGP security keys.

Clicking :guilabel:`Download` opens a dialog to save an *.iso* file.
This bootable installer must be
:ref:`written to physical media <Preparing the Media>` before it can be
used to install FreeNAS\ :sup:`®`.


.. index:: Verify download files

Checking Installer Integrity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

FreeNAS\ :sup:`®` uses the
`OpenPGP standard <https://en.wikipedia.org/wiki/Pretty_Good_Privacy#OpenPGP>`__
to confirm that downloaded files have been provided by a trustworthy
source. OpenPGP compliant software like
`gnupg <https://www.freebsd.org/cgi/man.cgi?query=gpg>`__,
`Kleopatra <https://www.openpgp.org/software/kleopatra/>`__,
or `Gpg4win <https://gpg4win.org/>`__ can check the PGP signature of a
FreeNAS\ :sup:`®` installer file.

The :file:`sha256.txt` file is used to confirm the integrity of the
downloaded :file:`.iso`. See :ref:`SHA256 Verification` for more
details.


PGP Verification
^^^^^^^^^^^^^^^^

To verify the :file:`.iso` source, go to
`<https://www.freenas.org/download-freenas-release/>`__ and click
:guilabel:`PGP Signature` to download the software signature file. Open
the :guilabel:`PGP Public key` link and note the browser address and
:literal:`Search results` string.

Use one of the OpenPGP encryption tools mentioned above to import the
public key and verify the PGP signature.

This example shows verifying the FreeNAS\ :sup:`®` :file:`.iso` using
:command:`gpg` in a command prompt:

* Go to the :file:`.iso` and :file:`.iso.gpg` download location and
  import the public key using the keyserver address and search results
  string:

.. code-block:: none

   tmoore@Observer ~> cd Downloads/
   tmoore@Observer ~/Downloads> gpg --keyserver sks-keyservers.net --recv-keys 0xc8d62def767c1db0dff4e6ec358eaa9112cf7946
   gpg: /usr/home/tmoore/.gnupg/trustdb.gpg: trustdb created
   gpg: key 358EAA9112CF7946: public key "IX SecTeam <security-officer@ixsystems.com>" imported
   gpg: Total number processed: 1
   gpg:               imported: 1
   tmoore@Observer ~/Downloads>

* Use :command:`gpg --verify` to compare the :file:`.iso` and
  :file:`.iso.gpg` files:

.. code-block:: none

   tmoore@Observer ~/Downloads> gpg --verify FreeNAS-11.2-U6.iso.gpg FreeNAS-11.2-U6.iso
   gpg: Signature made Tue Nov  5 13:48:18 2019 EST
   gpg:                using RSA key C8D62DEF767C1DB0DFF4E6EC358EAA9112CF7946
   gpg: Good signature from "IX SecTeam <security-officer@ixsystems.com>" [unknown]
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: C8D6 2DEF 767C 1DB0 DFF4  E6EC 358E AA91 12CF 7946
   tmoore@Observer ~/Downloads>

* This response means the signature is correct but still untrusted. Go
  back to the browser page that has the :guilabel:`PGP Public key` open
  and manually confirm that the key was issued for the iX Security Team
  on October 15, 2019 and has been signed by iXsystems accounts.

.. _SHA256 Verification:

SHA256 Verification
^^^^^^^^^^^^^^^^^^^

The command to verify the checksum varies by operating system:

* on a BSD system use the command :samp:`sha256 {isofile}`

* on a Linux system use the command :samp:`sha256sum {isofile}`

* on a Mac system use the command :samp:`shasum -a 256 {isofile}`

* Windows or Mac users can install additional utilities like
  `HashCalc <http://www.slavasoft.com/hashcalc/>`__
  or
  `HashTab <http://implbits.com/products/hashtab/>`__.

The value produced by running the command must match the value shown
in the :file:`sha256.txt` file. Different checksum values indicate a
corrupted installer file that should not be used.


.. index:: Burn ISO, ISO
.. _Preparing the Media:

Preparing the Media
-------------------

The FreeNAS\ :sup:`®` installer can run from either a CD or a |usb-stick|.

A CD burning utility is needed to write the :file:`.iso` file to a
CD.

The :file:`.iso` file can also be written to a |usb-stick|. The
method used to write the file depends on the operating system. Examples
for several common operating systems are shown below.

.. note:: To install from a |usb-stick| to another |usb-stick|, **two**
   USB ports are needed, each with an inserted USB device. One
   |usb-stick| contains the installer.  The other |usb-stick| is the
   destination for the FreeNAS\ :sup:`®` installation. Take care to select the
   correct USB device for the FreeNAS\ :sup:`®` installation. It is **not**
   possible to install FreeNAS\ :sup:`®` onto the same |usb-stick| containing the
   installer. After installation, remove the installer |usb-stick|. It
   might also be necessary to adjust the BIOS configuration to boot
   from the new FreeNAS\ :sup:`®` |usb-stick|.

Ensure the |os-device| order in the BIOS is set to boot from
the device containing the FreeNAS\ :sup:`®` installer media, then boot the
system to start the installation.


.. _On FreeBSD or Linux:

On FreeBSD or Linux
~~~~~~~~~~~~~~~~~~~

On a FreeBSD or Linux system, the :command:`dd` command is used to
write the :file:`.iso` file to an inserted |usb-stick|.

.. warning:: The :command:`dd` command is very powerful and can
   destroy any existing data on the specified device. Make
   **absolutely sure** of the device name to write to and do not
   mistype the device name when using :command:`dd`! This command can
   be avoided by writing the :file:`.iso` file to a CD instead.


This example demonstrates writing the image to the first USB device
connected to a FreeBSD system. This first device usually reports as
*/dev/da0*. Replace :samp:`{FreeNAS-RELEASE.iso}` with the filename
of the downloaded FreeNAS\ :sup:`®` ISO file. Replace :samp:`{/dev/da0}` with
the device name of the device to write.

.. code-block:: none

   dd if=FreeNAS-RELEASE.iso of=/dev/da0 bs=64k
   6117+0 records in
   6117+0 records out
   400883712 bytes transferred in 88.706398 secs (4519220 bytes/sec)


When using the :command:`dd` command:

* **if=** refers to the input file, or the name of the file to write
  to the device.

* **of=** refers to the output file; in this case, the device name of
  the flash card or removable |usb-stick|. Note that USB device numbers
  are dynamic, and the target device might be *da1* or *da2* or
  another name depending on which devices are attached. Before
  attaching the target |usb-stick|, use :command:`ls /dev/da*`.  Then
  attach the target |usb-stick|, wait ten seconds, and run :command:`ls
  /dev/da*` again to see the new device name and number of the target
  |usb-stick|. On Linux, use :samp:`/dev/sd{X}`, where *X* refers to the
  letter of the USB device.

* **bs=** refers to the block size, the amount of data to write at a
  time. The larger 64K block size shown here helps speed up writes to
  the |usb-stick|.


.. _On Windows:

On Windows
~~~~~~~~~~

`Image Writer <https://launchpad.net/win32-image-writer/>`__
and
`Rufus <http://rufus.akeo.ie/>`__
can be used for writing images to |usb-sticks| on Windows.

.. _On macOS:

On macOS
~~~~~~~~

Insert the |usb-stick|. In Finder, go to
:menuselection:`Applications --> Utilities --> Disk Utility`.
Unmount any mounted partitions on the |usb-stick|. Check that the
|usb-stick| has only one partition, or partition table errors will
be shown on boot. If needed, use Disk Utility to set up one partition
on the |usb-stick|. Selecting :guilabel:`Free space` when creating the
partition works fine.

Determine the device name of the inserted |usb-stick|. From
TERMINAL, navigate to the Desktop, then type this command:

.. code-block:: none

 diskutil list
 /dev/disk0

 #:	TYPE NAME		SIZE		IDENTIFIER
 0:	GUID_partition_scheme	*500.1 GB	disk0
 1:	EFI			209.7 MB	disk0s1
 2:	Apple_HFS Macintosh HD	499.2 GB	disk0s2
 3:	Apple_Boot Recovery HD	650.0 MB	disk0s3

 /dev/disk1
 #:	TYPE NAME		SIZE		IDENTIFIER
 0:	FDisk_partition_scheme	*8.0 GB		disk1
 1:	DOS_FAT_32 UNTITLED	8.0 GB		disk1s1


This shows which devices are available to the system. Locate the
target |usb-stick| and record the path. To determine the correct path
for the |usb-stick|, remove the device, run the
command again, and compare the difference. Once sure of the device
name, navigate to the Desktop from TERMINAL, unmount the |usb-stick|,
and use the :command:`dd` command to write the image to the |usb-stick|.
In this example, the |usb-stick| is :file:`/dev/disk1`. It is
first unmounted. The :command:`dd` command is used to write the
image to the faster "raw" version of the device (note the extra
:literal:`r` in :file:`/dev/rdisk1`).

When running these commands, replace :samp:`{FreeNAS-RELEASE.iso}`
with the name of the FreeNAS\ :sup:`®` ISO and :samp:`{/dev/rdisk1}` with the
correct path to the |usb-stick|:

.. code-block:: none

   diskutil unmountDisk /dev/disk1
   Unmount of all volumes on disk1 was successful

   dd if=FreeNAS-RELEASE.iso of=/dev/rdisk1 bs=64k


.. note:: If the error "Resource busy" is shown when the
   :command:`dd` command is run, go to
   :menuselection:`Applications --> Utilities --> Disk Utility`,
   find the |usb-stick|, and click on its partitions to make sure
   all of them are unmounted. If a "Permission denied" error is shown,
   use :command:`sudo` for elevated rights:
   :samp:`sudo dd if={FreeNAS-11.0-RELEASE.iso} of={/dev/rdisk1} bs=64k`.
   This will prompt for the password.


The :command:`dd` command can take some minutes to complete. Wait
until the prompt returns and a message is displayed with information
about how long it took to write the image to the |usb-stick|.


.. index:: Install
.. _Performing the Installation:

Performing the Installation
---------------------------

With the installation media inserted, boot the system from that media.

The FreeNAS\ :sup:`®` installer boot menu is displayed as is shown in
:numref:`Figure %s <installer_boot_menu_fig>`.


.. _installer_boot_menu_fig:

.. figure:: images/console/installer-boot-menu.png

   Installer Boot Menu


The FreeNAS\ :sup:`®` installer automatically boots into the default option after
ten seconds. If needed, choose another boot option by pressing the
:kbd:`Spacebar` to stop the timer and then enter the number of the
desired option.

.. tip:: The :guilabel:`Serial Console` option is useful on systems
   which do not have a keyboard or monitor, but are accessed through a
   serial port, *Serial over LAN*, or :ref:`IPMI`.


.. note:: If the installer does not boot, verify that the installation
   device is listed first in the boot order in the BIOS. When booting
   from a CD, some motherboards may require connecting the CD device
   to SATA0 (the first connector) to boot from CD. If the installer
   stalls during bootup, double-check the SHA256 hash of the
   :file:`.iso` file. If the hash does not match, re-download the
   file. If the hash is correct, burn the CD again at a lower speed or
   write the file to a different |usb-stick|.

Once the installer has finished booting, the installer menu is displayed
as shown in :numref:`Figure %s <installer_menu_fig>`.


.. _installer_menu_fig:

.. figure:: images/console/installer-install-menu.png

   Installer Menu


Press :kbd:`Enter` to select the default option,
:guilabel:`1 Install/Upgrade`. The next menu, shown in
:numref:`Figure %s <select_drive_fig>`,
lists all available drives. This includes any inserted |os-devices|,
which have names beginning with *da*.

.. note:: A minimum of 8 GiB of RAM is required and the installer will
   present a warning message if less than 8 GiB is detected.

In this example, the user is performing a test installation using
VirtualBox and has created a 16 GiB virtual disk to hold the operating
system.


.. _select_drive_fig:

.. figure:: images/console/installer-drive.png

   Selecting the Install Drive


Use the arrow keys to highlight the destination SSD, hard drive,
|usb-stick|, or virtual disk. Press the :kbd:`spacebar` to select
it.

To mirror the |os-device|, move to additional devices and press
:kbd:`spacebar` to select them also. If all of the selected devices
are larger than 64 GiB and none are connected through USB, a 16 GiB
swap partition is also created.

After making selections, press :kbd:`Enter`. The warning shown in
:numref:`Figure %s <install_warning_fig>`
is displayed, a reminder not to install the operating system on a
drive that is meant for storage. Press :kbd:`Enter` to continue on to
the screen shown in
:numref:`Figure %s <set_root_pass_fig>`.


.. _install_warning_fig:

.. figure:: images/console/installer-drive-warning.png

   Installation Warning


See the :ref:`operating system device <The Operating System Device>`
section to ensure the minimum requirements are met.

The installer recognizes existing installations of previous versions
of FreeNAS\ :sup:`®`. When an existing installation is present, the menu shown in
:numref:`Figure %s <fresh_install_fig>`
is displayed.  To overwrite an existing installation, use the arrows
to move to :guilabel:`Fresh Install` and press :kbd:`Enter` twice to
continue to the screen shown in
:numref:`Figure %s <set_root_pass_fig>`.


.. _fresh_install_fig:

.. figure:: images/console/installer-upgrade-or-fresh-install.png

   Performing a Fresh Install


The screen shown in
:numref:`Figure %s <set_root_pass_fig>`
prompts for the *root* password
which is used to log in to the |web-ui|.


.. _set_root_pass_fig:

.. figure:: images/console/installer-root-password.png

   Set the Root Password


Setting a password is mandatory and the password cannot be blank.
Since this password provides access to the |web-ui|, it
needs to be hard to guess. Enter the password, press the down arrow key,
and confirm the password. Then press :kbd:`Enter` to continue with the
installation. Choosing :guilabel:`Cancel` skips setting a root password
during the installation, but the |web-ui| will require setting a
root password when logging in for the first time.

.. note:: For security reasons, the SSH service and *root* SSH logins
   are disabled by default. Unless these are set, the only way to
   access a shell as *root* is to gain physical access to the console
   menu or to access the web shell within the |web-ui|. This
   means that the FreeNAS\ :sup:`®` system needs to be kept physically secure and
   that the |web-ui| needs to be behind a properly configured
   firewall and protected by a secure password.


FreeNAS\ :sup:`®` can be configured to boot with the standard BIOS boot
mechanism or UEFI booting as shown
:numref:`Figure %s <uefi_or_bios_fig>`.
BIOS booting is recommended for legacy and enterprise hardware. UEFI
is used on newer consumer motherboards.


.. _uefi_or_bios_fig:

.. figure:: images/console/installer-boot-mode.png

   Choose UEFI or BIOS Booting


.. note:: Most UEFI systems can also boot in BIOS mode if CSM
   (Compatibility Support Module) is enabled in the UEFI setup
   screens.

The message in
:numref:`Figure %s <install_complete_fig>`
is shown after the installation is complete.


.. _install_complete_fig:

.. figure:: images/console/installer-complete.png

   Installation Complete


Press :kbd:`Enter` to return to :ref:`installer_menu_fig`.
Highlight :guilabel:`3 Reboot System` and press :kbd:`Enter`. If
booting from CD, remove the CDROM. As the system reboots, make sure
that the device where FreeNAS\ :sup:`®` was installed is listed as the first
boot entry in the BIOS so the system will boot from it.

FreeNAS\ :sup:`®` boots into the :guilabel:`Console Setup` menu described in
:ref:`Booting` after waiting five seconds in the
:ref:`boot menu <boot_menu_fig>`. Press the :kbd:`Spacebar` to stop the
timer and use the boot menu.


.. _Installation Troubleshooting:

Installation Troubleshooting
----------------------------

If the system does not boot into FreeNAS\ :sup:`®`, there are several things
that can be checked to resolve the situation.

Check the system BIOS and see if there is an option to change the USB
emulation from CD/DVD/floppy to hard drive. If it still will not boot,
check to see if the card/drive is UDMA compliant.

If the system BIOS does not support EFI with BIOS emulation, see if it
has an option to boot using legacy BIOS mode.

When the system starts to boot but hangs with this repeated error
message:

.. code-block:: none

   run_interrupt_driven_hooks: still waiting after 60 seconds for xpt_config


go into the system BIOS and look for an onboard device configuration
for a 1394 Controller. If present, disable that device and try booting
again.

If the system starts to boot but hangs at a *mountroot>* prompt,
follow the instructions in
`Workaround/Semi-Fix for Mountroot Issues with 9.3
<https://forums.freenas.org/index.php?threads/workaround-semi-fix-for-mountroot-issues-with-9-3.26071/>`__.

If the burned image fails to boot and the image was burned using a
Windows system, wipe the |usb-stick| before trying a second burn using a
utility such as
`Active@ KillDisk <http://how-to-erase-hard-drive.com/>`__.
Otherwise, the second burn attempt will fail as Windows does not
understand the partition which was written from the image file. Be
very careful to specify the correct |usb-stick| when using a wipe
utility!


.. index:: Upgrade
.. _Upgrading:

Upgrading
---------

FreeNAS\ :sup:`®` provides flexibility for keeping the operating system
up-to-date:

#. Upgrades to major releases, for example from version 9.3 to 9.10,
   can still be performed using either an ISO or the
   |web-ui|. Unless the Release Notes for the new
   major release indicate that the current version requires an ISO
   upgrade, either upgrade method can be used.

#. Minor releases have been replaced with signed updates. This means
   that it is not necessary to wait for a minor release to update the
   system with a system update or newer versions of drivers and
   features.  It is also no longer necessary to manually download an
   upgrade file and its associated checksum to update the system.

#. The updater automatically creates a boot environment, making
   updates a low-risk operation. Boot environments provide the
   option to return to the previous version of the operating system by
   rebooting the system and selecting the previous boot environment
   from the boot menu.

This section describes how to perform an upgrade from an earlier
version of FreeNAS\ :sup:`®` to |release|. After |release| has been installed,
use the instructions in :ref:`Update` to keep the system updated.


.. _Caveats:

Caveats
~~~~~~~

Be aware of these caveats **before** attempting an upgrade to
|release|:

* **Warning: upgrading the ZFS pool can make it impossible to go back
  to a previous version.** For this reason, the update process does
  not automatically upgrade the ZFS pool, though the :ref:`Alert`
  system shows when newer :ref:`ZFS Feature Flags` are available for a
  pool. Unless a new feature flag is needed, it is safe to leave the
  pool at the current version and uncheck the alert. If the pool is
  upgraded, it will not be possible to boot into a previous version that
  does not support the newer feature flags.

* Upgrading the firmware of Broadcom SAS HBAs to the latest version is
  recommended.

* If upgrading from 9.3.x, read the
  `FAQ: Updating from 9.3 to 9.10
  <https://forums.freenas.org/index.php?threads/faq-updating-from-9-3-to-9-10.54260/>`__
  first.

* **Upgrades from** FreeNAS\ :sup:`®` **0.7x are not supported.** The system
  has no way to import configuration settings from 0.7x versions of
  FreeNAS\ :sup:`®`. The configuration must be manually recreated.  If
  supported, the FreeNAS\ :sup:`®` 0.7x pools or disks must be manually
  imported.

* **Upgrades on 32-bit hardware are not supported.** However, if the
  system is currently running a 32-bit version of FreeNAS\ :sup:`®` **and** the
  hardware supports 64-bit, the system can be upgraded.  Any
  archived reporting graphs will be lost during the upgrade.

* **UFS is not supported.** If the data currently resides on **one**
  UFS-formatted disk, create a ZFS pool using **other** disks after the
  upgrade, then use the instructions in :ref:`Importing a Disk` to moun
  t the UFS-formatted disk and copy the data to the ZFS pool. With only
  one disk, back up its data to another system or media before the
  upgrade, format the disk as ZFS after the upgrade, then restore the
  backup. If the data currently resides on a UFS RAID of disks, it is
  not possible to directly import that data to the ZFS pool. Instead,
  back up the data before the upgrade, create a ZFS pool after the
  upgrade, then restore the data from the backup.


.. _Initial Preparation:

Initial Preparation
~~~~~~~~~~~~~~~~~~~

Before upgrading the operating system, perform the following steps:

#.  **Back up the** FreeNAS\ :sup:`®` **configuration** in
    :menuselection:`System --> General --> Save Config`.

#.  If any pools are encrypted, **remember** to set a passphrase
    and download a copy of the encryption key and the latest
    recovery key.
    After the upgrade is complete, use the instructions in
    :ref:`Importing a Pool` to import the encrypted pools.

#.  Warn users that the FreeNAS\ :sup:`®` shares will be unavailable during the
    upgrade; it is recommended to schedule the upgrade for a time
    that will least impact users.

#.  Stop all services in
    :menuselection:`Services`.


.. _Upgrading Using the ISO:

Upgrading Using the ISO
~~~~~~~~~~~~~~~~~~~~~~~

To perform an upgrade using this method,
`download <http://download.freenas.org/latest/>`__
the :file:`.iso` to the computer that will be used to prepare the
installation media. Burn the downloaded :file:`.iso` file to a CD or
|usb-stick| using the instructions in
:ref:`Preparing the Media`.

Insert the prepared media into the system and boot from it. The
installer waits ten seconds in the
:ref:`installer boot menu <installer_boot_menu_fig>` before booting the
default option. If needed, press the :kbd:`Spacebar` to stop the timer
and choose another boot option. After the media finishes booting into
the installation menu, press :kbd:`Enter` to select the default option
of :guilabel:`1 Install/Upgrade.` The installer presents a screen
showing all available drives.

.. warning:: *All* drives are shown, including boot drives and storage
   drives. Only choose boot drives when upgrading. Choosing the wrong
   drives to upgrade or install will cause loss of data. If unsure
   about which drives contain the FreeNAS\ :sup:`®` operating system, reboot and
   remove the install media. In the FreeNAS\ :sup:`®` |web-ui|, use
   :menuselection:`System --> Boot`
   to identify the boot drives. More than one drive is shown when a
   mirror has been used.

Move to the drive where FreeNAS\ :sup:`®` is installed and press the
:kbd:`Spacebar` to mark it with a star. If a mirror has been used for
the operating system, mark all of the drives where the FreeNAS\ :sup:`®`
operating system is installed. Press :kbd:`Enter` when done.

The installer recognizes earlier versions of FreeNAS\ :sup:`®` installed on the
boot drive or drives and presents the message shown in
:numref:`Figure %s <upgrade_install_fig>`.


.. _upgrade_install_fig:

.. figure:: images/console/installer-upgrade-or-fresh-install.png

   Upgrading a FreeNAS\ :sup:`®` Installation


To perform an upgrade, press :kbd:`Enter` to accept the default of
:guilabel:`Upgrade Install`. Again, the installer will display a
reminder that the operating system should be installed on a disk
that is not used for storage.


.. _install_new_boot_environment_fig:

.. figure:: images/console/installer-upgrade-method.png

   Install in New Boot Environment or Format


The updated system can be installed in a new boot environment,
or the entire |os-device| can be formatted to start fresh. Installing
into a new boot environment preserves the old code, allowing a
roll-back to previous versions if necessary. Formatting the boot
device is usually not necessary but can reclaim space. User data and
settings are preserved when installing to a new boot environment and
also when formatting the |os-device|. Move the highlight to one of the
options and press :kbd:`Enter` to start the upgrade.

The installer unpacks the new image and displays the menu shown in
:numref:`Figure %s <preserve_migrate_fig>`.
The database file that is preserved and migrated contains your FreeNAS\ :sup:`®`
configuration settings.


.. _preserve_migrate_fig:

.. figure:: images/console/installer-upgrade-preserved-database.png

   Preserve and Migrate Settings


Press :kbd:`Enter`. FreeNAS\ :sup:`®` indicates that the upgrade is complete and
a reboot is required. Press :guilabel:`OK`, highlight
:guilabel:`3 Reboot System`, then press :kbd:`Enter` to reboot the
system. If the upgrade installer was booted from CD, remove the CD.

During the reboot there can be a conversion of the previous
configuration database to the new version of the database. This
happens during the "Applying database schema changes" line in the
reboot cycle. This conversion can take a long time to finish,
sometimes fifteen minutes or more, and can cause the system
to reboot again. The system will start
normally afterwards. If database errors are shown but the |web-ui|
is accessible, go to
:menuselection:`Settings --> General`
and use the :guilabel:`UPLOAD CONFIG` button to upload the
configuration that was saved before starting the upgrade.


.. _Upgrading From the Web Interface:

Upgrading From the Web Interface
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To perform an upgrade using this method, go to
:menuselection:`System --> Update`. See :ref:`Update` for more
information on upgrading the system.

The connection is lost temporarily when the update is complete. It
returns after the FreeNAS\ :sup:`®` system reboots into the new version of the
operating system. The FreeNAS\ :sup:`®` system normally receives the same
IP address from the DHCP server. Refresh the browser after a moment
to see if the system is accessible.


.. _If Something Goes Wrong:

If Something Goes Wrong
~~~~~~~~~~~~~~~~~~~~~~~

If an update fails, an alert is issued and the details are written to
:file:`/data/update.failed`.

To return to a previous version of the operating system, physical or IPMI
access to the FreeNAS\ :sup:`®` console is needed. Reboot the system and watch for
the boot menu:

.. _boot_menu_fig:

.. figure:: images/console/boot-menu.png

   Boot Menu


FreeNAS\ :sup:`®` waits five seconds before booting into the default boot
environment. Press the :kbd:`Spacebar` to stop the automatic
boot timer. Press :kbd:`4` to display the available boot environments
and press :kbd:`3` as needed to scroll through multiple pages.

.. _boot_env_fig:

.. figure:: images/console/boot-menu-environments.png

   Boot Environments


In the example shown in :numref:`Figure %s <boot_env_fig>`, the first
entry in :guilabel:`Boot Environments` is
:literal:`11.2-MASTER-201807250900`. This is the current version of the
operating system, after the update was applied. Since it is the first
entry, it is the default selection.

The next entry is :literal:`Initial-Install`. This is the original boot
environment created when FreeNAS\ :sup:`®` was first installed. Since there are no
other entries between the initial installation and the first entry, only
one update has been applied to this system since its initial
installation.

To boot into another version of the operating system, enter the number
of the boot environment to set it as :guilabel:`Active`. Press
:kbd:`Backspace` to return to the :ref:`Boot Menu <boot_menu_fig>` and
press :kbd:`Enter` to boot into the chosen :guilabel:`Active` boot
environment.

If an |os-device| fails and the system no longer boots, don't panic.
The data is still on the disks and there is still a copy of the saved
configuration. The system can be recovered with a few steps:

#.  Perform a fresh installation on a new |os-device|.

#.  Import the pools in
    :menuselection:`Storage --> Auto Import Pool`.

#.  Restore the configuration in
    :menuselection:`System --> General --> Upload Config`.

.. note:: It is not possible to restore a saved configuration that is
   newer than the installed version. For example, if a reboot
   into an older version of the operating system is performed,
   a configuration created in a later version cannot be restored.

.. index:: Upgrade ZFS Pool
.. _Upgrading a ZFS Pool:

Upgrading a ZFS Pool
~~~~~~~~~~~~~~~~~~~~

In FreeNAS\ :sup:`®`, ZFS pools can be upgraded from the graphical
administrative interface.

Before upgrading an existing ZFS pool, be aware of these caveats
first:

* the pool upgrade is a one-way street, meaning that
  **if you change your mind you cannot go back to an earlier ZFS
  version or downgrade to an earlier version of the software that
  does not support those ZFS features.**

* before performing any operation that may affect the data on a
  storage disk, **always back up all data first and verify the
  integrity of the backup.**
  While it is unlikely that the pool upgrade will affect the data,
  it is always better to be safe than sorry.

* upgrading a ZFS pool is **optional**. Do not upgrade the pool if the
  the possibility of reverting to an earlier version of FreeNAS\ :sup:`®` or
  repurposing the disks in another operating system that supports ZFS
  is desired. It is not necessary to upgrade the pool unless the end
  user has a specific need for the newer :ref:`ZFS Feature Flags`. If a
  pool is upgraded to the latest feature flags, it will not be possible
  to import that pool into another operating system that does not yet
  support those feature flags.

To perform the ZFS pool upgrade, go to
:menuselection:`Storage --> Pools` and click |ui-settings|
to upgrade. Click the
:guilabel:`Upgrade Pool` button as shown in
:numref:`Figure %s <upgrading_pool_fig>`.

.. note:: If the :guilabel:`Upgrade Pool` button does not appear, the
   pool is already at the latest feature flags and does not need to be
   upgraded.


.. _upgrading_pool_fig:

.. figure:: images/storage-pools-upgrade.png

   Upgrading a Pool


The warning serves as a reminder that a pool upgrade is not
reversible. Click :guilabel:`OK` to proceed with the upgrade.

The upgrade itself only takes a few seconds and is non-disruptive.
It is not necessary to stop any sharing services to upgrade the
pool. However, it is best to upgrade when the pool is not being
heavily used. The upgrade process will suspend I/O for a short
period, but is nearly instantaneous on a quiet pool.


.. index:: Virtualization, VM
.. _Virtualization:

Virtualization
--------------

FreeNAS\ :sup:`®` can be run inside a virtual environment for development,
experimentation, and educational purposes. Note that running
FreeNAS\ :sup:`®` in production as a virtual machine is `not recommended
<https://forums.freenas.org/index.php?threads/please-do-not-run-freenas-in-production-as-a-virtual-machine.12484/>`__.
When using FreeNAS\ :sup:`®` within a virtual environment,
`read this post first
<https://forums.freenas.org/index.php?threads/absolutely-must-virtualize-freenas-a-guide-to-not-completely-losing-your-data.12714/>`__
as it contains useful guidelines for minimizing the risk of losing
data.

To install or run FreeNAS\ :sup:`®` within a virtual environment, create a
virtual machine that meets these minimum requirements:

* **at least** 8192 MiB (8 GiB) base memory size

* a virtual disk **at least 8 GiB in size** to hold the operating
  system and boot environments

* at least one additional virtual disk **at least 4 GiB in size** to be
  used as data storage

* a bridged network adapter

This section demonstrates how to create and access a virtual machine
within VirtualBox and VMware ESXi environments.


.. _VirtualBox:

VirtualBox
~~~~~~~~~~

`VirtualBox <https://www.virtualbox.org/>`__
is an open source virtualization program originally created by Sun
Microsystems. VirtualBox runs on Windows, BSD, Linux, Macintosh, and
OpenSolaris. It can be configured to use a downloaded FreeNAS\ :sup:`®`
:file:`.iso` file, and makes a good testing environment for practicing
configurations or learning how to use the features provided by
FreeNAS\ :sup:`®`.

To create the virtual machine, start VirtualBox and click the
:guilabel:`New` button, shown in
:numref:`Figure %s <vb_initial_fig>`,
to start the new virtual machine wizard.


.. _vb_initial_fig:

.. figure:: images/virtual/virtualbox.png

   Initial VirtualBox Screen


Click the :guilabel:`Next` button to see the screen in
:numref:`Figure %s <vb_nameos_fig>`.
Enter a name for the virtual machine, click the
:guilabel:`Operating System` drop-down menu and select BSD, and select
:guilabel:`FreeBSD (64-bit)` from the :guilabel:`Version` dropdown.


.. _vb_nameos_fig:

.. figure:: images/virtual/virtualbox-create-name-os.png

   Enter Name and Operating System for the New Virtual Machine


Click :guilabel:`Next` to see the screen in
:numref:`Figure %s <vb_mem_fig>`.
The base memory size must be changed to **at least 8192 MiB**. When
finished, click :guilabel:`Next` to see the screen in
:numref:`Figure %s <vb_hd_fig>`.


.. _vb_mem_fig:

.. figure:: images/virtual/virtualbox-create-memory.png

   Select the Amount of Memory Reserved for the Virtual Machine


.. _vb_hd_fig:

.. figure:: images/virtual/virtualbox-create-hard-drive.png

   Select Existing or Create a New Virtual Hard Drive


Click :guilabel:`Create` to launch the
:guilabel:`Create Virtual Hard Drive Wizard` shown in
:numref:`Figure %s <vb_virt_drive_fig>`.


.. _vb_virt_drive_fig:

.. figure:: images/virtual/virtualbox-create-hard-drive-file-type.png

   Create New Virtual Hard Drive Wizard


Select :guilabel:`VDI` and click the :guilabel:`Next` button to see
the screen in
:numref:`Figure %s <vb_virt_type_fig>`.


.. _vb_virt_type_fig:

.. figure:: images/virtual/virtualbox-create-storage-type.png

   Select Storage Type for Virtual Disk


Choose either :guilabel:`Dynamically allocated` or
:guilabel:`Fixed-size` storage. The first option uses disk space as
needed until it reaches the maximum size that is set in the next
screen. The second option creates a disk the full amount of disk
space, whether it is used or not. Choose the first option to conserve
disk space; otherwise, choose the second option, as it allows
VirtualBox to run slightly faster. After selecting :guilabel:`Next`,
the screen in
:numref:`Figure %s <vb_virt_filename_fig>`
is shown.


.. _vb_virt_filename_fig:

.. figure:: images/virtual/virtualbox-create-disk-filename-size.png

   Select File Name and Size of Virtual Disk


This screen is used to set the size (or upper limit) of the virtual
disk. **Set the default size to a minimum of 8 GiB**. Use the folder
icon to browse to a directory on disk with sufficient space to hold the
virtual disk files.  Remember that there will be a system disk of
at least 8 GiB and at least one data storage disk of at least 4 GiB.

Use the :guilabel:`Back` button to return to a previous screen if any
values need to be modified. After making a selection and pressing
:guilabel:`Create`, the new VM is created. The new virtual machine is
listed in the left frame, as shown in the example in
:numref:`Figure %s <vb_new_vm_fig>`. Open the :guilabel:`Machine Tools`
drop-down menu and select :guilabel:`Details` to see extra information
about the VM.

.. _vb_new_vm_fig:

.. figure:: images/virtual/virtualbox-new-vm.png

   The New Virtual Machine


Create the virtual disks to be used for storage. Highlight the VM and
click :guilabel:`Settings` to open the menu. Click the
:guilabel:`Storage` option in the left frame to access the storage
screen seen in
:numref:`Figure %s <vb_storage_settings_fig>`.

.. _vb_storage_settings_fig:

.. figure:: images/virtual/virtualbox-vm-settings-storage.png

   Storage Settings of the Virtual Machine


Click the :guilabel:`Add Attachment` button, select
:guilabel:`Add Hard Disk` from the pop-up menu, then click the
:guilabel:`Create new disk` button. This launches the
:guilabel:`Create Virtual Hard Disk` wizard seen in
:numref:`Figure %s <vb_virt_drive_fig>` and
:numref:`%s <vb_virt_type_fig>`.

Create a disk large enough to hold the desired  data. The minimum
size is **4 GiB.**
To practice with RAID configurations, create as many virtual disks as
needed. Two disks can be created on each IDE controller. For
additional disks, click the :guilabel:`Add Controller` button to
create another controller for attaching additional disks.

Create a device for the installation media. Highlight the word
"Empty", then click the :guilabel:`CD` icon as shown in
:numref:`Figure %s <vb_config_iso_fig>`.

.. _vb_config_iso_fig:

.. figure:: images/virtual/virtualbox-vm-settings-storage-add-iso.png

   Configuring ISO Installation Media


Click :guilabel:`Choose Virtual Optical Disk File...` to browse to
the location of the :file:`.iso` file. If the :file:`.iso` was burned
to CD, select the detected :guilabel:`Host Drive`.

Depending on the extensions available in the host CPU, it might not be
possible to boot the VM from an :file:`.iso`. If
"your CPU does not support long mode" is shown when trying to boot
the :file:`.iso`, the host CPU either does not have the required
extension or AMD-V/VT-x is disabled in the system BIOS.

.. note:: If there is a kernel panic when booting into the ISO,
   stop the virtual machine. Then, go to :guilabel:`System` and check
   the box :guilabel:`Enable IO APIC`.


To configure the network adapter, go to
:menuselection:`Settings --> Network --> Adapter 1`.
In the :guilabel:`Attached to` drop-down menu select
:guilabel:`Bridged Adapter`, then choose the name of the physical
interface from the :guilabel:`Name` drop-down menu. In the example
shown in
:numref:`Figure %s <vb_bridged_fig>`,
the Intel Pro/1000 Ethernet card is attached to the network and has a
device name of *em0*.

.. _vb_bridged_fig:

.. figure:: images/virtual/virtualbox-vm-settings-network-bridged.png

   Configuring a Bridged Adapter in VirtualBox


After configuration is complete, click the :guilabel:`Start` arrow and
install FreeNAS\ :sup:`®` as described in :ref:`Performing the Installation`.
After FreeNAS\ :sup:`®` is installed, press :kbd:`F12` when the VM starts to
boot to access the boot menu. Select the primary hard disk as the boot
option. You can permanently boot from disk by removing the
:guilabel:`Optical` device in :guilabel:`Storage` or by unchecking
:guilabel:`Optical` in the :guilabel:`Boot Order` section of
:guilabel:`System`.


.. _VMware ESXi:

VMware ESXi
~~~~~~~~~~~

ESXi is a bare-metal hypervisor architecture created by VMware Inc.
Commercial and free versions of the VMware vSphere Hypervisor
operating system (ESXi) are available from the
`VMware website
<https://www.vmware.com/products/esxi-and-esx.html>`__.

Install and use the VMware vSphere client to connect to the
ESXi server. Enter the username and password created when installing
ESXi to log in to the interface. After logging in, go to *Storage* to
upload the FreeNAS\ :sup:`®` :file:`.iso`.
Click :guilabel:`Datastore browser` and select a datastore for the
FreeNAS\ :sup:`®` :file:`.iso`. Click :guilabel:`Upload` and choose
the FreeNAS\ :sup:`®` :file:`.iso` from the host system.


Click :guilabel:`Create / Register VM` to create a new VM. The *New
virtual machine* wizard opens:

#. **Select creation type**: Select
   :literal:`Create a new virtual machine` and click :guilabel:`Next`.

   .. _esxi creation type:

   .. figure:: images/virtual/esxi_create_type.png

#. **Select a name and guest OS**: Enter a name for the VM. Leave ESXi
   compatibility version at the default. Select :literal:`Other` as the
   Guest OS family. Select
   :literal:`FreeBSD12 or later versions (64-bit)` as the Guest OS
   version. Click :guilabel:`Next`.

   .. _exsi name and guest OS:

   .. figure:: images/virtual/esxi_name_os.png

#. **Select storage**: Select a datastore for the VM. The datastore
   must be at least 32 GiB.

   .. _esxi select storage:

   .. figure:: images/virtual/esxi_select_storage.png

#. **Customize settings**: Enter the recommended minimums of at least
   *8 GiB* of memory and *32 GiB* of storage. Select
   :literal:`Datastore ISO file` from the :guilabel:`CD/DVD Drive 1`
   drop-down. Use the Datastore browser to select the uploaded FreeNAS\ :sup:`®`
   :file:`.iso`. Click :guilabel:`Next`.

   .. _esxi customize settings:

   .. figure:: images/virtual/esxi_custom_settings.png

#. **Ready to complete**: Review the VM settings. Click
   :guilabel:`Finish` to create the new VM.

   .. _esxi ready to complete:

   .. figure:: images/virtual/esxi_ready_complete.png

To add more disks to a VM, right-click the VM and click
:guilabel:`Edit Settings`.


Click
:menuselection:`Add hard disk --> New standard hard disk`.
Enter the desired capacity and click :guilabel:`Save`.


.. _esxi_add_disk:

.. figure:: images/virtual/esxi-add-disk.png

   Adding a Storage Disk

Virtual HPET hardware can prevent the virtual machine from booting on
some older versions of VMware. If the virtual machine does not boot,
remove the virtual HPET hardware:

* On ESXi, right-click the VM and click
  :guilabel:`Edit Settings`. Click
  :menuselection:`VM Options --> Advanced --> Edit Configuration...`.
  Change :guilabel:`hpet0.present` from *TRUE* to *FALSE* and click
  :guilabel:`OK`. Click :guilabel:`Save` to save the new settings.

* On Workstation or Player, while in :guilabel:`Edit Settings`,
  click :menuselection:`Options --> Advanced --> File Locations`.
  Locate the path for the Configuration file named
  :file:`filename.vmx`. Open the file in a text editor and change
  :guilabel:`hpet0.present` from *true* to *false*, then save the
  change.
