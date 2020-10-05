---
author:
- iXsystems
date: 'Oct 05, 2020'
title: 'FreeNAS® 11.3-U5 User Guide'
---

\[\]

<span>note</span><span>Note:</span> Starting with version 12.0, <span>
</span> (https://www.ixsystems.com/blog/freenastruenasunification/.)
into “TrueNAS”. Documentation for TrueNAS 12.0 and later releases has
been unified and moved to the <span> </span>
(https://www.truenas.com/docs/).

FreeNAS$^{\text{®}}$ is © 20112020 iXsystems

FreeNAS$^{\text{®}}$ and the FreeNAS$^{\text{®}}$ logo are registered
trademarks of iXsystems

FreeBSD$^{\text{®}}$ is a registered trademark of the FreeBSD Foundation

Written by users of the FreeNAS$^{\text{®}}$ networkattached storage
operating system.

Version 11.3

Copyright © 20112020 <span> </span> (https://www.ixsystems.com/)

Welcome {#welcome .unnumbered}
=======

This Guide covers the installation and use of FreeNAS$^{\text{®}}$ 11.3.

The FreeNAS$^{\text{®}}$ User Guide is a work in progress and relies on
the contributions of many individuals. If you are interested in helping
us to improve the Guide, read the instructions in the <span> </span>
(https://github.com/freenas/freenasdocs/blob/master/README.md). IRC
Freenode users are welcome to join the channel where you will find other
FreeNAS$^{\text{®}}$ users.

The FreeNAS$^{\text{®}}$ User Guide is freely available for sharing and
redistribution under the terms of the <span> </span>
(https://creativecommons.org/licenses/by/3.0/). This means that you have
permission to copy, distribute, translate, and adapt the work as long as
you attribute iXsystems as the original source of the Guide.

FreeNAS$^{\text{®}}$ and the FreeNAS$^{\text{®}}$ logo are registered
trademarks of iXsystems.

Active Directory$^{\text{®}}$ is a registered trademark or trademark of
Microsoft Corporation in the United States and/or other countries.

Apple, Mac and Mac OS are trademarks of Apple Inc., registered in the
U.S. and other countries.

Asigra Inc. Asigra, the Asigra logo, Asigra Cloud Backup, Recovery is
Everything, Recovery Tracker and AttackLoop are trademarks of Asigra
Inc.

Broadcom is a trademark of Broadcom Corporation.

Chelsio$^{\text{®}}$ is a registered trademark of Chelsio
Communications.

Cisco$^{\text{®}}$ is a registered trademark or trademark of Cisco
Systems, Inc. and/or its affiliates in the United States and certain
other countries.

Django$^{\text{®}}$ is a registered trademark of Django Software
Foundation.

Facebook$^{\text{®}}$ is a registered trademark of Facebook Inc.

FreeBSD$^{\text{®}}$ and the FreeBSD$^{\text{®}}$ logo are registered
trademarks of the FreeBSD Foundation$^{\text{®}}$.

Intel, the Intel logo, Pentium Inside, and Pentium are trademarks of
Intel Corporation in the U.S. and/or other countries.

LinkedIn$^{\text{®}}$ is a registered trademark of LinkedIn Corporation.

Linux$^{\text{®}}$ is a registered trademark of Linus Torvalds.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates.

Twitter is a trademark of Twitter, Inc. in the United States and other
countries.

UNIX$^{\text{®}}$ is a registered trademark of The Open Group.

VirtualBox$^{\text{®}}$ is a registered trademark of Oracle.

VMware$^{\text{®}}$ is a registered trademark of VMware, Inc.

Wikipedia$^{\text{®}}$ is a registered trademark of the Wikimedia
Foundation, Inc., a nonprofit organization.

Windows$^{\text{®}}$ is a registered trademark of Microsoft Corporation
in the United States and other countries.

Typographic Conventions {#typographic-conventions .unnumbered}
=======================

The FreeNAS$^{\text{®}}$ 11.3 User Guide uses these typographic
conventions:

<span>|&gt;p<span>0.50-2</span> |&gt;p<span>0.50-2</span>|</span>

\[\]\
Item & Visual Example\

\
Item & Visual Example\

\

Graphical elements: buttons, icons, fields, columns, and boxes & Click
the button.\
Menu selections & Select .\
Commands & Use the command.\
File names and pool and dataset names & Locate the file.\
Keyboard keys & Press the key.\
Important points &\
Values entered into fields, or device names & Enter in the address
field.\

<span>|&gt;p<span>0.35-2</span> |&gt;p<span>0.65-2</span>|</span>

\[\]\
Icon & Usage\

\
Icon & Usage\

\

& Add a new item.\
(Settings) & Show a settings menu.\
(Options) & Show an Options menu.\
(Browse) & Shows an expandable view of system directories.\
(Power) & Show a power options menu.\
(Show) & Reveal characters in a password field.\
(Hide) & Hide characters in a password field.\
(Configure) & Edit settings.\
(Launch) & Launch a service.\
(Start) & Start jails.\
(Stop) & Stop jails.\
(Update) & Update jails.\
(Delete) & Delete jails.\
(Encryption Options) & Encryption options for a pool.\
(Pin) & Pin a help box to the screen.\
(Close) & Close a help box.\

Introduction {#\detokenize{intro:introduction}}
============

\[\]\[\]

FreeNAS$^{\text{®}}$ is an embedded open source networkattached storage
(NAS) operating system based on FreeBSD and released under a <span>
</span> (https://opensource.org/licenses/BSD2Clause). A NAS has an
operating system optimized for file storage and sharing.

FreeNAS$^{\text{®}}$ provides a browserbased, graphical configuration
interface. The builtin networking protocols provide storage access to
multiple operating systems. A plugin system is provided for extending
the builtin features by installing additional software.

New Features in 11.3 {#\detokenize{intro:new-features-in-release}}
--------------------

\[\] FreeNAS$^{\text{®}}$ 11.3 is a feature release, which includes new
significant features, many improvements and bug fixes to existing
features, and version updates to the operating system, base
applications, and drivers. Users are encouraged to () to this release in
order to take advantage of these improvements and bug fixes.

The replication framework has been redesigned, adding new backend
systems, files, and screen options to the () and (). The redesign adds
these features:

-   New peers/credentials API for creating and managing credentials.
    The () and () screens have been added and a wizard makes it easy to
    generate new keypairs. Existing SFTP and SSH replication keys
    created in 11.2 or earlier will be automatically added as entries
    to () during upgrade.

-   New transport API adds netcat support, for greatly improved speed
    of transfer.

-   Snapshot creation has been decoupled from replication tasks,
    allowing replication of manually created snapshots.

-   The ability to use custom names for snapshots.

-   Configurable snapshot retention on the remote side.

-   A new replication wizard makes it easy to configure replication
    scenarios, including local replication and replication to systems
    running legacy replication (pre11.3).

-   Replication is resumable and failed replication tasks will
    automatically try to resume from a previous checkpoint. Each task
    has its own log which can be accessed from the column.

-   Replications run in parallel as long as they do not conflict with
    each other. Completion time depends on the number and size of
    snapshots and the bandwidth available between the source and
    destination computers.

() has been redesigned to streamline management of both physical and
virtual interfaces using one screen. VLANs and LAGGs are now classified
as interface types and support for the () type has been added. The
addressing details for all physical interfaces, including DHCP, are now
displayed but are readonly if the interface is a member of a LAGG. When
applying interface changes, the web interface provides a window to
cancel the change and revert to the previous network configuration. A
new MTU field makes it easier to set the MTU as it no longer has to be
typed in as an Auxiliary Parameter.

<span> </span>
(https://ietfwgacme.github.io/acme/draftietfacmeacme.html) support has
been added. ACME simplifies the process of issuing and renewing
certificates using a set of DNS challenges to verify a user is the owner
of the domain. While the new API supports the addition of multiple DNS
authenticators, support for <span> </span>
(https://aws.amazon.com/route53/) has been added as the initial
implementation. The () screen is used for authenticator configuration
which adds the () option for Certificate Signing Requests. Once
configured, FreeNAS$^{\text{®}}$ will automatically renew ACME
certificates as they expire.

Support for collecting daily anonymous usage statistics has been added.
Collected nonidentifying data includes hardware information such as CPU
type, number and size of disks, and configured NIC types as well as an
indication of which services, types of shares, and Plugins are
configured. The collected data will assist in determining where to best
focus engineering and testing efforts. Collection is enabled by default.
To optout, unset

The () system has been improved:

-   Support for oneshot critical alerts has been added. These alerts
    remain active until dismissed by the user.

-   () has been reorganized: alerts are grouped functionally rather than
    alphabetically and peralert severity and alert thresholds
    are configurable.

-   Periodic alert scripts have been replaced by the () framework.
    Periodic alert emails are disabled by default and previous email
    alert conditions have been added to the FreeNAS$^{\text{®}}$
    alert system. Email or other alert methods can be configured in ().

A () in the top menu bar displays the status and progress of configured
tasks.

The Dashboard has been rewritten to provide an overview of the current
state of the system rather than repeat the historical data found in ().
It now uses middleware to handle data collection and provide the web
interface with realtime events. Line charts have been replaced with
meters and gauges. CPU graphs have been consolidated into a single
widget which provides average usage and perthread statistics for both
temperature and usage. Interfaces are represented as a separate card per
physical NIC unless they are part of a LAGG card. Pool and Interface
widgets feature mobileinspired lateral navigation, allowing users to
“drill down” into the data without leaving the page.

() has been greatly improved. Data is now prepared on the backend by the
middleware and operating system. Any remaining data manipulation is done
in a web worker, keeping expensive processing off of the main UI
thread/context. The SVGbased charting library was replaced with a
GPUaccelerated canvasbased library. Virtual scroll and lazy loading
prevent overloading the browser and eliminate the need for a pager.
Users can zoom by X or Y axis and reset the zoom level with a double
click. Graphs do not display if there is no related data. Support for
UPS and NFS statistics has been added.

Options for configuring the reporting database have been moved to . This
screen adds the ability to configure as well as the number of points for
each hourly, daily, weekly, monthly, or yearly graph (). The location of
the reporting database defaults to tmpfs and a configurable alert if the
database exceeds 1 GiB has been added to ().

The web interface has received many improvements and bug fixes.
Usability enhancements include: ability to move, pin, and copy help
text, persistent layout customizations, customizable column views, size
units which accept humanized input, improved caching and browser
support, and improved error messages, popup dialogs, and help text. An
iX Official theme has been added which is the default for new
installations.

NAT support has been added as the default for most (). With NAT, a
plugin is contained in its own network and does not require any
knowledge of the physical network to work properly. This removes the
need to manually configure IP addresses or have a DHCP server running.
When installing a plugin into a virtualized environment, NAT removes the
requirement to enable Promiscuous Mode for the network.

The () page has been streamlined so that most operations can be
performed without having to go to the () page. Support for collections
has been added to differentiate between iXsystems plugins, which receive
updates every few weeks, and Community plugins. In addition, there have
been many bug fixes and improvements to iocage, the Plugins backend,
resulting in a much better Plugins user experience.

An () has been added to (Options) and the () has been redesigned.

A new iSCSI wizard in () makes it easy to configure iSCSI shares.

There have been several () improvements. The labels and tooltips for
encryption operations are clearer. Disk type, rotation rate, and
manufacturer information makes it easier to differentiate between
selectable disks when creating a pool. A button makes it easy to create
large pools using the same vdev layout, such as a series of striped
mirrors.

Significant improvements to <span> </span>
(https://jira.ixsystems.com/browse/NAS102108) include ZFS user quotas
support, web service discovery support, and improved directory listing
performance for newlycreated shares.

The middleware and websockets APIv2 rewrite is complete. APIv1 remains
for backwards compatibility but will be deprecated and no longer
available in the next major release.

-   The legacy web interface has been removed and no longer appears as
    an option in the ().

-   Warden has been removed along with all CLI and web interface support
    for warden jails or plugins installed using FreeNAS$^{\text{®}}$
    11.1 or earlier.

-   Hipchat has been removed from () as it has been <span> </span>
    (https://www.atlassian.com/partnerships/slack). The web interface
    can still be used to delete an existing Hipchat configuration.

-   has been removed from ().

-   has been removed from () due to a longstanding upstream memory leak.
    <span> </span> (https://www.ixsystems.com/truecommand/) provides
    similar reporting plus advanced management capabilities for single
    or multiple FreeNAS$^{\text{®}}$ systems and is free to use to
    manage up to 50 drives.

-   The builtin Docker template has been removed from (). Instructions
    for manually installing Docker can be found in ().

<!-- -->

-   The FreeBSD operating system has been patched up to <span> </span>
    (https://www.freebsd.org/security/advisories/FreeBSDEN19:18.tzdata.asc)
    and <span>
    </span> (https://security.freebsd.org/advisories/FreeBSDSA19:26.mcu.asc).

-   OS support for reporting the CPU temperature of AMD Family 15h,
    Model &gt;=60h has been added.

-   QLogic 10 Gigabit Ethernet driver support has been added with <span>
    </span> (https://www.freebsd.org/cgi/man.cgi?query=qlxgbe).

-   The base FreeBSD ports have been updated to their latest versions as
    of September 24, 2019.

-   Python has been updated to version <span> </span>
    (https://www.python.org/downloads/release/python375/) to address
    <span> </span> (https://nvd.nist.gov/vuln/detail/CVE201915903).

-   Angular has been updated to version <span>
    </span> (https://github.com/angular/angular/blob/master/CHANGELOG.md).

-   Samba has been updated to version <span>
    </span> (https://www.samba.org/samba/history/samba4.10.10.html).

-   Netatalk has been updated to version <span>
    </span> (http://netatalk.sourceforge.net/3.1/ReleaseNotes3.1.12.html).

-   Rclone has been updated to version <span>
    </span> (https://rclone.org/changelog/\#v149420190929).

-   collectd has been updated to version <span>
    </span> (https://collectd.org/wiki/index.php/Version\_5.8).

-   sudo has been updated to version 1.8.29 to address <span>
    </span> (https://nvd.nist.gov/vuln/detail/CVE201914287).

-   <span> </span> (http://p7zip.sourceforge.net/) has been added.

-   The <span> </span> (https://github.com/freenas/zettarepl)
    replication tool has been added.

<!-- -->

-   The and set in () are shown under the iXsystems logo at the top left
    of the web interface.

-   The web interface now indicates when a ().

-   () has been added to the top toolbar row.

-   The has been removed from the top navigation bar. The theme is now
    selected in ().

-   The redundant entry has been removed from the gear icon of the top
    navigation bar.

-   , , and have been removed from ().

-   has been added to ().

-   Rightclick help dialog has been added to the ().

<!-- -->

-   The , , , and fields have been added to and the field has been
    removed from ().

-   The and fields in the () system options have been updated to allow
    selecting multiple IP addresses.

-   The field can now be sorted by or .

-   An option has been added to the ().

-   has been renamed to (). and information about the operating system
    device have been moved to .

-   has been removed from the () system options because periodic script
    notifications have been replaced by alerts.

-   has been removed from the () system options.

-   Setting in the () system options provides a button to show console
    messages on busy spinner dialogs.

-   and have been moved to ().

-   has been added to ().

-   has moved from () to .

-   has been added and the button removed from the () options.

-   has been added to the ().

-   SNMP Trap has been added to ().

-   , , , , and have been added to (). has been changed to .

-   and have been removed from the , , , , and providers in the ()
    options.

-   has been added to the () options.

-   has been added to the () options.

-   has been changed to in the ().

-   has been changed to in ().

-   has been renamed to in ().

-   <span> </span>
    (https://en.wikipedia.org/wiki/Ellipticcurve\_cryptography) key
    support has been added to the options for () and ().

-   has been added to the () and () options.

-   has been added to the () options.

<!-- -->

-   The () has been added to the column for created ().

-   has been added to the ().

-   The log entries for individual () can be displayed and downloaded by
    clicking the of the task.

-   The FreeBSD () criteria have been applied to the field in ().

-   has been added to the ().

-   , , and have been added to the ().

-   can be specifed in ().

-   The replication log has been moved to . The log entries for
    individual () can be displayed and downloaded by clicking the of
    the task.

-   A column has been added to ().

-   , , and have been added to the ().

-   has been renamed to in the () and accepts various size units like
    and .

-   in (). only appears when is chosen for type.

-   , , , , , , , , , , and have been added to the ().

-   The log entries for individual () can be displayed and downloaded by
    clicking the of the task.

<!-- -->

-   The field has been renamed to and the and fields have been added
    to ().

<!-- -->

-   Disk type, rotation rate, and manufacturer information can be viewed
    on the () page and when ().

-   The () dialog shows system services that are affected by the
    export action.

-   The dataset () has been redesigned. The , , , and fields have been
    removed and has been added.

-   has been added to the ().

-   A dataset deletion confirmation dialog with a force delete option
    has been added to the ().

-   displays when the pool has an active scrub in ().

-   has been added to the () options.

-   , , , and fields have been added to ().

-   and options have been added to ().

-   The option behavior in () has been updated to select the detected
    filesystem of the chosen disk. After importing a disk, a dialog
    allows viewing or downloading the disk import log.

-   () shows () when a dataset reaches a certain percent of the quota.

<!-- -->

-   has been added and the , , , , , and fields have been removed
    from ().

-   dynamically appears in () when the FreeNAS$^{\text{®}}$ system is
    joined to an Active Directory domain.

-   and have been removed from the ().

-   has been added to () and () configuration options.

-   The checkbox has been added and the , , , , , , and fields have been
    removed from ().

-   The in () supports multiple hostnames as a failover priority list.

<!-- -->

-   has been added to the (). has been removed from () as permissions
    are now configured using ().

-   The , , , , , , , , , , , , , , , , , , , , , , , , and () have been
    removed and the VFS object has been added.

-   has been renamed to for () Portals, Initiators, and Extents.

<!-- -->

-   has been removed from the (). S.M.A.R.T. alerts are configured as
    part of an (). Note that email addresses previously configured to
    receive S.M.A.R.T. alerts now receive all FreeNAS$^{\text{®}}$ ().

-   , , , , , and have been removed from the ().

-   , , and have been removed from the (). These options are now
    dynamically enabled.

-   has been added to the ().

-   has been added to the (), search functionality has been added to ,
    and USB port detection has been added to the .

-   UPS events now generate ().

-   <span> </span> (http://networkupstools.org/) (Network UPS Tools) now
    listens on (IPv6 localhost) in addition to 127.0.0.1
    (IPv4 localhost).

<!-- -->

-   Grub boot loader support has been added for virtual machines that
    will not boot with other loaders.

-   and have been added to the (). The Wizard now displays system memory
    and has been added to the first step of the Wizard.

-   An optional, custom name can be specifed when ().

-   Log files for each VM are stored in . Log files have the same name
    as the VM.

<!-- -->

-   , , and have been added to ().

-   () can now be created from the web interface.

-   , , , , , and options have been added in ().

-   and the associated , , and fields have been added to the section
    of ().

-   and in () have been renamed to and .

-   Log files for jail status and command output are stored in .

### U1 {#\detokenize{intro:u1}}

U1 is the first maintenance release to 11.3RELEASE, including nearly one
hundred bug fixes and other improvements. For a detailed change list,
see the completed tickets in the <span>
ttps://jira.ixsystems.com/issues/?jql=project%20%3D%20NAS%20AND%20resolution%20in%20(Complete%2C%20Done)%20AND%20fixVersion%20%3D%2011.3-U1</span><span>FreeNAS/TrueNAS
Jira Project</span>
(https://jira.ixsystems.com/issues/?jql=project%20%3D%20NAS%20AND%20resolution%20in%20(Complete%2C%20Done)%20AND%20fixVersion%20%3D%2011.3U1).

### U2 {#\detokenize{intro:u2}}

This release nearly includes a combined 150 bug fixes, updates, and
improvements. Some highlights of this version include:

-   An update to Samba, version 4.10.13 (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105349))

-   Bug fix when importing a pool (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105297))

-   Fix for a middleware memory leak (<span>
    </span> (https://jira.ixsystems.com/browse/NAS104437))

-   Mitigation for specific LSI 9X00 cards (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105568))

For a complete, detailed list of updates, see the list of <span>
ttps://jira.ixsystems.com/issues/?filter=-4&jql=fixVersion%20IN%20(11303)</span><span>FreeNAS
11.3U2 Jira tickets</span>
(https://jira.ixsystems.com/issues/?filter=4&jql=fixVersion%20IN%20(11303)).

The 11.3U2.1 release is a hotfix that only addresses a critical issue
when exporting and destroying pools (<span> </span>
(https://jira.ixsystems.com/browse/NAS105782)).

### U3 {#\detokenize{intro:u3}}

FreeNAS 11.3U3 is a maintenance release that includes over one hundred
bug fixes and quality of life improvements for the software. Notable
fixes include:

-   Network Interfaces section updates (<span> </span>
    (https://jira.ixsystems.com/browse/NAS105964), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105963), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105960), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105959), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105958), <span>
    </span> (https://jira.ixsystems.com/browse/NAS105965))

-   Allow mounting NFS shares with either Kerberos or default security
    when is disabled. (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105956))

-   Import a Samba 4 patch for an Apple Time Machine bug (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105911))

-   UI visual improvements (<span> </span>
    (https://jira.ixsystems.com/browse/NAS105909), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105916), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105927), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105907), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105862), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105800), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105713), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105661), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105601), <span>
    </span> (https://jira.ixsystems.com/browse/NAS105513))

-   Improve Active Directory autorejoin (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105853))

-   Merge FreeBSD patches and update FreeNAS Kernel to 11.3RELEASEp8
    (<span> </span> (https://jira.ixsystems.com/browse/NAS105837))

-   Improvements to the alert system (<span> </span>
    (https://jira.ixsystems.com/browse/NAS105785), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105792), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105833), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105876), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105715), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105684), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105664), <span>
    </span> (https://jira.ixsystems.com/browse/NAS105660))

-   Make fstab handling for Jail mount points more robust (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105735))

-   Temperature reporting fallback for drives on a SCSI HBA (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105656))

-   SMB sharing improvements (<span> </span>
    (https://jira.ixsystems.com/browse/NAS105395), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105782), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105443), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105445), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105951), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105578), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105703), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105833), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105835), <span> </span>
    (https://jira.ixsystems.com/browse/NAS105911), <span> </span>
    (https://jira.ixsystems.com/browse/NAS106049), <span>
    </span> (https://jira.ixsystems.com/browse/NAS106047))

The <span>
ttps://jira.ixsystems.com/issues/?filter=-4&jql=fixVersion%20IN%20(11901)</span><span>Jira
FreeNAS 11.3U3</span>
(https://jira.ixsystems.com/issues/?filter=4&jql=fixVersion%20IN%20(11901))
issue tracker has a full list of changes included in this release.

<span>note</span><span>Note:</span> There is a current issue where the
UI can become unresponsive after upgrading. If this occurs, clear the
site data and refresh the page.

### U4 {#\detokenize{intro:u4}}

FreeNAS 11.3U4 is another maintenance release of FreeNAS 11.3 that has
over one hundred and thirty bug fixes to the FreeNAS middleware and user
interface, including:

-   Updating Samba to 4.10.16 (<span>
    </span> (https://jira.ixsystems.com/browse/NAS106500))

-   Merging FreeBSD Security Advisory SA20:17 (<span>
    </span> (https://jira.ixsystems.com/browse/NAS106415))

-   Using a Google Team Drive with Cloud Sync Tasks (<span>
    </span> (https://jira.ixsystems.com/browse/NAS106195))

-   Unlocking SelfEncrypting Drives (SEDs) (<span>
    </span> (https://jira.ixsystems.com/browse/NAS106004))

-   Cloud sync to Backblaze B2 (<span>
    </span> (https://jira.ixsystems.com/browse/NAS106541))

-   Recursive Replication (<span>
    </span> (https://jira.ixsystems.com/browse/NAS106435))

-   OAuth client ID and Secret for Google Drive and Onedrive (<span>
    </span> (https://jira.ixsystems.com/browse/NAS106407))

-   Deleting expired snapshots (<span>
    </span> (https://jira.ixsystems.com/browse/NAS105966))

For full release notes for FreeNAS 11.3U4, see .

### U5 {#\detokenize{intro:u5}}

iXsystems is pleased to announce the general availability of the fifth
update to FreeNAS version 11.3! 11.3U5 is a maintenance release that has
over 100 bug fixes to the Middleware and Web Interface. This is now the
most stable and performant release of FreeNAS 11.3, and users are
encouraged to update immediately! Here is the full changelog for FreeNAS
11.3U5:

<span>|l|l|l|</span> Key & Summary & Component/s\

\
Key & Summary & Component/s\

\

<span> </span> (https://jira.ixsystems.com/browse/NAS107603) &
Replication that worked in 11.3U4 and 12.0Beta2 fails in 12.0RC1 &
Replication\
<span> </span> (https://jira.ixsystems.com/browse/NAS107544) & SMART and
scrub tasks are not running & Tasks\
<span> </span> (https://jira.ixsystems.com/browse/NAS107533) & Unable to
remove certificate in s3 service & Certificates\
<span> </span> (https://jira.ixsystems.com/browse/NAS107531) & Comment
and restrict change of large blocks support in replication &
Replication\
<span> </span> (https://jira.ixsystems.com/browse/NAS107506) &
Additional Domains don’t show up on save & Middleware, WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS107468) & Cloud
sync to Wasabi fails with “Can’t mix absolute and relative paths” &
Tasks\
<span> </span> (https://jira.ixsystems.com/browse/NAS107411) & No Task
Manager Progress is shown & Replication\
<span> </span> (https://jira.ixsystems.com/browse/NAS107316) & UPS
Settings Saving Bug & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS107315) &
middlewared memory leak & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS107314) &
Replicated dataset is not set to readonly & Replication\
<span> </span> (https://jira.ixsystems.com/browse/NAS107292) & Unable to
Delete Expired ACME Certificate & Certificates\
<span> </span> (https://jira.ixsystems.com/browse/NAS107235) & Error
when updating a Jail 11.3RELEASEp6 to 11.3RELEASEp612 & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS107160) & Apparent
crash on delete of share to invalid directory & Sharing\
<span> </span> (https://jira.ixsystems.com/browse/NAS107148) & Generate
a random default serial extent & Sharing\
<span> </span> (https://jira.ixsystems.com/browse/NAS107133) & unable to
delete iscsi file extents & Sharing\
<span> </span> (https://jira.ixsystems.com/browse/NAS107128) & When
creating pool, adding vdev, then removing it, leaves debris & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS107121) & and are
being overwritten as empty arrays & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS107120) & change
failover\_vhid to type instead of & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS107116) & allow
editing empty interfaces & Network\
<span> </span> (https://jira.ixsystems.com/browse/NAS107108) & Google
Drive Cloud Sync tasks fail with exportSizeLimitExceeded & Cloud
Credentials\
<span> </span> (https://jira.ixsystems.com/browse/NAS107107) & Clear any
potential stale state after leaving AD domain & Active Directory\
<span> </span> (https://jira.ixsystems.com/browse/NAS107104) & ACME DNS
renewals don’t work & Certificates\
<span> </span> (https://jira.ixsystems.com/browse/NAS107100) & Do not
run check\_available in a tight loop in case an exception happens &
Middlware\
<span> </span> (https://jira.ixsystems.com/browse/NAS107099) & Do not
display previous replication task status after deleting it and… &
Replication\
<span> </span> (https://jira.ixsystems.com/browse/NAS107096) & Custom
sync schedule forgotten when editing task & Tasks\
<span> </span> (https://jira.ixsystems.com/browse/NAS107090) & Merge
FreeBSD SA20:2130 EN20:1718 & Security\
<span> </span> (https://jira.ixsystems.com/browse/NAS107076) & Expand
regression tests for user api &\
<span> </span> (https://jira.ixsystems.com/browse/NAS107074) &
Permissions are incorrect on home directory move & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS107067) & Fix chown
of skel directory contents for new local users & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS107055) & Forums
user reported logs filled with fruit error messages & SMB\
<span> </span> (https://jira.ixsystems.com/browse/NAS107053) & Pool in
dashboard omits special vdevs from count and status & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS107037) & Have ftp
reload method reload proftpd rather than restart it & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS107035) & Swap size
setting not honored on 4k sector disks & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS107032) & Unable to
upload 8TB file to backblaze. & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS107029) & Unable to
configure UPS on TrueNAS 12 & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS107023) & Expand
list of error strings that should trigger an AD rejoin & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106993) & Reassign
sys.{stdout,stderr} after log rollover & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106984) & “jls”
hostname does not reflect modified hostname & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106978) & Add
regression tests for AD machine account keytab generation & Active
Directory\
<span> </span> (https://jira.ixsystems.com/browse/NAS106966) & collectd:
blank warning emails & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106965) &
qBittorrent Plugin Not Installing & Plugins\
<span> </span> (https://jira.ixsystems.com/browse/NAS106948) & Recycle
bin versioning not enabled & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106918) & Replacing
boot usb drive problem & Boot Environments\
<span> </span> (https://jira.ixsystems.com/browse/NAS106866) &
Proper/better errno for failed authentication & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106864) & SED
doesn’t work for nvme & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106854) & plugin
boot checkbox reenables itself & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS106842) & Setting
IPMI to DHCP should grayout IP addresses & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS106840) & setting
invalid VHID value fails silently. & HA, WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS106808) & Ensure
monpwd/monuser fields are provided for UPS service & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS106798) & api
context /services/iscsi/targettoextent does not allow null value for
iscsi\_lunid & API, iSCSI\
<span> </span> (https://jira.ixsystems.com/browse/NAS106797) & Periodic
Snapshot Tasks “Enabled” checkboxes are not unique inputs & Snapshot,
Tasks\
<span> </span> (https://jira.ixsystems.com/browse/NAS106787) & iSCSI
webUI columns COMPLETELY break when edited & iSCSI, WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS106745) & Cloud
Sync Bandwidth Limit Field Validation & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS106713) & Cron job
still runs despite being deactivated and then deleted & Tasks\
<span> </span> (https://jira.ixsystems.com/browse/NAS106690) & Can’t
clear Kerberos Principal from GUI & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS106682) &
Validation Error on creation of Manual SSH Connection for Replication
Task & Replication\
<span> </span> (https://jira.ixsystems.com/browse/NAS106675) & dashboard
is completely blank no widgets & Dashboard\
<span> </span> (https://jira.ixsystems.com/browse/NAS106658) & ZFS
replication does not create datasets on target & Replication, Tasks\
<span> </span> (https://jira.ixsystems.com/browse/NAS106583) & FreeNAS
disks forget their assigned pool & ZFS\
<span> </span> (https://jira.ixsystems.com/browse/NAS106496) & System
crash after middlewared.set\_sysctl():407 Failed to set sysctl &
Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106133) &
Categories for support proxy & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106110) & UPS ups
is on battery power alerts since upgrade to 11.3 & Middleware\
<span> </span> (https://jira.ixsystems.com/browse/NAS106038) &
Replication progress report error & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS105099) & Periodic
Snapshot are missing the lifetime in its name & WebUI\
<span> </span> (https://jira.ixsystems.com/browse/NAS104906) & Rsync
tasks view shows incorrect remote path & Tasks\
<span> </span> (https://jira.ixsystems.com/browse/NAS102808) & Running
Cloud Sync tasks keep on running after deletion in GUI & Cloud
Credentials, Middleware\

Due to numerous improvements in the replication engine and ZFS,
FreeNAS/TrueNAS 11.3 will no longer replicate to FreeNAS/TrueNAS 9.10
systems (or earlier). Solution: update the destination system to
FreeNAS/TrueNAS 11.3 or newer.

\[t\]<span>|T|T|T|</span> Key & Summary & Workaround\
N/A & The web interface can become unresponsive after upgrading. & Clear
the browser cache and refresh the page (Shift + F5).\
<span> </span> (https://jira.ixsystems.com/browse/NAS106882) & Some
plugins are not showing their version. & None: some plugins remain
unversioned and will be moved to the “Community” plugins list for
TrueNAS 12.0 (<span> </span>
(https://jira.ixsystems.com/browse/NAS106610)).\
<span> </span> (https://jira.ixsystems.com/browse/NAS107132) &
Replication from FreeNAS/TrueNAS 11.3 (and newer) to FreeNAS/TrueNAS
9.10 (or earlier) is not functional. & Update the destination system to
FreeNAS/ TrueNAS 11.3 or newer.\

Path and Name Lengths {#\detokenize{intro:path-and-name-lengths}}
---------------------

\[\] Names of files, directories, and devices are subject to some limits
imposed by the FreeBSD operating system. The limits shown here are for
names using plaintext characters that each occupy one byte of space.
Some UTF8 characters take more than a single byte of space, and using
those characters reduces these limits proportionally. System overhead
can also reduce the length of these limits by one or more bytes.

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Type & Maximum Length & Description\

\
Type & Maximum Length & Description\

\

File Paths & 1023 bytes & Total file path length (). The full path
includes directory separator slash characters, subdirectory names, and
the name of the file itself. For example, the path is 42 bytes long.

Using very long file or directory names can be problematic. If a path
with long directory and file names exceeds the 1023byte limit, it
prevents direct access to that file until the directory names or
filename are shortened or the file is moved into a directory with a
shorter total path length.\
File and Directory Names & 255 bytes & Individual directory or file name
length ().\
Mounted Filesystem Paths & 88 bytes & Mounted filesystem path length ().
Longer paths can prevent a device from being mounted.\
Device Filesystem Paths & 63 bytes & <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=devfs) device path lengths
(). Longer paths can prevent a device from being created.\

<span>note</span><span>Note:</span> 88 bytes is equal to 88 ASCII
characters. The number of characters varies when using Unicode.

<span>warning</span><span>Warning:</span> If the mounted path length for
a snapshot exceeds 88 bytes, the data in the snapshot is safe but
inaccessible. When the mounted path length of the snapshot is less than
the 88 byte limit, the data will be accessible again.

The 88 byte limit affects automatic and manual snapshot mounts in
slightly different ways:

-   ZFS temporarily mounts a snapshot whenever a user attempts to view
    or search the files within the snapshot. The mountpoint used will be
    in the hidden directory within the same ZFS dataset. For example,
    the snapshot is mounted at . If the length of this path exceeds 88
    bytes the snapshot will not be automatically mounted by ZFS and the
    snapshot contents will not be visible or searchable. This can be
    resolved by renaming the ZFS pool or dataset containing the snapshot
    to shorter names ( or ), or by shortening the second part of the
    snapshot name (), so that the total mounted path length does not
    exceed 88 bytes. ZFS will automatically perform any necessary
    unmount or remount of the file system as part of the
    rename operation. After renaming, the snapshot data will be visible
    and searchable again.

-   The same example snapshot is mounted manually from the () with . The
    path must not exceed 88 bytes, and the length of the snapshot name
    is irrelevant. When renaming a manual mountpoint, any object mounted
    on the mountpoint must be manually unmounted with the command before
    renaming the mountpoint. It can be remounted afterwards.

<span>note</span><span>Note:</span> A snapshot that cannot be mounted
automatically by ZFS can still be mounted manually from the () with a
shorter mountpoint path. This makes it possible to mount and access
snapshots that cannot be accessed automatically in other ways, such as
from the web interface or from features such as “File History” or
“Versions”.

Using the Web Interface {#\detokenize{intro:using-the-web-ui}}
-----------------------

\[\]

### Tables and Columns {#\detokenize{intro:tables-and-columns}}

Tables show a subset of all available columns. Additional columns can be
shown or hidden with the button. Set a checkmark by the fields to be
shown in the table. Column settings are remembered from session to
session.

The original columns can be restored by clicking in the column list.

Each row in a table can be expanded to show all the information by
clicking the (Expand) button.

### Advanced Scheduler {#\detokenize{intro:advanced-scheduler}}

\[\] When choosing a schedule for different FreeNAS$^{\text{®}}$ (),
clicking opens the custom schedule dialog.

Choosing a preset schedule fills in the rest of the fields. To customize
a schedule, enter <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=crontab&sektion=5) values for
the .

These fields accept standard values. The simplest option is to enter a
single number in the field. The task runs when the time value matches
that number. For example, entering means that the job runs when the time
is ten minutes past the hour.

An asterisk () means “match all values”.

Specific time ranges are set by entering hyphenated number values. For
example, entering in the field sets the task to run at minutes 30, 31,
32, 33, 34, and 35.

Lists of values can also be entered. Enter individual values separated
by a comma (). For example, entering in the field means the task runs at
1:00 AM (0100) and 2:00 PM (1400).

A slash () designates a step value. For example, while entering in means
the task runs every day of the month, means the task runs every other
day.

Combining all these examples together creates a schedule running a task
each minute from 1:301:35 AM and 2:302:35 PM every other day.

There is an option to select which the task will run. Leaving each month
unset is the same as selecting every month.

The schedules the task to run on specific days. This is in addition to
any listed . For example, entering in and setting for creates a schedule
that starts a task on the first day of the month every Wednesday of the
month.

shows when the current schedule settings will cause the task to run.

### Schedule Calendar {#\detokenize{intro:schedule-calendar}}

\[\] The column has a calendar icon (). Clicking this icon opens a
dialog showing scheduled dates and times for the related task to run.

\[\]

() can have a number of set. The configured scrub task continues to
follow the displayed calendar schedule, but it does not run until the
configured number of threshold days have elapsed.

### Changing FreeNAS$^{\text{®}}$ Settings {#\detokenize{intro:changing-freenas-settings}}

It is important to use the web interface or the Console Setup menu for
all configuration changes. FreeNAS$^{\text{®}}$ stores configuration
settings in a database. Commands entered at the command line . This
means that changes made at the command line will be lost after a restart
and overwritten by the values in the settings database.

### Web Interface Troubleshooting {#\detokenize{intro:web-ui-troubleshooting}}

If the web interface is shown but seems unresponsive or incomplete:

-   Make sure the browser allows cookies, Javascript, and custom fonts
    from the FreeNAS$^{\text{®}}$ system.

-   Try a different browser. <span> </span>
    (https://www.mozilla.org/enUS/firefox/all/) is recommended.

If a web browser cannot connect to the FreeNAS$^{\text{®}}$ system by IP
address, DNS hostname, or mDNS name:

-   Check or disable proxy settings in the browser.

-   Verify the network connection by pinging the FreeNAS$^{\text{®}}$
    system by IP address from another computer on the same network. For
    example, if the FreeNAS$^{\text{®}}$ system is at IP address
    192.168.1.19, enter on the command line of the other computer. If
    there is no response, check network configuration.

### Help Text {#\detokenize{intro:help-text}}

\[\] Most fields and settings in the web interface have a (Help Text)
icon. Additional information about the field or setting can be shown by
clicking (Help Text). The help text window can be dragged to any
location, and will remain there until (Close) or (Help Text) is clicked
to close the window.

### Humanized Fields {#\detokenize{intro:humanized-fields}}

\[\] Some numeric value fields accept values. This means that the field
accepts numbers or numbers followed by a unit, like or for megabytes or
or for gigabytes. Entering or are equivalent. Units of KiB, MiB, GiB,
TiB, and PiB are available, and decimal values like are supported when
the field allows them. Some fields have minimum or maximum limits on the
values which can restrict the units available.

### File Browser {#\detokenize{intro:file-browser}}

\[\] Certain sections of the web interface have a built in file browser.
The file browser is used to traverse through directories and choose
datasets on the system. Datasets that have () are tagged so they can be
distinguished from nonACL datasets.

Hardware Recommendations {#\detokenize{intro:hardware-recommendations}}
------------------------

\[\]\[\] FreeNAS$^{\text{®}}$ 11.3 is based on FreeBSD 11.3 and supports
the same hardware found in the <span> </span>
(https://www.freebsd.org/releases/11.3R/hardware.html). Supported
processors are listed in section <span> </span>
(https://www.freebsd.org/releases/11.3R/hardware.html\#proc).
FreeNAS$^{\text{®}}$ is only available for 64bit processors. This
architecture is called by AMD and by Intel.

<span>note</span><span>Note:</span> FreeNAS$^{\text{®}}$ boots from a
GPT partition. This means that the system BIOS must be able to boot
using either the legacy BIOS firmware interface or EFI.

Actual hardware requirements vary depending on the workflow of your
FreeNAS$^{\text{®}}$ system. This section provides some starter
guidelines. The <span> </span>
(https://www.ixsystems.com/community/forums/hardwarediscussion/) has
performance tips from FreeNAS$^{\text{®}}$ users and is a place to post
questions regarding the hardware best suited to meet specific
requirements. <span> </span>
(https://www.ixsystems.com/blog/hardwareguide/) gives indepth
recommendations for every component needed in a FreeNAS$^{\text{®}}$
build. <span> </span>
(https://forums.freenas.org/index.php?threads/buildingburninandtestingyourfreenassystem.17750/)
has detailed instructions on testing new hardware.

<span>note</span><span>Note:</span> The FreeNAS$^{\text{®}}$ team highly
recommends <span> </span>
(https://shop.westerndigital.com/products/internaldrives/wdredprosatahdd\#WD4003FFBX)
disk drives with CMR technology as the preferred storage drives of
FreeNAS$^{\text{®}}$.

### RAM {#\detokenize{intro:ram}}

\[\] The best way to get the most out of a FreeNAS$^{\text{®}}$ system
is to install as much RAM as possible. More RAM allows ZFS to provide
better performance. The <span> </span>
(https://www.ixsystems.com/community/) provide anecdotal evidence from
users on how much performance can be gained by adding more RAM.

General guidelines for RAM:

-   Additional features require additional RAM, and large amounts of
    storage require more RAM for cache. A general recommendation is to
    start with 8 GiB RAM and add 1 GiB RAM for each drive above 8 in
    the system. For example, a system with 10 drives is recommended to
    have at least 10 GiB RAM.

-   To use Active Directory with many users, add an additional 2 GiB of
    RAM for the winbind internal cache.

-   For iSCSI, install at least 16 GiB of RAM if performance is not
    critical, or at least 32 GiB of RAM if good performance is
    a requirement.

-   () are very memoryefficient, but can still use memory that would
    otherwise be available for ZFS. If the system will be running many
    jails, or a few resourceintensive jails, adding 1 to 4 additional
    gigabytes of RAM can be helpful. This memory is shared by the host
    and will be used for ZFS when not being used by jails.

-   () require additional RAM beyond any amounts listed here. Memory
    used by virtual machines is not available to the host while the VM
    is running, and is not included in the amounts described above. For
    example, a system that will be running two VMs that each need 1 GiB
    of RAM requires an additional 2 GiB of RAM.

-   When installing FreeNAS$^{\text{®}}$ on a headless system, disable
    the shared memory settings for the video card in the BIOS.

-   For ZFS deduplication, ensure the system has at least 5 GiB of RAM
    per terabyte of storage to be deduplicated.

If the hardware supports it, install ECC RAM. While more expensive, ECC
RAM is highly recommended as it prevents inflight corruption of data
before the errorcorrecting properties of ZFS come into play, thus
providing consistency for the checksumming and parity calculations
performed by ZFS. If your data is important, use ECC RAM. This <span>
</span>
(http://research.cs.wisc.edu/adsl/Publications/zfscorruptionfast10.pdf)
describes the risks associated with memory corruption.

Do not use FreeNAS$^{\text{®}}$ to store data without at least 8 GiB of
RAM. Many users expect FreeNAS$^{\text{®}}$ to function with less
memory, just at reduced performance. The bottom line is that these
minimums are based on feedback from many users. Requests for help in the
forums or IRC are sometimes ignored when the installed system does not
have at least 8 GiB of RAM because of the abundance of information that
FreeNAS$^{\text{®}}$ may not behave properly with less memory.

### The Operating System Device {#\detokenize{intro:the-operating-system-device}}

\[\] The FreeNAS$^{\text{®}}$ operating system is installed to at least
one device that is separate from the storage disks. The device can be an
SSD, a small hard drive, or a USB stick.

<span>note</span><span>Note:</span> To write the installation file to a
USB stick, USB ports are needed, each with an inserted USB device. One
USB stick contains the installer, while the other USB stick is the
destination for the FreeNAS$^{\text{®}}$ installation. Be careful to
select the correct USB device for the FreeNAS$^{\text{®}}$ installation.
FreeNAS$^{\text{®}}$ cannot be installed onto the same device that
contains the installer. After installation, remove the installer USB
stick. It might also be necessary to adjust the BIOS configuration to
boot from the new FreeNAS$^{\text{®}}$ operating system device.

When determining the type and size of the target device where
FreeNAS$^{\text{®}}$ is to be installed, keep these points in mind:

-   The absolute size is 8 GiB. That does not provide much room. The
    minimum is 16 GiB. This provides room for the operating system and
    several boot environments created by updates. More space provides
    room for more boot environments and 32 GiB or more is preferred.

-   SSDs (Solid State Disks) are fast and reliable, and make very good
    FreeNAS$^{\text{®}}$ operating system devices. Their one
    disadvantage is that they require a disk connection which might be
    needed for storage disks.

    Even a relatively large SSD (120 or 128 GiB) is useful as a
    boot device. While it might appear that the unused space is wasted,
    that space is instead used internally by the SSD for wear leveling.
    This makes the SSD last longer and provides greater reliability.

-   When planning to add your own boot environments, budget about 1 GiB
    of storage per boot environment. Consider deleting older boot
    environments after making sure they are no longer needed. Boot
    environments can be created and deleted using .

-   Use quality, namebrand USB sticks, as ZFS will quickly reveal errors
    on cheap, poorlymade sticks. USB sticks can also wear out or fail
    unexpectedly, causing system errors. It is recommended to regularly
    back up your system configuration and have replacement USB
    sticks prepared.

-   For a more reliable boot disk, use two identical devices and select
    them both during the installation. This will create a mirrored
    boot device.

<span>note</span><span>Note:</span> Current versions of
FreeNAS$^{\text{®}}$ run directly from the operating system device.
Early versions of FreeNAS$^{\text{®}}$ ran from RAM, but that has not
been the case for years.

### Storage Disks and Controllers {#\detokenize{intro:storage-disks-and-controllers}}

\[\] The <span> </span>
(https://www.freebsd.org/releases/11.3R/hardware.html\#disk) of the
FreeBSD Hardware List shows supported disk controllers.

FreeNAS$^{\text{®}}$ supports hotpluggable SATA drives when AHCI is
enabled in the BIOS. The FreeNAS$^{\text{®}}$ team highly recommends
<span> </span>
(https://www.westerndigital.com/products/internaldrives/wdredhdd) NAS
Disk Drives as the preferred storage drive of FreeNAS$^{\text{®}}$.

Suggestions for testing disks can be found in this <span> </span>
(https://forums.freenas.org/index.php?threads/checkingnewhddsinraid.12082/\#post55936).
<span> </span> (https://linux.die.net/man/8/badblocks) is installed with
FreeNAS$^{\text{®}}$ for disk testing.

ZFS <span> </span>
(https://docs.oracle.com/cd/E1925301/8195461/6n7ht6r12/index.html)
recommends a minimum of 16 GiB of disk space. FreeNAS$^{\text{®}}$
allocates 2 GiB of swap space on each drive.

New ZFS users purchasing hardware should read through <span> </span>
(https://web.archive.org/web/20161028084224/http://www.solarisinternals.com/wiki/index.php/ZFS\_Best\_Practices\_Guide\#ZFS\_Storage\_Pools\_Recommendations)
first.

ZFS , groups of disks that act like a single device, can be created
using disks of different sizes. However, the capacity available on each
disk is limited to the same capacity as the smallest disk in the group.
For example, a vdev with one 2 TiB and two 4 TiB disks will only be able
to use 2 TiB of space on each disk. In general, use disks that are the
same size for the best space usage and performance.

The <span> </span>
(https://forums.freenas.org/index.php?threads/zfsdrivesizeandcostcomparisonspreadsheet.38092/)
is available to compare usable space provided by different quantities
and sizes of disks.

### Network Interfaces {#\detokenize{intro:network-interfaces}}

\[\] The <span> </span>
(https://www.freebsd.org/releases/11.3R/hardware.html\#ethernet) of the
FreeBSD Hardware Notes indicates which interfaces are supported by each
driver. While many interfaces are supported, FreeNAS$^{\text{®}}$ users
have seen the best performance from Intel and Chelsio interfaces, so
consider these brands when purchasing a new NIC. Realtek cards often
perform poorly under CPU load as interfaces with these chipsets do not
provide their own processors.

At a minimum, a GigE interface is recommended. While GigE interfaces and
switches are affordable for home use, modern disks can easily saturate
their 110 MiB/s throughput. For higher network throughput, multiple GigE
cards can be bonded together using the LACP type of (). The Ethernet
switch must support LACP, which means a more expensive managed switch is
required.

When network performance is a requirement and there is some money to
spend, use 10 GigE interfaces and a managed switch. Managed switches
with support for LACP and jumbo frames are preferred, as both can be
used to increase network throughput. Refer to the <span> </span>
(https://forums.freenas.org/index.php?threads/10gignetworkingprimer.25749/)
for more information.

<span>note</span><span>Note:</span> At present, these are not supported:
InfiniBand, FibreChannel over Ethernet, or wireless interfaces.

Both hardware and the type of shares can affect network performance. On
the same hardware, SMB is slower than FTP or NFS because Samba is <span>
</span>
(https://www.samba.org/samba/docs/old/Samba3DevelopersGuide/architecture.html).
So a fast CPU can help with SMB performance.

Wake on LAN (WOL) support depends on the FreeBSD driver for the
interface. If the driver supports WOL, it can be enabled using <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=ifconfig). To
determine if WOL is supported on a particular interface, use the
interface name with the following command. In this example, the
capabilities line indicates that WOL is supported for the interface:

\[commandchars=\
{}\] \[root@freenas \] ifconfig m igb0 igb0:
flags=8943UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST metric 0 mtu
1500
options=6403bbRXCSUM,TXCSUM,VLANMTU,VLANHWTAGGING,JUMBOMTU,VLANHWCSUM,
TSO4,TSO6,VLANHWTSO,RXCSUMIPV6,TXCSUMIPV6
capabilities=653fbbRXCSUM,TXCSUM,VLANMTU,VLANHWTAGGING,JUMBOMTU,
VLANHWCSUM,TSO4,TSO6,LRO,WOLUCAST,WOLMCAST,WOLMAGIC,VLANHWFILTER,VLANHWTSO,
RXCSUMIPV6,TXCSUMIPV6

If WOL support is shown but not working for a particular interface,
create a bug report using the instructions in ().

Getting Started with ZFS {#\detokenize{intro:getting-started-with-zfs}}
------------------------

\[\] Readers new to ZFS should take a moment to read the ().

Installing and Upgrading {#\detokenize{install:installing-and-upgrading}}
========================

\[\]\[\] The FreeNAS$^{\text{®}}$ operating system has to be installed
on a separate device from the drives which hold the storage data. With
only one disk drive, the FreeNAS$^{\text{®}}$ web interface is
available, but there is no place to store any data. And storing data is,
after all, the whole point of a NAS system. Home users experimenting
with FreeNAS$^{\text{®}}$ can install FreeNAS$^{\text{®}}$ on an
inexpensive USB stick and use the computer disks for storage.

This section describes:

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

Getting FreeNAS$^{\text{®}}$ {#\detokenize{install:getting-freenas}}
----------------------------

\[\]\[\] The latest STABLE version of FreeNAS$^{\text{®}}$ 11.3 is
available for download from .

The download page has links to FreeNAS$^{\text{®}}$ release notes,
integrity checksums, and PGP security keys.

Clicking opens a dialog to save an file. This bootable installer must be
() before it can be used to install FreeNAS$^{\text{®}}$.

### Checking Installer Integrity {#\detokenize{install:checking-installer-integrity}}

\[\] FreeNAS$^{\text{®}}$ uses the <span> </span>
(https://en.wikipedia.org/wiki/Pretty\_Good\_Privacy\#OpenPGP) to
confirm that downloaded files have been provided by a trustworthy
source. OpenPGP compliant software like <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=gpg), <span> </span>
(https://www.openpgp.org/software/kleopatra/), or <span> </span>
(https://gpg4win.org/) can check the PGP signature of a
FreeNAS$^{\text{®}}$ installer file.

The file is used to confirm the integrity of the downloaded . See () for
more details.

#### PGP Verification {#\detokenize{install:pgp-verification}}

To verify the source, go to and click to download the software signature
file. Open the link and note the browser address and string.

Use one of the OpenPGP encryption tools mentioned above to import the
public key and verify the PGP signature.

This example shows verifying the FreeNAS$^{\text{®}}$ using in a command
prompt:

-   Go to the and download location and import the public key using the
    keyserver address and search results string:

\[commandchars=\
{}\] tmoore@Observer cd Downloads/ tmoore@Observer /Downloads gpg
keyserver skskeyservers.net recvkeys
0xc8d62def767c1db0dff4e6ec358eaa9112cf7946 gpg:
/usr/home/tmoore/.gnupg/trustdb.gpg: trustdb created gpg: key
358EAA9112CF7946: public key IX SecTeam securityofficer@ixsystems.com
imported gpg: Total number processed: 1 gpg: imported: 1 tmoore@Observer
/Downloads

-   Use to compare the and files:

\[commandchars=\
{}\] tmoore@Observer /Downloads gpg verify FreeNAS11.2U6.iso.gpg
FreeNAS11.2U6.iso gpg: Signature made Tue Nov 5 13:48:18 2019 EST gpg:
using RSA key C8D62DEF767C1DB0DFF4E6EC358EAA9112CF7946 gpg: Good
signature from IX SecTeam securityofficer@ixsystems.com \[unknown\] gpg:
WARNING: This key is not certified with a trusted signature! gpg: There
is no indication that the signature belongs to the owner. Primary key
fingerprint: C8D6 2DEF 767C 1DB0 DFF4 E6EC 358E AA91 12CF 7946
tmoore@Observer /Downloads

-   This response means the signature is correct but still untrusted. Go
    back to the browser page that has the open and manually confirm that
    the key was issued for the iX Security Team on October 15, 2019 and
    has been signed by iXsystems accounts.

#### SHA256 Verification {#\detokenize{install:sha256-verification}}

\[\] The command to verify the checksum varies by operating system:

-   on a BSD system use the command

-   on a Linux system use the command

-   on a Mac system use the command

-   Windows or Mac users can install additional utilities like <span>
    </span> (http://www.slavasoft.com/hashcalc/) or <span>
    </span> (http://implbits.com/products/hashtab/).

The value produced by running the command must match the value shown in
the file. Different checksum values indicate a corrupted installer file
that should not be used.

Preparing the Media {#\detokenize{install:preparing-the-media}}
-------------------

\[\]\[\] The FreeNAS$^{\text{®}}$ installer can run from either a CD or
a USB stick.

A CD burning utility is needed to write the file to a CD.

The file can also be written to a USB stick. The method used to write
the file depends on the operating system. Examples for several common
operating systems are shown below.

<span>note</span><span>Note:</span> To install from a USB stick to
another USB stick, USB ports are needed, each with an inserted USB
device. One USB stick contains the installer. The other USB stick is the
destination for the FreeNAS$^{\text{®}}$ installation. Take care to
select the correct USB device for the FreeNAS$^{\text{®}}$ installation.
It is possible to install FreeNAS$^{\text{®}}$ onto the same USB stick
containing the installer. After installation, remove the installer USB
stick. It might also be necessary to adjust the BIOS configuration to
boot from the new FreeNAS$^{\text{®}}$ USB stick.

Ensure the operating system device order in the BIOS is set to boot from
the device containing the FreeNAS$^{\text{®}}$ installer media, then
boot the system to start the installation.

### On FreeBSD or Linux {#\detokenize{install:on-freebsd-or-linux}}

\[\] On a FreeBSD or Linux system, the command is used to write the file
to an inserted USB stick.

<span>warning</span><span>Warning:</span> The command is very powerful
and can destroy any existing data on the specified device. Make of the
device name to write to and do not mistype the device name when using !
This command can be avoided by writing the file to a CD instead.

This example demonstrates writing the image to the first USB device
connected to a FreeBSD system. This first device usually reports as .
Replace with the filename of the downloaded FreeNAS$^{\text{®}}$ ISO
file. Replace with the device name of the device to write.

\[commandchars=\
{}\] dd if=FreeNASRELEASE.iso of=/dev/da0 bs=64k 6117+0 records in
6117+0 records out 400883712 bytes transferred in 88.706398 secs
(4519220 bytes/sec)

When using the command:

-   refers to the input file, or the name of the file to write to
    the device.

-   refers to the output file; in this case, the device name of the
    flash card or removable USB stick. Note that USB device numbers are
    dynamic, and the target device might be or or another name depending
    on which devices are attached. Before attaching the target USB
    stick, use . Then attach the target USB stick, wait ten seconds, and
    run again to see the new device name and number of the target
    USB stick. On Linux, use , where refers to the letter of the
    USB device.

-   refers to the block size, the amount of data to write at a time. The
    larger 64K block size shown here helps speed up writes to the
    USB stick.

### On Windows {#\detokenize{install:on-windows}}

\[\] <span> </span> (https://launchpad.net/win32imagewriter/) and <span>
</span> (http://rufus.akeo.ie/) can be used for writing images to USB
sticks on Windows.

### On macOS {#\detokenize{install:on-macos}}

\[\] Insert the USB stick. In Finder, go to . Unmount any mounted
partitions on the USB stick. Check that the USB stick has only one
partition, or partition table errors will be shown on boot. If needed,
use Disk Utility to set up one partition on the USB stick. Selecting
when creating the partition works fine.

Determine the device name of the inserted USB stick. From TERMINAL,
navigate to the Desktop, then type this command:

\[commandchars=\
{}\] diskutil list /dev/disk0

: TYPE NAME SIZE IDENTIFIER 0: GUIDpartitionscheme \*500.1 GB disk0 1:
EFI 209.7 MB disk0s1 2: AppleHFS Macintosh HD 499.2 GB disk0s2 3:
AppleBoot Recovery HD 650.0 MB disk0s3

/dev/disk1 : TYPE NAME SIZE IDENTIFIER 0: FDiskpartitionscheme \*8.0 GB
disk1 1: DOSFAT32 UNTITLED 8.0 GB disk1s1

This shows which devices are available to the system. Locate the target
USB stick and record the path. To determine the correct path for the USB
stick, remove the device, run the command again, and compare the
difference. Once sure of the device name, navigate to the Desktop from
TERMINAL, unmount the USB stick, and use the command to write the image
to the USB stick. In this example, the USB stick is . It is first
unmounted. The command is used to write the image to the faster “raw”
version of the device (note the extra in ).

When running these commands, replace with the name of the
FreeNAS$^{\text{®}}$ ISO and with the correct path to the USB stick:

\[commandchars=\
{}\] diskutil unmountDisk /dev/disk1 Unmount of all volumes on disk1 was
successful

dd if=FreeNASRELEASE.iso of=/dev/rdisk1 bs=64k

<span>note</span><span>Note:</span> If the error “Resource busy” is
shown when the command is run, go to , find the USB stick, and click on
its partitions to make sure all of them are unmounted. If a “Permission
denied” error is shown, use for elevated rights: . This will prompt for
the password.

The command can take some minutes to complete. Wait until the prompt
returns and a message is displayed with information about how long it
took to write the image to the USB stick.

Performing the Installation {#\detokenize{install:performing-the-installation}}
---------------------------

\[\]\[\] With the installation media inserted, boot the system from that
media.

The FreeNAS$^{\text{®}}$ installer boot menu is displayed as is shown in
.

\[\]

The FreeNAS$^{\text{®}}$ installer automatically boots into the default
option after ten seconds. If needed, choose another boot option by
pressing the to stop the timer and then enter the number of the desired
option.

<span>tip</span><span>Tip:</span> The option is useful on systems which
do not have a keyboard or monitor, but are accessed through a serial
port, , or ().

<span>note</span><span>Note:</span> If the installer does not boot,
verify that the installation device is listed first in the boot order in
the BIOS. When booting from a CD, some motherboards may require
connecting the CD device to SATA0 (the first connector) to boot from CD.
If the installer stalls during bootup, doublecheck the SHA256 hash of
the file. If the hash does not match, redownload the file. If the hash
is correct, burn the CD again at a lower speed or write the file to a
different USB stick.

Once the installer has finished booting, the installer menu is displayed
as shown in .

\[\]

Press to select the default option, . The next menu, shown in , lists
all available drives. This includes any inserted operating system
devices, which have names beginning with .

<span>note</span><span>Note:</span> A minimum of 8 GiB of RAM is
required and the installer will present a warning message if less than 8
GiB is detected.

In this example, the user is performing a test installation using
VirtualBox and has created a 16 GiB virtual disk to hold the operating
system.

\[\]

Use the arrow keys to highlight the destination SSD, hard drive, USB
stick, or virtual disk. Press the to select it.

To mirror the operating system device, move to additional devices and
press to select them also. If all of the selected devices are larger
than 64 GiB and none are connected through USB, a 16 GiB swap partition
is also created.

After making selections, press . The warning shown in is displayed, a
reminder not to install the operating system on a drive that is meant
for storage. Press to continue on to the screen shown in .

\[\]

See the () section to ensure the minimum requirements are met.

The installer recognizes existing installations of previous versions of
FreeNAS$^{\text{®}}$. When an existing installation is present, the menu
shown in is displayed. To overwrite an existing installation, use the
arrows to move to and press twice to continue to the screen shown in .

\[\]

The screen shown in prompts for the password which is used to log in to
the web interface.

\[\]

Setting a password is mandatory and the password cannot be blank. Since
this password provides access to the web interface, it needs to be hard
to guess. Enter the password, press the down arrow key, and confirm the
password. Then press to continue with the installation. Choosing skips
setting a root password during the installation, but the web interface
will require setting a root password when logging in for the first time.

<span>note</span><span>Note:</span> For security reasons, the SSH
service and SSH logins are disabled by default. Unless these are set,
the only way to access a shell as is to gain physical access to the
console menu or to access the web shell within the web interface. This
means that the FreeNAS$^{\text{®}}$ system needs to be kept physically
secure and that the web interface needs to be behind a properly
configured firewall and protected by a secure password.

FreeNAS$^{\text{®}}$ can be configured to boot with the standard BIOS
boot mechanism or UEFI booting as shown . BIOS booting is recommended
for legacy and enterprise hardware. UEFI is used on newer consumer
motherboards.

\[\]

<span>note</span><span>Note:</span> Most UEFI systems can also boot in
BIOS mode if CSM (Compatibility Support Module) is enabled in the UEFI
setup screens.

The message in is shown after the installation is complete.

\[\]

Press to return to (). Highlight and press . If booting from CD, remove
the CDROM. As the system reboots, make sure that the device where
FreeNAS$^{\text{®}}$ was installed is listed as the first boot entry in
the BIOS so the system will boot from it.

FreeNAS$^{\text{®}}$ boots into the menu described in () after waiting
five seconds in the (). Press the to stop the timer and use the boot
menu.

Installation Troubleshooting {#\detokenize{install:installation-troubleshooting}}
----------------------------

\[\] If the system does not boot into FreeNAS$^{\text{®}}$, there are
several things that can be checked to resolve the situation.

Check the system BIOS and see if there is an option to change the USB
emulation from CD/DVD/floppy to hard drive. If it still will not boot,
check to see if the card/drive is UDMA compliant.

If the system BIOS does not support EFI with BIOS emulation, see if it
has an option to boot using legacy BIOS mode.

When the system starts to boot but hangs with this repeated error
message:

\[commandchars=\
{}\] runinterruptdrivenhooks: still waiting after 60 seconds for
xptconfig

go into the system BIOS and look for an onboard device configuration for
a 1394 Controller. If present, disable that device and try booting
again.

If the system starts to boot but hangs at a prompt, follow the
instructions in <span> </span>
(https://forums.freenas.org/index.php?threads/workaroundsemifixformountrootissueswith93.26071/).

If the burned image fails to boot and the image was burned using a
Windows system, wipe the USB stick before trying a second burn using a
utility such as <span> </span> (http://howtoeraseharddrive.com/).
Otherwise, the second burn attempt will fail as Windows does not
understand the partition which was written from the image file. Be very
careful to specify the correct USB stick when using a wipe utility!

Upgrading {#\detokenize{install:upgrading}}
---------

\[\]\[\] FreeNAS$^{\text{®}}$ provides flexibility for keeping the
operating system uptodate:

1.  Upgrades to major releases, for example from version 9.3 to 9.10,
    can still be performed using either an ISO or the web interface.
    Unless the Release Notes for the new major release indicate that the
    current version requires an ISO upgrade, either upgrade method can
    be used.

2.  Minor releases have been replaced with signed updates. This means
    that it is not necessary to wait for a minor release to update the
    system with a system update or newer versions of drivers
    and features. It is also no longer necessary to manually download an
    upgrade file and its associated checksum to update the system.

3.  The updater automatically creates a boot environment, making updates
    a lowrisk operation. Boot environments provide the option to return
    to the previous version of the operating system by rebooting the
    system and selecting the previous boot environment from the
    boot menu.

This section describes how to perform an upgrade from an earlier version
of FreeNAS$^{\text{®}}$ to 11.3. After 11.3 has been installed, use the
instructions in () to keep the system updated.

### Caveats {#\detokenize{install:caveats}}

\[\] Be aware of these caveats attempting an upgrade to 11.3:

-   For this reason, the update process does not automatically upgrade
    the ZFS pool, though the () system shows when newer () are available
    for a pool. Unless a new feature flag is needed, it is safe to leave
    the pool at the current version and uncheck the alert. If the pool
    is upgraded, it will not be possible to boot into a previous version
    that does not support the newer feature flags.

-   Upgrading the firmware of Broadcom SAS HBAs to the latest version
    is recommended.

-   If upgrading from 9.3.x, read the <span> </span>
    (https://forums.freenas.org/index.php?threads/faqupdatingfrom93to910.54260/) first.

-   FreeNAS$^{\text{®}}$ The system has no way to import configuration
    settings from 0.7x versions of FreeNAS$^{\text{®}}$. The
    configuration must be manually recreated. If supported, the
    FreeNAS$^{\text{®}}$ 0.7x pools or disks must be manually imported.

-   However, if the system is currently running a 32bit version of
    FreeNAS$^{\text{®}}$ the hardware supports 64bit, the system can
    be upgraded. Any archived reporting graphs will be lost during
    the upgrade.

-   If the data currently resides on UFSformatted disk, create a ZFS
    pool using disks after the upgrade, then use the instructions in ()
    to moun t the UFSformatted disk and copy the data to the ZFS pool.
    With only one disk, back up its data to another system or media
    before the upgrade, format the disk as ZFS after the upgrade, then
    restore the backup. If the data currently resides on a UFS RAID of
    disks, it is not possible to directly import that data to the
    ZFS pool. Instead, back up the data before the upgrade, create a ZFS
    pool after the upgrade, then restore the data from the backup.

### Initial Preparation {#\detokenize{install:initial-preparation}}

\[\] Before upgrading the operating system, perform the following steps:

1.  FreeNAS$^{\text{®}}$ in .

2.  If any pools are encrypted, to set a passphrase and download a copy
    of the encryption key and the latest recovery key. After the upgrade
    is complete, use the instructions in () to import the
    encrypted pools.

3.  Warn users that the FreeNAS$^{\text{®}}$ shares will be unavailable
    during the upgrade; it is recommended to schedule the upgrade for a
    time that will least impact users.

4.  Stop all services in .

### Upgrading Using the ISO {#\detokenize{install:upgrading-using-the-iso}}

\[\] To perform an upgrade using this method, <span> </span>
(http://download.freenas.org/latest/) the to the computer that will be
used to prepare the installation media. Burn the downloaded file to a CD
or USB stick using the instructions in ().

Insert the prepared media into the system and boot from it. The
installer waits ten seconds in the () before booting the default option.
If needed, press the to stop the timer and choose another boot option.
After the media finishes booting into the installation menu, press to
select the default option of The installer presents a screen showing all
available drives.

<span>warning</span><span>Warning:</span> drives are shown, including
boot drives and storage drives. Only choose boot drives when upgrading.
Choosing the wrong drives to upgrade or install will cause loss of data.
If unsure about which drives contain the FreeNAS$^{\text{®}}$ operating
system, reboot and remove the install media. In the FreeNAS$^{\text{®}}$
web interface, use to identify the boot drives. More than one drive is
shown when a mirror has been used.

Move to the drive where FreeNAS$^{\text{®}}$ is installed and press the
to mark it with a star. If a mirror has been used for the operating
system, mark all of the drives where the FreeNAS$^{\text{®}}$ operating
system is installed. Press when done.

The installer recognizes earlier versions of FreeNAS$^{\text{®}}$
installed on the boot drive or drives and presents the message shown in
.

\[\]

To perform an upgrade, press to accept the default of . Again, the
installer will display a reminder that the operating system should be
installed on a disk that is not used for storage.

\[\]

The updated system can be installed in a new boot environment, or the
entire operating system device can be formatted to start fresh.
Installing into a new boot environment preserves the old code, allowing
a rollback to previous versions if necessary. Formatting the boot device
is usually not necessary but can reclaim space. User data and settings
are preserved when installing to a new boot environment and also when
formatting the operating system device. Move the highlight to one of the
options and press to start the upgrade.

The installer unpacks the new image and displays the menu shown in . The
database file that is preserved and migrated contains your
FreeNAS$^{\text{®}}$ configuration settings.

\[\]

Press . FreeNAS$^{\text{®}}$ indicates that the upgrade is complete and
a reboot is required. Press , highlight , then press to reboot the
system. If the upgrade installer was booted from CD, remove the CD.

During the reboot there can be a conversion of the previous
configuration database to the new version of the database. This happens
during the “Applying database schema changes” line in the reboot cycle.
This conversion can take a long time to finish, sometimes fifteen
minutes or more, and can cause the system to reboot again. The system
will start normally afterwards. If database errors are shown but the web
interface is accessible, go to and use the button to upload the
configuration that was saved before starting the upgrade.

### Upgrading From the Web Interface {#\detokenize{install:upgrading-from-the-web-interface}}

\[\] To perform an upgrade using this method, go to . See () for more
information on upgrading the system.

The connection is lost temporarily when the update is complete. It
returns after the FreeNAS$^{\text{®}}$ system reboots into the new
version of the operating system. The FreeNAS$^{\text{®}}$ system
normally receives the same IP address from the DHCP server. Refresh the
browser after a moment to see if the system is accessible.

### If Something Goes Wrong {#\detokenize{install:if-something-goes-wrong}}

\[\] If an update fails, an alert is issued and the details are written
to .

To return to a previous version of the operating system, physical or
IPMI access to the FreeNAS$^{\text{®}}$ console is needed. Reboot the
system and watch for the boot menu:

\[\]

FreeNAS$^{\text{®}}$ waits five seconds before booting into the default
boot environment. Press the to stop the automatic boot timer. Press to
display the available boot environments and press as needed to scroll
through multiple pages.

\[\]

In the example shown in , the first entry in is . This is the current
version of the operating system, after the update was applied. Since it
is the first entry, it is the default selection.

The next entry is . This is the original boot environment created when
FreeNAS$^{\text{®}}$ was first installed. Since there are no other
entries between the initial installation and the first entry, only one
update has been applied to this system since its initial installation.

To boot into another version of the operating system, enter the number
of the boot environment to set it as . Press to return to the () and
press to boot into the chosen boot environment.

If an operating system device fails and the system no longer boots,
don’t panic. The data is still on the disks and there is still a copy of
the saved configuration. The system can be recovered with a few steps:

1.  Perform a fresh installation on a new operating system device.

2.  Import the pools in .

3.  Restore the configuration in .

<span>note</span><span>Note:</span> It is not possible to restore a
saved configuration that is newer than the installed version. For
example, if a reboot into an older version of the operating system is
performed, a configuration created in a later version cannot be
restored.

### Upgrading a ZFS Pool {#\detokenize{install:upgrading-a-zfs-pool}}

\[\]\[\] In FreeNAS$^{\text{®}}$, ZFS pools can be upgraded from the
graphical administrative interface.

Before upgrading an existing ZFS pool, be aware of these caveats first:

-   the pool upgrade is a oneway street, meaning that

-   before performing any operation that may affect the data on a
    storage disk, While it is unlikely that the pool upgrade will affect
    the data, it is always better to be safe than sorry.

-   upgrading a ZFS pool is . Do not upgrade the pool if the the
    possibility of reverting to an earlier version of
    FreeNAS$^{\text{®}}$ or repurposing the disks in another operating
    system that supports ZFS is desired. It is not necessary to upgrade
    the pool unless the end user has a specific need for the newer ().
    If a pool is upgraded to the latest feature flags, it will not be
    possible to import that pool into another operating system that does
    not yet support those feature flags.

To perform the ZFS pool upgrade, go to and click (Settings) to upgrade.
Click the button as shown in .

<span>note</span><span>Note:</span> If the button does not appear, the
pool is already at the latest feature flags and does not need to be
upgraded.

\[\]

The warning serves as a reminder that a pool upgrade is not reversible.
Click to proceed with the upgrade.

The upgrade itself only takes a few seconds and is nondisruptive. It is
not necessary to stop any sharing services to upgrade the pool. However,
it is best to upgrade when the pool is not being heavily used. The
upgrade process will suspend I/O for a short period, but is nearly
instantaneous on a quiet pool.

Virtualization {#\detokenize{install:virtualization}}
--------------

\[\]\[\] FreeNAS$^{\text{®}}$ can be run inside a virtual environment
for development, experimentation, and educational purposes. Note that
running FreeNAS$^{\text{®}}$ in production as a virtual machine is
<span> </span>
(https://forums.freenas.org/index.php?threads/pleasedonotrunfreenasinproductionasavirtualmachine.12484/).
When using FreeNAS$^{\text{®}}$ within a virtual environment, <span>
</span>
(https://forums.freenas.org/index.php?threads/absolutelymustvirtualizefreenasaguidetonotcompletelylosingyourdata.12714/)
as it contains useful guidelines for minimizing the risk of losing data.

To install or run FreeNAS$^{\text{®}}$ within a virtual environment,
create a virtual machine that meets these minimum requirements:

-   8192 MiB (8 GiB) base memory size

-   a virtual disk to hold the operating system and boot environments

-   at least one additional virtual disk to be used as data storage

-   a bridged network adapter

This section demonstrates how to create and access a virtual machine
within VirtualBox and VMware ESXi environments.

### VirtualBox {#\detokenize{install:virtualbox}}

\[\] <span> </span> (https://www.virtualbox.org/) is an open source
virtualization program originally created by Sun Microsystems.
VirtualBox runs on Windows, BSD, Linux, Macintosh, and OpenSolaris. It
can be configured to use a downloaded FreeNAS$^{\text{®}}$ file, and
makes a good testing environment for practicing configurations or
learning how to use the features provided by FreeNAS$^{\text{®}}$.

To create the virtual machine, start VirtualBox and click the button,
shown in , to start the new virtual machine wizard.

\[\]

Click the button to see the screen in . Enter a name for the virtual
machine, click the dropdown menu and select BSD, and select from the
dropdown.

\[\]

Click to see the screen in . The base memory size must be changed to .
When finished, click to see the screen in .

\[\]

\[\]

Click to launch the shown in .

\[\]

Select and click the button to see the screen in .

\[\]

Choose either or storage. The first option uses disk space as needed
until it reaches the maximum size that is set in the next screen. The
second option creates a disk the full amount of disk space, whether it
is used or not. Choose the first option to conserve disk space;
otherwise, choose the second option, as it allows VirtualBox to run
slightly faster. After selecting , the screen in is shown.

\[\]

This screen is used to set the size (or upper limit) of the virtual
disk. . Use the folder icon to browse to a directory on disk with
sufficient space to hold the virtual disk files. Remember that there
will be a system disk of at least 8 GiB and at least one data storage
disk of at least 4 GiB.

Use the button to return to a previous screen if any values need to be
modified. After making a selection and pressing , the new VM is created.
The new virtual machine is listed in the left frame, as shown in the
example in . Open the dropdown menu and select to see extra information
about the VM.

\[\]

Create the virtual disks to be used for storage. Highlight the VM and
click to open the menu. Click the option in the left frame to access the
storage screen seen in .

\[\]

Click the button, select from the popup menu, then click the button.
This launches the wizard seen in and .

Create a disk large enough to hold the desired data. The minimum size is
To practice with RAID configurations, create as many virtual disks as
needed. Two disks can be created on each IDE controller. For additional
disks, click the button to create another controller for attaching
additional disks.

Create a device for the installation media. Highlight the word “Empty”,
then click the icon as shown in .

\[\]

Click to browse to the location of the file. If the was burned to CD,
select the detected .

Depending on the extensions available in the host CPU, it might not be
possible to boot the VM from an . If “your CPU does not support long
mode” is shown when trying to boot the , the host CPU either does not
have the required extension or AMDV/VTx is disabled in the system BIOS.

<span>note</span><span>Note:</span> If there is a kernel panic when
booting into the ISO, stop the virtual machine. Then, go to and check
the box .

To configure the network adapter, go to . In the dropdown menu select ,
then choose the name of the physical interface from the dropdown menu.
In the example shown in , the Intel Pro/1000 Ethernet card is attached
to the network and has a device name of .

\[\]

After configuration is complete, click the arrow and install
FreeNAS$^{\text{®}}$ as described in (). After FreeNAS$^{\text{®}}$ is
installed, press when the VM starts to boot to access the boot menu.
Select the primary hard disk as the boot option. You can permanently
boot from disk by removing the device in or by unchecking in the section
of .

### VMware ESXi {#\detokenize{install:vmware-esxi}}

\[\] ESXi is a baremetal hypervisor architecture created by VMware Inc.
Commercial and free versions of the VMware vSphere Hypervisor operating
system (ESXi) are available from the <span> </span>
(https://www.vmware.com/products/esxiandesx.html).

Install and use the VMware vSphere client to connect to the ESXi server.
Enter the username and password created when installing ESXi to log in
to the interface. After logging in, go to to upload the
FreeNAS$^{\text{®}}$ . Click and select a datastore for the
FreeNAS$^{\text{®}}$ . Click and choose the FreeNAS$^{\text{®}}$ from
the host system.

Click to create a new VM. The wizard opens:

1.  : Select and click .

2.  : Enter a name for the VM. Leave ESXi compatibility version at
    the default. Select as the Guest OS family. Select as the Guest
    OS version. Click .

3.  : Select a datastore for the VM. The datastore must be at least
    32 GiB.

4.  : Enter the recommended minimums of at least of memory and
    of storage. Select from the dropdown. Use the Datastore browser to
    select the uploaded FreeNAS$^{\text{®}}$ . Click .

5.  : Review the VM settings. Click to create the new VM.

To add more disks to a VM, rightclick the VM and click .

Click . Enter the desired capacity and click .

\[\]

Virtual HPET hardware can prevent the virtual machine from booting on
some older versions of VMware. If the virtual machine does not boot,
remove the virtual HPET hardware:

-   On ESXi, rightclick the VM and click . Click . Change from to and
    click . Click to save the new settings.

-   On Workstation or Player, while in , click . Locate the path for the
    Configuration file named . Open the file in a text editor and change
    from to , then save the change.

Booting {#\detokenize{booting:booting}}
=======

\[\]\[\] The Console Setup menu, shown in , appears at the end of the
boot process. If the FreeNAS$^{\text{®}}$ system has a keyboard and
monitor, this Console Setup menu can be used to administer the system.

<span>note</span><span>Note:</span> When connecting to the
FreeNAS$^{\text{®}}$ system with SSH or the web (), the Console Setup
menu is not shown by default. It can be started by the user or another
user with root permissions by typing .

The Console Setup menu can be disabled by unchecking in .

\[\]

The menu provides these options:

provides a configuration wizard to set up the system’s network
interfaces.

is for creating or deleting link aggregations.

is used to create or delete VLAN interfaces.

is used to set the IPv4 or IPv6 default gateway. When prompted, enter
the IP address of the default gateway.

prompts for the destination network and gateway IP address. Reenter this
option for each static route needed.

prompts for the name of the DNS domain and the IP address of the first
DNS server. When adding multiple DNS servers, press to enter the next
one. Press twice to leave this option.

is used to reset a lost or forgotten password. Select this option and
follow the prompts to set the password.

! This option deletes of the configuration settings made in the
administrative GUI and is used to reset a FreeNAS$^{\text{®}}$ system
back to defaults. After this option is selected, the configuration is
reset to defaults and the system reboots. can be used to reimport pools.

starts a shell for running FreeBSD commands. To leave the shell, type .

reboots the system.

shuts down the system.

<span>note</span><span>Note:</span> The numbering and quantity of
options on this menu can change due to software updates, service
agreements, or other factors. Please carefully check the menu before
selecting an option, and keep this in mind when writing local
procedures.

Obtaining an IP Address {#\detokenize{booting:obtaining-an-ip-address}}
-----------------------

\[\] During boot, FreeNAS$^{\text{®}}$ automatically attempts to connect
to a DHCP server from all live network interfaces. After
FreeNAS$^{\text{®}}$ successfully receives an IP address, the address is
displayed so it can be used to access the web interface. The example in
shows a FreeNAS$^{\text{®}}$ system that is accessible at .

Some FreeNAS$^{\text{®}}$ systems are set up without a monitor, making
it challenging to determine which IP address has been assigned. On
networks that support Multicast DNS (mDNS), the hostname and domain can
be entered into the address bar of a browser. By default, this value is
.

If the FreeNAS$^{\text{®}}$ server is not connected to a network with a
DHCP server, use the console network configuration menu to manually
configure the interface as shown here. In this example, the
FreeNAS$^{\text{®}}$ system has one network interface, .

\[commandchars=\
{}\] Enter an option from 111: 1 1) em0 Select an interface (q to quit):
1 Remove the current settings of this interface? (This causes a
momentary disconnec tion of the network.) (y/n) n Configure interface
for DHCP? (y/n) n Configure IPv4? (y/n) y Interface name: (press enter,
the name can be blank) Several input formats are supported Example 1
CIDR Notation: 192.168.1.1/24 Example 2 IP and Netmask separate: IP:
192.168.1.1 Netmask: 255.255.255.0, or /24 or 24 IPv4 Address:
192.168.1.108/24 Saving interface configuration: Ok Configure IPv6?
(y/n) n Restarting network: ok

...

The web user interface is at http://192.168.1.108

Accessing the Web Interface {#\detokenize{booting:accessing-the-web-ui}}
===========================

\[\]\[\] On a computer that can access the same network as the
FreeNAS$^{\text{®}}$ system, enter the IP address in a web browser to
connect to the web interface. The password for the root user is
requested.

\[\]

Enter the password chosen during the installation. A prompt is shown to
set a root password if it was not set during installation.

The web interface is displayed after login:

\[\]

The shows details about the system. These details are grouped into
sections about the hardware components, networking, storage, and other
categories.

Web Interface Troubleshooting {#\detokenize{booting:web-ui-troubleshooting}}
-----------------------------

If the user interface is not accessible by IP address from a browser,
check these things:

-   Are proxy settings enabled in the browser configuration? If so,
    disable the settings and try connecting again.

-   If the page does not load, make sure that a reaches the
    FreeNAS$^{\text{®}}$ system’s IP address. If the address is in a
    private IP address range, it is only accessible from within that
    private network.

If the UI becomes unresponsive after an upgrade or other system
operation, clear the site data and refresh the browser.

The rest of this User Guide describes the FreeNAS$^{\text{®}}$ web
interface in more detail. The layout of this User Guide follows the
order of the menu items in the tree located in the left frame of the web
interface.

Settings {#\detokenize{settings:settings}}
========

\[\]\[\]\[\] The (Settings) menu provides options to change the
administrator password, set preferences, and view system information.

Change Password {#\detokenize{settings:change-password}}
---------------

\[\] To change the account password, click (Settings) and . The current
password must be entered before a new password can be saved.

Preferences {#\detokenize{settings:preferences}}
-----------

\[\] The FreeNAS$^{\text{®}}$ User Interface can be adjusted to match
the user preferences. Go to the page by clicking the (Settings) menu in
the upperright and clicking .

### Web Interface Preferences {#\detokenize{settings:web-interface-preferences}}

\[\]\[\] This page has options to adjust global settings in the web
interface, manage custom themes, and create new themes. shows the
different options:

\[\]

These options are applied to the entire web interface:

-   : Change the active theme. Custom themes are added to this list.

-   : Set to preserve screen space and only display icons and tooltips
    instead of text labels.

-   : When set, an icon appears next to password fields. Clicking the
    icon reveals the password. Clicking it again hides the password.

-   : Set to reset all tables to display default columns.

Make any changes and click to save the new selections.

### Themes {#\detokenize{settings:themes}}

\[\] The FreeNAS$^{\text{®}}$ web interface supports dynamically
changing the active theme and creating new, fully customizable themes.

#### Create New Themes {#\detokenize{settings:create-new-themes}}

\[\]\[\] This page is used to create and preview custom
FreeNAS$^{\text{®}}$ themes. shows many of the theming and preview
options:

\[\]

To create a new custom theme, click . Colors from an existing theme can
be used when creating a new custom theme. Select a theme from the
dropdown to use the colors from that theme for the new custom theme.
describes each option:

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.11-2</span>
|&gt;p<span>0.68-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Custom Theme Name & string & Enter a name to identify the new theme.\
Menu Label & string & Enter a short name to use for the
FreeNAS$^{\text{®}}$ menus.\
Menu Swatch & dropdown menu & Choose a color from the theme to display
next to the menu entry of the custom theme.\
Description & string & Enter a short description of the new theme.\
Enable Dark Logo & checkbox & Set this to give the FreeNAS Logo a dark
fill color.\
Choose Primary & dropdown menu & Choose from either a generic color or
import a specific color setting to use as the primary theme color. The
primary color changes the top bar of the web interface and the color of
many of the buttons.\
Choose Accent & dropdown menu & Choose from either a generic color or
import a specific color setting to use as the accent color for the
theme. This color is used for many of the buttons and smaller elements
in the web interface.\

Choose the different for this new theme after setting these general
options. Click the color swatch to open a small popup with sliders to
adjust the color. Color values can also be entered as a hexadecimal
value.

Changing any color value automatically updates the column. This section
is completely interactive and shows how the custom theme is applied to
all the different elements in the web interface.

Click when finished with all the and options. The new theme is added to
the list of available themes in .

Click to apply the unsaved custom theme to the current session of the
FreeNAS$^{\text{®}}$ web interface. Activating allows going to other
pages in the web interface and live testing the new custom theme.

<span>note</span><span>Note:</span> Setting a custom theme as a does
save that theme! Be sure to go back to , complete any remaining options,
and click to save the current settings as a new theme.

API Documentation {#\detokenize{settings:api-documentation}}
-----------------

\[\] Click to see documentation for the <span> </span>
(https://en.wikipedia.org/wiki/WebSocket) used in FreeNAS$^{\text{®}}$.

About {#\detokenize{settings:about}}
-----

\[\] Click (Settings) and to view a popup window with basic system
information. This includes system , , , address, , CPU , and .

Accounts {#\detokenize{accounts:accounts}}
========

\[\]\[\] is used to manage users and groups. This section contains these
entries:

-   (): used to manage UNIXstyle groups on the
    FreeNAS$^{\text{®}}$ system.

-   (): used to manage UNIXstyle accounts on the
    FreeNAS$^{\text{®}}$ system.

Each entry is described in more detail in this section.

Groups {#\detokenize{accounts:groups}}
------

\[\]\[\] The Groups interface provides management of UNIXstyle groups on
the FreeNAS$^{\text{®}}$ system.

<span>note</span><span>Note:</span> It is unnecessary to recreate the
network users or groups when a directory service is running on the same
network. Instead, import the existing account information into
FreeNAS$^{\text{®}}$. Refer to () for details.

This section describes how to create a group and assign user accounts to
it. The page lists all groups, including those built in and used by the
operating system.

\[\]

The table displays group names, group IDs (GID), builtin groups, and
whether is permitted. Clicking the (Options) icon on a usercreated group
entry displays , , and options. Click to view and modify the group
membership. Builtin groups are required by the FreeNAS$^{\text{®}}$
system and cannot be edited or deleted.

The button opens the screen shown in . summarizes the available options
when creating a group.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

GID & string & The next available group ID is suggested. By convention,
UNIX groups containing user accounts have an ID greater than 1000 and
groups required by a service have an ID equal to the default port number
used by the service. Example: the group has an ID of 22. This setting
cannot be edited once the group is created.\
Name & string & Enter an alphanumeric name for the new group. Group
names cannot begin with a hyphen () or contain a space, tab, or these
characters: . can only be used as the last character of the group name.\
Permit Sudo & checkbox & Set to allow group members to use <span>
</span> (https://www.sudo.ws/). When using , a user is prompted for
their own password.\
Allow Duplicate GIDs & checkbox & . Allow more than one group to have
the same group ID.\

To change which users are members of a group, expand the group from the
list and click . To add users to the group, select users in the left
frame and click . To remove users from the group, select users in the
right frame and click . Click when finished changing the group members.

, shows adding a user as a member of a group.

\[\]

The button deletes a group. The popup message asks if all users with
this primary group should also be deleted, and to confirm the action.
Note builtin groups do not have a button.

Users {#\detokenize{accounts:users}}
-----

\[\]\[\] FreeNAS$^{\text{®}}$ supports users, groups, and permissions,
allowing flexibility in configuring which users have access to the data
stored on FreeNAS$^{\text{®}}$. To assign permissions to shares, select
one of these options:

1.  Create a guest account for all users, or create a user account for
    every user in the network where the name of each account is the same
    as a login name used on a computer. For example, if a Windows system
    has a login name of , create a user account with the name on
    FreeNAS$^{\text{®}}$. A common strategy is to create groups with
    different sets of permissions on shares, then assign users to
    those groups.

2.  If the network uses a directory service, import the existing account
    information using the instructions in ().

lists all system accounts installed with the FreeNAS$^{\text{®}}$
operating system, as shown in .

\[\]

By default, each user entry displays the username, User ID (UID),
whether the user is built into FreeNAS$^{\text{®}}$, and full name. This
table is adjustable by clicking and setting the desired columns.

Clicking a column name sorts the list by that value. An arrow indicates
which column controls the view sort order. Click the arrow to reverse
the sort order.

Click (Options) on the user created account to display the and buttons.
Note builtin users do not have a button.

<span>note</span><span>Note:</span> Setting the email address for the
builtin user account is recommended as important system messages are
sent to the user. For security reasons, password logins are disabled for
the account and changing this setting is highly discouraged.

Except for the user, the accounts that come with FreeNAS$^{\text{®}}$
are system accounts. Each system account is used by a service and should
not be used as a login account. For this reason, the default shell on
system accounts is <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=nologin). For security
reasons and to prevent breakage of system services, modifying the system
accounts is discouraged.

The button opens the screen shown in . summarizes the options that are
available when user accounts are created or modified.

<span>warning</span><span>Warning:</span> When using (), Windows user
passwords must be set from within Windows.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.55-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Username & string & Usernames can be up to 16 characters long. When
using NIS or other legacy software with limited username lengths, keep
usernames to eight characters or less for compatibility. Usernames
cannot begin with a hyphen () or contain a space, tab, or these
characters: . can only be used as the last character of the username.\
Full Name & string & This field is mandatory and may contain spaces.\
Email & string & The email address associated with the account.\
Password & string & Mandatory unless is . Cannot contain a . Click
(Show) to view or obscure the password characters.\
Confirm Password & string & Required to match the value of .\
User ID & integer & Grayed out if the user already exists. When creating
an account, the next numeric ID is suggested. By convention, user
accounts have an ID greater than 1000 and system accounts have an ID
equal to the default port number used by the service.\
New Primary Group & checkbox & Set by default to create a new a primary
group with the same name as the user. Unset to select a different
primary group name.\
Primary Group & dropdown menu & Unset to access this menu. For security
reasons, FreeBSD will not give a user permissions if is not their
primary group. To give a user access, add them to the group in .\
Auxiliary groups & dropdown menu & Select which groups the user will be
added to.\
Home Directory & browse button & Choose a path to the user’s home
directory. If the directory exists and matches the username, it is set
as the user’s home directory. When the path does not end with a
subdirectory matching the username, a new subdirectory is created. The
full path to the user’s home directory is shown here when editing a
user.\
Home Directory Permissions & checkboxes & Sets default Unix permissions
of user’s home directory. This is for builtin users.\
SSH Public Key & string & Paste the user’s SSH key to be used for
keybased authentication.\
Disable Password & dropdown & : Disables the fields and removes the
password from the account. The account cannot use passwordbased logins
for services. For example, disabling the password prevents using account
credentials to log in to an SMB share or open an SSH session on the
system. The and options are also removed.

: Requires adding a to the account. The account can use the saved to
authenticate with passwordbased services.\
Shell & dropdown menu & Select the shell to use for local and SSH
logins. The user shell is used for web interface () sessions. See for an
overview of available shells.\
Lock User & checkbox & Prevent the user from logging in or using
passwordbased services until this option is unset. Locking an account is
only possible when is and a has been created for the account.\
Permit Sudo & checkbox & Give this user permission to use <span> </span>
(https://www.sudo.ws/). When using sudo, a user is prompted for their
account .\
Microsoft Account & checkbox & Set if the user is connecting from a
Windows 8 or newer system or when using a Microsoft cloud service.\

<span>note</span><span>Note:</span> Some fields cannot be changed for
builtin users and are grayed out.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.66-2</span>|</span>

\[\]\
Shell & Description\

\
Shell & Description\

\

csh & <span> </span> (https://en.wikipedia.org/wiki/C\_shell)\
sh & <span> </span> (https://en.wikipedia.org/wiki/Bourne\_shell)\
tcsh & <span> </span> (https://en.wikipedia.org/wiki/Tcsh)\
bash & <span>
ttps://en.wikipedia.org/wiki/Bash\_%28Unix\_shell%29</span><span>Bourne
Again shell</span>
(https://en.wikipedia.org/wiki/Bash\_%28Unix\_shell%29)\
ksh93 & <span> </span> (http://www.kornshell.com/)\
mksh & <span> </span> (https://www.mirbsd.org/mksh.htm)\
rbash & <span> </span>
(http://www.gnu.org/software/bash/manual/html\_node/TheRestrictedShell.html)\
rzsh & <span> </span>
(http://www.csse.uwa.edu.au/programming/linux/zshdoc/zsh\_14.html)\
scponly & Select <span> </span>
(https://github.com/scponly/scponly/wiki) to restrict the user’s SSH
usage to only the and commands.\
zsh & <span> </span> (http://www.zsh.org/)\
gitshell & <span> </span> (https://gitscm.com/docs/gitshell)\
nologin & Use when creating a system account or to create a user account
that can authenticate with shares but which cannot login to the FreeNAS
system using .\

Builtin user accounts needed by the system cannot be removed. A button
appears for custom users that were added by the system administrator.
Clicking opens a popup window to confirm the action and offer an option
to keep the user primary group when the user is deleted.

System {#\detokenize{system:system}}
======

\[\]\[\] The System section of the web interface contains these entries:

-   () configures general settings such as HTTPS access, the language,
    and the timezone

-   () adds, edits, and deletes Network Time Protocol servers

-   () creates, renames, and deletes boot environments. It also shows
    the condition of the Boot Pool.

-   () configures advanced settings such as the serial console, swap
    space, and console messages

-   () configures the email address to receive notifications

-   () configures the location where logs and reporting graphs are
    stored

-   () configures services used to notify the administrator about
    system events.

-   () lists the available () conditions and provides configuration of
    the notification frequency for each alert.

-   () is used to enter connection credentials for remote cloud service
    providers

-   () manages connecting to a remote system with SSH.

-   () manages all private and public SSH key pairs.

-   () provides a frontend for tuning in realtime and to load additional
    kernel modules at boot time

-   () performs upgrades and checks for system updates

-   (): import or create internal or intermediate CAs
    (Certificate Authorities)

-   (): import existing certificates, create selfsigned certificates, or
    configure ACME certificates.

-   (): automate domain authentication for compatible CAs
    and certificates.

-   (): report a bug or request a new feature.

Each of these is described in more detail in this section.

General {#\detokenize{system:general}}
-------

\[\] contains options for configuring the web interface and other basic
system settings.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

GUI SSL Certificate & dropdown menu & The system uses a selfsigned () to
enable encrypted web interface connections. To change the default
certificate, select a different created or imported certificate.\
WebGUI IPv4 Address & dropdown menu & Choose a recent IP addresses to
limit the usage when accessing the web interface. The builtin HTTP
server binds to the wildcard address of (any address) and issues an
alert if the specified address becomes unavailable.\
WebGUI IPv6 Address & dropdown menu & Choose a recent IPv6 addresses to
limit the usage when accessing the web interface. The builtin HTTP
server binds to the wildcard address of (any address) and issues an
alert if the specified address becomes unavailable.\
WebGUI HTTP Port & integer & Allow configuring a nonstandard port for
accessing the web interface over HTTP. Changing this setting might
require changing a <span> </span>
(https://www.redbrick.dcu.ie/\~d\_fens/articles/Firefox:\_This\_Address\_is\_Restricted).\
WebGUI HTTPS Port & integer & Allow configuring a nonstandard port to
access the web interface over HTTPS.\
WebGUI HTTP &gt; HTTPS Redirect & checkbox & Redirect connections to . A
is required for . Activating this also sets the <span> </span>
(https://en.wikipedia.org/wiki/HTTP\_Strict\_Transport\_Security)
maximum age to seconds (one year). This means that after a browser
connects to the FreeNAS$^{\text{®}}$ web interface for the first time,
the browser continues to use HTTPS and renews this setting every year.\
Language & combo box & Select a language from the dropdown menu. The
list can be sorted by or <span> </span>
(https://en.wikipedia.org/wiki/List\_of\_ISO\_6391\_codes). View the
translated status of a language in the <span> </span>
(https://github.com/freenas/webui/tree/master/src/assets/i18n). Refer to
() for more information about assisting with translations.\
Console Keyboard Map & dropdown menu & Select a keyboard layout.\
Timezone & dropdown menu & Select a timezone.\
Syslog level & dropdown menu & When is defined, only logs matching this
level are sent.\
Syslog server & string & Remote syslog server DNS hostname or IP
address. Nonstandard port numbers can be used by adding a colon and the
port number to the hostname, like . Log entries are written to local
logs and sent to the remote syslog server.\
Crash reporting & checkbox & Send failed HTTP request data which can
include client and server IP addresses, failed method call tracebacks,
and middleware log file contents to iXsystems.\
Usage Collection & checkbox & Enable sending anonymous usage statistics
to iXsystems.\

After making any changes, click . Changes to any of the fields can
interrupt web interface connectivity while the new settings are applied.

This screen also contains these buttons:

\[\]

-   : save a backup copy of the current configuration database in the
    format to the computer accessing the web interface. Saving the
    configuration after making any configuration changes is
    highly recommended. FreeNAS$^{\text{®}}$ automatically backs up the
    configuration database to the system dataset every morning at 3:45.
    However, this backup does not occur if the system is shut down at
    that time. If the system dataset is stored on the boot pool and the
    boot pool becomes unavailable, the backup will also not
    be available. The location of the system dataset can be viewed or
    set using .

    <span>note</span><span>Note:</span> () keys are not stored in the
    configuration database and must be backed up separately. System host
    keys are files with names beginning with in . The root user keys are
    stored in .

    There are two types of passwords. User account passwords for the
    base operating system are stored as hashed values, do not need to be
    encrypted to be secure, and are saved in the system
    configuration backup. Other passwords, like iSCSI CHAP passwords,
    Active Directory bind credentials, and cloud credentials are stored
    in an encrypted form to prevent them from being visible as plain
    text in the saved system configuration. The key or for this
    encryption is normally stored only on the operating system device.
    When is chosen, a dialog gives two options. includes passwords in
    the configuration file which allows the configuration file to be
    restored to a different operating system device where the decryption
    seed is not already present. Configuration backups containing the
    seed must be physically secured to prevent decryption of passwords
    and unauthorized access.

    <span>warning</span><span>Warning:</span> The option is off by
    default and should only be used when making a configuration backup
    that will be stored securely. After moving a configuration to new
    hardware, media containing a configuration backup with a decryption
    seed should be securely erased before reuse.

    includes the encryption keys of encrypted pools in the
    configuration file. The encyrption keys are restored if the
    configuration file is uploaded to the system using .

-   : allows browsing to the location of a previously saved
    configuration file to restore that configuration.

-   : reset the configuration database to the default base version. This
    does not delete user SSH keys or any other data stored in a user
    home directory. Since configuration changes stored in the
    configuration database are erased, this option is useful when a
    mistake has been made or to return a test system to the
    original configuration.

NTP Servers {#\detokenize{system:ntp-servers}}
-----------

\[\]\[\] The network time protocol (NTP) is used to synchronize the time
on the computers in a network. Accurate time is necessary for the
successful operation of time sensitive applications such as Active
Directory or other directory services. By default, FreeNAS$^{\text{®}}$
is preconfigured to use three public NTP servers. If the network is
using a directory service, ensure that the FreeNAS$^{\text{®}}$ system
and the server running the directory service have been configured to use
the same NTP servers.

Available NTP servers can be found at . For time accuracy, choose NTP
servers that are geographically close to the physical location of the
FreeNAS$^{\text{®}}$ system.

Click and to add an NTP server. shows the configuration options.
summarizes the options available when adding or editing an NTP server.
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=ntp.conf)
explains these options in more detail.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Address & string & Enter the hostname or IP address of the NTP server.\
Burst & checkbox & Recommended when is greater than . Only use on
personal servers. use with a public NTP server.\
IBurst & checkbox & Speed up the initial synchronization, taking seconds
rather than minutes.\
Prefer & checkbox & This option is only recommended for highly accurate
NTP servers, such as those with time monitoring hardware.\
Min Poll & integer & The minimum polling interval, in seconds, as a
power of 2. For example, means 2\^6, or 64 seconds. The default is ,
minimum value is .\
Max Poll & integer & The maximum polling interval, in seconds, as a
power of 2. For example, means 2\^10, or 1,024 seconds. The default is ,
maximum value is .\
Force & checkbox & Force the addition of the NTP server, even if it is
currently unreachable.\

Boot {#\detokenize{system:boot}}
----

\[\]\[\] FreeNAS$^{\text{®}}$ supports a ZFS feature known as multiple
boot environments. With multiple boot environments, the process of
updating the operating system becomes a lowrisk operation. The updater
automatically creates a snapshot of the current boot environment and
adds it to the boot menu before applying the update.

If an update fails, reboot the system and select the previous boot
environment, using the instructions in (), to instruct the system to go
back to that system state.

<span>note</span><span>Note:</span> Boot environments are separate from
the configuration database. Boot environments are a snapshot of the at a
specified time. When a FreeNAS$^{\text{®}}$ system boots, it loads the
specified boot environment, or operating system, then reads the
configuration database to load the current configuration values. If the
intent is to make configuration changes rather than operating system
changes, make a backup of the configuration database first using the
instructions in ().

The example shown in , includes the two boot environments that are
created when FreeNAS$^{\text{®}}$ is installed. The boot environment can
be booted into if the system needs to be returned to a nonconfigured
version of the installation.

\[\]

Each boot environment entry contains this information:

-   the name of the boot entry as it will appear in the boot menu.
    Alphanumeric characters, dashes (), underscores (), and periods ()
    are allowed.

-   indicates which entry will boot by default if the user does not
    select another entry in the boot menu.

-   indicates the date and time the boot entry was created.

-   displays the size of the boot environment.

-   indicates whether or not this boot environment can be pruned if an
    update does not have enough space to proceed. Click (Options) and
    for an entry if that boot environment should not be
    automatically pruned.

Click (Options) on an entry to access actions specific to that entry:

-   only appears on entries which are not currently set to . Changes the
    selected entry to the default boot entry on next boot. The status
    changes to and the current entry changes from to , indicating that
    it was used on the last boot but will not be used on the next boot.

-   makes a new boot environment from the selected boot environment.
    When prompted for the name of the clone, alphanumeric characters,
    dashes (), underscores (), and periods () are allowed.

-   used to change the name of the boot environment. Alphanumeric
    characters, dashes (), underscores (), and periods () are allowed.

-   used to delete the highlighted entry, which also removes that entry
    from the boot menu. Since an activated entry cannot be deleted, this
    button does not appear for the active boot environment. To delete an
    entry that is currently activated, first activate another entry.
    Note that this button does not appear for the boot environment as
    this entry is needed to return the system to the original
    installation state.

-   used to toggle whether or not the updater can prune
    (automatically delete) this boot environment if there is not enough
    space to proceed with the update.

Click to:

-   make a new boot environment from the active environment. The active
    boot environment contains the text in the column. Only alphanumeric
    characters, underscores, and dashes are allowed in the .

-   display statistics for the operating system device: condition, total
    and used size, and date and time of the last scrub. By default, the
    operating system device is scrubbed every 7 days. To change the
    default, input a different number in the field and click .

-   display the status of each device in the operating system device,
    including any read, write, or checksum errors.

-   perform a manual scrub of the operating system device.

### Operating System Device Mirroring {#\detokenize{system:os-device-mirroring}}

\[\]\[\] is used to manage the devices comprising the operating system
device. An example is seen in .

\[\]

FreeNAS$^{\text{®}}$ supports 2device mirrors for the operating system
device. In a mirrored configuration, a failed device can be detached and
replaced.

An additional device can be attached to an existing onedevice operating
system device, with these caveats:

-   The new device must have at least the same capacity as the
    existing device. Larger capacity devices can be added, but the
    mirror will only have the capacity of the smallest device. Different
    models of devices which advertise the same nominal size are not
    necessarily the same actual size. For this reason, adding another
    device of the same model of is recommended.

-   It is to use SSDs rather than USB devices when creating a mirrored
    operating system device.

Click (Options) on a device entry to access actions specific to that
device:

-   use to add a second device to create a mirrored operating system
    device. If another device is available, it appears in the
    dropdown menu. Select the desired device. The option controls the
    capacity made available to the operating system device. By default,
    the new device is partitioned to the same size as the
    existing device. When is enabled, the entire capacity of the new
    device is used. If the original operating system device fails and is
    detached, the boot mirror will consist of just the newer drive, and
    will grow to whatever capacity it provides. However, new devices
    added to this mirror must now be as large as the new capacity. Click
    to attach the new disk to the mirror.

-   remove the failed device from the mirror so that it can be replaced.

-   once the failed device has been detached, select the new replacement
    device from the dropdown menu to rebuild the mirror.

Advanced {#\detokenize{system:advanced}}
--------

\[\] is shown in . The configurable settings are summarized in .

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Show Text Console without Password Prompt & checkbox & Set for the text
console to be available without entering a password.\
Enable Serial Console & checkbox & enable this option if the serial port
is disabled. Adds the and fields.\
Serial Port & string & Select the serial port address in hex.\
Serial Speed & dropdown menu & Select the speed in bps used by the
serial port.\
Swap size in GiB & nonzero number & By default, all data disks are
created with this amount of swap. This setting does not affect log or
cache devices as they are created without swap. Setting to disables swap
creation completely. This is discouraged.\
Enable autotune & checkbox & Enable the () script which attempts to
optimize the system based on the installed hardware. : Autotuning is
only used as a temporary measure and is not a permanent fix for system
hardware issues.\
Enable Debug Kernel & checkbox & Use a debug version of the kernel on
the next boot.\
Show console messages & checkbox & Display console messages from in real
time at bottom of browser window. Click the console to bring up a
scrollable screen. Set the option in the scrollable screen to pause
updates. Unset to continue watching messages as they occur. When this
option is set, a button to show the console log appears on busy spinner
dialogs.\
MOTD banner & string & This message is shown when a user logs in with
SSH.\
Show advanced fields by default & checkbox & Show fields by default.\
Use FQDN for logging & checkbox & Include the FullyQualified Domain Name
(FQDN) in logs to precisely identify systems with similar hostnames.\
ATA Security User & dropdown menu & User passed to for unlocking SEDs.
Values are or .\
SED Password & string & Global password used to unlock ().\
Reset SED Password & checkbox & Select to clear the column of .\

Click the button after making any changes.

This tab also contains this button:

: used to generate text files that contain diagnostic information. After
the debug data is collected, the system prompts for a location to save
the compressed file.

### Autotune {#\detokenize{system:autotune}}

\[\]\[\] FreeNAS$^{\text{®}}$ provides an autotune script which
optimizes the system depending on the installed hardware. For example,
if a pool exists on a system with limited RAM, the autotune script
automatically adjusts some ZFS sysctl values in an attempt to minimize
memory starvation issues. It should only be used as a temporary measure
on a system that hangs until the underlying hardware issue is addressed
by adding more RAM. Autotune will always slow such a system, as it caps
the ARC.

The option in is off by default. Enable this option to run the autotuner
at boot. To run the script immediately, reboot the system.

If the autotune script adjusts any settings, the changed values appear
in . Note that deleting tunables that were created by autotune only
affects the current session, as autotuneset tunables are recreated at
boot. This means that any autotuneset value that is manually changed
will revert back to the value set by autotune on reboot. To permanently
change a value set by autotune, change the description of the tunable.
For example, changing the description to prevents autotune from
reverting that tunable back to the autotune default value.

When attempting to increase the performance of the FreeNAS$^{\text{®}}$
system, and particularly when the current hardware may be limiting
performance, try enabling autotune.

For those who wish to see which checks are performed, the autotune
script is located in .

### SelfEncrypting Drives {#\detokenize{system:self-encrypting-drives}}

\[\]\[\] FreeNAS$^{\text{®}}$ version 11.1U5 introduced SelfEncrypting
Drive (SED) support.

These SED specifications are supported:

-   Legacy interface for older ATA devices.

-   <span> </span>
    (https://trustedcomputinggroup.org/wpcontent/uploads/Opal\_SSC\_1.00\_rev3.00Final.pdf)
    legacy specification

-   <span> </span>
    (https://trustedcomputinggroup.org/wpcontent/uploads/TCG\_StorageOpal\_SSC\_v2.01\_rev1.00.pdf)
    standard for newer consumergrade devices

-   <span> </span>
    (https://trustedcomputinggroup.org/wpcontent/uploads/TCG\_StorageOpalite\_SSC\_FAQ.pdf)
    is a reduced form of OPAL 2

-   TCG Pyrite <span> </span>
    (https://trustedcomputinggroup.org/wpcontent/uploads/TCG\_StoragePyrite\_SSC\_v1.00\_r1.00.pdf)
    and <span> </span>
    (https://trustedcomputinggroup.org/wpcontent/uploads/TCG\_StoragePyrite\_SSC\_v2.00\_r1.00\_PUB.pdf)
    are similar to Opalite, but hardware encryption is removed. Pyrite
    provides a logical equivalent of the legacy ATA security for
    nonATA devices. Only the drive firmware is used to protect
    the device.

    <span>danger</span><span>Danger:</span> Pyrite Version 1 SEDs do not
    have PSID support and

-   <span> </span>
    (https://trustedcomputinggroup.org/wpcontent/uploads/TCG\_StorageSSC\_Enterprisev1.01\_r1.00.pdf)
    is designed for systems with many data disks. These SEDs do not have
    the functionality to be unlocked before the operating system boots.

See this Trusted Computing Group$^{\text{®}}$ and NVM
Express$^{\text{®}}$ <span> </span>
(https://nvmexpress.org/wpcontent/uploads/TCGandNVMe\_Joint\_White\_PaperTCG\_Storage\_Opal\_and\_NVMe\_FINAL.pdf)
for more details about these specifications.

FreeNAS$^{\text{®}}$ implements the security capabilities of <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=camcontrol) for
legacy devices and <span> </span> (https://www.mankier.com/8/sedutilcli)
for TCG devices. When managing a SED from the command line, it is
recommended to use the wrapper script for to ease SED administration and
unlock the full capabilities of the device. Examples of using these
commands to identify and deploy SEDs are provided below.

A SED can be configured before or after assigning the device to a ().

By default, SEDs are not locked until the administrator takes ownership
of them. Ownership is taken by explicitly configuring a global or
perdevice password in the FreeNAS$^{\text{®}}$ web interface and adding
the password to the SEDs. Adding SED passwords to FreeNAS$^{\text{®}}$
also allows FreeNAS$^{\text{®}}$ to automatically unlock SEDs.

A passwordprotected SED protects the data stored on the device when the
device is physically removed from the FreeNAS$^{\text{®}}$ system. This
allows secure disposal of the device without having to first wipe the
contents. Repurposing a SED on another system requires the SED password.

#### Deploying SEDs {#\detokenize{system:deploying-seds}}

\[\] Run in the () to detect and list devices. The second column of the
results identifies the drive type:

-   indicates a nonSED device

-   indicates a legacy TCG OPAL 1 device

-   indicates a modern TCG OPAL 2 device

-   indicates a TCG Opalite device

-   indicates a TCG Pyrite 1 device

-   indicates a TCG Pyrite 2 device

-   indicates a TCG Enterprise device

Example:

\[commandchars=\
{}\] root@truenas1: sedutilcli scan Scanning for Opal compliant disks
/dev/ada0 No 32GB SATA Flash Drive SFDK003L /dev/ada1 No 32GB SATA Flash
Drive SFDK003L /dev/da0 No HGST HUS726020AL4210 A7J0 /dev/da1 No HGST
HUS726020AL4210 A7J0 /dev/da10 E WDC WUSTR1519ASS201 B925 /dev/da11 E
WDC WUSTR1519ASS201 B925

FreeNAS$^{\text{®}}$ supports setting a global password for all detected
SEDs or setting individual passwords for each SED. Using a global
password for all SEDs is strongly recommended to simplify deployment and
avoid maintaining separate passwords for each SED.

##### Setting a global password for SEDs {#\detokenize{system:setting-a-global-password-for-seds}}

\[\] Go to and enter the password.

Now the SEDs must be configured with this password. Go to the () and
enter , where is the global password entered in .

ensures that all detected SEDs are properly configured to use the
provided password:

\[commandchars=\
{}\] root@truenas1: sedhelper setup abcd1234 da9 \[OK\] da10 \[OK\] da11
\[OK\]

Rerun every time a new SED is placed in the system to apply the global
password to the new SED.

##### Creating separate passwords for each SED {#\detokenize{system:creating-separate-passwords-for-each-sed}}

\[\] Go to . Click (Options) for the confirmed SED, then . Enter and
confirm the password in the and fields.

The screen shows which disks have a configured SED password. The column
shows a mark when the disk has a password. Disks that are not a SED or
are unlocked using the global password are not marked in this column.

The SED must be configured to use the new password. Go to the () and
enter , where is the SED to configure and is the created password from .

This process must be repeated for each SED and any SEDs added to the
system in the future.

<span>danger</span><span>Danger:</span> Remember SED passwords! If the
SED password is lost, SEDs cannot be unlocked and their data is
unavailable. Always record SED passwords whenever they are configured or
modified and store them in a secure place!

#### Check SED Functionality {#\detokenize{system:check-sed-functionality}}

\[\] When SED devices are detected during system boot,
FreeNAS$^{\text{®}}$ checks for configured global and devicespecific
passwords.

Unlocking SEDs allows a pool to contain a mix of SED and nonSED devices.
Devices with individual passwords are unlocked with their password.
Devices without a devicespecific password are unlocked using the global
password.

To verify SED locking is working correctly, go to the (). Enter , where
is the SED and is the global or individual password for that SED. The
command returns , , and for drives with locking enabled:

\[commandchars=\
{}\] root@truenas1: sedutilcli listLockingRange 0 abcd1234 /dev/da9
Band\[0\]: Name: GlobalRange CommonName: Locking RangeStart: 0
RangeLength: 0 ReadLockEnabled: 1 WriteLockEnabled:1 ReadLocked: 0
WriteLocked: 0 LockOnReset: 1

#### Managing SED Passwords and Data {#\detokenize{system:managing-sed-passwords-and-data}}

\[\]\[\] This section contains command line instructions to manage SED
passwords and data. The command used is <span> </span>
(https://www.mankier.com/8/sedutilcli). Most SEDs are TCGE (Enterprise)
or TCGOpal (<span> </span>
(https://trustedcomputinggroup.org/wpcontent/uploads/TCG\_StorageOpal\_SSC\_v2.01\_rev1.00.pdf)).
Commands are different for the different drive types, so the first step
is identifying which type is being used.

<span>warning</span><span>Warning:</span> These commands can be
destructive to data and passwords. Keep backups and use the commands
with caution.

Check SED version on a single drive, in this example:

\[commandchars=\
{}\] root@truenas: sedutilcli isValidSED /dev/da0 /dev/da0 SED E
Micron5N/A U402

All connected disks can be checked at once:

\[commandchars=\
{}\] root@truenas: sedutilcli scan Scanning for Opal compliant disks
/dev/ada0 No 32GB SATA Flash Drive SFDK003L /dev/ada1 No 32GB SATA Flash
Drive SFDK003L /dev/da0 E Micron5N/A U402 /dev/da1 E Micron5N/A U402
/dev/da12 E SEAGATE XS3840TE70014 0103 /dev/da13 E SEAGATE XS3840TE70014
0103 /dev/da14 E SEAGATE XS3840TE70014 0103 /dev/da2 E Micron5N/A U402
/dev/da3 E Micron5N/A U402 /dev/da4 E Micron5N/A U402 /dev/da5 E
Micron5N/A U402 /dev/da6 E Micron5N/A U402 /dev/da9 E Micron5N/A U402 No
more disks present ending scan root@truenas:

##### TCGOpal Instructions {#\detokenize{system:tcg-opal-instructions}}

\[\] Reset the password without losing data:

Use of these commands to change the password without destroying data:

<span>0em</span>

Wipe data and reset password to default MSID:

Wipe data and reset password using the PSID: where is the PSID located
on the pysical drive with no dashes ().

##### TCGE Instructions {#\detokenize{system:tcg-e-instructions}}

\[\] Use of these commands to reset the password without losing data:

<span>0em</span>

Use of these commands to change the password without destroying data:

<span>0em</span>

Wipe data and reset password to default MSID:

<span>0em</span>

Wipe data and reset password using the PSID: where is the PSID located
on the pysical drive with no dashes ().

Email {#\detokenize{system:email}}
-----

\[\]\[\] An automatic script sends a nightly email to the user account
containing important information such as the health of the disks. ()
events are also emailed to the user account. Problems with () are
reported separately in an email sent at 03:00AM.

<span>note</span><span>Note:</span> () reports are mailed separately to
the address configured in that service.

The administrator typically does not read email directly on the
FreeNAS$^{\text{®}}$ system. Instead, these emails are usually sent to
an external email address where they can be read more conveniently. It
is important to configure the system so it can send these emails to the
administrator’s remote email account so they are aware of problems or
status changes.

The first step is to set the remote address where email will be sent. Go
to , click (Options) and for the user. In the field, enter the email
address on the remote system where email is to be sent, like . Click to
save the settings.

Additional configuration is performed with , shown in .

\[\]

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

From Email & string & The envelope From address shown in the email. This
can be set to make filtering mail on the receiving system easier.\
From Name & string & The friendly name to show in front of the sending
email address.\
Outgoing Mail Server & string or IP address & Hostname or IP address of
SMTP server used for sending this email.\
Mail Server Port & integer & SMTP port number. Typically , (secure
SMTP), or (submission).\
Security & dropdown menu & Choose an encryption type. Choices are , , or
.\
SMTP Authentication & checkbox & Enable or disable <span> </span>
(https://en.wikipedia.org/wiki/SMTP\_Authentication) using PLAIN SASL.
Setting this enables the required and optional fields.\
Username & string & Enter the SMTP username when the SMTP server
requires authentication.\
Password & string & Enter the SMTP account password if needed for
authentication. Only plain text characters (7bit ASCII) are allowed in
passwords. UTF or composed characters are not allowed.\

Click the button to verify that the configured email settings are
working. If the test email fails, doublecheck that the field of the user
is correctly configured by clicking the button for the account in .

Configuring email for TLS/SSL email providers is described in <span>
</span>
(https://forums.freenas.org/index.php?threads/areyouhavingtroublegettingfreenastoemailyouingmail.22517/).

System Dataset {#\detokenize{system:system-dataset}}
--------------

\[\]\[\] , shown in , is used to select the pool which contains the
persistent system dataset. The system dataset stores debugging core
files, () for encrypted pools, and Samba4 metadata such as the
user/group cache and share level permissions.

\[\]

Use the dropdown menu to select the volume (pool) to contain the system
dataset. The system dataset can be moved to unencrypted volumes (pools)
or encrypted volumes which do not have passphrases. If the system
dataset is moved to an encrypted volume, that volume is no longer
allowed to be locked or have a passphrase set.

Moving the system dataset also requires restarting the () service. A
dialog warns that the SMB service must be restarted, causing a temporary
outage of any active SMB connections.

System logs can also be stored on the system dataset. Storing this
information on the system dataset is recommended when large amounts of
data is being generated and the system has limited memory or a limited
capacity operating system device.

Set to store system logs on the system dataset. Leave unset to store
system logs in on the operating system device.

Click to save changes.

If the pool storing the system dataset is changed at a later time,
FreeNAS$^{\text{®}}$ migrates the existing data in the system dataset to
the new location.

<span>note</span><span>Note:</span> Depending on configuration, the
system dataset can occupy a large amount of space and receive frequent
writes. Do not put the system dataset on a flash drive or other media
with limited space or write life.

Reporting {#\detokenize{system:reporting}}
---------

\[\]\[\] This section contains settings to customize some of the
reporting tools. These settings are described in

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Report CPU usage in percent & checkbox & Report CPU usage in percent
instead of units of kernel time.\
Remote Graphite Server Hostname & string & Hostname or IP address of a
remote <span> </span> (http://graphiteapp.org/) server.\
Graph Age in Months & integer & Maximum time a graph is stored in months
(allowed values are ). Changing this value causes the dialog to appear.
Changes do not take effect until the existing reporting database is
destroyed.\
Number of Graph Points & integer & Number of points for each hourly,
daily, weekly, monthly, or yearly graph (allowed values are ). Changing
this value causes the checkbox to appear. Changes do not take effect
until the existing reporting database is destroyed.\

Changes to () clear the report history. To keep history with the old
settings, cancel the warning dialog. Click to restore the original
settings.

Alert Services {#\detokenize{system:alert-services}}
--------------

\[\]\[\] FreeNAS$^{\text{®}}$ can use a number of methods to notify the
administrator of system events that require attention. These events are
system ().

Available alert services:

-   <span> </span> (https://aws.amazon.com/sns/)

-   Email

-   <span> </span> (https://www.influxdata.com/)

-   <span> </span> (https://about.mattermost.com/)

-   <span> </span> (https://www.opsgenie.com/)

-   <span> </span> (https://www.pagerduty.com/)

-   <span> </span> (https://slack.com/)

-   <span> </span> (http://www.dpstele.com/snmp/trapbasics.php)

-   <span> </span> (https://victorops.com/)

<span>warning</span><span>Warning:</span> These alert services might use
a third party commercial vendor not directly affiliated with iXsystems.
Please investigate and fully understand that vendor’s pricing policies
and services before using their alert service. iXsystems is not
responsible for any charges incurred from the use of third party vendors
with the Alert Services feature.

Select to show the Alert Services screen, .

\[\]

Click to display the form, .

\[\]

Select the to choose an alert service to configure.

Alert services can be set for a particular severity . All alerts of that
level are then sent out with that alert service. For example, if the
alert service is set to , any level alerts are sent by that service.
Multiple alert services can be set to the same level. For instance,
alerts can be sent both by email and PagerDuty by setting both alert
services to the level.

The configurable fields and required information differ for each alert
service. Set to activate the service. Enter any other required
information and click .

Click to test the chosen alert service.

All saved alert services are displayed in . To delete an alert service,
click (Options) and . To disable an alert service temporarily, click
(Options) and , then unset the option.

Alert Settings {#\detokenize{system:alert-settings}}
--------------

\[\]\[\] has options to configure each FreeNAS$^{\text{®}}$ ().

\[\]

Alerts are grouped by web interface feature or service monitor. To
customize alert importance, use the dropdown. To adjust how often alert
notifications are sent, use the dropdown. Setting the to prevents that
alert from being added to alert notifications, but the alert can still
show in the web interface if it is triggered.

To configure where alert notifications are sent, use ().

Cloud Credentials {#\detokenize{system:cloud-credentials}}
-----------------

\[\]\[\] FreeNAS$^{\text{®}}$ can use cloud services for features like
(). The <span> </span> (https://rclone.org/) credentials to provide
secure connections with cloud services are entered here. Amazon S3,
Backblaze B2, Box, Dropbox, FTP, Google Cloud Storage, Google Drive,
HTTP, hubiC, Mega, Microsoft Azure Blob Storage, Microsoft OneDrive,
pCloud, SFTP, WebDAV, and Yandex are available.

<span>note</span><span>Note:</span> The hubiC cloud service has <span>
</span> (https://www.ovh.co.uk/subscriptionshubicended/).

<span>warning</span><span>Warning:</span> Cloud Credentials are stored
in encrypted form. To be able to restore Cloud Credentials from a (),
“Export Password Secret Seed” must be set when saving that
configuration.

Click to see the screen shown in .

\[\]

The list shows the and for each credential. There are options to and a
credential after clicking (Options) for a credential.

Click to add a new cloud credential. Choose a to display any specific
options for that provider. shows an example configuration:

\[\]

Enter a descriptive and unique name for the cloud credential in the
field. The remaining options vary by , and are shown in . Clicking a
provider name opens a new browser tab to the <span> </span>
(https://rclone.org/docs/) for that provider.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
Provider & Setting & Description\

\
Provider & Setting & Description\

\

<span> </span> (https://rclone.org/s3/) & Access Key ID & Enter the
Amazon Web Services Key ID. This is found on <span> </span>
(https://aws.amazon.com) by going through . Must be alphanumeric and
between 5 and 20 characters.\
<span> </span> (https://rclone.org/s3/) & Secret Access Key & Enter the
Amazon Web Services password. If the Secret Access Key cannot be found
or remembered, go to and create a new key pair. Must be alphanumeric and
between 8 and 40 characters.\
<span> </span> (https://rclone.org/s3/) & Endpoint URL & Set to access
this option. S3 API <span> </span>
(https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteEndpoints.html).
When using AWS, the endpoint field can be empty to use the default
endpoint for the region, and available buckets are automatically
fetched. Refer to the AWS Documentation for a list of <span> </span>
(https://docs.aws.amazon.com/general/latest/gr/rande.html\#s3\_website\_region\_endpoints).\
<span> </span> (https://rclone.org/s3/) & Region & <span> </span>
(https://docs.aws.amazon.com/general/latest/gr/randemanage.html). Leave
empty to automatically detect the correct public region for the bucket.
Entering a private region name allows interacting with Amazon buckets
created in that region. For example, enter to discover buckets created
in the eastern <span> </span>
(https://docs.aws.amazon.com/govcloudus/latest/UserGuide/whatis.html)
region.\
<span> </span> (https://rclone.org/s3/) & Disable Endpoint Region & Set
to access this option. Skip automatic detection of the region. Set this
when configuring a custom .\
<span> </span> (https://rclone.org/s3/) & Use Signature Version 2 & Set
to access this option. Force using <span> </span>
(https://docs.aws.amazon.com/general/latest/gr/signatureversion2.html)
to sign API requests. Set this when configuring a custom .\
<span> </span> (https://rclone.org/b2/) & Key ID, Application Key &
Alphanumeric <span> </span>
(https://www.backblaze.com/b2/cloudstorage.html) application keys. To
generate a new application key, log in to the Backblaze account, go to
the page, and add a new application key. Copy the and strings into the
FreeNAS$^{\text{®}}$ web interface fields.\
<span> </span> (https://rclone.org/box/) & Access Token & Configured
with ().\
<span> </span> (https://rclone.org/dropbox/) & Access Token & Configured
with (). The access token can be manually created by going to the
Dropbox <span> </span> (https://www.dropbox.com/developers/apps). After
creating an app, go to and click under the Generated access token
field.\
<span> </span> (https://rclone.org/ftp/) & Host, Port & Enter the FTP
host and port.\
<span> </span> (https://rclone.org/ftp/) & Username, Password & Enter
the FTP username and password.\
<span> </span> (https://rclone.org/googlecloudstorage/) & JSON Service
Account Key & Upload a Google <span> </span>
(https://rclone.org/googlecloudstorage/\#serviceaccountsupport). The
file is created with the <span> </span>
(https://console.cloud.google.com/apis/credentials).\
<span> </span> (https://rclone.org/drive/) & Access Token, Team Drive ID
& The is configured with (). is only used when connecting to a <span>
</span>
(https://developers.google.com/drive/api/v3/reference/teamdrives). The
ID is also the ID of the top level folder of the Team Drive.\
<span> </span> (https://rclone.org/http/) & URL & Enter the HTTP host
URL.\
<span> </span> (https://rclone.org/hubic/) & Access Token & Enter the
access token. See the <span> </span> (https://api.hubic.com/sandbox/)
for instructions to obtain an access token.\
<span> </span> (https://rclone.org/mega/) & Username, Password & Enter
the <span> </span> (https://mega.nz/) username and password.\
<span> </span> (https://rclone.org/azureblob/) & Account Name, Account
Key & Enter the Azure Blob Storage account name and key.\
<span> </span> (https://rclone.org/onedrive/) & Access Token, Drives
List, Drive Account Type, Drive ID & The is configured with ().
Authenticating a Microsoft account adds the and selects the correct .

The shows all the drives and IDs registered to the Microsoft account.
Selecting a drive automatically fills the field.\
<span> </span> (https://rclone.org/pcloud/) & Access Token & Configured
with ().\
<span> </span> (https://rclone.org/sftp/) & Host, Port, Username,
Password, Private Key ID & Enter the SFTP host and port. Enter an
account user name that has SSH access to the host. Enter the password
for that account import the private key from an existing (). To create a
new SSH key for this credential, open the dropdown and select .\
<span> </span> (https://rclone.org/webdav/) & URL, WebDAV service &
Enter the URL and use the dropdown to select the WebDAV service.\
<span> </span> (https://rclone.org/webdav/) & Username, Password & Enter
the username and password.\
<span> </span> (https://rclone.org/yandex/) & Access Token & Configured
with ().\

For Amazon S3, and values are found on the Amazon AWS website by
clicking on the account name, then and . Copy the Access Key value to
the FreeNAS$^{\text{®}}$ Cloud Credential field, then enter the value
saved when the key pair was created. If the Secret Key value is unknown,
a new key pair can be created on the same Amazon screen.

\[\] <span> </span> (https://openauthentication.org/) is used with some
cloud providers. These providers have a button that opens a dialog to
log in to that provider and fill the field with valid credentials.

Enter the information and click . displays when the credential
information is verified.

More details about individual settings are available in the <span>
</span> (https://rclone.org/about/).

SSH Connections {#\detokenize{system:ssh-connections}}
---------------

\[\]\[\] <span> </span>
(https://searchsecurity.techtarget.com/definition/SecureShell) is a
network protocol that provides a secure method to access and transfer
files between two hosts while using an unsecure network. SSH can use
user account credentials to establish secure connections, but often uses
key pairs shared between host systems for authentication.

FreeNAS$^{\text{®}}$ uses to quickly create SSH connections and show any
saved connections. These connections are required when creating a new ()
to back up dataset snapshots.

The remote system must be configured to allow SSH connections. Some
situations can also require allowing root account access to the remote
system. For FreeNAS$^{\text{®}}$ systems, go to and edit the () service
to allow SSH connections and root account access.

To add a new SSH connection, go to and click .

\[\]\[\]

\[t\]<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.64-2</span>|</span> Setting & Value & Description\
Name & string & Descriptive name of this SSH connection. SSH connection
names must be unique.\
Setup Method & dropdown menu & How to configure the connection:

requires configuring authentication on the remote system. This can
require copying SSH keys and modifying the user account on that system.
See ().

is only functional when configuring an SSH connection between
FreeNAS$^{\text{®}}$ systems. After authenticating the connection, all
remaining connection options are automatically configured. See ().\
Host & string & Enter the hostname or IP address of the remote system.
Only available with configurations.\
Port & integer & Port number on the remote system to use for the SSH
connection. Only available with configurations.\
FreeNAS URL & string & Hostname or IP address of the remote
FreeNAS$^{\text{®}}$ system. Only available with configurations. A valid
URL scheme is required. Example:\
Username & string & User account name to use for logging in to the
remote system\
Password & string & User account password used to log in to the
FreeNAS$^{\text{®}}$ system. Only available with configurations.\
Private Key & dropdown menu & Choose a saved () or select to create a
new keypair and apply it to this connection.\
Remote Host Key & string & Remote system SSH key for this system to
authenticate the connection. Only available with configurations. When
all other fields are properly configured, click to query the remote
system and automatically populate this field.\
Cipher & dropdown menu & Connection security level:

-   is most secure, but has the greatest impact on connection speed.

-   is less secure than but can give reasonable transfer rates for
    devices with limited cryptographic speed.

-   removes all security in favor of maximizing connection speed.
    Disabling the security should only be used within a secure,
    trusted network.

\
Connect Timeout & integer & Time (in seconds) before the system stops
attempting to establish a connection with the remote system.\

Saved connections can be edited or deleted. Deleting an SSH connection
also deletes or disables paired (), (), and ().

### Manual Setup {#\detokenize{system:manual-setup}}

\[\] Choosing to manually set up the SSH connection requires copying a
public encryption key from the local to remote system. This allows a
secure connection without a password prompt.

The examples here and in () refer to the FreeNAS$^{\text{®}}$ system
that is configuring a new connection in as . The FreeNAS$^{\text{®}}$
system that is receiving the encryption key is .

On , go to and create a new (). Highlight the entire text, rightclick in
the highlighted area, and click .

Log in to and go to . Click (Options) for the account, then . Paste the
copied key into the field and click as shown in .

\[\]

Switch back to and go to and click . Set the to , select the previously
created keypair as the , and fill in the rest of the connection details
for . Click to obtain the remote system key. Click to store this SSH
connection.

### SemiAutomatic Setup {#\detokenize{system:semi-automatic-setup}}

\[\] FreeNAS$^{\text{®}}$ offers a semiautomatic setup mode that
simplifies setting up an SSH connection with another FreeNAS or TrueNAS
system. When administrator account credentials are known for ,
semiautomatic setup allows configuring the SSH connection without
logging in to to transfer SSH keys.

In , go to and create a new (). Go to and click .

Choose as the . Enter the URL in using the format , where is the
hostname or IP address.

Enter credentials for an user account that can accept SSH connection
requests and modify . This is typically the account.

Select the SSH keypair that was just created for the .

Fill in the remaining connection configuration fields and click . can
use this saved configuration to establish a connection to and exchange
the remaining authentication keys.

SSH Keypairs {#\detokenize{system:ssh-keypairs}}
------------

\[\]\[\] FreeNAS$^{\text{®}}$ generates and stores <span>
ttps://en.wikipedia.org/wiki/RSA\_%28cryptosystem%29</span><span>RSAencrypted</span>
(https://en.wikipedia.org/wiki/RSA\_%28cryptosystem%29) SSH public and
private keypairs in . These are generally used when configuring () or
(). Encrypted keypairs or keypairs with passphrases are not supported.

To generate a new keypair, click , enter a name, and click . The and
fields fill with the key strings. SSH key pair names must be unique.

\[\]

Click to store the new keypair. These saved keypairs can be selected
later in the web interface wihout having to manually copy the key
values.

Keys are viewed or modified by going to and clicking (Options) and for
the keypair name.

Deleting an SSH Keypair also deletes any associated (). () or SFTP ()
that use this keypair are disabled but not removed.

Tunables {#\detokenize{system:tunables}}
--------

\[\]\[\] can be used to manage:

1.  a <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=sysctl)
    makes changes to the FreeBSD kernel running on a
    FreeNAS$^{\text{®}}$ system and can be used to tune the system.

2.  a loader is only loaded when a FreeBSDbased system boots and can be
    used to pass a parameter to the kernel or to load an additional
    kernel module such as a FreeBSD hardware driver.

3.  <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=rc.conf)
    is used to pass system configuration options to the system startup
    scripts as the system boots. Since FreeNAS$^{\text{®}}$ has been
    optimized for storage, not all of the services mentioned
    in rc.conf(5) are available for configuration. Note that in
    FreeNAS$^{\text{®}}$, customized rc.conf options are stored in .

<span>warning</span><span>Warning:</span> Adding a sysctl, loader, or
option is an advanced feature. A sysctl immediately affects the kernel
running the FreeNAS$^{\text{®}}$ system and a loader could adversely
affect the ability of the FreeNAS$^{\text{®}}$ system to successfully
boot.

Since sysctl, loader, and rc.conf values are specific to the kernel
parameter to be tuned, the driver to be loaded, or the service to
configure, descriptions and suggested values can be found in the man
page for the specific driver and in many sections of the <span> </span>
(https://www.freebsd.org/doc/en\_US.ISO88591/books/handbook/).

To add a loader, sysctl, or option, go to and click to access the screen
shown in .

\[\]

summarizes the options when adding a tunable.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Variable & string & The name of the sysctl or driver to load.\
Value & integer or string & Set a value for the . Refer to the man page
for the specific driver or the <span> </span>
(https://www.freebsd.org/doc/en\_US.ISO088591/books/handbook/) for
suggested values.\
Type & dropdown menu & Choices are , , and .\
Description & string & Optional. Enter a description of this tunable.\
Enabled & checkbox & Deselect this option to disable the tunable without
deleting it.\

<span>note</span><span>Note:</span> As soon as a is added or edited, the
running kernel changes that variable to the value specified. However,
when a or value is changed, it does not take effect until the system is
rebooted. Regardless of the type of tunable, changes persist at each
boot and across upgrades unless the tunable is deleted or the option is
deselected.

Existing tunables are listed in . To change the value of an existing
tunable, click (Options) and . To remove a tunable, click (Options) and
.

Restarting the FreeNAS$^{\text{®}}$ system after making sysctl changes
is recommended. Some sysctls only take effect at system startup, and
restarting the system guarantees that the setting values correspond with
what is being used by the running system.

The web interface does not display the sysctls that are preset when
FreeNAS$^{\text{®}}$ is installed. FreeNAS$^{\text{®}}$ 11.3 ships with
the sysctls set:

\[commandchars=\
{}\] kern.corefile=/var/tmp/N.core kern.metadelay=3 kern.dirdelay=4
kern.filedelay=5 kern.coredump=1 kern.sugidcoredump=1
vfs.timestampprecision=3 net.link.lagg.lacp.defaultstrictmode=0
vfs.zfs.minautoashift=12

as doing so may render the system unusable.

The web interface does not display the loaders that are preset when
FreeNAS$^{\text{®}}$ is installed. FreeNAS$^{\text{®}}$ 11.3 ships with
these loaders set:

\[commandchars=\
{}\] product=FreeNAS autobootdelay=5 loaderlogo=FreeNAS
loadermenutitle=Welcome to FreeNAS loaderbrand=FreeNAS loaderversion=
kern.cam.bootdelay=30000 debug.debuggeronpanic=1
debug.ddb.textdump.pending=1 hw.hptrr.attachgeneric=0
vfs.mountroot.timeout=30 ispfwload=YES ipmiload=YES
freenassysctlload=YES hint.isp.0.role=2 hint.isp.1.role=2
hint.isp.2.role=2 hint.isp.3.role=2
modulepath=/boot/kernel;/boot/modules;/usr/local/modules
net.inet6.ip6.autolinklocal=0 vfs.zfs.vol.mode=2
kern.geom.label.diskident.enable=0 kern.geom.label.ufs.enable=0
kern.geom.label.ufsid.enable=0 kern.geom.label.reiserfs.enable=0
kern.geom.label.ntfs.enable=0 kern.geom.label.msdosfs.enable=0
kern.geom.label.ext2fs.enable=0 hint.ahciem.0.disabled=1
hint.ahciem.1.disabled=1 kern.msgbufsize=524288 hw.mfi.mrsasenable=1
hw.usb.noshutdownwait=1 vfs.nfsd.fha.write=0
vfs.nfsd.fha.maxnfsdsperfh=32 vm.lowmemperiod=0

Changing the default tunables can make the system unusable.

The ZFS version used in 11.3 deprecates these tunables:

\[commandchars=\
{}\] kvfs.zfs.writelimitoverride vfs.zfs.writelimitinflated
vfs.zfs.writelimitmax vfs.zfs.writelimitmin vfs.zfs.writelimitshift
vfs.zfs.nowritethrottle

After upgrading from an earlier version of FreeNAS$^{\text{®}}$, these
tunables are automatically deleted. Please do not manually add them
back.

Update {#\detokenize{system:update}}
------

\[\] FreeNAS$^{\text{®}}$ has an integrated update system to make it
easy to keep up to date.

### Preparing for Updates {#\detokenize{system:preparing-for-updates}}

\[\] It is best to perform updates at times the FreeNAS$^{\text{®}}$
system is idle, with no clients connected and no scrubs or other disk
activity going on. Most updates require a system reboot. Plan updates
around scheduled maintenance times to avoid disrupting user activities.

The update process will not proceed unless there is enough free space in
the boot pool for the new update files. If a space warning is shown, go
to () to remove unneeded boot environments.

### Updates and Trains {#\detokenize{system:updates-and-trains}}

\[\] Cryptographically signed update files are used to update
FreeNAS$^{\text{®}}$. Update files provide flexibility in deciding when
to upgrade the system. Go to () to test an update.

FreeNAS$^{\text{®}}$ defines software branches, known as . There are
several trains available for updates, but the web interface only
displays trains that can be selected as an upgrade.

Update trains are labeled with a numeric version followed by a short
description. The current version receives regular bug fixes and new
features. Supported older versions of FreeNAS$^{\text{®}}$ only receive
maintenance updates. Several specific words are used to describe the
type of train:

-   Bug fixes and new features are available from this train. Upgrades
    available from a train are tested and ready to apply to a
    production environment.

-   Experimental train used for testing future versions of
    FreeNAS$^{\text{®}}$.

-   Software Developer Kit train. This has additional tools for testing
    and debugging FreeNAS$^{\text{®}}$.

<span>warning</span><span>Warning:</span> The UI will warn if the
currently selected train is not suited for production use. Before using
a nonproduction train, be prepared to experience bugs or problems.
Testers are encouraged to submit bug reports at .

### Checking for Updates {#\detokenize{system:checking-for-updates}}

\[\] shows an example of the screen.

\[\]

The system checks daily for updates and downloads an update if one is
available. An alert is issued when a new update becomes available. The
automatic check and download of updates is disabled by unsetting . Click
(Refresh) to perform another check for updates.

To change the train, use the dropdown menu to make a different
selection.

<span>note</span><span>Note:</span> The train selector does not allow
downgrades. For example, the STABLE train cannot be selected while
booted into a Nightly boot environment, or a 9.10 train cannot be
selected while booted into a 11 boot environment. To go back to an
earlier version after testing or running a more recent version, reboot
and select a boot environment for that earlier version. This screen can
then be used to check for updates that train.

In the example shown in , information about the update is displayed
along with a link to the . It is important to read the release notes
before updating to determine if any of the changes in that release
impact the use of the system.

\[\]

### Saving the Configuration File {#\detokenize{system:saving-the-configuration-file}}

\[\] A dialog to save the system () appears before installing updates.

<span>warning</span><span>Warning:</span> Keep the system configuration
file secure after saving it. The security information in the
configuration file could be used for unauthorized access to the
FreeNAS$^{\text{®}}$ system.

### Applying Updates {#\detokenize{system:applying-updates}}

Make sure the system is in a lowusage state as described above in ().

Click to immediately download and install an update.

The () dialog appears so the current configuration can be saved to
external media.

A confirmation window appears before the update is installed. When is
set and, clicking downloads, applies the updates, and then automatically
reboots the system. The update can be downloaded for a later manual
installation by unsetting the option.

is visible when an update is downloaded and ready to install. Click the
button to see a confirmation window. Setting and clicking installs the
update and reboots the system.

<span>warning</span><span>Warning:</span> Each update creates a boot
environment. If the update process needs more space, it attempts to
remove old boot environments. Boot environments marked with the
attribute as shown in () are not removed. If space for a new boot
environment is not available, the upgrade fails. Space on the operating
system device can be manually freed using . Review the boot environments
and remove the attribute or delete any boot environments that are no
longer needed.

### Manual Updates {#\detokenize{system:manual-updates}}

Updates can also be manually downloaded and applied in .

<span>note</span><span>Note:</span> Manual updates cannot be used to
upgrade from older major versions.

Go to and find an update file of the desired version. Manual update file
names end with .

Download the file to a desktop or laptop computer. Connect to
FreeNAS$^{\text{®}}$ with a browser and go to . Click .

The () dialog opens. This makes it possible to save a copy of the
current configuration to external media for backup in case of an update
problem.

After the dialog closes, the manual update screen is shown:

The current version of FreeNAS$^{\text{®}}$ is shown for verification.

Select the manual update file with the button. Set to reboot the system
after the update has been installed. Click to begin the update.

### Update in Progress {#\detokenize{system:update-in-progress}}

\[\] Starting an update shows a progress dialog. When an update is in
progress, the web interface shows an  icon in the top row. Dialogs also
appear in every active web interface session to warn that a system
update is in progress. interrupt a system update.

CAs {#\detokenize{system:cas}}
---

\[\]\[\] FreeNAS$^{\text{®}}$ can act as a Certificate Authority (CA).
When encrypting SSL or TLS connections to the FreeNAS$^{\text{®}}$
system, either import an existing certificate, or create a CA on the
FreeNAS$^{\text{®}}$ system, then create a certificate. This certificate
will appear in the dropdown menus for services that support SSL or TLS.

For secure LDAP, the public key of an existing CA can be imported with ,
or a new CA created on the FreeNAS$^{\text{®}}$ system and used on the
LDAP server also.

shows the screen after clicking .

\[\]

If the organization already has a CA, the CA certificate and key can be
imported. Click and set the to to see the configuration options shown in
. The configurable options are summarized in .

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Identifier & string & Enter a descriptive name for the CA using only
alphanumeric, underscore (), and dash () characters.\
Type & dropdown menu & Choose the type of CA. Choices are , , and .\
Certificate & string & Mandatory. Paste in the certificate for the CA.\
Private Key & string & If there is a private key associated with the ,
paste it here. Private keys must be at least 1024 bits long.\
Passphrase & string & If the is protected by a passphrase, enter it here
and repeat it in the “Confirm Passphrase” field.\

To create a new CA, first decide if it will be the only CA which will
sign certificates for internal use or if the CA will be part of a <span>
</span> (https://en.wikipedia.org/wiki/Root\_certificate).

To create a CA for internal use only, click and set the to . shows the
available options.

\[\]

The configurable options are described in . When completing the fields
for the certificate authority, supply the information for the
organization.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Identifier & string & Enter a descriptive name for the CA using only
alphanumeric, underscore (), and dash () characters.\
Type & dropdown menu & Choose the type of CA. Choices are , , and .\
Key Type & dropdown menu & Cryptosystem for the certificate authority
key. Choose between (<span> </span>
(https://en.wikipedia.org/wiki/RSA\_(cryptosystem))) and (<span> </span>
(https://en.wikipedia.org/wiki/Ellipticcurve\_cryptography))
encryption.\
EC Curve & dropdown menu & Elliptic curve to apply to the certificate
authority key. Choose from different or curve parameters. See <span>
</span> (https://tools.ietf.org/html/rfc5639) and <span> </span>
(http://www.secg.org/sec2v2.pdf) for more details. Applies to keys
only.\
Key Length & dropdown menu & For security reasons, a minimum of is
recommended. Applies to keys only.\
Digest Algorithm & dropdown menu & The default is acceptable unless the
organization requires a different algorithm.\
Lifetime & integer & The lifetime of a CA is specified in days.\
Country & dropdown menu & Select the country for the organization.\
State & string & Enter the state or province of the organization.\
Locality & string & Enter the location of the organization.\
Organization & string & Enter the name of the company or organization.\
Organizational Unit & string & Organizational unit of the entity.\
Email & string & Enter the email address for the person responsible for
the CA.\
Common Name & string & Enter the fullyqualified hostname (FQDN) of the
system. The be unique within a certificate chain.\
Subject Alternate Names & string & Multidomain support. Enter additional
space separated domain names.\

To create an intermediate CA which is part of a certificate chain, set
the to . This screen adds one more option to the screen shown in :

-   this dropdown menu is used to specify the root CA in the
    certificate chain. This CA must first be imported or created.

Imported or created CAs are added as entries in . The columns in this
screen indicate the name of the CA, whether it is an internal CA,
whether the issuer is selfsigned, the CA lifetime (in days), the common
name of the CA, the date and time the CA was created, and the date and
time the CA expires.

Click (Options) on an existing CA to access these configuration buttons:

-   use this option to view the contents of an existing , , or to edit
    the .

-   used to sign internal Certificate Signing Requests created using .
    Signing a request adds a new certificate to .

-   prompts to browse to the location to save a copy of the CA’s X.509
    certificate on the computer being used to access the
    FreeNAS$^{\text{®}}$ system.

-   prompts to browse to the location to save a copy of the CA’s private
    key on the computer being used to access the
    FreeNAS$^{\text{®}}$ system. This option only appears if the CA has
    a private key.

-   prompts for confirmation before deleting the CA.

Certificates {#\detokenize{system:certificates}}
------------

\[\]\[\] FreeNAS$^{\text{®}}$ can import existing certificates or
certificate signing requests, create new certificates, and issue
certificate signing requests so that created certificates can be signed
by the CA which was previously imported or created in ().

Go to to add or view certificates.

\[\]

FreeNAS$^{\text{®}}$ uses a selfsigned certificate to enable encrypted
access to the web interface. This certificate is generated at boot and
cannot be deleted until a different certificate is chosen as the ().

To import an existing certificate, click and set the to . shows the
options. When importing a certificate chain, paste the primary
certificate, followed by any intermediate certificates, followed by the
root CA certificate.

The configurable options are summarized in .

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Identifier & string & Enter a descriptive name for the certificate using
only alphanumeric, underscore (), and dash () characters.\
Type & dropdown menu & Choose the type of certificate. Choices are , , ,
and .\
CSR exists on this system & checkbox & Set when the certificate being
imported already has a Certificate Signing Request (CSR) on the system.\
Signing Certificate Authority & dropdown menu & Select a previously
created or imported CA. Active when is set.\
Certificate & string & Paste the contents of the certificate.\
Private Key & string & Paste the private key associated with the
certificate. Private keys must be at least 1024 bits long. Active when
is unset.\
Passphrase & string & If the private key is protected by a passphrase,
enter it here and repeat it in the field. Active when is unset.\

Importing a certificate signing request requires copying the contents of
the signing request and key files into the form. Having the signing
request and strings visible in a separate window simplifies the import
process.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Identifier & string & Enter a descriptive name for the certificate using
only alphanumeric, underscore (), and dash () characters.\
Type & dropdown menu & Choose the type of certificate. Choices are , , ,
and .\
Signing Request & dropdown menu & Paste the string from the signing
request.\
Private Key & string & Paste the private key associated with the
certificate signing request. Private keys must be at least 1024 bits
long.\
Passphrase & string & If the private key is protected by a passphrase,
enter it here and repeat it in the field.\

To create a new selfsigned certificate, set the to to see the options
shown in . The configurable options are summarized in . When completing
the fields for the certificate authority, use the information for the
organization. Since this is a selfsigned certificate, use the CA that
was imported or created with () as the signing authority.

\[\]

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Identifier & string & Enter a descriptive name for the certificate using
only alphanumeric, underscore (), and dash () characters.\
Type & dropdown menu & Choose the type of certificate. Choices are , ,
and .\
Signing Certificate Authority & dropdown menu & Select the CA which was
previously imported or created using ().\
Key Type & dropdown menu & Cryptosystem for the certificate key. Choose
between (<span> </span>
(https://en.wikipedia.org/wiki/RSA\_(cryptosystem))) and (<span> </span>
(https://en.wikipedia.org/wiki/Ellipticcurve\_cryptography))
encryption.\
EC Curve & dropdown menu & Elliptic curve to apply to the certificate
key. Choose from different or curve parameters. See <span> </span>
(https://tools.ietf.org/html/rfc5639) and <span> </span>
(http://www.secg.org/sec2v2.pdf) for more details. Applies to keys
only.\
Key Length & dropdown menu & For security reasons, a minimum of is
recommended. Applies to keys only.\
Digest Algorithm & dropdown menu & The default is acceptable unless the
organization requires a different algorithm.\
Lifetime & integer & The lifetime of the certificate is specified in
days.\
Country & dropdown menu & Select the country for the organization.\
State & string & State or province of the organization.\
Locality & string & Location of the organization.\
Organization & string & Name of the company or organization.\
Organizational Unit & string & Organizational unit of the entity.\
Email & string & Enter the email address for the person responsible for
the CA.\
Common Name & string & Enter the fullyqualified hostname (FQDN) of the
system. The be unique within a certificate chain.\
Subject Alternate Names & string & Multidomain support. Enter additional
domain names and separate them with a space.\

If the certificate is signed by an external CA, such as Verisign,
instead create a certificate signing request. To do so, set the to . The
options from display, but without the and fields.

Certificates that are imported, selfsigned, or for which a certificate
signing request is created are added as entries to . In the example
shown in , a selfsigned certificate and a certificate signing request
have been created for the fictional organization . The selfsigned
certificate was issued by the internal CA named and the administrator
has not yet sent the certificate signing request to Verisign so that it
can be signed. Once that certificate is signed and returned by the
external CA, it should be imported with a new certificate set to . This
makes the certificate available as a configurable option for encrypting
connections.

\[\]

Clicking (Options) for an entry shows these configuration buttons:

-   use this option to view the contents of an existing , , or to edit
    the .

-   use an () authenticator to verify, issue, and renew a certificate.
    Only visible with certificate signing requests.

-   saves a copy of the certificate or certificate signing request to
    the system being used to access the FreeNAS$^{\text{®}}$ system. For
    a certificate signing request, send the exported certificate to the
    external signing authority so that it can be signed.

-   saves a copy of the private key associated with the certificate or
    certificate signing request to the system being used to access the
    FreeNAS$^{\text{®}}$ system.

-   is used to delete a certificate or certificate signing request.

### ACME Certificates {#\detokenize{system:acme-certificates}}

\[\] <span> </span>
(https://ietfwgacme.github.io/acme/draftietfacmeacme.html) is available
for automating certificate issuing and renewal. The user must verify
ownership of the domain before certificate automation is allowed.

ACME certificates can be created for existing certificate signing
requests. These certificates use an () authenticator to confirm domain
ownership, then are automatically issued and renewed. To create a new
ACME certificate, go to , click (Options) for an existing certificate
signing request, and click .

\[\]

<span>|&gt;p<span>0.22-2</span> |&gt;p<span>0.15-2</span>
|&gt;p<span>0.62-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Identifier & string & Internal identifier of the certificate. Only
alphanumeric characters, dash (), and underline () are allowed.\
Terms of Service & checkbox & Please accept the terms of service for the
given ACME Server.\
Renew Certificate Day & integer & Number of days to renew certificate
before expiring.\
ACME Server Directory URI & dropdown menu & URI of the ACME Server
Directory. Choose a preconfigured URI or enter a custom URI.\
Authenticator for {Domain Name} ({Domain Name} dynamically changes) &
dropdown menu & Authenticator to validate the Domain. Choose a
previously configured () authenticator.\

ACME DNS {#\detokenize{system:acme-dns}}
--------

\[\]\[\] Go to and click to show options to add a new DNS authenticator
to FreeNAS$^{\text{®}}$. This is used to create () that are
automatically issued and renewed after being validated.

\[\]

Enter a name for the authenticator. This is only used to identify the
authenticator in the FreeNAS$^{\text{®}}$ web interface. Choose a DNS
provider and configure any required :

-   Amazon DNS web service. Requires entering an Amazon account and .
    See the <span> </span>
    (https://docs.aws.amazon.com/IAM/latest/UserGuide/id\_credentials\_accesskeys.html)
    for more details about generating these keys.

Click to register the DNS Authenticator and add it to the list of
authenticator options for ().

Support {#\detokenize{system:support}}
-------

\[\]\[\] The FreeNAS$^{\text{®}}$ option, shown in , provides a builtin
ticketing system for generating bug reports and feature requests.

\[\]

This screen provides a builtin interface to the FreeNAS$^{\text{®}}$
issue tracker located at .

An account is required to create tickets and receive notifications as
issues are addressed.

Log in to an existing account to enter an issue. If you do not have an
account yet, go to , click , and fill out the form. Reply to the
registration email to validate the account before logging in.

Before creating a bug report or feature request, ensure that an existing
report does not already exist at . If a similar issue is already present
and has not been marked or , comment on that issue, adding new
information to help solve it. When similar issues are or , create a new
issue and refer to the previous issue.

<span>note</span><span>Note:</span> Update the system to the latest
version of STABLE and retest before reporting an issue. Newer versions
of the software might have already fixed the problem.

To generate a report using the builtin screen, complete these fields:

-   enter the login name created when registering at .

-   enter the password associated with the registered login name.

-   select when reporting an issue or when requesting a new feature.

-   this dropdown menu is empty until a registered and are entered. The
    field remains empty if either value is incorrect. After the and are
    validated, possible categories are populated to the dropdown menu.
    Select the one that best describes the bug or feature
    being reported.

-   enabling this option is recommended so an overview of the system
    hardware, build string, and configuration is automatically generated
    and included with the ticket. Generating and attaching a debug to
    the ticket can take some time.

    Debug file attachments are limited to 20 MiB. If the debug file is
    too large to include, unset the option to generate the debug file
    and let the system create an issue ticket as shown below. Manually
    create a debug file by going to and clicking .

    Go to the ticket at and add the debug file as a private attachment.

-   enter a descriptive title for the ticket. A good makes it easy to
    find similar reports.

-   enter a one to threeparagraph summary of the issue that describes
    the problem, and if applicable, what steps can be taken to
    reproduce it.

-   select screenshots on the client system to include with the report.

Click to automatically generate and upload the report to the issue
tracker (). This process can take several minutes while information is
collected and sent. All files included with the report are added to the
issue tracker ticket as private attachments and can only be accessed by
the creator of the ticket and iXsystems.

After the new ticket is created, the ticket URL is shown for viewing or
updating with more information.

Tasks {#\detokenize{tasks:tasks}}
=====

\[\]\[\]\[\] The Tasks section of the web interface is used to configure
repetitive tasks:

-   () schedules a command or script to automatically execute at a
    specified time

-   () configures a command or script to automatically execute during
    system startup or shutdown

-   () schedules data synchronization to another system

-   () schedules disk tests

-   () schedules automatic creation of filesystem snapshots

-   () automate the replication of snapshots to a remote system

-   () controls the priority of resilvers

-   () schedules scrubs as part of ongoing disk maintenance

-   () schedules data synchronization to cloud providers

Each of these tasks is described in more detail in this section.

<span>note</span><span>Note:</span> By default, () are run once a month
by an automaticallycreated task. () and () must be set up manually.

Cron Jobs {#\detokenize{tasks:cron-jobs}}
---------

\[\]\[\] <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=cron)
is a daemon that runs a command or script on a regular schedule as a
specified user.

Go to and click to create a cron job.

\[\]

lists the configurable options for a cron job.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Description & string & Enter a description of the cron job.\
Command & dropdown menu & Enter the to the command or script to be run.
If it is a script, testing it at the command line first is recommended.\
Run As User & string & Select a user account to run the command. The
user must have permissions allowing them to run the command or script.
Output from executing a cron task is emailed to this user if has been
configured for that ().\
Schedule & dropdown menu & Select a schedule preset or choose to open
the advanced scheduler. Note that an inprogress cron task postpones any
later scheduled instance of the same task until the running task is
complete.\
Hide Standard Output & checkbox & Hide standard output (stdout) from the
command. When unset, any standard output is mailed to the user account
cron used to run the command.\
Hide Standard Error & checkbox & Hide error output (stderr) from the
command. When unset, any error output is mailed to the user account cron
used to run the command.\
Enable & checkbox & Set to allow this scheduled cron task to activate.
Unsetting this option disables the cron task without deleting it.\

Cron jobs are shown in . This table displays the user, command,
description, schedule, and whether the job is enabled. This table is
adjustable by setting the different column checkboxes above it. Set to
display all options in the table. Click (Options) for to show the , ,
and options.

<span>note</span><span>Note:</span> symbols are automatically escaped
and do not need to be prefixed with backslashes. For example, use in a
cron job to generate a filename based on the date.

Init/Shutdown Scripts {#\detokenize{tasks:init-shutdown-scripts}}
---------------------

\[\] FreeNAS$^{\text{®}}$ provides the ability to schedule commands or
scripts to run at system startup or shutdown.

Go to and click .

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Type & dropdown menu & Select for an executable or for an executable
script.\
Command or Script & string & If is selected, enter the command with any
options. When is selected, click (Browse) to select the script from an
existing pool.\
When & dropdown menu & Select when the or runs:

-   : early in the boot process, after mounting filesystems and starting
    networking

-   : at the end of the boot process, before FreeNAS$^{\text{®}}$
    services start

-   : during the system power off process.

\
Enabled & checkbox & Enable this task. Unset to disable the task without
deleting it.\
Timeout & integer & Automatically stop the script or command after the
specified number of seconds.\

Scheduled commands must be in the default path. The full path to the
command can also be included in the entry. The path can be tested with
in the (). When available, the path to the command is shown:

\[commandchars=\
{}\] \[root@freenas \] which ls /bin/ls

When scheduling a script, test the script first to verify it is
executable and achieves the desired results.

<span>note</span><span>Note:</span> Init/shutdown scripts are run with .

Init/Shutdown tasks are shown in . Click (Options) for a task to or that
task.

Rsync Tasks {#\detokenize{tasks:rsync-tasks}}
-----------

\[\]\[\] <span> </span> (https://www.samba.org/ftp/rsync/rsync.html) is
a utility that copies specified data from one system to another over a
network. Once the initial data is copied, rsync reduces the amount of
data sent over the network by sending only the differences between the
source and destination files. Rsync is used for backups, mirroring data
on multiple systems, or for copying files between systems.

Rsync is most effective when only a relatively small amount of the data
has changed. There are also <span> </span>
(https://forums.freenas.org/index.php?threads/impairedrsyncpermissionssupportforwindowsdatasets.43973/).
For large amounts of data, data that has many changes from the previous
copy, or Windows files, () are often the faster and better solution.

Rsync is singlethreaded and gains little from multiple processor cores.
To see whether rsync is currently running, use from the ().

Both ends of an rsync connection must be configured:

-   this system pulls (receives) the data. This system is referred to as
    in the configuration examples.

-   this system pushes (sends) the data. This system is referred to as
    in the configuration examples.

FreeNAS$^{\text{®}}$ can be configured as either an or an . The opposite
end of the connection can be another FreeNAS$^{\text{®}}$ system or any
other system running rsync. In FreeNAS$^{\text{®}}$ terminology, an
defines which data is synchronized between the two systems. To
synchronize data between two FreeNAS$^{\text{®}}$ systems, create the on
the .

FreeNAS$^{\text{®}}$ supports two modes of rsync operation:

-   exports a directory tree, and the configured settings of the tree as
    a symbolic name over an unencrypted connection. This mode requires
    that at least one module be defined on the rsync server. It can be
    defined in the FreeNAS$^{\text{®}}$ web interface under . In other
    operating systems, the module is defined in <span>
    </span> (https://www.samba.org/ftp/rsync/rsyncd.conf.html).

-   synchronizes over an encrypted connection. Requires the
    configuration of SSH user and host public keys.

This section summarizes the options when creating an rsync task. It then
provides a configuration example between two FreeNAS$^{\text{®}}$
systems for each mode of rsync operation.

<span>note</span><span>Note:</span> If there is a firewall between the
two systems or if the other system has a builtin firewall, make sure
that TCP port 873 is allowed.

shows the screen that appears after navigating to and clicking .
summarizes the configuration options available when creating an rsync
task.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Path & browse button & to the path to be copied. FreeNAS$^{\text{®}}$
verifies that the remote path exists. () apply on the
FreeNAS$^{\text{®}}$ system. Other operating systems can have different
limits which might affect how they can be used as sources or
destinations.\
User & dropdown menu & Select the user to run the rsync task. The user
selected must have permissions to write to the specified directory on
the remote host.\
Remote Host & string & Enter the IP address or hostname of the remote
system that will store the copy. Use the format if the username differs
on the remote host.\
Remote SSH Port & integer & Only available in mode. Allows specifying an
SSH port other than the default of .\
Rsync mode & dropdown menu & The choices are mode or mode.\
Remote Module Name & string & At least one module must be defined in
<span> </span> (https://www.samba.org/ftp/rsync/rsyncd.conf.html) of the
rsync server or in the of another system.\
Remote Path & string & Only appears when using mode. Enter the path on
the remote host to sync with, for example, . Note that the path length
cannot be greater than 255 characters.\
Validate Remote Path & checkbox & Verifies the existence of the .\
Direction & dropdown menu & Direct the flow of the data to the remote
host. Choices are or . Default is to push to a remote host.\
Short Description & string & Enter a description of the rsync task.\
Schedule the Rsync Task & dropdown menu & Choose how often to run the
task. Choices are , , , , or . Selecting opens the ().\
Recursive & checkbox & Set to include all subdirectories of the
specified directory. When unset, only the specified directory is
included.\
Times & checkbox & Set to preserve the modification times of files.\
Compress & checkbox & Set to reduce the size of the data to transmit.
Recommended for slow connections.\
Archive & checkbox & When set, rsync is run recursively, preserving
symlinks, permissions, modification times, group, and special files.
When run as root, owner, device files, and special files are also
preserved. Equivalent to .\
Delete & checkbox & Set to delete files in the destination directory
that do not exist in the source directory.\
Quiet & checkbox & Suppress rsync task status ().\
Preserve permissions & checkbox & Set to preserve original file
permissions. This is useful when the user is set to .\
Preserve extended attributes & checkbox & <span> </span>
(https://en.wikipedia.org/wiki/Extended\_file\_attributes) are
preserved, but must be supported by both systems.\
Delay Updates & checkbox & Set to save the temporary file from each
updated file to a holding directory until the end of the transfer when
all transferred files are renamed into place.\
Extra options & string & Additional <span> </span>
(http://rsync.samba.org/ftp/rsync/rsync.html) options to include. Note:
The character must be escaped with a backslash () or used inside single
quotes. ()\
Enabled & checkbox & Enable this rsync task. Unset to disable this rsync
task without deleting it.\

If the rysnc server requires password authentication, enter in the
field, replacing with the appropriate path to the file containing the
password.

Created rsync tasks are listed in . Click (Options) for an entry to
display buttons for , , or .

The column shows the status of the rsync task. To view the detailed
rsync logs for a task, click the entry when the task is running or
finished.

Rsync tasks also generate an () on task completion. The alert shows if
the task succeeded or failed.

### Rsync Module Mode {#\detokenize{tasks:rsync-module-mode}}

\[\] This configuration example configures rsync module mode between the
two following FreeNAS$^{\text{®}}$ systems:

-   has existing data in . It will be the rsync client, meaning that an
    rsync task needs to be defined. It will be referred to as

-   has an existing pool named . It will be the rsync server, meaning
    that it will receive the contents of . An rsync module needs to be
    defined on this system and the rsyncd service needs to be started.
    It will be referred to as

On , an rsync task is defined in , . In this example:

-   the points to , the directory to be copied

-   the points to , the IP address of the rsync server

-   the is

-   the is ; this will need to be defined on the rsync server

-   the is

-   the rsync is scheduled to occur every 15 minutes

-   the is set to so it has permission to write anywhere

-   the option is enabled so that the original permissions are not
    overwritten by the user

On , an rsync module is defined in , . In this example:

-   the is ; this needs to match the setting on the rsync client

-   the is ; a directory called will be created to hold the contents of

-   the is set to so it has permission to write anywhere

Descriptions of the configurable options can be found in ().

-   is set to , the IP address of the rsync client

To finish the configuration, start the rsync service on in . If the
rsync is successful, the contents of will be mirrored to .

### Rsync over SSH Mode {#\detokenize{tasks:rsync-over-ssh-mode}}

\[\] SSH replication mode does not require the creation of an rsync
module or for the rsync service to be running on the rsync server. It
does require SSH to be configured before creating the rsync task:

-   a public/private key pair for the rsync user account (typically )
    must be generated on and the public key copied to the same user
    account on

-   to mitigate the risk of maninthemiddle attacks, the public host key
    of must be copied to

-   the SSH service must be running on

To create the public/private key pair for the rsync user account, open
() on and run . This example generates an RSA type public/private key
pair for the user. When creating the key pair, do not enter the
passphrase as the key is meant to be used for an automated task.

\[commandchars=\
{}\] sshkeygen t rsa Generating public/private rsa key pair. Enter file
in which to save the key (/root/.ssh/idrsa): Created directory
/root/.ssh. Enter passphrase (empty for no passphrase): Enter same
passphrase again: Your identification has been saved in
/root/.ssh/idrsa. Your public key has been saved in
/root/.ssh/idrsa.pub. The key fingerprint is:
f5:b0:06:d1:33:e4:95:cf:04:aa:bb:6e:a4:b7:2b:df root@freenas.local The
keys randomart image is: +\[ RSA 2048\]+ | .o. oo | | o+o. . | | . =o +
| | + + o | | S o . | | .o | | o. | | o oo | | \*\*oE | || | | ||

FreeNAS$^{\text{®}}$ supports RSA keys for SSH. When creating the key,
use to specify this type of key. Refer to <span> </span>
(https://www.freebsd.org/doc/en\_US.ISO88591/books/handbook/openssh.html\#securitysshkeygen)
for more information.

<span>note</span><span>Note:</span> If a different user account is used
for the rsync task, use the command after mounting the filesystem but
before generating the key. For example, if the rsync task is configured
to use the user account, use this command to become that user:

\[commandchars=\
{}\] su user1

Next, view and copy the contents of the generated public key:

\[commandchars=\
{}\] more .ssh/idrsa.pub sshrsa
AAAAB3NzaC1yc2EAAAADAQABAAABAQC1lBEXRgw1W8y8k+lXPlVR3xsmVSjtsoyIzV/PlQPo
SrWotUQzqILq0SmUpViAAv4Ik3T8NtxXyohKmFNbBczU6tEsVGHo/2BLjvKiSHRPHc/1DX9hofcFti4h
dcD7Y5mvU3MAEeDClt02/xoi5xS/RLxgP0R5dNrakw958Yn001sJS9VMf528fknUmasti00qmDDcp/kO
xT+S6DFNDBy6IYQN4heqmhTPRXqPhXqcD1G+rWr/nZK4H8Ckzy+l9RaEXMRuTyQgqJB/rsRcmJX5fApd
DmNfwrRSxLjDvUzfywnjFHlKk/+TQIT1gg1QQaj21PJD9pnDVF0AiJrWyWnR
root@freenas.local

Go to and paste (or append) the copied key into the field of (Options) ,
or the username of the specified rsync user account. The paste for the
above example is shown in . When pasting the key, ensure that it is
pasted as one long line and, if necessary, remove any extra spaces
representing line breaks.

\[\]

While on , verify that the SSH service is running in and start it if it
is not.

Next, copy the host key of using Shell on . The command copies the RSA
host key of the server used in our previous example. Be sure to include
the double bracket to prevent overwriting any existing entries in the
file:

\[commandchars=\
{}\] sshkeyscan t rsa 192.168.2.6 /root/.ssh/knownhosts

<span>note</span><span>Note:</span> If is a Linux system, use this
command to copy the RSA key to the Linux system:

\[commandchars=\
{}\] cat /.ssh/idrsa.pub | ssh user@192.168.2.6 cat .ssh/authorizedkeys

The rsync task can now be created on . To configure rsync SSH mode using
the systems in our previous example, the configuration is:

-   the points to , the directory to be copied

-   the points to , the IP address of the rsync server

-   the is

-   the rsync is scheduled to occur every 15 minutes

-   the is set to so it has permission to write anywhere; the public key
    for this user must be generated on and copied to

-   the option is enabled so that the original permissions are not
    overwritten by the user

Save the rsync task and the rsync will automatically occur according to
the schedule. In this example, the contents of will automatically appear
in after 15 minutes. If the content does not appear, use Shell on to
read . If the message indicates a (newline character) in the key, remove
the space in the pasted key–it will be after the character that appears
just before the in the error message.

S.M.A.R.T. Tests {#\detokenize{tasks:s-m-a-r-t-tests}}
----------------

\[\]\[\] <span> </span> (https://en.wikipedia.org/wiki/S.M.A.R.T.)
(SelfMonitoring, Analysis and Reporting Technology) is a monitoring
system for computer hard disk drives to detect and report on various
indicators of reliability. Replace the drive when a failure is
anticipated by S.M.A.R.T. Most modern ATA, IDE, and SCSI3 hard drives
support S.M.A.R.T. – refer to the drive documentation for confirmation.

Click and to add a new scheduled S.M.A.R.T. test. shows the
configuration screen that appears. Tests are listed under . After
creating tests, check the configuration in , then click the power button
for the S.M.A.R.T. service in to activate the service. The S.M.A.R.T.
service will not start if there are no pools.

<span>note</span><span>Note:</span> To prevent problems, do not enable
the S.M.A.R.T. service if the disks are controlled by a RAID controller.
It is the job of the controller to monitor S.M.A.R.T. and mark drives as
Predictive Failure when they trip.

\[\]

summarizes the configurable options when creating a S.M.A.R.T. test.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

All Disks & checkbox & Set to monitor all disks.\
Disks & dropdown menu & Select the disks to monitor. Available when is
unset.\
Type & dropdown menu & Choose the test type. See <span> </span>
(https://www.smartmontools.org/browser/trunk/smartmontools/smartctl.8.in)
for descriptions of each type. Some test types will degrade performance
or take disks offline. Avoid scheduling S.M.A.R.T. tests simultaneously
with scrub or resilver operations.\
Short description & string & Optional. Enter a description of the
S.M.A.R.T. test.\
Schedule the S.M.A.R.T. Test & dropdown menu & Choose how often to run
the task. Choices are , , , , or . Selecting opens the ().\

An example configuration is to schedule a once a week and a once a
month. These tests do not have a performance impact, as the disks
prioritize normal I/O over the tests. If a disk fails a test, even if
the overall status is , consider replacing that disk.

<span>warning</span><span>Warning:</span> Some S.M.A.R.T. tests cause
heavy disk activity and can drastically reduce disk performance. Do not
schedule S.M.A.R.T. tests to run at the same time as scrub or resilver
operations or during other periods of intense disk activity.

Which tests will run and when can be verified by typing within ().

The results of a test can be checked from () by specifying the name of
the drive. For example, to see the results for disk , type:

\[commandchars=\
{}\] smartctl l selftest /dev/ada0

Periodic Snapshot Tasks {#\detokenize{tasks:periodic-snapshot-tasks}}
-----------------------

\[\]\[\] A periodic snapshot task allows scheduling the creation of
readonly versions of pools and datasets at a given point in time.
Snapshots can be created quickly and, if little data changes, new
snapshots take up very little space. For example, a snapshot where no
files have changed takes 0 MB of storage, but as changes are made to
files, the snapshot size changes to reflect the size of the changes.

Snapshots keep a history of files, providing a way to recover an older
copy or even a deleted file. For this reason, many administrators take
snapshots often, store them for a period of time, and store them on
another system, typically using (). Such a strategy allows the
administrator to roll the system back to a specific point in time. If
there is a catastrophic loss, an offsite snapshot can be used to restore
the system up to the time of the last snapshot.

A pool must exist before a snapshot can be created. Creating a pool is
described in ().

View the list of periodic snapshot tasks by going to . If a periodic
snapshot task encounters an error, the status column will show . Click
the status to view the logs of the task.

To create a periodic snapshot task, navigate to and click . This opens
the screen shown in . describes the fields in this screen.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Dataset & dropdown menu & Select a pool, dataset, or zvol.\
Recursive & checkbox & Set to take separate snapshots of the dataset and
each of its child datasets. Leave unset to take a single snapshot only
of the specified dataset child datasets.\
Exclude & string & Exclude specific child datasets from the snapshot.
Use with recursive snapshots. Commaseparated list of paths to any child
datasets to exclude. Example: . A recursive snapshot of will include all
child datasets except .\
Snapshot Lifetime & integer and dropdown menu & Define a length of time
to retain the snapshot on this system. After the time expires, the
snapshot is removed. Snapshots which have been replicated to other
systems are not affected.\
Snapshot Lifetime Unit & dropdown & Select a unit of time to retain the
snapshot on this system.\
Naming Schema & string & Snapshot name format string. The default is .
Must include the strings , , , , and , which are replaced with the
fourdigit year, month, day of month, hour, and minute as defined in
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=strftime). For
example, snapshots of with a Naming Schema of have names like .\
Schedule the Periodic Snapshot Task & dropdown menu & When the periodic
snapshot task runs. Choose one of the preset schedules or choose to use
the ().\
Begin & dropdown menu & Hour and minute when the system can begin taking
snapshots.\
End & dropdown menu & Hour and minute the system must stop creating
snapshots. Snapshots already in progress will continue until complete.\
Allow Taking Empty Snapshots & checkbox & Creates dataset snapshots even
when there have been no changes to the dataset from the last snapshot.
Recommended for creating longterm restore points, multiple snapshot
tasks pointed at the same datasets, or to be compatible with snapshot
schedules or replications created in FreeNAS$^{\text{®}}$ 11.2 and
earlier. For example, allowing empty snapshots for a monthly snapshot
schedule allows that monthly snapshot to be taken, even when a daily
snapshot task has already taken a snapshot of any changes to the
dataset.\
Enabled & checkbox & To activate this periodic snapshot schedule, set
this option. To disable this task without deleting it, unset this
option.\

Setting adds child datasets to the snapshot. Creating separate snapshots
for each child dataset is not needed.

The can be manually adjusted to include more information. For example,
after configuring a periodic snapshot task with a lifetime of two weeks,
it could be helpful to define a that shows the lifetime: .

Click when finished customizing the task. Defined tasks are listed
alphabetically in .

Click (Options) for a periodic snapshot task to see options to or the
scheduled task.

Deleting a dataset does not delete snapshot tasks for that dataset. To
reuse the snapshot task for a different dataset, the task and choose the
new . The original dataset is shown in the dropdown, but cannot be
selected.

Deleting the last periodic snapshot task used by a replication task is
not permitted while that replication task remains active. The
replication task must be disabled before the related periodic snapshot
task can be deleted.

### Snapshot Autoremoval {#\detokenize{tasks:snapshot-autoremoval}}

\[\] The periodic snapshot task autoremoval process (which removes
snapshots after their configured ) is run whenever any periodic snapshot
task runs.

When the autoremoval process runs, all snapshots on the system are
checked for removal. First, each snapshot is matched with a periodic
snapshot task according to the following criteria:

-   : To match a task, a snapshot must be on the same as the task, or on
    a child dataset if the task is marked .

-   : To match a task, a snapshot’s name must match the defined in
    that task.

-   : To match a task, the time at which the snapshot was created
    (according to its name and naming schema) must match the schedule
    defined in the task ().

-   : To match a task, the periodic snapshot task must be .

At this point, if the snapshot does not match any periodic snapshot
tasks then it is not considered for autoremoval. However, if it does
match one (or possibly more than one) periodic snapshot task, it is
deleted if its creation time (according to its name and naming schema)
is older than the longest of any of the tasks it was matched with.

One notable detail of this process is that there is no saved memory of
which task created which snapshot, or what the parameters of the
periodic snapshot task were at the time a snapshot was created. All
checks for autoremoval are based on the current state of the system.

These details become important when existing periodic snapshot tasks are
edited, disabled, or deleted. When editing a periodic snapshot task, if
the is changed, is unchecked, or the task is rescheduled (), previously
created snapshots may not be automatically removed as expected since the
previously created snapshots may no longer match any periodic snapshot
tasks. Similarly, if a periodic snapshot task is deleted or marked not ,
snapshots previously created by that task will no longer be
automatically removed.

In these cases, the user must manually remove unneeded snapshots that
were previously created by the modified or deleted periodic snapshot
task.

Replication {#\detokenize{tasks:replication}}
-----------

\[\]\[\] is the process of copying () from one storage pool to another.
Replications can be configured to copy snapshots to another pool on the
local system or send copies to a remote system that is in a different
physical location.

Replication schedules are typically paired with () to generate local
copies of important data and replicate these copies to a remote system.

Replications require a source system with dataset snapshots and a
destination that can store the copied data. Remote replications require
a saved () on the source system and the destination system must be
configured to allow () connections. Local replications do not use SSH.

Snapshots are organized and sent to the destination according to the
creation date included in the snapshot name. When replicating manually
created snapshots, make sure snapshots are named according to their
actual creation date.

Firsttime replication tasks can take a long time to complete as the
entire dataset snapshot must be copied to the destination system.
Replicated data is not visible on the receiving system until the
replication task is complete.

Later replications only send incremental snapshot changes to the
destination system. This reduces both the total space required by
replicated data and the network bandwidth required for the replication
to complete.

The replication task asks to destroy destination dataset snapshots when
those snapshots are not related to the replication snapshots. Verify
that the snapshots in the destination dataset are unneeded or are backed
up in a different location! Allowing the replication task to continue
destroys the current snapshots in the destination dataset and replicates
a full copy of the source snapshots.

The target dataset on the destination system is created in mode to
protect the data. To mount or browse the data on the destination system,
use a clone of the snapshot. Clones are created in mode, making it
possible to browse or mount them. See () for more details.

Replications run in parallel as long as they do not conflict with each
other. Completion time depends on the number and size of snapshots and
the bandwidth available between the source and destination computers.

Examples in this section refer to the FreeNAS$^{\text{®}}$ system with
the original datasets for snapshot and replication as and the
FreeNAS$^{\text{®}}$ system that is storing replicated snapshots as .

### Replication Creation Wizard {#\detokenize{tasks:replication-creation-wizard}}

\[\]\[\] To create a new replication, go to and click .

\[\]

The wizard allows loading previously saved replication configurations
and simplifies many replication settings. To see all possible (), click
.

Using the wizard to create a new replication task begins by defining
what is being replicated and where. Choosing for either the or requires
an () to the remote system. Open the dropdown menu to choose an SSH
connection or click to add a new connection.

Start by selecting the datasets to be replicated. To choose a dataset,
click (Browse) and select the dataset from the expandable tree. The path
of the dataset can also be typed into the field. Multiple snapshot
sources can be chosen using a comma () to separate each selection.
replication will include all snapshots of any descendant datasets of the
chosen .

Source datasets on the local system are replicated using existing
snapshots of the chosen datasets. When no snapshots exist,
FreeNAS$^{\text{®}}$ automatically creates snapshots of the chosen
datasets before starting the replication. To manually define which
dataset snapshots to replicate, set and define a snapshot .

Source datasets on a remote system are replicated by defining a snapshot
. The schema is a pattern of the name and <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=strftime) , , , , and strings
that match names of the snapshots to include in the replication. For
example, to replicate a snapshot named from a remote source, enter as
the replication task .

The number of snapshots that will be replicated is shown. There is also
a option to include child datasets with the selected datasets.

Now choose the to receive the replicated snapshots. To choose a
destination path, click (Browse) and select the dataset from the
expandable tree or type a path to the location in the field. Only a
single path can be defined.

Using an SSH connection for replication adds the option. This sets the
data transfer security level. The connection is authenticated with SSH.
Data can be encrypted during transfer for security or left unencrypted
to maximize transfer speed. Encryption is recommended, but can be
disabled for increased speed on secure networks.

A suggested replication is shown. This can be changed to give a more
meaningful name to the task. When the source and destination have been
set, click to choose when the replication will run.

\[\]

The replication task can be configured to run on a schedule or left
unscheduled and manually activated. Choosing adds the dropdown to choose
from preset schedules or define a replication schedule. Choosing removes
all scheduling options.

determines when replicated snapshots are deleted from the destination
system:

-   : duplicate the configured value from the source dataset ().

-   : never delete snapshots from the destination system.

-   : define how long a snapshot remains on the destination system.
    Enter a number and choose a measure of time from the dropdown menus.

Clicking saves the replication configuration and activates the schedule.
When the replication configuration includes a source dataset on the
local system and has a schedule, a () of that dataset is also created.

Tasks set to will start immediately. If a onetime replication has no
valid local system source dataset snapshots, FreeNAS$^{\text{®}}$ will
snapshot the source datasets and immediately replicate those snapshots
to the destination dataset.

All replication tasks are displayed in . The task settings that are
shown by default can be adjusted by opening the dropdown. To see more
details about the last time the replication task ran, click the entry
under the column. Tasks can also be expanded by clicking (Expand) for
that task. Expanded tasks show all replication settings and have , , and
buttons.

### Advanced Replication Creation {#\detokenize{tasks:advanced-replication-creation}}

\[\]\[\] The advanced replication creation screen has more options for
finetuning a replication. It also allows creating local replications,
legacy engine replications from FreeNAS$^{\text{®}}$ 11.1 or earlier, or
even creating a onetime replication that is not linked to a periodic
snapshot task.

Go to , click and to see these options. This screen is also displayed
after clicking (Options) and for an existing replication.

The value changes many of the options for replication. shows abbreviated
names of the methods in the column to identify fields which appear when
that method is selected.

-   : All methods

-   :

-   :

-   :

-   :

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.13-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.55-2</span>|</span>

\[\]\
Setting & Transport & Value & Description\

\
Setting & Transport & Value & Description\

\

Name & All & string & Descriptive name for the replication.\
Direction & SSH, NCT, LEG & dropdown menu & sends snapshots to a
destination system. connects to a remote system and retrieves snapshots
matching a .\
Transport & All & dropdown menu & Method of snapshot transfer:

-   is supported by most systems. It requires a previously created ().

-   uses SSH to establish a connection to the destination system, then
    uses <span> </span> (https://github.com/freenas/pylibzfs) to send an
    unencrypted data stream for higher transfer transfer speeds. By
    default, this is supported by FreeNAS$^{\text{®}}$ systems with 11.2
    or later installed (11.3 or later is recommended). Destination
    systems that do not have FreeNAS$^{\text{®}}$ 11.2 or later
    installed might have to manually install .

-   efficiently replicates snapshots to another dataset on the
    same system.

-   uses the legacy replication engine from FreeNAS$^{\text{®}}$ 11.2
    and earlier.

\
SSH Connection & SSH, NCT, LEG & dropdown menu & Choose the ().\
Netcat Active Side & NCT & dropdown menu & Establishing a connection
requires that one of the connection systems has open TCP ports. Choose
which system ( or ) will open ports. Consult your IT department to
determine which systems are allowed to open ports.\
Netcat Active Side Listen Address & NCT & string & IP address on which
the connection listens. Defaults to .\
Netcat Active Side Min Port & NCT & integer & Lowest port number of the
active side listen address that is open to connections.\
Netcat Active Side Max Port & NCT & integer & Highest port number of the
active side listen address that is open to connections. The first
available port between the minimum and maximum is used.\
Netcat Active Side Connect Address & NCT & string & Hostname or IP
address used to connect to the active side system. When the active side
is , this defaults to the environment variable. When the active side is
, this defaults to the SSH connection hostname.\
Source & All & (Browse), string & Define the path to a system location
that has snapshots to replicate. Click the (Browse) to see all locations
on the source system or click in the field to manually type a location
(Example: ). Multiple source locations can be selected or manually
defined with a comma (literal:) separator.\
Destination & All & (Browse), string & Define the path to a system
location that will store replicated snapshots. Click the (Browse) to see
all locations on the destination system or click in the field to
manually type a location path (Example: ). Selecting a location defines
the full path to that location as the destination. Appending a name to
the path will create new zvol at that location.

For example, selecting will store snapshots in , but clicking the path
and typing after will create for snapshot storage.\
Recursive & All & checkbox & Replicate all child dataset snapshots. When
set, becomes visible.\
Exclude Child Datasets & SSH, NCT, LOC & string & Exclude specific child
dataset snapshots from the replication. Use with replications. List
child dataset names to exclude. Separate multiple entries with a comma
(). Example: . A recursive replication of snapshots includes all child
dataset snapshots except .\
Properties & SSH, NCT, LOC & checkbox & Include dataset properties with
the replicated snapshots.\
Periodic Snapshot Tasks & SSH, NCT, LOC & dropdown menu & Snapshot
schedule for this replication task. Choose from configured (). This
replication task must have the same and values as the chosen periodic
snapshot task. Selecting a periodic snapshot schedule removes the
field.\
Naming Schema & SSH, NCT, LOC & string & Visible with replications.
Pattern of naming custom snapshots to be replicated. Enter the name and
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=strftime) , ,
, , and strings that match the snapshots to include in the replication.\
Also Include Naming Schema & SSH, NCT, LOC & string & Visible with
replications. Pattern of naming custom snapshots to include in the
replication with the periodic snapshot schedule. Enter the <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=strftime) strings
that match the snapshots to include in the replication.

When a periodic snapshot is not linked to the replication, enter the
naming schema for manually created snapshots. Has the same , , , , and
string requirements as the in a ().\
Run Automatically & SSH, NCT, LOC & checkbox & Set to either start this
replication task immediately after the linked periodic snapshot task
completes or continue to create a separate for this replication.\
Schedule & SSH, NCT, LOC & checkbox and dropdown menu & Start time for
the replication task. Select a preset schedule or choose to use the
advanced scheduler. Adds the and fields.\
Begin & SSH, NCT, LOC & dropdown menu & Start time for the replication
task.\
End & SSH, NCT, LOC & dropdown menu & End time for the replication task.
A replication that is already in progress can continue to run past this
time.\
Replicate Specific Snapshots & SSH, NCT, LOC & checkbox and dropdown
menu & Only replicate snapshots that match a defined creation time. To
specify which snapshots will be replicated, set this checkbox and define
the snapshot creation times that will be replicated. For example,
setting this time frame to will only replicate snapshots that were
created at the beginning of each hour.\
Begin & SSH, NCT, LOC & dropdown menu & Daily time range for the
specific periodic snapshots to replicate, in 15 minute increments.
Periodic snapshots created before the time will not be included in the
replication.\
End & SSH, NCT, LOC & dropdown menu & Daily time range for the specific
periodic snapshots to replicate, in 15 minute increments. Snapshots
created after the time will not be included in the replication.\
Only Replicate Snapshots Matching Schedule & SSH, NCT, LOC & checkbox &
Set to use the in place of the time frame. The values are read over the
time frame.\
Replicate from scratch if incremental is not possible & SSH, NCT, LOC &
checkbox & If the destination system has snapshots but they do not have
any data in common with the source snapshots, destroy all destination
snapshots and do a full replication. enabling this option can cause data
loss or excessive data transfer if the replication is misconfigured.\
Hold Pending Snapshots & SSH, NCT, LOC & checkbox & Prevent source
system snapshots that have failed replication from being automatically
removed by the .\
Snapshot Retention Policy & SSH, NCT, LOC & dropdown menu & When
replicated snapshots are deleted from the destination system:

-   : use value from the source ().

-   : define a for the destination system.

-   : never delete snapshots from the destination system.

\
Snapshot Lifetime & All & integer and dropdown menu & Added with a
retention policy. How long a snapshot remains on the destination system.
Enter a number and choose a measure of time from the dropdown.\
Stream Compression & SSH & dropdown menu & Select a compression
algorithm to reduce the size of the data being replicated. Only appears
when is chosen for .\
Limit (Examples: 500 KiB, 500M, 2 TB) & SSH & integer & Limit
replication speed to this number of bytes per second. Zero means no
limit. This is a ().\
Send Deduplicated Stream & SSH, NCT, LOC & checkbox & Deduplicate the
stream to avoid sending redundant data blocks. The destination system
must also support deduplicated streams. See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=zfs).\
Allow Blocks Larger than 128KB & SSH, NCT, LOC & checkbox & Allow
sending large data blocks. The destination system must also support
large blocks. See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=zfs).\
Allow Compressed WRITE Records & SSH, NCT, LOC & checkbox & Use
compressed WRITE records to make the stream more efficient. The
destination system must also support compressed WRITE records. See
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=zfs).\
Number of retries for failed replications & SSH, NCT, LOC & integer &
Number of times the replication is attempted before stopping and marking
the task as failed.\
Logging Level & All & dropdown menu & Message verbosity level in the
replication task log.\
Enabled & All & checkbox & Activates the replication schedule.\

### Replication Tasks {#\detokenize{tasks:replication-tasks}}

\[\] Saved replications are shown on the page.

\[\]

The replication name and configuration details are shown in the list. To
adjust the default table view, open the menu and select the replication
details to show in the normal table view.

The column shows the status of the replication task. To view the
detailed replication logs for a task, click the entry when the task is
running or finished.

Expanding an entry shows additional buttons for starting or editing a
replication task.

### Limiting Replication Times {#\detokenize{tasks:limiting-replication-times}}

\[\] The , , and times in a replication task make it possible to
restrict when replication is allowed. These times can be set to only
allow replication after business hours, or at other times when disk or
network activity will not slow down other operations like snapshots or
(). The default settings allow replication to occur at any time.

These times control when replication task are allowed to start, but will
not stop a replication task that is already running. Once a replication
task has begun, it will run until finished.

### Troubleshooting Replication {#\detokenize{tasks:troubleshooting-replication}}

\[\] Replication depends on SSH, disks, network, compression, and
encryption to work. A failure or misconfiguration of any of these can
prevent successful replication.

Replication logs are saved in . Logs of individual replication tasks can
be viewed by clicking the replication .

#### SSH {#\detokenize{tasks:ssh}}

() must be able to connect from the source system to the destination
system with an encryption key. This is tested from () by making an ()
connection from the source system to the destination system. For
example, this is a connection from to at . Start the () on the source
machine (), then enter this command:

\[commandchars=\
{}\] ssh vv 10.0.0.118

On the first connection, the system might say

\[commandchars=\
{}\] No matching host key fingerprint found in DNS. Are you sure you
want to continue connecting (yes/no)?

Verify that this is the correct destination computer from the preceding
information on the screen and type . At this point, an () shell
connection is open to the destination system, .

If a password is requested, SSH authentication is not working. An SSH
key value must be present in the destination system file. file can show
diagnostic errors for login problems on the destination computer also.

#### Compression {#\detokenize{tasks:compression}}

Matching compression and decompression programs must be available on
both the source and destination computers. This is not a problem when
both computers are running FreeNAS$^{\text{®}}$, but other operating
systems might not have , , or compression programs installed by default.
An easy way to diagnose the problem is to set to . If the replication
runs, select the preferred compression method and check on the
FreeNAS$^{\text{®}}$ system for errors.

#### Manual Testing {#\detokenize{tasks:manual-testing}}

On , the source computer, the file can also show helpful messages to
locate the problem.

On the source computer, , open a () and manually send a single snapshot
to the destination computer, . The snapshot used in this example is
named . As before, it is located in the dataset. A symbol separates the
name of the dataset from the name of the snapshot in the command.

\[commandchars=\
{}\] zfs send alphapool/alphadata@auto20161206.11102w | ssh 10.0.0.118
zfs recv betapool

If a snapshot of that name already exists on the destination computer,
the system will refuse to overwrite it with the new snapshot. The
existing snapshot on the destination computer can be deleted by opening
a () on and running this command:

\[commandchars=\
{}\] zfs destroy R betapool/alphadata@auto20161206.11102w

Then send the snapshot manually again. Snapshots on the destination
system, , are listed from the () with or from .

Error messages here can indicate any remaining problems.

Resilver Priority {#\detokenize{tasks:resilver-priority}}
-----------------

\[\]\[\] Resilvering, or the process of copying data to a replacement
disk, is best completed as quickly as possible. Increasing the priority
of resilvers can help them to complete more quickly. The menu makes it
possible to increase the priority of resilvering at times where the
additional I/O or CPU usage will not affect normal usage. Select to
display the screen shown in . describes the fields on this screen.

\[\]

<span>|&gt;p<span>0.3-2</span> |&gt;p<span>0.2-2</span>
|&gt;p<span>0.5-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Enabled & checkbox & Set to run resilver tasks between the configured
times.\
Begin Time & dropdown & Choose the hour and minute when resilver tasks
can be started.\
End Time & dropdown & Choose the hour and minute when new resilver tasks
can no longer be started. This does not affect active resilver tasks.\
Days of the Week & checkboxes & Select the days to run resilver tasks.\

Scrub Tasks {#\detokenize{tasks:scrub-tasks}}
-----------

\[\]\[\] A scrub is the process of ZFS scanning through the data on a
pool. Scrubs help to identify data integrity problems, detect silent
data corruptions caused by transient hardware issues, and provide early
alerts of impending disk failures. FreeNAS$^{\text{®}}$ makes it easy to
schedule periodic automatic scrubs.

It is recommneded that each pool is scrubbed at least once a month. Bit
errors in critical data can be detected by ZFS, but only when that data
is read. Scheduled scrubs can find bit errors in rarelyread data. The
amount of time needed for a scrub is proportional to the quantity of
data on the pool. Typical scrubs take several hours or longer.

The scrub process is I/O intensive and can negatively impact
performance. Schedule scrubs for evenings or weekends to minimize impact
to users. Make certain that scrubs and other diskintensive activity like
() are scheduled to run on different days to avoid disk contention and
extreme performance impacts.

Scrubs only check used disk space. To check unused disk space, schedule
() of to run once or twice a month.

Scrubs are scheduled and managed with .

When a pool is created, a scrub is automatically scheduled. An entry
with the same pool name is added to . A summary of this entry can be
viewed with . displays the default settings for the pool named . In this
example, (Options) and for a pool is clicked to display the screen.
summarizes the options in this screen.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.16-2</span>
|&gt;p<span>0.66-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Pool & dropdown menu & Choose a pool to scrub.\
Threshold days & string & Days before a completed scrub is allowed to
run again. This controls the task schedule. For example, scheduling a
scrub to run daily and setting to means the scrub attempts to run daily.
When the scrub is successful, it continues to check daily but does not
run again until seven days have elapsed. Using a multiple of seven
ensures the scrub always occurs on the same weekday.\
Description & string & Describe the scrub task.\
Schedule the Scrub Task & dropdown menu & Choose how often to run the
scrub task. Choices are , , , , or . Selecting opens the ().\
Enabled & checkbox & Unset to disable the scheduled scrub without
deleting it.\

Review the default selections and, if necessary, modify them to meet the
needs of the environment. Scrub tasks cannot run for locked or unmounted
pools.

Scheduled scrubs can be deleted with the button, but this is not
recommended. If a scrub is too intensive for the hardware, consider
temporarily deselecting the button for the scrub until the hardware can
be upgraded.

Cloud Sync Tasks {#\detokenize{tasks:cloud-sync-tasks}}
----------------

\[\]\[\] Files or directories can be synchronized to remote cloud
storage providers with the feature.

<span>warning</span><span>Warning:</span> This Cloud Sync task might go
to a third party commercial vendor not directly affiliated with
iXsystems. Please investigate and fully understand that vendor’s pricing
policies and services before creating any Cloud Sync task. iXsystems is
not responsible for any charges incurred from the use of third party
vendors with the Cloud Sync feature.

() must be defined before a cloud sync is created. One set of
credentials can be used for more than one cloud sync. For example, a
single set of credentials for Amazon S3 can be used for separate cloud
syncs that push different sets of files or directories.

A cloud storage area must also exist. With Amazon S3, these are called .
The bucket must be created before a sync task can be created.

After the cloud credentials have been configured, is used to define the
schedule for running a cloud sync task. The time selected is when the
Cloud Sync task is allowed to begin. An inprogress cloud sync must
complete before another cloud sync can start. The cloud sync runs until
finished, even after the selected ending time. To stop the cloud sync
task before it is finished, click (Options) .

An example is shown in .

\[\]

The cloud sync indicates the state of most recent cloud sync. Clicking
the entry shows the task logs and includes an option to download them.

Click to display the menu shown in .

\[\]

shows the configuration options for Cloud Syncs.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value Type & Description\

\
Setting & Value Type & Description\

\

Description & string & A description of the Cloud Sync Task.\
Direction & dropdown menu & sends data to cloud storage. receives data
from cloud storage. Changing the direction resets the to .\
Credential & dropdown menu & Select the cloud storage provider
credentials from the list of available (). The credential is tested and
an error is displayed if a connection cannot be made. Click to go to the
configuration page for that (). is disabled until a valid credential is
selected.\
Bucket/Container & dropdown menu & : Only appears when an S3 credential
is the . Select the predefined S3 bucket to use.

: The preconfigured container name. Only appears when a or credential is
selected as the .\
Folder & browse button & The name of the predefined folder within the
selected bucket or container. Type the name or click (Browse) to list
the remote filesystem and choose the folder.\
Server Side Encryption & dropdown menu & Active encryption on the cloud
provider account. Choose or . Only visible when the cloud provider
supports encryption.\
Storage Class & dropdown menu & Classification for each S3 object.
Choose a class based on the specific use case or performance
requirements. See <span> </span>
(https://docs.aws.amazon.com/AmazonS3/latest/dev/storageclassintro.html)
for more information on which storage class to choose. only appears when
an S3 credential is the .\
Upload Chunk Size (MiB) & integer & Files are split into chunks of this
size before upload. The number of chunks that can be simultaneously
transferred is set by the number. The single largest file being
transferred must fit into no more than 10,000 chunks.\
Use –fastlist & checkbox & <span> </span>
(https://rclone.org/docs/\#fastlist). Modifying this setting can speed
up slow down the transfer. Only appears with a compatible .\
Directory/Files & browse button & Select directories or files to be sent
to the cloud for syncs, or the destination to be written for syncs. Be
cautious about the destination of jobs to avoid overwriting existing
files.\
Transfer Mode & dropdown menu & : Files on the destination are to match
those on the source. If a file does not exist on the source, it is also
from the destination. There are () to this behavior.

: Files from the source are to the destination. If files with the same
names are present on the destination, they are .

: After files are from the source to the destination, they are from the
source. Files with the same names on the destination are .\
Take Snapshot & checkbox & Take a snapshot of the dataset before a .
This cannot be enabled when the chosen dataset to has nested datasets.\
Prescript & string & A script to execute before the Cloud Sync Task is
run.\
Postscript & string & A script to execute after the Cloud Sync Task is
run.\
Remote Encryption & checkbox & Use <span> </span>
(https://rclone.org/crypt/) to manage data encryption during or
transfers:

Encrypt files before transfer and store the encrypted files on the
remote system. Files are encrypted using the and values.

Decrypt files that are being stored on the remote system before the
transfer. Transferring the encrypted files requires entering the same
and that was used to encrypt the files.

Adds the , , and options. Additional details about the encryption
algorithm and key derivation are available in the <span> </span>
(https://rclone.org/crypt/\#fileformats).\
Filename Encryption & checkbox & Encrypt () or decrypt () file names
with the rclone <span> </span>
(https://rclone.org/crypt/\#filenameencryptionmodes). The original
directory structure is preserved. A filename with the same name always
has the same encrypted filename.

tasks that have enabled and an incorrect or will not transfer any files
but still report that the task was successful. To verify that files were
transferred successfully, click the finished () to see a list of
transferred files.\
Encryption Password & string & Password to encrypt and decrypt remote
data. : Always securely back up this password! Losing the encryption
password will result in data loss.\
Encryption Salt & string & Enter a long string of random characters for
use as <span> </span>
(https://searchsecurity.techtarget.com/definition/salt) for the
encryption password. : Always securely back up the encryption salt
value! Losing the salt value will result in data loss.\
Schedule the Cloud Sync Task & dropdown menu & Choose how often or at
what time to start a sync. Choices are , , , , or . Selecting opens the
().\
Transfers & integer & Number of simultaneous file transfers. Enter a
number based on the available bandwidth and destination system
performance. See <span> </span> (https://rclone.org/docs/\#transfersn).\
Follow Symlinks & checkbox & Include symbolic link targets in the
transfer.\
Enabled & checkbox & Enable this Cloud Sync Task. Unset to disable this
Cloud Sync Task without deleting it.\
Bandwidth Limit & string & A single bandwidth limit or bandwidth limit
schedule in rclone format. Example: . Units can be specified with the
beginning letter: b, k (default), M, or G. See <span> </span>
(https://rclone.org/docs/\#bwlimitbandwidthspec)\
Exclude & string & List of files and directories to exclude from sync,
one per line. See .\

\[\] There are specific circumstances where a task does not delete files
from the destination:

-   If <span> </span> (https://rclone.org/commands/rclone\_sync/)
    encounters any errors, files are not deleted in the destination.
    This includes a common error when the Dropbox <span> </span>
    (https://techcrunch.com/2014/03/30/howdropboxknowswhenyouresharingcopyrightedstuffwithoutactuallylookingatyourstuff/)
    flags a file as copyrighted.

-   Syncing to a () does not delete files from the bucket, even when
    those files have been deleted locally. Instead, files are tagged
    with a version number or moved to a hidden state. To automatically
    delete old or unwanted files from the bucket, adjust the <span>
    </span> (https://www.backblaze.com/blog/backblazeb2lifecyclerules/)

-   Files stored in Amazon S3 Glacier or S3 Glacier Deep Archive cannot
    be deleted by <span> </span>
    (https://rclone.org/s3/\#glacierandglacierdeeparchive/). These files
    must first be restored by another means, like the <span>
    </span> (https://docs.aws.amazon.com/AmazonS3/latest/userguide/restorearchivedobjects.html).

To modify an existing cloud sync, click (Options) to access the , , and
options.

### Cloud Sync Example {#\detokenize{tasks:cloud-sync-example}}

\[\] This example shows a cloud sync that copies files from a
FreeNAS$^{\text{®}}$ pool to a cloud service provider.

The cloud service provider was configured with a location to store data
received from the FreeNAS$^{\text{®}}$ system.

In the FreeNAS$^{\text{®}}$ web interface, go to and click to configure
the cloud service provider credentials:

\[\]

Go to and click to create a cloud sync job. The is filled with a simple
note describing the job. Data is being sent to cloud storage, so this is
a . The provider comes from the cloud credentials defined in the
previous step, and the destination folder was configured in the cloud
provider account.

The is set to the file or directory to copy to the cloud provider.

The is set to so that only the files stored by the cloud provider are
modified.

The remaining requirement is to schedule the task. The default is to
send the data to cloud storage daily, but the schedule can be () to
finetune when the task runs.

The field is enabled by default, so this cloud sync will run at the next
scheduled time.

An example of a completed cloud sync task is shown in :

\[\]

Network {#\detokenize{network:network}}
=======

\[\]\[\]\[\] The Network section of the web interface contains these
components for viewing and configuring network settings on the
FreeNAS$^{\text{®}}$ system:

-   (): general network settings.

-   (): settings for each network interface and options to configure (),
    (), and () interfaces.

-   (): settings controlling connection to the appliance through the
    hardware sideband management interface if the user interface
    becomes unavailable.

-   (): add static routes.

Each of these is described in more detail in this section.

\[\]

<span>note</span><span>Note:</span> When any network changes are made an
animated icon appears in the upperright web interface panel to show
there are pending network changes. When the icon is clicked it prompts
to review the recent network changes. Reviewing the network changes goes
to where the changes can be permanently applied or discarded.

When is clicked the network changes are temporarily applied for 60
seconds by default. This value can be changed by entering a positive
integer in the seconds field. This feature is nice because the network
settings preview can automatically roll back any configuration errors
that are accidentally saved.

If the network settings applied work as intended, click . Otherwise, the
changes can be discarded by clicking .

Global Configuration {#\detokenize{network:global-configuration}}
--------------------

\[\] , shown in , is for general network settings that are not unique to
any particular network interface.

\[\]

summarizes the settings on the Global Configuration tab. and fields are
prefilled as shown in , but can be changed to meet requirements of the
local network.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Hostname & string & System host name. Upper and lower case alphanumeric,
, and characters are allowed. The and are also displayed under the
iXsystems logo at the top left of the main screen.\
Domain & string & System domain name. The and are also displayed under
the iXsystems logo at the top left of the main screen.\
Additional Domains & string & Additional spacedelimited domains to
search. Adding search domains can cause slow DNS lookups.\
IPv4 Default Gateway & IP address & Typically not set. See (). If set,
used instead of the default gateway provided by DHCP.\
IPv6 Default Gateway & IP address & Typically not set. See ().\
Nameserver 1 & IP address & Primary DNS server.\
Nameserver 2 & IP address & Secondary DNS server.\
Nameserver 3 & IP address & Tertiary DNS server.\
HTTP Proxy & string & Enter the proxy information for the network in the
format or .\
Enable netwait feature & checkbox & If enabled, network services do not
start at boot until the interface is able to ping the addresses listed
in the .\
Netwait IP list & string & Only appears when is set. Enter a
spacedelimited list of IP addresses to ping(8). Each address is tried
until one is successful or the list is exhausted. Leave empty to use the
default gateway.\
Host name database & string & Used to add one entry per line which will
be appended to . Use the format where multiple hostnames can be used if
separated by a space.\

When using Active Directory, set the IP address of the realm DNS server
in the field.

If the network does not have a DNS server, or NFS, SSH, or FTP users are
receiving “reverse DNS” or timeout errors, add an entry for the IP
address of the FreeNAS$^{\text{®}}$ system in the field.

\[\]

<span>note</span><span>Note:</span> In many cases, a
FreeNAS$^{\text{®}}$ configuration does not include default gateway
information as a way to make it more difficult for a remote attacker to
communicate with the server. While this is a reasonable precaution, such
a configuration does restrict inbound traffic from sources within the
local network. However, omitting a default gateway will prevent the
FreeNAS$^{\text{®}}$ system from communicating with DNS servers, time
servers, and mail servers that are located outside of the local network.
In this case, it is recommended to add () to be able to reach external
DNS, NTP, and mail servers which are configured with static IP
addresses. When a gateway to the Internet is added, make sure the
FreeNAS$^{\text{®}}$ system is protected by a properly configured
firewall.

Interfaces {#\detokenize{network:interfaces}}
----------

\[\] shows all physical Network Interface Controllers (NICs) connected
to the FreeNAS$^{\text{®}}$ system. These can be edited or new , , or
interfaces can be created and added to the interface list.

Be careful when configuring the network interface that controls the
FreeNAS$^{\text{®}}$ web interface or ().

To configure a new network interface, go to and click .

\[\]

Each of configurable network interface changes the available options.
shows which settings are available with each interface type.

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.55-2</span>|</span>

\[\]\
Setting & Value & Type & Description\

\
Setting & Value & Type & Description\

\

Type & dropdown menu & All & Choose the type of interface. creates a
logical link between multiple networks. combines multiple network
connections into a single interface. A virtual LAN () partitions and
isolates a segment of the connection.\
Name & string & All & Enter a name to use for the the interface. Use the
format laggX, vlanX, or bridgeX where X is a number representing a
nonparent interface.\
Description & string & All & Notes or explanatory text about this
interface.\
DHCP & checkbox & All & Enable <span> </span>
(https://en.wikipedia.org/wiki/Dynamic\_Host\_Configuration\_Protocol)
to autoassign an IPv4 address to this interface. Leave unset to create a
static IPv4 or IPv6 configuration. Only one interface can be configured
for DHCP.\
Autoconfigure IPv6 & dropdown menu & All & Automatically configure the
IPv6 address with <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=rtsol). Only one interface
can be configured this way.\
Disable Hardware Offloading & checkbox & All & Turn off hardware
offloading for network traffic processing. WARNING: disabling hardware
offloading can reduce network performance and is only recommended when
the interface is managing (), (), or ().\
Bridge Members & dropdown menu & Bridge & Network interfaces to include
in the bridge.\
Lagg Protocol & dropdown menu & Link Aggregation & Select the (). is the
recommended protocol if the network switch is capable of active LACP. is
the default protocol choice and should only be used if the network
switch does not support active LACP.\
Lagg Interfaces & dropdown menu & Link Aggregation & Select the
interfaces to use in the aggregation. Lagg creation fails when the
selected interfaces have manually assigned IP addresses.\
Parent Interface & dropdown menu & VLAN & Select the VLAN Parent
Interface. Usually an Ethernet card connected to a switch port
configured for the VLAN. A cannot be selected as a parent interface. New
() are not available until the system is restarted.\
Vlan Tag & integer & VLAN & The numeric tag provided by the switched
network.\
Priority Code Point & dropdown menu & VLAN & Select the <span> </span>
(https://en.wikipedia.org/wiki/Class\_of\_service). The available 802.1p
Class of Service ranges from to .\
MTU & integer & All & Maximum Transmission Unit, the largest protocol
data unit that can be communicated. The largest workable MTU size varies
with network interfaces and equipment. and are standard Ethernet MTU
sizes. Leaving blank restores the field to the default value of .\
Options & string & All & Additional parameters from <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=ifconfig). Separate multiple
parameters with a space. For example: increases the MTU for interfaces
which support jumbo frames. See () about MTU and lagg interfaces.\
IP Address & integer and dropdown menu & All & Static IPv4 or IPv6
address and subnet mask. Example: and . Click to add another IP address.
Clicking removes that .\

Multiple interfaces be members of the same subnet. See <span> </span>
(https://forums.freenas.org/index.php?threads/multiplenetworkinterfacesonasinglesubnet.20204/)
for more information. Check the subnet mask if an error is shown when
setting the IP addresses on multiple interfaces.

Saving a new interface adds an entry to the list in .

Expanding an entry in the list shows further details for that interface.

Editing an interface allows changing all the () except the interface and
.

### Network Bridges {#\detokenize{network:network-bridges}}

\[\]\[\] A network bridge allows multiple network interfaces to function
as a single interface.

To create a bridge, go to and click . Choose as the and continue to
configure the interface. See the () for descriptions of each option.

Enter for the , where is a unique interface number. Open the dropdown
menu and select each interface that will be part of the bridge. Click to
add the new bridge to and show options to confirm or revert the new
network settings.

### Link Aggregations {#\detokenize{network:link-aggregations}}

\[\]\[\] FreeNAS$^{\text{®}}$ uses the FreeBSD <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=lagg) interface to provide
link aggregation and link failover support. A lagg interface allows
combining multiple network interfaces into a single virtual interface.
This provides faulttolerance and highspeed multilink throughput. The
aggregation protocols supported by lagg both determines the ports to use
for outgoing traffic and if a specific port accepts incoming traffic.
The link state of the lagg interface is used to validate whether the
port is active.

Aggregation works best on switches supporting LACP, which distributes
traffic bidirectionally while responding to failure of individual links.
FreeNAS$^{\text{®}}$ also supports active/passive failover between pairs
of links. The LACP and loadbalance modes select the output interface
using a hash that includes the Ethernet source and destination address,
VLAN tag (if available), IP source and destination address, and flow
label (IPv6 only). The benefit can only be observed when multiple
clients are transferring files the NAS. The flow entering the NAS
depends on the Ethernet switch loadbalance algorithm.

The lagg driver currently supports several aggregation protocols,
although only is recommended on network switches that do not support :

the default protocol. Sends traffic only through the active port. If the
master port becomes unavailable, the next active port is used. The first
interface added is the master port. Any interfaces added later are used
as failover devices. By default, received traffic is only accepted when
received through the active port. This constraint can be relaxed, which
is useful for certain bridged network setups, by going to and clicking
to add a tunable. Set the to , the to a nonzero integer, and the to .

supports the IEEE 802.3ad Link Aggregation Control Protocol (LACP) and
the Marker Protocol. LACP negotiates a set of aggregable links with the
peer into one or more link aggregated groups (LAGs). Each LAG is
composed of ports of the same speed, set to fullduplex operation.
Traffic is balanced across the ports in the LAG with the greatest total
speed. In most situations there will be a single LAG which contains all
ports. In the event of changes in physical connectivity, link
aggregation quickly converges to a new configuration. LACP must be
configured on the network switch and LACP does not support mixing
interfaces of different speeds. Only interfaces that use the same
driver, like two ports, are recommended for LACP. Using LACP for iSCSI
is not recommended as iSCSI has builtin multipath features which are
more efficient.

<span>note</span><span>Note:</span> When using , verify the switch is
configured for active LACP. Passive LACP is not supported.

balances outgoing traffic across the active ports based on hashed
protocol header information and accepts incoming traffic from any active
port. This is a static setup and does not negotiate aggregation with the
peer or exchange frames to monitor the link. The hash includes the
Ethernet source and destination address, VLAN tag (if available), and IP
source and destination address. Requires a switch which supports IEEE
802.3ad static link aggregation.

distributes outgoing traffic using a roundrobin scheduler through all
active ports and accepts incoming traffic from any active port. This
mode can cause unordered packet arrival at the client. This has a side
effect of limiting throughput as reordering packets can be CPU intensive
on the client. Requires a switch which supports IEEE 802.3ad static link
aggregation.

this protocol disables any traffic without disabling the lagg interface
itself.

#### LACP, MPIO, NFS, and ESXi {#\detokenize{network:lacp-mpio-nfs-and-esxi}}

\[\] LACP bonds Ethernet connections to improve bandwidth. For example,
four physical interfaces can be used to create one mega interface.
However, it cannot increase the bandwidth for a single conversation. It
is designed to increase bandwidth when multiple clients are
simultaneously accessing the same system. It also assumes that quality
Ethernet hardware is used and it will not make much difference when
using inferior Ethernet chipsets such as a Realtek.

LACP reads the sender and receiver IP addresses and, if they are deemed
to belong to the same TCP connection, always sends the packet over the
same interface to ensure that TCP does not need to reorder packets. This
makes LACP ideal for load balancing many simultaneous TCP connections,
but does nothing for increasing the speed over one TCP connection.

MPIO operates at the iSCSI protocol level. For example, if four IP
addresses are created and there are four simultaneous TCP connections,
MPIO will send the data over all available links. When configuring MPIO,
make sure that the IP addresses on the interfaces are configured to be
on separate subnets with nonoverlapping netmasks, or configure static
routes to do pointtopoint communication. Otherwise, all packets will
pass through one interface.

LACP and other forms of link aggregation generally do not work well with
virtualization solutions. In a virtualized environment, consider the use
of iSCSI MPIO through the creation of an iSCSI Portal with at least two
network cards on different networks. This allows an iSCSI initiator to
recognize multiple links to a target, using them for increased bandwidth
or redundancy. This <span> </span>
(https://fojta.wordpress.com/2010/04/13/iscsiandesximultipathingandjumboframes/)
contains instructions for configuring MPIO on ESXi.

NFS does not understand MPIO. Therefore, one fast interface is needed,
since creating an iSCSI portal will not improve bandwidth when using
NFS. LACP does not work well to increase the bandwidth for pointtopoint
NFS (one server and one client). LACP is a good solution for link
redundancy or for one server and many clients.

#### Creating a Link Aggregation {#\detokenize{network:creating-a-link-aggregation}}

\[\] creating a link aggregation, see this () about changing the
interface that the web interface uses.

To create a link aggregation, go to and click . Choose as the and
continue to fill in the remaining configuration options. See the () for
descriptions of each option.

Enter for the , where is a unique interface number. There a several
options, but is preferred. Choose when the network switch does not
support LACP. Open the dropdown menu to associate NICs with the lagg
device. Click to add the new aggregation to and show options to confirm
or revert the new network settings.

<span>note</span><span>Note:</span> If interfaces are installed but do
not appear in the list, check for a <span> </span>
(https://www.freebsd.org/releases/11.2R/hardware.html\#ethernet) for the
interface.

#### Link Aggregation Options {#\detokenize{network:link-aggregation-options}}

Options are set at the lagg level from . Find the lagg interface, expand
the entry with (Expand), and click . Scroll to the field. Changes are
typically made at the lagg level as each interface member inherits
settings from the lagg. Configuring at the interface level requires
repeating the configuration for each interface within the lagg. Setting
options at the individual interface level is done by editing the parent
interface in the same way as the lagg interface.

\[\] If the MTU settings on the lagg member interfaces are not
identical, the smallest value is used for the MTU of the entire lagg.

<span>note</span><span>Note:</span> A reboot is required after changing
the MTU to create a jumbo frame lagg.

Link aggregation load balancing can be tested with:

\[commandchars=\
{}\] systat ifstat

More information about this command can be found at <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=systat).

### VLANs {#\detokenize{network:vlans}}

\[\]\[\] FreeNAS$^{\text{®}}$ uses <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=vlan) to demultiplex frames
with IEEE 802.1q tags. This allows nodes on different VLANs to
communicate through a layer 3 switch or router. A vlan interface must be
assigned a parent interface and a numeric VLAN tag. A single parent can
be assigned to multiple vlan interfaces provided they have different
tags.

<span>note</span><span>Note:</span> VLAN tagging is the only 802.1q
feature that is implemented. Additionally, not all Ethernet interfaces
support full VLAN processing. See the HARDWARE section of <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=vlan) for details.

To add a new VLAN interface, go to and click . Choose as the and
continue filling in the remaining fields. See the () for descriptions of
each option.

The parent interface of a VLAN must be up, but it can either have an IP
address or be unconfigured, depending upon the requirements of the VLAN
configuration. This makes it difficult for the web interface to do the
right thing without trampling the configuration. To remedy this, add the
VLAN interface, then select , and click (Options) and for the parent
interface. Enter in the field and click . This brings up the parent
interface. If an IP address is required, configure it using the rest of
the options in the edit screen.

<span>warning</span><span>Warning:</span> Creating a VLAN causes an
interruption to network connectivity. The web interface requires
confirming the new network configuration before it is permanently
applied to the FreeNAS$^{\text{®}}$ system.

IPMI {#\detokenize{network:ipmi}}
----

\[\] Beginning with version 9.2.1, FreeNAS$^{\text{®}}$ provides a
graphical screen for configuring an IPMI interface. This screen will
only appear if the system hardware includes a Baseboard Management
Controller (BMC).

IPMI provides sideband management if the graphical administrative
interface becomes unresponsive. This allows for a few vital functions,
such as checking the log, accessing the BIOS setup, and powering on the
system without requiring physical access to the system. IPMI is also
used to give another person remote access to the system to assist with a
configuration or troubleshooting issue. Before configuring IPMI, ensure
that the management interface is physically connected to the network.
The IPMI device may share the primary Ethernet interface, or it may be a
dedicated separate IPMI interface.

<span>warning</span><span>Warning:</span> It is recommended to first
ensure that the IPMI has been patched against the Remote Management
Vulnerability before enabling IPMI. This <span> </span>
(https://www.ixsystems.com/blog/howtofixtheipmiremotemanagementvulnerability/)
provides more information about the vulnerability and how to fix it.

<span>note</span><span>Note:</span> Some IPMI implementations require
updates to work with newer versions of Java. See <span> </span>
(https://forums.freenas.org/index.php?threads/psajava8update131breaksasrocksipmivirtualconsole.53911/)
for more information.

IPMI is configured from . The IPMI configuration screen, shown in ,
provides a shortcut to the most basic IPMI configuration. Those already
familiar with IPMI management tools can use them instead. summarizes the
options available when configuring IPMI with the FreeNAS$^{\text{®}}$
web interface.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Channel & dropdown menu & Select the <span> </span>
(https://www.thomaskrenn.com/en/wiki/IPMI\_Basics\#Channel\_Model) to
use. Available channel numbers vary by hardware.\
Password & string & Enter the password used to connect to the IPMI
interface from a web browser. The maximum length accepted in the UI is
20 characters, but different hardware might require shorter passwords.\
DHCP & checkbox & If left unset, , , and must be set.\
IPv4 Address & string & IP address used to connect to the IPMI web
interface.\
IPv4 Netmask & dropdown menu & Subnet mask associated with the IP
address.\
IPv4 Default Gateway & string & Default gateway associated with the IP
address.\
VLAN ID & string & Enter the VLAN identifier if the IPMI outofband
management interface is not on the same VLAN as management networking.\
IDENTIFY LIGHT & button & Show a dialog to activate an IPMI identify
light on the compatible connected hardware.\

After configuration, the IPMI interface is accessed using a web browser
and the IP address specified in the configuration. The management
interface prompts for a username and the configured password. Refer to
the IPMI device documentation to determine the default administrative
username.

After logging in to the management interface, the default administrative
username can be changed, and additional users created. The appearance of
the IPMI utility and the functions that are available vary depending on
the hardware.

Network Summary {#\detokenize{network:network-summary}}
---------------

\[\] shows a quick summary of the addressing information of every
configured interface. For each interface name, the configured IPv4 and
IPv6 addresses, default routes, and DNS namerservers are displayed.

Static Routes {#\detokenize{network:static-routes}}
-------------

\[\]\[\] No static routes are defined on a default FreeNAS$^{\text{®}}$
system. If a static route is required to reach portions of the network,
add the route by going to , and clicking . This is shown in .

\[\]

The available options are summarized in .

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Destination & integer & Use the format where is the CIDR mask.\
Gateway & integer & Enter the IP address of the gateway.\
Description & string & Optional. Add any notes about the route.\

Added static routes are shown in . Click (Options) on a route entry to
access the and buttons.

Storage {#\detokenize{storage:storage}}
=======

\[\]\[\] The Storage section of the web interface allows configuration
of these options:

-   (): Change the swap space size.

-   (): create and manage storage pools.

-   (): manage local snapshots.

-   (): coordinate OpenZFS snapshots with a VMware datastore.

-   (): view and manage disk options.

-   (): import a disk that is formatted with the UFS, NTFS, MSDOS, or
    EXT2 filesystem.

-   (): View multipath information for systems with compatible hardware.

Swap Space {#\detokenize{storage:swap-space}}
----------

\[\]\[\] Swap is space on a disk set aside to be used as memory. When
the FreeNAS$^{\text{®}}$ system runs low on memory, lessused data can be
“swapped” onto the disk, freeing up main memory.

For reliability, FreeNAS$^{\text{®}}$ creates swap space as mirrors of
swap partitions on pairs of individual disks. For example, if the system
has three hard disks, a swap mirror is created from the swap partitions
on two of the drives. The third drive is not used, because it does not
have redundancy. On a system with four drives, two swap mirrors are
created.

Swap space is allocated when drives are partitioned before being added
to a (). A 2 GiB partition for swap space is created on each data drive
by default. The size of space to allocate can be changed in in the
field. Changing the value does not affect the amount of swap on existing
disks, only disks added after the change. This does not affect log or
cache devices, which are created without swap. Swap can be disabled by
entering , but that is .

Pools {#\detokenize{storage:pools}}
-----

\[\]\[\] is used to create and manage ZFS pools, datasets, and zvols.

Proper storage design is important for any NAS.

### Creating Pools {#\detokenize{storage:creating-pools}}

\[\] Before creating a pool, determine the level of required redundancy,
how many disks will be added, and if any data exists on those disks.
Creating a pool overwrites disk data, so save any required data to
different media before adding disks to a pool.

Go to and click . Select and click to open the screen shown in .

\[\]

Enter a name for the pool in the field. Ensure that the chosen name
conforms to these <span> </span>
(https://docs.oracle.com/cd/E23824\_01/html/8211448/gbcpt.html).
Choosing a name that will stick out in the logs is recommended, rather
than generic names like “data” or “freenas”.

To encrypt data on the underlying disks as a protection against physical
theft, set the option. A dialog displays a reminder to back up the ().
The data on the disks is inaccessible without the key. Select then click
.

<span>warning</span><span>Warning:</span> Refer to the warnings in ()
before enabling encryption!

From the section, select disks to add to the pool. Enter a value in or
to change the displayed disk order. These fields support <span> </span>
(http://php.net/manual/en/reference.pcre.pattern.syntax.php) for
filtering. For example, to show only and disks in , type in .

Type and maximum capacity is displayed for available disks. To show the
disk , , and , click (Expand).

After selecting disks, click the right arrow to add them to the section.
The usable space of each disk in a vdev is limited to the size of the
smallest disk in the vdev. Additional data vdevs must have the same
configuration as the initial vdev.

Any disks that appear in are used to create the pool. To remove a disk
from that section, select the disk and click the left arrow to return it
to the section.

After adding one data vdev, additional data vdevs can be added with .
This creates additional vdevs of the same layout as the initial vdev.
Select the number of additional vdevs and click .

returns all disks to the area and closes all but one table.

arranges all disks in an optimal layout for both redundancy and
capacity.

The pool layout is dependent upon the number of disks added to and the
number of available layouts increases as disks are added. To view the
available layouts, ensure that at least one disk appears in and select
the dropdown menu under this section. The web interface will
automatically update the when a layout is selected. These layouts are
supported:

-   requires at least one disk

-   requires at least two disks

-   requires at least three disks

-   requires at least four disks

-   requires at least five disks

<span>warning</span><span>Warning:</span> Refer to the () for more
information on redundancy and disk layouts. When more than five disks
are used, consideration must be given to the optimal layout for the best
performance and scalability.It is important to realize that different
layouts of virtual devices () affect which operations can be performed
on that pool later. For example, drives can be added to a mirror to
increase redundancy, but that is not possible with RAIDZ arrays.

After the desired layout is configured, click . A dialog shows a
reminder that all disk contents will be erased. Click , then to create
the pool.

<span>note</span><span>Note:</span> To instead preserve existing data,
click the button and refer to () and () to see if the existing format is
supported. If so, perform that action instead. If the current storage
format is not supported, it is necessary to back up the data to external
media, create the pool, then restore the data to the new pool.

Depending on the size and number of disks, the type of controller, and
whether encryption is selected, creating the pool may take some time. If
the option was selected, a dialog provides a link to . Click the link
and save the key to a safe location. When finished, click .

shows the new .

\[\] Select the pool to see more information. The first entry in the
list represents the root dataset and has the same name as the pool.

The column shows the estimated storage space before <span> </span>
(https://en.wikipedia.org/wiki/Data\_compression). The column shows the
estimated space used after compression. These numbers come from .

Other utilities can report different storage estimates. For example, the
available space shown in is the cumulative space of all drives in the
pool, regardless of pool configuration or compression.

Other information shown is the type of compression, the compression
ratio, whether it is mounted as readonly, whether deduplication has been
enabled, the mountpoint path, and any comments entered for the pool.

Pool status is indicated by one of these symbols:

<span>|&gt;p<span>0.15-2</span> |&gt;p<span>0.1-2</span>
|&gt;p<span>0.35-2</span>|</span>

\[\]\
Symbol & Color & Meaning\

\
Symbol & Color & Meaning\

\

HEALTHY & Green & The pool is healthy.\
DEGRADED & Orange & The pool is in a degraded state.\
UNKNOWN & Blue & Pool status cannot be determined.\
LOCKED & Yellow & The pool is locked.\
Pool Fault & Red & The pool has a critical error.\

There is an option to . This upgrades the pool to the latest (). See the
warnings in () before selecting this option. This button does not appear
when the pool is running the latest version of the feature flags.

\[\]

Creating a pool adds a card to the . Available space, disk details, and
pool status is shown on the card. The background color of the card
indicates the pool status:

-   Green: healthy or locked

-   Yellow: unknown, offline, or degraded

-   Red: faulted or removed

### Managing Encrypted Pools {#\detokenize{storage:managing-encrypted-pools}}

\[\]\[\] FreeNAS$^{\text{®}}$ uses <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=geli) full disk encryption
for ZFS pools. This type of encryption is intended to protect against
the risks of data being read or copied when the system is powered down,
when the pool is locked, or when disks are physically stolen.

FreeNAS$^{\text{®}}$ encrypts disks and pools, not individual
filesystems. The partition table on each disk is not encrypted, but only
identifies the location of partitions on the disk. On an encrypted pool,
the data in each partition is encrypted. These are generally called
“encrypted drives”, even though the partition table is not encrypted. To
use drive firmware to completely encrypt the drive, see ().

<span>note</span><span>Note:</span> Processors with support for the
<span> </span> (https://en.wikipedia.org/wiki/AES\_instruction\_set)
instruction set are strongly recommended. These processors can handle
encryption of a small number of disks with negligible performance
impact. They also retain performance better as the number of disks
increases. Older processors without the AESNI instructions see
significant performance impact with even a single encrypted disk. This
<span> </span>
(https://forums.freenas.org/index.php?threads/encryptionperformancebenchmarks.12157/)
compares the performance of various processors.

All drives in an encrypted pool are encrypted, including L2ARC (read
cache) and SLOG (write cache). Drives added to an existing encrypted
pool are encrypted with the same method specified when the pool was
created. Data in memory, including ARC, is not encrypted. ZFS data on
disk, including L2ARC and SLOG, are encrypted if the underlying disks
are encrypted. Swap data on disk is always encrypted.

Encryption performance depends upon the number of disks encrypted. The
more drives in an encrypted pool, the more encryption and decryption
overhead, and the greater the impact on performance. . Please benchmark
encrypted pools before using them in production.

Creating an encrypted pool means GELI encrypts the data on the disk and
generates a to decrypt this data. This master key is also encrypted.
Loss of a disk master key due to disk corruption is equivalent to any
other disk failure, and in a redundant pool, other disks will contain
accessible copies of the uncorrupted data. While it is to separately
back up disk master keys, it is usually not necessary or useful.

There are two that can be used to unlock the master key and then decrypt
the disks. In FreeNAS$^{\text{®}}$, these user keys are named the and
the . Because data cannot be read without first providing a key,
encrypted disks containing sensitive data can be safely removed, reused,
or discarded without secure wiping or physical destruction of the media.

When discarding disks that still contain encrypted sensitive data, the
encryption and recovery keys should also be destroyed or securely
deleted. Keys that are not destroyed must be stored securely and kept
physically separate from the discarded disks. Data is vulnerable to
decryption when the encryption key is present with the discarded disks
or can be obtained by the same person who gains access to the disks.

This encryption method is designed to protect against unauthorized
access when the pool is already unlocked. Before sensitive data is
stored on the system, ensure that only authorized users have access to
the web interface and that permissions with appropriate restrictions are
set on shares.

Here are some important points about FreeNAS$^{\text{®}}$ behavior to
remember when creating or using an encrypted pool:

-   At present, there is no onestep way to encrypt an existing pool. The
    data must be copied to an existing or new encrypted pool. After
    that, the original pool and any unencrypted backup should be
    destroyed to prevent unauthorized access and any disks that
    contained unencrypted data should be wiped.

-   Hybrid pools are not supported. Added vdevs must match the existing
    encryption scheme. () automatically encrypts a new vdev being added
    to an existing encrypted pool.

-   FreeNAS$^{\text{®}}$ encryption differs from the encryption used in
    the Oracle proprietary version of ZFS. To convert between these
    formats, both pools must be unlocked, and the data copied
    between them.

-   Each pool has a separate encryption key. Pools can also add a unique
    recovery key to use if the passphrase is forgotten or encryption
    key invalidated.

-   Encryption applies to a pool, not individual users. The data from an
    unlocked pool is accessible to all users with permissions to
    access it. Encrypted pools with a passphrase can be locked on demand
    by users that know the passphrase. Pools are automatically locked
    when the system is shut down.

-   Encrypted data cannot be accessed when the disks are removed or the
    system has been shut down. On a running system, encrypted data
    cannot be accessed when the pool is locked.

-   Encrypted pools that have no passphrase are unlocked at startup.
    Pools with a passphrase remain locked until a user enters the
    passphrase to unlock them.

#### Encryption and Recovery Keys {#\detokenize{storage:encryption-and-recovery-keys}}

\[\] FreeNAS$^{\text{®}}$ generates a randomized whenever a new
encrypted pool is created. This key is stored in the (). It is the
primary key used to unlock the pool each time the system boots. Creating
a passphrase for the pool adds a passphrase component to the encryption
key and allows the pool to be locked.

A pool encryption key backup can be downloaded to allow disk decryption
on a different system in the event of failure or to allow the
FreeNAS$^{\text{®}}$ stored key to be deleted for extra security. The
combination of encryption key location and passphrase usage provide
several different security scenarios:

-   : the encrypted pool is decrypted and accessible when the
    system running. Protects “data at rest” only.

-   : the encrypted pool is not accessible until the passphrase is
    entered by the FreeNAS$^{\text{®}}$ administrator.

-   : the encrypted pool is not accessible until the
    FreeNAS$^{\text{®}}$ administrator uploads the key file. When the
    key also has a passphrase, it must be provided with the key file.

Encrypted pools cannot be locked in the web interface until a passphrase
is created for the encryption key.

The recovery key is an optional keyfile that is generated by
FreeNAS$^{\text{®}}$, provided for download, and wiped from the system.
It is designed as an emergency backup to unlock or import an encrypted
pool if the passphrase is forgotten or the encryption key is somehow
invalidated. This file is not stored anywhere on the
FreeNAS$^{\text{®}}$ system and only one recovery key can exist for each
encrypted pool. Adding a new recovery key invalidates any previously
downloaded recovery key file for that pool.

Existing encryption or recovery keys can be invalidated in several
situations:

-   An encryption rekey invalidates all encryption and recovery keys as
    well as an existing passphrase.

-   Using a recovery key file to import an encrypted pool invalidates
    the existing encryption key and passphrase for that pool.
    FreeNAS$^{\text{®}}$ generates a new encryption key for the imported
    pool, but a new passphrase must be created before the pool can
    be locked.

-   Creating or changing a passphrase invalidates any existing
    recovery key.

-   Adding a new recovery key invalidates any existing recovery key
    files for the pool.

-   () invalidates all encryption and recovery keys as well as an
    existing passphrase.

Be sure to download and securely store copies of the most current
encryption and recovery keys. Protect and backup encryption key
passphrases.

#### Encryption Operations {#\detokenize{storage:encryption-operations}}

\[\] Encryption operations are seen by clicking (Encryption Options) for
the encrypted pool in . These options are available:

-   : Only appears after a passphrase is created. Locking a pool
    restricts data accessability in FreeNAS$^{\text{®}}$ until the pool
    is unlocked. Selecting this action requires entering the passphrase.
    The pool status changes to , are limited to , and
    (Encryption Options) changes to (Unlock).

-   : Decrypt the pool by clicking (Unlock) and entering the passphrase
    uploading the recovery key file. Only the passphrase is used when
    both a passphrase and a recovery key are entered. The services
    listed in restart when the pool is unlocked. This enables
    FreeNAS$^{\text{®}}$ to begin accessing the decrypted data.
    Individual services can be prevented from restarting by opening and
    deselecting them. Deselecting services can prevent them from
    properly accessing the unlocked pool.

-   : Create or change the encryption key passphrase and download a
    backup of the encryption key. Unlike a password, a passphrase can
    contain spaces and is typically a series of words. A good passphrase
    is easy to remember but hard to guess.

    \[\]

    The administrator password is required for encryption key changes.
    Setting invalidates the current pool passphrase. Creating or
    changing a passphrase invalidates the pool recovery key.

-   : Generate and download a new recovery key file or invalidate an
    existing recovery key. The FreeNAS$^{\text{®}}$ administrative
    password is required. Generating a new recovery key file invalidates
    previously downloaded recovery key files for the pool.

    \[\]

-   : Reset the encryption on the pool GELI master key and invalidate
    all encryption keys, recovery keys, and any passphrase for the pool.
    A dialog opens to save a backup of the new encryption key. A new
    passphrase can be created and a new pool recovery key file can
    be downloaded. The administrator password is required to reset
    pool encryption.

    If a key reset fails on a multidisk system, an alert is generated.
    as doing so may result in the loss of data.

### Adding Cache or Log Devices {#\detokenize{storage:adding-cache-or-log-devices}}

\[\] () can be used either during or after pool creation to add an SSD
as a cache or log device to improve performance of the pool under
specific use cases. Before adding a cache or log device, refer to the ()
to determine if the system will benefit or suffer from the addition of
the device.

To add a Cache or Log device during pool creation, click the or button.
Select the disk from and use the next to or to add it to that section.

To add a device to an existing pool, () that pool.

### Removing Cache or Log Devices {#\detokenize{storage:removing-cache-or-log-devices}}

\[\]\[\] Cache or log devices can be removed by going to . Choose the
desired pool and click (Settings) . Choose the log or cache device to
remove, then click (Options) .

### Adding Spare Devices {#\detokenize{storage:adding-spare-devices}}

\[\]\[\] ZFS provides the ability to have “hot” . These are drives that
are connected to a pool, but not in use. If the pool experiences the
failure of a data drive, the system uses the hot spare as a temporary
replacement. If the failed drive is replaced with a new drive, the hot
spare drive is no longer needed and reverts to being a hot spare. If the
failed drive is detached from the pool, the spare is promoted to a full
member of the pool.

Hot spares can be added to a pool during or after creation. On
FreeNAS$^{\text{®}}$, hot spare actions are implemented by <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=zfsd).

To add a spare during pool creation, click the . button. Select the disk
from and use the next to to add it to the section.

To add a device to an existing pool, () that pool.

### Extending a Pool {#\detokenize{storage:extending-a-pool}}

\[\] To increase the capacity of an existing pool, click the pool name,
(Settings), then .

If the existing pool is (), an additional warning message shows a
reminder that . Extending an encrypted pool opens a dialog to download
the new encryption key file. Remember to use the () to set a new
passphrase and create a new recovery key file.

When adding disks to increase the capacity of a pool, ZFS supports the
addition of virtual devices, or , to an existing ZFS pool. , but a new
vdev can be striped with another of the to increase the overall size of
the pool. To extend a pool, the vdev being added must be the same type
as existing vdevs. The button is only enabled when the vdev being added
is the same type as the existing vdevs. Some vdev extending examples:

-   to extend a ZFS mirror, add the same number of drives. The result is
    a striped mirror. For example, if ten new drives are available, a
    mirror of two drives could be created initially, then extended by
    adding another mirror of two drives, and repeating three more times
    until all ten drives have been added.

-   to extend a threedrive RAIDZ1, add another three drives. The
    resulting pool is a stripe of two RAIDZ1 vdevs, similar to RAID 50
    on a hardware controller.

-   to extend a fourdrive RAIDZ2, add another four drives. The result is
    a stripe of RAIDZ2 vdevs, similar to RAID 60 on a
    hardware controller.

### Export/Disconnect a Pool {#\detokenize{storage:export-disconnect-a-pool}}

\[\] is used to cleanly disconnect a pool from the system. This is used
before physically disconnecting the pool so it can be imported on
another system, or to optionally detach and erase the pool so the disks
can be reused.

To export or destroy an existing pool, click the pool name, (Settings),
then . A dialog shows which system () will be disrupted by exporting the
pool and additional warnings for encrypted pools. Keep or erase the
contents of the pool by setting the options shown in .

> \[\]

<span>warning</span><span>Warning:</span> Do not export/disconnect an
encrypted pool if the passphrase has not been set! When in doubt, use
the instructions in () to set a passphrase.

The screen provides these options:

<span>|&gt;p<span>0.5-2</span> |&gt;p<span>0.5-2</span>|</span>

\[\]\
Setting & Description\

\
Setting & Description\

\

Destroy data on this pool? & Destroy all data on the disks in the pool.
.\
Delete configuration of shares & Delete any share configurations set up
on the pool.\
Confirm export/disconnect & Confirm the export/disconnect operation.\

If the pool is encrypted, is also shown to download the () for that
pool.

To the pool and keep the data and configurations of shares, set and
click .

To instead destroy the data and share configurations on the pool, also
set the option. To verify that data on the pool is to be destroyed, type
the name of the pool and click . Data on the pool is destroyed,
including share configuration, zvols, datasets, and the pool itself. The
disk is returned to a raw state.

<span>danger</span><span>Danger:</span> Before destroying a pool, ensure
that any needed data has been backed up to a different pool or system.

### Importing a Pool {#\detokenize{storage:importing-a-pool}}

\[\] A pool that has been exported and disconnected from the system can
be reconnected with , then selecting . This works for pools that were
exported/disconnected from the current system, created on another
system, or to reconnect a pool after reinstalling the
FreeNAS$^{\text{®}}$ system.

When physically installing ZFS pool disks from another system, use the
command or a web interface equivalent to export the pool on that system.
Then shut it down and connect the drives to the FreeNAS$^{\text{®}}$
system. This prevents an “in use by another machine” error during the
import to FreeNAS$^{\text{®}}$.

Existing ZFS pools can be imported by clicking and . Select , then click
as shown in .

\[\]

To import a pool, click then as shown in .

\[\]

Select the pool from the dropdown menu and click to confirm the options
and it.

If hardware is not being detected, run from (). If the disk does not
appear in the output, check to see if the controller driver is supported
or if it needs to be loaded using ().

Before importing an (), disks must first be decrypted. Click . This is
shown in .

\[\]

Use the dropdown menu to select the disks to decrypt. Click to select
the encryption key file stored on the client system. Enter the
associated with the encryption key, then click to continue importing the
pool.

<span>danger</span><span>Danger:</span> The encryption key file and
passphrase are required to decrypt the pool. If the pool cannot be
decrypted, it cannot be reimported after a failed upgrade or lost
configuration. This means it is to save a copy of the key and to
remember the passphrase that was configured for the key. Refer to () for
instructions on managing keys.

Select the pool to import and confirm the settings. Click to finish the
process.

<span>note</span><span>Note:</span> For security reasons, encrypted pool
keys are not saved in a configuration backup file. When
FreeNAS$^{\text{®}}$ has been installed to a new device and a saved
configuration file restored to it, the keys for encrypted disks will not
be present, and the system will not request them. To correct this,
export the encrypted pool with (Configure) , making sure that is set.
Then import the pool again. During the import, the encryption keys can
be entered as described above.

### Viewing Pool Scrub Status {#\detokenize{storage:viewing-pool-scrub-status}}

\[\]\[\] Scrubs and how to set their schedule are described in more
detail in ().

To view the scrub status of a pool, click the pool name, (Settings),
then . The resulting screen will display the status and estimated time
remaining for a running scrub or the statistics from the last completed
scrub.

A button is provided to cancel a scrub in progress. When a scrub is
cancelled, it is abandoned. The next scrub to run starts from the
beginning, not where the cancelled scrub left off.

### Adding Datasets {#\detokenize{storage:adding-datasets}}

\[\]\[\] An existing pool can be divided into datasets. Permissions,
compression, deduplication, and quotas can be set on a perdataset basis,
allowing more granular control over access to storage data. Like a
folder or directory, permissions can be set on dataset. Datasets are
also similar to filesystems in that properties such as quotas and
compression can be set, and snapshots created.

<span>note</span><span>Note:</span> ZFS provides thick provisioning
using quotas and thin provisioning using reserved space.

To create a dataset, select an existing pool in , click (Options), then
select This will display the screen shown in .

\[\]

shows the options available when creating a dataset.

Some settings are only available in . To see these settings, either
click the button, or configure the system to always display advanced
settings by enabling the option in .

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.10-2</span>
|&gt;p<span>0.10-2</span> |&gt;p<span>0.59-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Name & string && Required. Enter a unique name for the dataset.\
Comments & string && Enter any additional comments or user notes about
this dataset.\
Sync & dropdown menu && Set the data write synchronization. inherits the
sync settings from the parent dataset, uses the sync settings that have
been requested by the client software, waits for data writes to
complete, and never waits for writes to complete.\
Compression Level & dropdown menu && Refer to the section on () for a
description of the available algorithms.\
Enable atime & Inherit, On, or Off && Choose to update the access time
for files when they are read. Choose to prevent producing log traffic
when reading files. This can result in significant performance gains.\
Quota for this dataset & integer & $\checkmark$ & Default of disables
quotas. Specifying a value means to use no more than the specified size
and is suitable for user datasets to prevent users from hogging
available space.\
Quota warning alert at, % & integer & $\checkmark$ & Set Inherit to
apply the same quota warning alert settings as the parent dataset.\
Quota critical alert at, % & integer & $\checkmark$ & Set Inherit to
apply the same quota critical alert settings as the parent dataset.\
Quota for this dataset and all children & integer & $\checkmark$ & A
specified value applies to both this dataset and any child datasets.\
Quota warning alert at, % & integer & $\checkmark$ & Set Inherit to
apply the same quota warning alert settings as the parent dataset.\
Quota critical alert at, % & integer & $\checkmark$ & Set Inherit to
apply the same quota critical alert settings as the parent dataset.\
Reserved space for this dataset & integer & $\checkmark$ & Default of is
unlimited. Specifying a value means to keep at least this much space
free and is suitable for datasets containing logs which could otherwise
take up all available free space.\
Reserved space for this dataset and all children & integer &
$\checkmark$ & A specified value applies to both this dataset and any
child datasets.\
ZFS Deduplication & dropdown menu && Read the section on () before
making a change to this setting.\
Readonly & dropdown menu & $\checkmark$ & Choices are , , or .\
Exec & dropdown menu & $\checkmark$ & Choices are , , or . Setting to
prevents the installation of () or ().\
Snapshot directory & dropdown menu & $\checkmark$ & Choose if the
snapshot directory is Visible or Invisible on this dataset.\
Copies & dropdown menu & $\checkmark$ & Set the number of data copies on
this dataset.\
Record Size & dropdown menu & $\checkmark$ & While ZFS automatically
adapts the record size dynamically to adapt to data, if the data has a
fixed size (such as database records), matching its size might result in
better performance. choosing a smaller record size than the suggested
value can reduce disk performance and space efficiency.\
ACL Mode & dropdown menu & $\checkmark$ & Determine how <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=chmod) behaves when adjusting
file ACLs. See the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=zfs).

only updates ACL entries that are related to the file or directory mode.

does not allow to make changes to files or directories with a nontrivial
ACL. An ACL is trivial if it can be fully expressed as a file mode
without losing any access rules. Setting the to is typically used to
optimize a dataset for (), but can require further optimizations. For
example, configuring an () with this dataset could require adding in the
task field.\
Case Sensitivity & dropdown menu && Choices are (default, assumes
filenames are case sensitive), (assumes filenames are not case
sensitive), or (understands both types of filenames). This can only be
set when creating a new dataset.\
Share Type & dropdown menu && Select the type of share that will be used
on the dataset. Choose between for most sharing options or for a ().
Choosing sets the to and to . This field is only available when creating
a new dataset.\

After a dataset is created it appears in . Click (Options) on an
existing dataset to configure these options:

\[\] create a nested dataset, or a dataset within a dataset.

add a zvol to the dataset. Refer to () for more information about zvols.

edit the pool properties described in . Note that and are readonly as
they cannot be edited after dataset creation.

refer to () for more information about permissions.

<span>danger</span><span>Danger:</span> Removing a dataset is a
permanent action and results in data loss!

see () for details about modifying an Access Control List (ACL).

removes the dataset, snapshots of that dataset, and any objects stored
within the dataset. To remove the dataset, set , click , verify that the
correct dataset to be deleted has been chosen by entering the dataset
name, and click . When the dataset has active shares or is still being
used by other parts of the system, the dialog shows what is still using
it and allows forcing the deletion anyway. : forcing the deletion of an
inuse dataset can cause data loss or other problems.

only appears on clones. When a clone is promoted, the origin filesystem
becomes a clone of the clone making it possible to destroy the
filesystem that the clone was created from. Otherwise, a clone cannot be
deleted while the origin filesystem exists.

create a onetime snapshot. A dialog opens to name the snapshot. Options
to include child datasets in the snapshot and synchronize with VMware
can also be shown. To schedule snapshot creation, use ().

#### Deduplication {#\detokenize{storage:deduplication}}

\[\]\[\] Deduplication is the process of ZFS transparently reusing a
single copy of duplicated data to save space. Depending on the amount of
duplicate data, deduplicaton can improve storage capacity, as less data
is written and stored. However, deduplication is RAM intensive. A
general rule of thumb is 5 GiB of RAM per terabyte of deduplicated
storage.

In FreeNAS$^{\text{®}}$, deduplication can be enabled during dataset
creation. Be forewarned that , as disabling deduplication has on
existing data. The more data written to a deduplicated dataset, the more
RAM it requires. When the system starts storing the DDTs (dedup tables)
on disk because they no longer fit into RAM, performance craters.
Further, importing an unclean pool can require between 35 GiB of RAM per
terabyte of deduped data, and if the system does not have the needed
RAM, it will panic. The only solution is to add more RAM or recreate the
pool. This <span> </span>
(https://constantin.glez.de/2011/07/27/zfstodedupeornotdedupe/) provides
a good description of the value versus cost considerations for
deduplication.

For performance reasons, consider using compression rather than turning
this option on.

If deduplication is changed to , duplicate data blocks are removed
synchronously. The result is that only unique data is stored and common
components are shared among files. If deduplication is changed to , ZFS
will do a bytetobyte comparison when two blocks have the same signature
to make sure that the block contents are identical. Since hash
collisions are extremely rare, is usually not worth the performance hit.

<span>note</span><span>Note:</span> After deduplication is enabled, the
only way to disable it is to use the command from (). However, any data
that has already been deduplicated will not be undeduplicated. Only
newly stored data after the property change will not be deduplicated.
The only way to remove existing deduplicated data is to copy all of the
data off of the dataset, set the property to off, then copy the data
back in again. Alternately, create a new dataset with left at , copy the
data to the new dataset, and destroy the original dataset.

<span>tip</span><span>Tip:</span> Deduplication is often considered when
using a group of very similar virtual machine images. However, other
features of ZFS can provide deduplike functionality more efficiently.
For example, create a dataset for a standard VM, then clone a snapshot
of that dataset for other VMs. Only the difference between each created
VM and the main dataset are saved, giving the effect of deduplication
without the overhead.

#### Compression {#\detokenize{storage:compression}}

\[\]\[\] When selecting a compression type, balancing performance with
the amount of disk space saved by compression is recommended.
Compression is transparent to the client and applications as ZFS
automatically compresses data as it is written to a compressed dataset
or zvol and automatically decompresses that data as it is read. These
compression algorithms are supported:

-   default and recommended compression method as it allows compressed
    datasets to operate at near realtime speed. This algorithm only
    compresses files that will benefit from compression.

-   levels 1, 6, and 9 where (level 1) gives the least compression and
    (level 9) provides the best compression but is discouraged due to
    its performance impact.

-   fast but simple algorithm which eliminates runs of zeroes.

If is selected as the when creating a dataset or zvol, compression will
not be used on that dataset/zvol. This is not recommended as using has a
negligible performance impact and allows for more storage capacity.

### Adding Zvols {#\detokenize{storage:adding-zvols}}

\[\]\[\] A zvol is a feature of ZFS that creates a raw block device over
ZFS. The zvol can be used as an () device extent.

To create a zvol, select an existing ZFS pool or dataset, click
(Options), then to open the screen shown in .

\[\]

The configuration options are described in .

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.10-2</span>
|&gt;p<span>0.10-2</span> |&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

zvol name & string && Enter a short name for the zvol. Using a zvol name
longer than 63characters can prevent accessing zvols as devices. For
example, a zvol with a 70character filename or path cannot be used as an
iSCSI extent. This setting is mandatory.\
Comments & string && Enter any notes about this zvol.\
Size for this zvol & integer && Specify size and value. Units like , ,
and can be used. The size of the zvol can be increased later, but cannot
be reduced. If the size is more than 80% of the available capacity, the
creation will fail with an “out of space” error unless is also enabled.\
Force size & checkbox && By default, the system will not create a zvol
if that operation will bring the pool to over 80% capacity. , enabling
this option will force the creation of the zvol.\
Sync & dropdown menu && Sets the data write synchronization. inherits
the sync settings from the parent dataset, uses the sync settings that
have been requested by the client software, waits for data writes to
complete, and never waits for writes to complete.\
Compression level & dropdown menu && Compress data to save space. Refer
to () for a description of the available algorithms.\
ZFS Deduplication & dropdown menu && ZFS feature to transparently reuse
a single copy of duplicated data to save space. this option is RAM
intensive. Read the section on () before making a change to this
setting.\
Sparse & checkbox && Used to provide thin provisioning. Use with caution
as writes will fail when the pool is low on space.\
Block size & dropdown menu & $\checkmark$ & The default is based on the
number of disks in the pool. This can be set to match the block size of
the filesystem which will be formatted onto the iSCSI target. Choosing a
smaller record size than the suggested value can reduce disk performance
and space efficiency.\

Click (Options) next to the desired zvol in to access the , , , and, for
an existing zvol snapshot, options.

Similar to datasets, a zvol name cannot be changed.

Choosing a zvol for deletion shows a warning that all snapshots of that
zvol will also be deleted.

### Setting Permissions {#\detokenize{storage:setting-permissions}}

\[\] Setting permissions is an important aspect of managing data access.
The web interface is meant to set the permissions for a pool or dataset
to make it available as a share. When a share is made available, the
client operating system and () is used to finetune the permissions of
the files and directories that are created by the client.

() contains configuration examples for several types of permission
scenarios. This section provides an overview of the options available
for configuring the initial set of permissions.

<span>note</span><span>Note:</span> For users and groups to be
available, they must either be first created using the instructions in
() or imported from a directory service using the instructions in ().
The dropdown menus described in this section are automatically truncated
to 50 entries for performance reasons. To find an unlisted entry, begin
typing the desired user or group name for the dropdown menu to show
matching results.

To set the permissions on a dataset, select it in , click (Options),
then . describes the options in this screen.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Path & string & Displays the path to the dataset or zvol directory.\
User & dropdown menu & Select the user to control the dataset. Users
created manually or imported from a directory service appear in the
dropdown menu.\
Group & dropdown menu & Select the group to control the dataset. Groups
created manually or imported from a directory service appear in the
dropdown menu.\
Access Mode & checkboxes & Set the read, write, and execute permissions
for the dataset.\
Apply Permissions Recursively & checkbox & Apply permissions recursively
to all directories and files within the current dataset.\
Traverse & checkbox & Movement permission for this dataset. Allows users
to view or interact with child datasets even when those users do not
have permission to view or manage the contents of this dataset.\

### ACL Management {#\detokenize{storage:acl-management}}

\[\]\[\] An Access Control List (ACL) is a set of account permissions
associated with a dataset and applied to directories or files within
that dataset. These permissions control the actions users can perform on
the dataset contents. ACLs are typically used to manage user
interactions with (). Datasets with an ACL have appended to their name
in the directory browser.

The ACL for a new file or directory is typically determined by the
parent directory ACL. An exception is when there are no or () in the
parent ACL , , or entries. These noninheriting entries are appended to
the ACL of the newly created file or directory based on the <span>
</span> (https://www.samba.org/samba/docs/using\_samba/ch08.html) or the
<span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=umask&sektion=2) value.

By default, a file ACL is preserved when it is moved or renamed within
the same dataset. The () can override this behavior to force an ACL to
be recalculated whenever the file moves, even within the same dataset.

Datasets optimized for SMB sharing can restrict ACL changes. See in the
().

ACLs are modified by adding or removing Access Control Entries (ACEs) in
. Find the desired dataset, click (Options), and select . The opens. The
ACL manager must be used to modify permissions on a dataset with an ACL.

\[\]

The ACL Manager options are split into the , , and sections. sorts these
options by their section.

<span>|&gt;p<span>0.15-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Section & Value & Description\

\
Setting & Section & Value & Description\

\

Path & File Information & string & Location of the dataset that is being
modified. Readonly.\
User & File Information & dropdown menu & User who controls the dataset.
This user always has permissions to read or write the ACL and read or
write attributes. Users created manually or imported from a () appear in
the dropdown menu.\
Apply User & File Information & checkbox & Confirm changes to User. To
prevent errors, changes to the User are submitted only when this box is
set.\
Group & File Information & dropdown menu & The group which controls the
dataset. This group has all permissions that are granted to the . Groups
created manually or imported from a () appear in the dropdown menu.\

\[t\] Apply Group | File Information

-

& checkbox & Confirm changes to Group. To prevent errors, changes to the
Group are submitted only when this box is set.\
Default ACL Options & File Information & dropdown menu & Default ACLs.
Choosing an entry loads a preset ACL that is configured to match general
permissions situations.\
Who & Access Control List & dropdown menu & Access Control Entry (ACE)
user or group. Select a specific or for this entry, to apply this entry
to the selected , to apply this entry to the selected , or to apply this
entry to all users and groups. See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=setfacl).\
User & Access Control List & dropdown menu & User account to which this
ACL entry applies. Only visible when is the chosen .\
Group & Access Control List & dropdown menu & Group to which this ACL
entry applies. Only visible when is the chosen .\
ACL Type & Access Control List & dropdown menu & How the are applied to
the chosen . Choose to grant the specified permissions and to restrict
the specified permissions.\
Permissions Type & Access Control List & dropdown menu & Choose the type
of permissions. shows general permissions. shows each specific type of
permission for finer control.\
Permissions & Access Control List & dropdown menu & Select permissions
to apply to the chosen . Choices change depending on the . See the ()
for descriptions of each permission.\
Flags Type & Access Control List & dropdown menu & Select the set of ACE
inheritance to display. shows unspecific inheritance options. shows
specific inheritance settings for finer control.\
Flags & Access Control List & dropdown menu & How this ACE is applied to
newly created directories and files within the dataset. flags enable or
disable ACE inheritance. flags allow further control of how the ACE is
applied to files and directories in the dataset. See the () for
descriptions of inheritance flags.\
Apply permissions recursively & Advanced & checkbox & Apply permissions
recursively to all directories and files in the current dataset.\
Apply permissions to child datasets & Advanced & checkbox & Apply
permissions recursively to all child datasets of the current dataset.
Only visible when is set.\
Strip ACLs & Advanced & checkbox & Set to remove all ACLs from the
current dataset. ACLs are also recursively stripped from directories and
child datasets when and are set.\

Additional ACEs are created by clicking and configuring the added
fields. One ACE is required in the ACL.

See <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=setfacl),
<span> </span> (https://linux.die.net/man/5/nfs4\_acl), and <span>
</span> (https://tools.ietf.org/html/draftfalknernfsv4acls00) for more
details about Access Control Lists, permissions, and inheritance flags.
The following lists show each permission or flag that can be applied to
an ACE with a brief description.

\[\] An ACE can have a variety of basic or advanced permissions:

-   : view file or directory contents, attributes, named attributes,
    and ACL. Includes the permission.

-   : adjust file or directory contents, attributes, and
    named attributes. Create new files or subdirectories. Includes
    the permission. Changing the ACL contents or owner is not allowed.

-   : Execute a file or move through a directory. Directory contents are
    restricted from view unless the permission is also applied. To
    traverse and view files in a directory, but not be able to open
    individual files, set the and permissions, then add the
    advanced flag.

-   : Apply all permissions.

<!-- -->

-   : View file contents or list directory contents.

-   : Create new files or modify any part of a file.

-   : Add new data to the end of a file.

-   : view the named attributes directory.

-   : create a named attribute directory. Must be paired with
    the permission.

-   : Execute a file, move through, or search a directory.

-   : delete files or subdirectories from inside a directory.

-   : view file or directory nonACL attributes.

-   : change file or directory nonACL attributes.

-   : remove the file or directory.

-   : view the ACL.

-   : change the ACL and the ACL mode.

-   : change the user and group owners of the file or directory.

-   : synchronous file read/write with the server. This permission does
    not apply to FreeBSD clients.

\[\] Basic inheritance flags only enable or disable ACE inheritance.
Advanced flags offer finer control for applying an ACE to new files or
directories.

-   : The ACE is inherited with subdirectories and files. It applies to
    new files.

-   : new subdirectories inherit the full ACE.

-   : The ACE can only be inherited once.

-   : Remove the ACE from permission checks but allow it to be inherited
    by new files or subdirectories. is removed from these new objects.

-   : set when the ACE has been inherited from another dataset.

Snapshots {#\detokenize{storage:snapshots}}
---------

\[\]\[\] To view and manage the listing of created snapshots, use . An
example is shown in .

<span>note</span><span>Note:</span> If snapshots do not appear, check
that the current time configured in () does not conflict with the , ,
and settings. If the snapshot was attempted but failed, an entry is
added to . This log file can be viewed in ().

\[\]

Each entry in the list includes the name of the dataset and snapshot.
Click (Expand) to view these options:

shows the exact time and date of the snapshot creation.

is the amount of space consumed by this dataset and all of its
descendants. This value is checked against the dataset quota and
reservation. The space used does not include the dataset reservation,
but does take into account the reservations of any descendent datasets.
The amount of space that a dataset consumes from its parent, as well as
the amount of space freed if this dataset is recursively deleted, is the
greater of its space used and its reservation. When a snapshot is
created, the space is initially shared between the snapshot and the
filesystem, and possibly with previous snapshots. As the filesystem
changes, space that was previously shared becomes unique to the
snapshot, and is counted in the used space of the snapshot. Deleting a
snapshot can increase the amount of space unique to, and used by, other
snapshots. The amount of space used, available, or referenced does not
take into account pending changes. While pending changes are generally
accounted for within a few seconds, disk changes do not necessarily
guarantee that the space usage information is updated immediately.

<span>tip</span><span>Tip:</span> Space used by individual snapshots can
be seen by running from ().

indicates the amount of data accessible by this dataset, which may or
may not be shared with other datasets in the pool. When a snapshot or
clone is created, it initially references the same amount of space as
the filesystem or snapshot it was created from, since its contents are
identical.

shows a confirmation dialog. Child clones must be deleted before their
parent snapshot can be deleted. While creating a snapshot is
instantaneous, deleting a snapshot can be I/O intensive and can take a
long time, especially when deduplication is enabled. In order to delete
a block in a snapshot, ZFS has to walk all the allocated blocks to see
if that block is used anywhere else; if it is not, it can be freed.

prompts for the name of the new dataset created from the cloned
snapshot. A default name is provided based on the name of the original
snapshot. Click the button to finish cloning the snapshot.

A clone is a writable copy of the snapshot. Since a clone is actually a
dataset which can be mounted, it appears in the screen rather than the
screen. By default, is added to the name of a snapshot when a clone is
created.

Clicking (Options) asks for confirmation before rolling back to the
chosen snapshot state. Clicking causes all files in the dataset to
revert to the state they were in when the snapshot was created.

<span>note</span><span>Note:</span> Rollback is a potentially dangerous
operation and causes any configured replication tasks to fail as the
replication system uses the existing snapshot when doing an incremental
backup. To restore the data within a snapshot, the recommended steps
are:

1.  Clone the desired snapshot.

2.  Share the clone with the share type or service running on the
    FreeNAS$^{\text{®}}$ system.

3.  After users have recovered the needed data, delete the clone in
    the tab.

This approach does not destroy any ondisk data and has no impact on
replication.

A range of snapshots can be deleted. Set the left column checkboxes for
each snapshot and click the icon above the table. Be careful when
deleting multiple snapshots.

Periodic snapshots can be configured to appear as shadow copies in newer
versions of Windows Explorer, as described in (). Users can access the
files in the shadow copy using Explorer without requiring any
interaction with the FreeNAS$^{\text{®}}$ web interface.

To quickly search through the snapshots list by name, type a matching
criteria into the text area. The listing will change to only display the
snapshot names that match the filter text.

<span>warning</span><span>Warning:</span> A snapshot and any files it
contains will not be accessible or searchable if the mount path of the
snapshot is longer than 88 characters. The data within the snapshot will
be safe, and the snapshot will become accessible again when the mount
path is shortened. For details of this limitation, and how to shorten a
long mount path, see ().

### Browsing a Snapshot Collection {#\detokenize{storage:browsing-a-snapshot-collection}}

\[\] All snapshots for a dataset are accessible as an ordinary
hierarchical filesystem, which can be reached from a hidden file located
at the root of every dataset. A user with permission to access that file
can view and explore all snapshots for a dataset like any other files
from the or via services such as , and . This is an advanced capability
which requires some command line actions to achieve. In summary, the
main changes to settings that are required are:

-   Snapshot visibility must be manually enabled in the ZFS properties
    of the dataset.

-   In Samba auxillary settings, the command must be modified to not
    hide the file, and the setting must be added.

The effect will be that any user who can access the dataset contents
will be able to view the list of snapshots by navigating to the
directory of the dataset. They will also be able to browse and search
any files they have permission to access throughout the entire snapshot
collection of the dataset.

A user’s ability to view files within a snapshot will be limited by any
permissions or ACLs set on the files when the snapshot was taken.
Snapshots are fixed as “readonly”, so this access does not permit the
user to change any files in the snapshots, or to modify or delete any
snapshot, even if they had write permission at the time when the
snapshot was taken.

<span>note</span><span>Note:</span> ZFS has a command which can list the
files that have changed between any two snapshot versions within a
dataset, or between any snapshot and the current data.

### Creating a Single Snapshot {#\detokenize{storage:creating-a-single-snapshot}}

\[\] To create a snapshot separately from a (), go to and click .

\[\]

Select an existing ZFS pool, dataset, or zvol to snapshot. To include
child datasets with the snapshot, set .

The snapshot can have a custom or be automatically named by a . Using a
allows the snapshot to be included in (). The dropdown is populated with
previously created schemas from ().

VMwareSnapshots {#\detokenize{storage:vmware-snapshots}}
---------------

\[\]\[\] is used to coordinate ZFS snapshots when using
FreeNAS$^{\text{®}}$ as a VMware datastore. When a ZFS snapshot is
created, FreeNAS$^{\text{®}}$ automatically snapshots any running VMware
virtual machines before taking a scheduled or manual ZFS snapshot of the
dataset or zvol backing that VMware datastore. Virtual machines for
FreeNAS$^{\text{®}}$ snapshots to be copied to VMware. The temporary
VMware snapshots are then deleted on the VMware side but still exist in
the ZFS snapshot and can be used as stable resurrection points in that
snapshot. These coordinated snapshots are listed in ().

shows the menu for adding a VMware snapshot and summarizes the available
options.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Hostname & string & Enter the IP address or hostname of the VMware host.
When clustering, use the IP address or hostname of the vCenter server
for the cluster.\
Username & string & Enter a user account name created on the VMware
host. The account must have permission to snapshot virtual machines.\
Password & string & Enter the password associated with .\
ZFS Filesystem & browse button & to the filesystem to snapshot.\
Datastore & dropdown menu & After entering the , , and , click to
populate the menu, then select the datastore to be synchronized.\

FreeNAS$^{\text{®}}$ connects to the VMware host after the credentials
are entered. The and dropdown menus are populated with information from
the VMware host. Choosing a datastore also selects any previously mapped
dataset.

Disks {#\detokenize{storage:disks}}
-----

\[\]\[\] To view all of the disks recognized by the FreeNAS$^{\text{®}}$
system, use . As seen in the example in , each disk entry displays its
device name, serial number, size, advanced power management settings,
acoustic level settings, and whether () tests are enabled. The pool
associated with the disk is displayed in the column. is displayed if the
disk is not being used in a pool. Click and select additional
information to be shown as columns in the table. Additional information
not shown in the table can be seen by clicking (Expand).

\[\]

To edit the options for a disk, click (Options) on a disk, then to open
the screen shown in . lists the configurable options.

To bulk edit disks, set the checkbox for each disk in the table then
click (Edit Disks). The page displays which disks are being edited and a
short list of configurable options. The () indicates the options
available when editing multiple disks.

To offline, online, or or replace the device, see ().

\[\]

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.10-2</span>
|&gt;p<span>0.10-2</span> |&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Value & Bulk Edit & Description\

\
Setting & Value & Bulk Edit & Description\

\

Name & string && This is the FreeBSD device name for the disk.\
Serial & string && This is the serial number of the disk.\
Description & string && Enter any notes about this disk.\
HDD Standby & dropdown menu & $\checkmark$ & Time of inactivity in
minutes before the drive enters standby mode to conserve energy. This
<span> </span>
(https://forums.freenas.org/index.php?threads/howtofindoutifadriveisspinningdownproperly.2068/)
shows how to determine if a drive has spun down. Temperature monitoring
is disabled if the disk is set to enter standby.\
Advanced Power Management & dropdown menu & $\checkmark$ & Select a
power management profile from the menu. The default value is .\
Acoustic Level & dropdown menu & $\checkmark$ & Default is . Other
values can be selected for disks that understand <span> </span>
(https://en.wikipedia.org/wiki/Automatic\_acoustic\_management).\
Enable S.M.A.R.T. & checkbox & $\checkmark$ & Enabled by default when
the disk supports S.M.A.R.T. Disabling S.M.A.R.T. tests prevents
collecting new temperature data for this disk. Historical temperature
data is still displayed in ().\
S.M.A.R.T. extra options & string & $\checkmark$ & Enter additional
<span> </span>
(https://www.smartmontools.org/browser/trunk/smartmontools/smartctl.8.in)
options.\
Critical & string && Threshold temperature in Celsius. If the drive
temperature is higher than this value, a level log entry is created and
an email is sent. disables this check.\
Difference & string && Report if the temperature of a drive has changed
by this many degrees Celsius since the last report. disables the
report.\
Informational & string && Report if drive temperature is at or above
this temperature in Celsius. disables the report.\
SED Password & string && Set or change the password of this SED. This
password is used instead of the global SED password in . See ().\
Clear SED Password & checkbox && Clear the SED password for this disk.\

<span>tip</span><span>Tip:</span> If the serial number for a disk is not
displayed in this screen, use the command from (). For example, to
determine the serial number of disk , type .

The function is used to discard an unused disk.

<span>warning</span><span>Warning:</span> Ensure all data is backed up
and the disk is no longer in use. Triplecheck that the correct disk is
being selected to be wiped, as recovering data from a wiped disk is
usually impossible. If there is any doubt, physically remove the disk,
verify that all data is still present on the FreeNAS$^{\text{®}}$
system, and wipe the disk in a separate computer.

Clicking offers several choices. erases only the partitioning
information on a disk, making it easy to reuse but without clearing
other old data. For more security, overwrites the entire disk with
zeros, while overwrites the entire disk with random binary data.

Quick wipes take only a few seconds. A wipe of a large disk can take
several hours, and a takes longer. A progress bar is displayed during
the wipe to track status.

### Replacing a Failed Disk {#\detokenize{storage:replacing-a-failed-disk}}

\[\]\[\] With any form of redundant RAID, failed drives must be replaced
as soon as possible to repair the degraded state of the RAID. Depending
on the hardware capabilities, it might be necessary to reboot to replace
the failed drive. Hardware that supports AHCI does not require a reboot.

Striping (RAID0) does not provide redundancy. Disk failure in a stripe
results in losing the pool. The pool must be recreated and data stored
in the failed stripe will have to be restored from backups.

<span>warning</span><span>Warning:</span> Encrypted pools must have a
valid passphrase to replace a failed disk. Set a passphrase and back up
the encryption key using the pool () attempting to replace the failed
drive.

Before physically removing the failed device, go to . Select the pool
name then click (Settings). Select and locate the failed disk. Then
perform these steps:

1.  Click (Options) on the disk entry, then to change the disk status
    to OFFLINE. This step removes the device from the pool and prevents
    swap issues. encrypted disks that are set cannot be set back . If
    the hardware supports hotpluggable disks, click the disk button and
    pull the disk, then skip to step 3. If there is no but only , the
    disk is already offlined and this step can be skipped.

    <span>note</span><span>Note:</span> If the process of changing the
    disk status to OFFLINE fails with a “disk offline failed no valid
    replicas” message, the pool must be scrubbed first with the button
    in . After the scrub completes, try again before proceeding.

2.  After the disk is replaced and is showing as OFFLINE,
    click (Options) on the disk again and then . Select the replacement
    disk from the dropdown menu and click the button. After clicking the
    button, the pool begins resilvering.

    \[\] Encrypted pools require entering the () when choosing a
    replacement disk. Clicking begins the process to reformat the
    replacement, apply the current pool encryption algorithm, and
    resilver the pool. The current pool encryption key and passphrase
    remains valid, but any pool recovery key file is invalidated by the
    replacement process. To maximize pool security, it is recommended
    to ().

3.  After the drive replacement process is complete, readd the replaced
    disk in the () screen.

To refresh the screen with updated entries, click . If any problems
occur during a disk replacement process, one of the disks can be
detached. To detach a disk in the replacement process, find the disk to
be replaced and click (Options) .

shows an example of going to and replacing a disk in an active pool.

\[\]

After the resilver is complete, the pool status shows a resilver status
and indicates any errors. indicates that the disk replacement was
successful in this example.

<span>note</span><span>Note:</span> A disk that is failing but has not
completely failed can be replaced in place, without first removing it.
Whether this is a good idea depends on the overall condition of the
failing disk. A disk with a few newlybad blocks that is otherwise
functional can be left in place during the replacement to provide data
redundancy. A drive that is experiencing continuous errors can actually
slow down the replacement. In extreme cases, a disk with serious
problems might spend so much time retrying failures that it could
prevent the replacement resilvering from completing before another drive
fails.

\[\]

#### Removing a Log or Cache Device {#\detokenize{storage:removing-a-log-or-cache-device}}

\[\] Added log or cache devices appear in . Clicking the device enables
the and buttons.

Log and cache devices can be safely removed or replaced with these
buttons. Both types of devices improve performance, and throughput can
be impacted by their removal.

### Replacing Disks to Grow a Pool {#\detokenize{storage:replacing-disks-to-grow-a-pool}}

\[\] The recommended method for expanding the size of a ZFS pool is to
preplan the number of disks in a vdev and to stripe additional vdevs
from () as additional capacity is needed.

But adding vdevs is not an option if there are not enough unused disk
ports. If there is at least one unused disk port or drive bay, a single
disk at a time can be replaced with a larger disk, waiting for the
resilvering process to include the new disk into the pool, removing the
old disk, then repeating with another disk until all of the original
disks have been replaced. At that point, the pool capacity automatically
increases to include the new space.

One advantage of this method is that disk redundancy is present during
the process.

<span>note</span><span>Note:</span> A pool that is configured as a
<span> </span>
(https://en.wikipedia.org/wiki/Standard\_RAID\_levels\#RAID\_0) can only
be increased by following the steps in ().

1.  Connect the new, larger disk to the unused disk port or drive bay.

2.  Go to .

3.  Select the pool and click (Settings) .

4.  Select one of the old, smaller disks in the pool. Click (Options) .
    Choose the new disk as the replacement.

The status of the resilver process is shown on the screen, or can be
viewed with . When the new disk has resilvered, the old one is
automatically offlined. It can then be removed from the system, and that
port or bay used to hold the next new disk.

If a unused disk port or bay is not available, a drive can be replaced
with a larger one as shown in (). This process is slow and places the
system in a degraded state. Since a failure at this point could be
disastrous, Replace one drive at a time and wait for the resilver
process to complete on the replaced drive before replacing the next
drive. After all the drives are replaced and the final resilver
completes, the added space appears in the pool.

Importing a Disk {#\detokenize{storage:importing-a-disk}}
----------------

\[\] The screen, shown in , is used to import disks that are formatted
with UFS (BSD Unix), FAT(MSDOS) or NTFS (Windows), or EXT2 (Linux)
filesystems. This is a designed to be used as a onetime import, copying
the data from that disk into a dataset on the FreeNAS$^{\text{®}}$
system. Only one disk can be imported at a time.

<span>note</span><span>Note:</span> Imports of EXT3 or EXT4 filesystems
are possible in some cases, although neither is fully supported. EXT3
journaling is not supported, so those filesystems must have an external
utility, like the one provided by <span> </span>
(http://e2fsprogs.sourceforge.net/), run on them before import. EXT4
filesystems with extended attributes or inodes greater than 128 bytes
are not supported. EXT4 filesystems with EXT3 journaling must have an
run on them before import, as described above.

\[\]

Use the dropdown menu to select the disk to import, confirm the detected
filesystem is correct, and browse to the ZFS dataset that will hold the
copied data. If the filesystem is selected, an additional dropdown menu
is displayed. Use this menu to select the locale if nonASCII characters
are present on the disk.

After clicking , the disk is mounted and its contents are copied to the
specified dataset. The disk is unmounted after the copy operation
completes.

After importing a disk, a dialog allows viewing or downloading the disk
import log.

Multipaths {#\detokenize{storage:multipaths}}
----------

\[\] This option is only displayed on systems that contain
multipathcapable hardware like a chassis equipped with a dual SAS
expander backplane or an external JBOD that is wired for multipath.

FreeNAS$^{\text{®}}$ uses <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=gmultipath) to provide <span>
</span> (https://en.wikipedia.org/wiki/Multipath\_I/O) support on
systems containing multipathcapable hardware.

Multipath hardware adds fault tolerance to a NAS as the data is still
available even if one disk I/O path has a failure.

FreeNAS$^{\text{®}}$ automatically detects active/active and
active/passive multipathcapable hardware. Discovered multipathcapable
devices are placed in multipath units with the parent devices hidden.
The configuration is displayed in .

Overprovisioning {#\detokenize{storage:overprovisioning}}
================

\[\] Overprovisioning SSDs can be done using the command in the (). This
can be useful for many different scenarios. Perhaps the most useful
benefit of overprovisioning is that it can extend the life of an SSD
greatly. Overprovisioning an SSD distributes the total number of writes
and erases across more flash blocks on the drive. Read more about
overprovisioning SSDs <span> </span>
(https://www.seagate.com/techinsights/ssdoverprovisioningbenefitsmasterti/).

The command to overprovision an SSD is , where is the device name of the
SSD and is the desired size of the provision in or . Here is an example
of the command: . When no size is specified, it reverts the provision
back the full size of the device.

\[\]

<span>note</span><span>Note:</span> Some SATA devices may be limited to
one resize per power cycle. Some BIOS may block resize during boot and
require a live power cycle.

Directory Services {#\detokenize{directoryservices:directory-services}}
==================

\[\]\[\] FreeNAS$^{\text{®}}$ supports integration with these directory
services:

-   () (for Windows 2000 and higher networks)

-   ()

-   ()

FreeNAS$^{\text{®}}$ also supports (), (), and the ability to add more
parameters to ().

This section summarizes each of these services and the available
configuration options within the FreeNAS$^{\text{®}}$ web interface.
After successfully enabling a directory service, appears in the top
toolbar row. Click to show the menu. This menu shows the name and status
of each directory service.

Active Directory {#\detokenize{directoryservices:active-directory}}
----------------

\[\] Active Directory (AD) is a service for sharing resources in a
Windows network. AD can be configured on a Windows server that is
running Windows Server 2000 or higher or on a Unixlike operating system
that is running <span> </span>
(https://wiki.samba.org/index.php/Setting\_up\_Samba\_as\_an\_Active\_Directory\_Domain\_Controller\#Provisioning\_a\_Samba\_Active\_Directory).
Since AD provides authentication and authorization services for the
users in a network, it is not necessary to recreate the same user
accounts on the FreeNAS$^{\text{®}}$ system. Instead, configure the
Active Directory service so account information and imported users can
be authorized to access the SMB shares on the FreeNAS$^{\text{®}}$
system.

Many changes and improvements have been made to Active Directory support
within FreeNAS$^{\text{®}}$. It is strongly recommended to update the
system to the latest FreeNAS$^{\text{®}}$ 11.3 before attempting Active
Directory integration.

Ensure name resolution is properly configured before configuring the
Active Directory service. the domain name of the Active Directory domain
controller from () on the FreeNAS$^{\text{®}}$ system. If the fails,
check the DNS server and default gateway settings in on the
FreeNAS$^{\text{®}}$ system.

By default, in the () is enabled. This adds FreeNAS$^{\text{®}}$ () DNS
records to the Active Directory DNS when the domain is joined. Disabling
means that the Active Directory DNS records must be updated manually.

Active Directory relies on Kerberos, a timesensitive protocol. During
the domain join process the <span> </span>
(https://docs.microsoft.com/enus/openspecs/windows\_protocols/msadts/f96ff8ecc6604d6c924fc0dbbcac1527)
server is added as the preferred NTP server. The time on the
FreeNAS$^{\text{®}}$ system and the Active Directory Domain Controller
cannot be out of sync by more than five minutes in a default Active
Directory environment. An () is sent when the time is out of sync.

To ensure both systems are set to the same time:

-   use the same NTP server (set in on the FreeNAS$^{\text{®}}$ system)

-   set the same timezone

-   set either localtime or universal time at the BIOS level

shows settings.

\[\]

describes the configurable options. Some settings are only available in
Advanced Mode. Click the button to show the Advanced Mode settings. Go
to and set the option to always show advanced options.

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.14-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.54-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Domain Name & string && Name of the Active Directory domain () or child
domain (). This field is mandatory. will be inactive until valid input
is entered. Hidden when a is selected.\
Domain Account Name & string && Name of the Active Directory
administrator account. This field is mandatory. will be inactive until
valid input is entered. Hidden when a is selected.\
Domain Account Password & string && Password for the Active Directory
administrator account. Required the first time a domain is configured.
After initial configuration, the password is not needed to edit, start,
or stop the service.\
Encryption Mode & dropdown & $\checkmark$ & Choices are , , or . See and
for more information about SSL and TLS.\
Certificate & dropdown menu & $\checkmark$ & Select the Active Directory
server certificate if SSL connections are used. If a certificate does
not exist, create or import a (), then create a certificate on the
Active Directory server. Import the certificate to the
FreeNAS$^{\text{®}}$ system using the () menu. It is recommended to
leave this dropdown unset when configuring LDAPs.

To clear a saved certificate, choose the blank entry and click .\
Validate Certificate & checkbox & $\checkmark$ & Check server
certificates in a TLS session.\
Verbose logging & checkbox & $\checkmark$ & Set to log attempts to join
the domain to .\
Allow Trusted Domains & checkbox & $\checkmark$ & Do not set this unless
the network has active <span> </span>
(https://docs.microsoft.com/enus/previousversions/windows/itpro/windowsserver2003/cc757352(v=ws.10))
and managing files on multiple domains is required. Setting this option
generates more winbindd traffic and slows down filtering with user and
group information. If enabled, also configuring the idmap ranges and a
backend for each trusted domain in the environment is recommended.\
Use Default Domain & checkbox & $\checkmark$ & Unset to prepend the
domain name to the username. Unset to prevent name collisions when is
set and multiple domains use the same username.\
Allow DNS updates & checkbox & $\checkmark$ & Set to enable Samba to do
DNS updates when joining a domain.\
Disable FreeNAS Cache & checkbox & $\checkmark$ & Disable caching AD
users and groups. Setting this hides all AD users and groups from web
interface dropdown menus and autocompletion suggestions, but manually
entering names is still allowed. This can help when unable to bind to a
domain with a large number of users or groups.\
Site Name & string & $\checkmark$ & Autodetected site name. Do not
change this unless the detected site name is incorrect for the
particular AD environment.\
Kerberos Realm & dropdown menu & $\checkmark$ & Select the realm created
using the instructions in ().\
Kerberos Principal & dropdown menu & $\checkmark$ & Select a keytab
created using the instructions in (). Selecting a principal hides the
and fields. An existing account name is not overwritten by the
principal.\
Computer Account OU & string & $\checkmark$ & The OU in which new
computer accounts are created. The OU string is read from top to bottom
without RDNs. Slashes () are used as delimiters, like . The backslash ()
is used to escape characters but not as a separator. Backslashes are
interpreted at multiple levels and might require doubling or even
quadrupling to take effect. When this field is blank, new computer
accounts are created in the Active Directory default OU.\
AD Timeout & integer & $\checkmark$ & Increase the number of seconds
before timeout if the AD service does not immediately start after
connecting to the domain.\
DNS Timeout & integer & $\checkmark$ & Increase the number of seconds
before a timeout occurs if AD DNS queries timeout.\
Idmap backend & dropdown menu and Edit Idmap button & $\checkmark$ &
Choose the backend to map Windows security identifiers (SIDs) to UNIX
UIDs and GIDs. See for a summary of the available backends. Click to
configure the selected backend.\
Windbind NSS Info & dropdown menu & $\checkmark$ & Choose the schema to
use when querying AD for user/group information. uses the RFC2307 schema
support included in Windows 2003 R2, is for Services For Unix 3.0 or
3.5, and is for Services For Unix 2.0.\
SASL wrapping & dropdown menu & $\checkmark$ & Choose how LDAP traffic
is transmitted. Choices are (plain text), (signed only), or (signed and
encrypted). Windows 2000 SP3 and newer can be configured to enforce
signed LDAP connections. This should be set to when using Microsft
Active Directory.

This can be set to or when using Samba Active Directory if has been
explicitly enabled in the Samba Domain Controller configuration.\
Enable (requires password or Kerberos principal) & checkbox && Activate
the Active Directory service.\
Netbios Name & string & $\checkmark$ & Name for the computer object
generated in AD. Limited to 15 characters. Automatically populated with
the original hostname of the system. This be different from the name.\
NetBIOS alias & string & $\checkmark$ & Limited to 15 characters.\

summarizes the backends which are available in the dropdown menu. Each
backend has its own <span> </span>
(http://samba.org.ru/samba/docs/man/manpages/) that gives implementation
details.

Changing idmap backends automatically refreshes the resolver cache by
sending SIGHUP (signal hang up) to the parent process. To find this
parent process, start an () session with the FreeNAS$^{\text{®}}$ system
and enter . To manually send the SIGHUP, enter , where is the parent
process ID.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.66-2</span>|</span>

\[\]\
Value & Description\

\
Value & Description\

\

ad & AD server uses RFC2307 or Services For Unix schema extensions.
Mappings must be provided in advance by adding the uidNumber attributes
for users and gidNumber attributes for groups in the AD.\
autorid & Similar to , but automatically configures the range to be used
for each domain, so there is no need to specify a specific range for
each domain in the forest. The only needed configuration is the range of
UID or GIDs to use for user and group mappings and an optional size for
the ranges.\
ldap & Stores and retrieves mapping tables in an LDAP directory service.
Default for LDAP directory service.\
nss & Provides a simple means of ensuring that the SID for a Unix user
is reported as the one assigned to the corresponding domain user.\
rfc2307 & IDs for AD users stored as <span> </span>
(https://tools.ietf.org/html/rfc2307) ldap schema extensions. This
module can either look up the IDs in the AD LDAP servers or an external
(nonAD) LDAP server.\
rid & Default for AD. Requires an explicit idmap configuration for each
domain, using disjoint ranges where a writeable default idmap range is
to be defined, using a backend like tdb or ldap.\
script & Stores mapping tables for clustered environments in the
winbind\_cache tdb.\
tdb & Default backend used by winbindd for storing mapping tables.\

immediately refreshes the web interface directory service cache. This
occurs automatically once a day as a cron job.

If there are problems connecting to the realm, <span> </span>
(https://support.microsoft.com/enus/help/909264/namingconventionsinactivedirectoryforcomputersdomainssitesand)
the settings do not include any disallowed characters. Active Directory
does not allow characters in Domain or NetBIOS names. The length of
those names is also limited to 15 characters. The Administrator account
password cannot contain the character.

It can take a few minutes after configuring the Active Directory service
for the AD information to be populated to the FreeNAS$^{\text{®}}$
system. To check the AD join progress, open the web interface Task
Manager in the upperright corner. Any errors during the join process are
also displayed in the Task Manager.

Once populated, the AD users and groups will be available in the
dropdown menus of the screen of a dataset.

The Active Directory users and groups that are imported to the
FreeNAS$^{\text{®}}$ system are shown by typing commands in the
FreeNAS$^{\text{®}}$ ():

-   View users:

-   View groups:

In addition, shows the domains and tests the connection. When
successful, shows a message similar to:

\[commandchars=\
{}\] checking the trust secret for domain YOURDOMAIN via RPC calls
succeeded

To manually check that a specified user can authenticate, open the ()
and enter , where is the SMB share name, is the name of the trusted
domain, and is the user account for authentication testing.

and can provide more troubleshooting information if no users or groups
are listed in the output.

<span>tip</span><span>Tip:</span> Sometimes network users do not appear
in the dropdown menu of a screen but the commands display these users.
This is typically due to the FreeNAS$^{\text{®}}$ system taking longer
than the default ten seconds to join Active Directory. Increase the
value of to 60 seconds.

### Leaving the Domain {#\detokenize{directoryservices:leaving-the-domain}}

\[\] A button appears on the service dialog when a domain is connected.
To leave the domain, click the button and enter credentials with
privileges sufficient to permit leaving.

### Troubleshooting Tips {#\detokenize{directoryservices:troubleshooting-tips}}

\[\] Active Directory uses DNS to determine the location of the domain
controllers and global catalog servers in the network. Use to determine
the SRV records of the network and change the weight and/or priority of
the SRV record to reflect the fastest server. More information about SRV
records can be found in the Technet article <span> </span>
(https://docs.microsoft.com/enus/previousversions/windows/itpro/windowsserver2003/cc759550(v=ws.10)).

The realm used depends on the priority in the SRV DNS record. DNS can
override the system Active Directory settings. When unable to connect to
the correct realm, check the SRV records on the DNS server.

An expired password for the administrator account will cause to fail.
Ensure the password is still valid and doublecheck the password on the
AD account being used does not include any spaces, special symbols, and
is not unusually long.

If the Windows server version is lower than 2008 R2, try creating a
entry on the Windows server Organizational Unit (OU). When creating this
entry, enter the FreeNAS$^{\text{®}}$ hostname in the field. Make sure
it is under 15 characters, the same name as the one set in the field in
, and the same in settings.

If the cache becomes out of sync due to an AD server being taken off and
back online, resync the cache using .

If any of the commands fail or result in a traceback, create a bug
report at . Include the commands in the order in which they were run and
the exact wording of the error message or traceback.

LDAP {#\detokenize{directoryservices:ldap}}
----

\[\] FreeNAS$^{\text{®}}$ includes an <span> </span>
(http://www.openldap.org/) client for accessing information from an LDAP
server. An LDAP server provides directory services for finding network
resources such as users and their associated permissions. Examples of
LDAP servers include Mac OS X Server, Novell eDirectory, and OpenLDAP
running on a BSD or Linux system. If an LDAP server is running on the
network, configure the FreeNAS$^{\text{®}}$ LDAP service so network
users can authenticate to the LDAP server and have authorized access to
the data stored on the FreeNAS$^{\text{®}}$ system.

<span>note</span><span>Note:</span> LDAP authentication for SMB shares
is disabled unless the LDAP directory has been configured for and
populated with Samba attributes. The most popular script for performing
this task is <span> </span>
(https://wiki.samba.org/index.php/4.1\_smbldaptools). The LDAP server
must support SSL/TLS and the certificate for the LDAP server CA must be
imported with . NonCA certificates are not currently supported.

<span>tip</span><span>Tip:</span> Apple’s <span> </span>
(https://manuals.info.apple.com/MANUALS/0/MA954/en\_US/Open\_Directory\_Admin\_v10.5\_3rd\_Ed.pdf)
is an LDAPcompatible directory service into which FreeNAS$^{\text{®}}$
can be integrated. The forum post <span> </span>
(https://forums.freenas.org/index.php?threads/howtofreenaswithopendirectoryinmacosxenvironments.46493/)
has more information.

shows the LDAP Configuration section from .

\[\]

summarizes the available configuration options. Some settings are only
available in Advanced Mode. Click the button to show the Advanced Mode
settings. Go to and set the option to always show advanced options.

Those new to LDAP terminology should read the <span> </span>
(http://www.openldap.org/doc/admin24/).

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.14-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.54-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Hostname & string && LDAP server hostnames or IP addresses. Separate
entries with an empty space. Multiple hostnames or IP addresses can be
entered to create an LDAP failover priority list. If a host does not
respond, the next host in the list is tried until a new connection is
established.\
Base DN & string && Top level of the LDAP directory tree to be used when
searching for resources (Example: ).\
Bind DN & string && Administrative account name on the LDAP server
(Example: ).\
Bind Password & string && Password for the . Click to view or obscure
the password characters.\
Allow Anonymous Binding & checkbox & $\checkmark$ & Instruct the LDAP
server to disable authentication and allow read and write access to any
client.\
Kerberos Realm & dropdown menu & $\checkmark$ & The realm created using
the instructions in ().\
Kerberos Principal & dropdown menu & $\checkmark$ & The location of the
principal in the keytab created as described in ().\
Encryption Mode & dropdown menu & $\checkmark$ & Options for encrypting
the LDAP connection:

-   do not encrypt the LDAP connection.

-   encrypt the LDAP connection with SSL on port .

-   encrypt the LDAP connection with STARTTLS on the default LDAP port .

\
Certificate & dropdown menu & $\checkmark$ & () to use when performing
LDAP certificatebased authentication. To configure LDAP certificatebased
authentication, create a Certificate Signing Request for the LDAP
provider to sign. A certificate is not required when using
username/password or Kerberos authentication.\
Validate Certificate & checkbox & $\checkmark$ & Verify certificate
authenticity.\
Disable LDAP User/Group Cache & checkbox & $\checkmark$ & Disable
caching LDAP users and groups in large LDAP environments. When caching
is disabled, LDAP users and groups do not appear in dropdown menus, but
are still accepted when manually entered.\
LDAP timeout & integer & $\checkmark$ & Increase this value in seconds
if obtaining a Kerberos ticket times out.\
DNS timeout & integer & $\checkmark$ & Increase this value in seconds if
DNS queries timeout.\
Idmap Backend & dropdown menu & $\checkmark$ & Backend used to map
Windows security identifiers (SIDs) to UNIX UIDs and GIDs. See for a
summary of the available backends. To configure the selected backend,
click .\
Samba Schema & checkbox & $\checkmark$ & Set if LDAP authentication for
SMB shares is required the LDAP server is configured with Samba
attributes.\
Auxiliary Parameters & string & $\checkmark$ & Additional options for
<span> </span> (https://arthurdejong.org/nsspamldapd/nslcd.conf.5).\
Schema & dropdown menu & $\checkmark$ & If is set, select the schema to
use. Choices are and .\
Enable & checkbox && Unset to disable the configuration without deleting
it.\

LDAP users and groups appear in the dropdown menus of the screen of a
dataset after configuring the LDAP service. Type in the
FreeNAS$^{\text{®}}$ () to verify the users have been imported. Type to
verify the groups have been imported. When the is enabled, LDAP users
also appear in the output of .

If the users and groups are not listed, refer to <span> </span>
(http://www.openldap.org/doc/admin24/appendixcommonerrors.html) for
common errors and how to fix them.

Any LDAP bind errors are displayed during the LDAP bind process. When
troubleshooting LDAP, you can open the FreeNAS$^{\text{®}}$ () and find
errors in . When is enabled, any Samba errors are recorded in .
Additional details are saved in .

To clear LDAP users and groups from FreeNAS$^{\text{®}}$, go to , clear
the field, unset , and click . Confirm LDAP users and groups are cleared
by going to the and viewing the output of the and commands.

NIS {#\detokenize{directoryservices:nis}}
---

\[\] The Network Information Service (NIS) maintains and distributes a
central directory of Unix user and group information, hostnames, email
aliases, and other textbased tables of information. If an NIS server is
running on the network, the FreeNAS$^{\text{®}}$ system can be
configured to import the users and groups from the NIS directory.

Click the button if a new NIS user needs immediate access to
FreeNAS$^{\text{®}}$. This occurs automatically once a day as a cron
job.

<span>note</span><span>Note:</span> In Windows Server 2016, Microsoft
removed the Identity Management for Unix (IDMU) and NIS Server Role. See
<span> </span>
(https://blogs.technet.microsoft.com/activedirectoryua/2016/02/09/identitymanagementforunixidmuisdeprecatedinwindowsserver/).

shows the section. summarizes the configuration options.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

NIS domain & string & Name of NIS domain.\
NIS servers & string & Commadelimited list of hostnames or IP
addresses.\
Secure mode & checkbox & Set to have <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=ypbind) refuse to bind to any
NIS server not running as root on a TCP port over 1024.\
Manycast & checkbox & Set to have to bind to the server that responds
the fastest. This is useful when no local NIS server is available on the
same subnet.\
Enable & checkbox & Unset to disable the configuration without deleting
it.\

Kerberos Realms {#\detokenize{directoryservices:kerberos-realms}}
---------------

\[\] A default Kerberos realm is created for the local system in
FreeNAS$^{\text{®}}$. can be used to view and add Kerberos realms. If
the network contains a Key Distribution Center (KDC), click to add the
realm. The configuration screen is shown in .

\[\]

summarizes the configurable options. Some settings are only available in
Advanced Mode. To see these settings, either click or configure the
system to always display these settings by setting in .

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.14-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.54-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Realm & string && Name of the realm.\
KDC & string & $\checkmark$ & Name of the Key Distribution Center.\
Admin Server & string & $\checkmark$ & Server where all changes to the
database are performed.\
Password Server & string & $\checkmark$ & Server where all password
changes are performed.\

Kerberos Keytabs {#\detokenize{directoryservices:kerberos-keytabs}}
----------------

\[\] Kerberos keytabs are used to do Active Directory or LDAP joins
without a password. This means the password for the Active Directory or
LDAP administrator account does not need to be saved into the
FreeNAS$^{\text{®}}$ configuration database, which is a security risk in
some environments.

When using a keytab, it is recommended to create and use a less
privileged account for performing the required queries as the password
for that account will be stored in the FreeNAS$^{\text{®}}$
configuration database. To create the keytab on a Windows system, use
the <span> </span>
(https://docs.microsoft.com/enus/windowsserver/administration/windowscommands/ktpass)
command:

\[commandchars=\
{}\] ktpass.exe /out freenas.keytab /princ http/useraccount@EXAMPLE.COM
/mapuser useraccount /ptype KRB5NTPRINCIPAL /crypto ALL /pass userpass

where:

-   is the file to upload to the FreeNAS$^{\text{®}}$ server.

-   is the name of the user account for the FreeNAS$^{\text{®}}$ server
    generated in <span>
    </span> (https://technet.microsoft.com/enus/library/aa998508(v=exchg.65).aspx).

-   is the principal name written in the format . By convention, the
    kerberos realm is written in all caps, but make sure the case used
    for the () matches the realm name. See <span> </span>
    (https://docs.microsoft.com/enus/windowsserver/administration/windowscommands/ktpass\#BKMK\_remarks)
    about using for more details.

-   is the password associated with .

Setting to allows using all supported cryptographic types. These keys
can be specified instead of :

-   is used for compatibility.

-   adheres more closely to the MIT implementation and is used
    for compatibility.

-   uses 128bit encryption.

-   uses AES256CTSHMACSHA196 encryption.

-   uses AES128CTSHMACSHA196 encryption.

This will create a keytab with sufficient privileges to grant tickets.

After the keytab is generated, add it to the FreeNAS$^{\text{®}}$ system
using .

To instruct the Active Directory service to use the keytab, select the
installed keytab using the dropdown menu in Advanced Mode. When using a
keytab with Active Directory, make sure that username and userpass in
the keytab matches the Domain Account Name and Domain Account Password
fields in .

To instruct LDAP to use a principal from the keytab, select the
principal from the dropdown menu in Advanced Mode.

Kerberos Settings {#\detokenize{directoryservices:kerberos-settings}}
-----------------

\[\] Configure additional Kerberos parameters in the section. shows the
fields available:

\[\]

-   Define any additional settings for use by some
    Kerberos applications. The available settings and syntax is listed
    in the <span>
    </span> (http://web.mit.edu/kerberos/krb51.12/doc/admin/conf\_files/krb5\_conf.html\#appdefaults).

-   Define any settings used by the Kerberos library. The available
    settings and their syntax are listed in the <span>
    </span> (http://web.mit.edu/kerberos/krb51.12/doc/admin/conf\_files/krb5\_conf.html\#libdefaults).

Sharing {#\detokenize{sharing:sharing}}
=======

\[\]\[\] Shares provide and control access to an area of storage.
Consider factors like operating system, security, transfer speed, and
user access before creating a new share. This information can help
determine the type of share, if multiple datasets are needed to divide
the storage into areas with different access and permissions, and the
complexity of setting up permissions.

Note that shares are only used to provide access to data. Deleting a
share configuration does not affect the data that was being shared.

These types of shares and services are available:

-   (): Apple Filing Protocol shares are used when the client computers
    all run macOS. Apple has deprecated AFP in favor of (). Using AFP in
    modern networks is no longer recommended.

-   (): Network File System shares are accessible from macOS, Linux,
    BSD, and the professional and enterprise versions (but not the
    home editions) of Windows. This can be a good choice when the client
    computers do not all run the same operating system but NFS client
    software is available for all of them.

-   (): WebDAV shares are accessible using an authenticated web
    browser (readonly) or <span> </span>
    (https://en.wikipedia.org/wiki/WebDAV\#Client\_support) running on
    any operating system.

-   (): Server Message Block shares, also known as Common Internet File
    System (CIFS) shares, are accessible by Windows, macOS, Linux, and
    BSD computers. Access is slower than an NFS share due to the
    singlethreaded design of Samba. SMB provides more configuration
    options than NFS and is a good choice on a network for Windows or
    Mac systems. However, it is a poor choice if the CPU on the
    FreeNAS$^{\text{®}}$ system is limited. If it is maxed out, upgrade
    the CPU or consider a different type of share.

-   (): Block or iSCSI shares appear as an unformatted disk to clients
    running iSCSI initiator software or a virtualization solution such
    as VMware. These are usually used as virtual drives.

Fast access from any operating system can be obtained by configuring the
() service instead of a share and using a crossplatform FTP file manager
application such as <span> </span> (https://filezillaproject.org/).
Secure FTP can be configured if the data needs to be encrypted.

When data security is a concern and the network users are familiar with
SSH command line utilities or <span> </span>
(https://winscp.net/eng/index.php), consider using the () service
instead of a share. It is slower than unencrypted FTP due to the
encryption overhead, but the data passing through the network is
encrypted.

<span>note</span><span>Note:</span> It is generally a mistake to share a
pool or dataset with more than one share type or access method.
Different types of shares and services use different file locking
methods. For example, if the same pool is configured to use both NFS and
FTP, NFS will lock a file for editing by an NFS user, but an FTP user
can simultaneously edit or delete that file. This results in lost edits
and confused users. Another example: if a pool is configured for both
AFP and SMB, Windows users can be confused by the “extra” filenames used
by Mac files and delete them. This corrupts the files on the AFP share.
Pick the one type of share or service that makes the most sense for the
types of clients accessing that pool, and use that single type of share
or service. To support multiple types of shares, divide the pool into
datasets and use one dataset per share.

This section demonstrates configuration and finetuning of AFP, NFS, SMB,
WebDAV, and iSCSI shares. FTP and SSH configurations are described in
().

Apple (AFP) Shares {#\detokenize{sharing:apple-afp-shares}}
------------------

\[\]\[\] FreeNAS$^{\text{®}}$ uses the <span> </span>
(http://netatalk.sourceforge.net/) AFP server to share data with Apple
systems. This section describes the configuration screen for finetuning
AFP shares. It then provides configuration examples for configuring Time
Machine to back up to a dataset on the FreeNAS$^{\text{®}}$ system and
for connecting to the share from a macOS client.

Create a share by clicking , then .

New AFP shares are visible in the menu.

The configuration options shown in appear after clicking (Options) on an
existing share, and selecting the option. The values showing for these
options will vary, depending upon the information given when the share
was created.

\[\]

<span>note</span><span>Note:</span> summarizes the options available to
finetune an AFP share. Leaving these options at the default settings is
recommended as changing them can cause unexpected behavior. Most
settings are only available with . Do change an advanced option without
fully understanding the function of that option. Refer to <span> </span>
(http://netatalk.sourceforge.net/2.2/htmldocs/configuration.html) for a
more detailed explanation of these options.

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.14-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.54-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Path & browse button && Browse to the pool or dataset to share. Do not
nest additional pools, datasets, or symbolic links beneath this path
because Netatalk does not fully support that.\
Name & string && Enter the pool name that appears in macOS after
selecting in the Finder menu. Limited to 27 characters and cannot
contain a period.\
Comment & string & $\checkmark$ & Optional comment.\
Allow list & string & $\checkmark$ & Commadelimited list of allowed
users and/or groups where groupname begins with a . Note that adding an
entry will deny any user/group that is not specified.\
Deny list & string & $\checkmark$ & Commadelimited list of denied users
and/or groups where groupname begins with a . Note that adding an entry
will allow all users/groups that are not specified.\
Read Only Access & string & $\checkmark$ & Commadelimited list of users
and/or groups who only have read access where groupname begins with a .\
Read/Write Access & string & $\checkmark$ & Commadelimited list of users
and/or groups who have read and write access where groupname begins with
a .\
Time Machine & checkbox && Set to advertise FreeNAS$^{\text{®}}$ as a
Time Machine disk so it can be found by Macs. Setting multiple shares
for Time Machine use is not recommended. When multiple Macs share the
same pool, low diskspace issues and intermittently failed backups can
occur.\
Time Machine Quota & integer && Appears when is set. Enter a storage
quota for each Time Machine backup on this share. The share must be
remounted for any changes to this value to take effect.\
Use as home share & checkbox && Allows the share to host user home
directories. Each user is given a personal home directory when
connecting to the share which is not accessible by other users. This
allows for a personal, dynamic share. Only one share can be used as the
home share.\
Zero Device Numbers & checkbox & $\checkmark$ & Enable when the device
number is not constant across a reboot.\
No Stat & checkbox & $\checkmark$ & If set, AFP does not stat the pool
path when enumerating the pools list. Useful for automounting or pools
created by a preexec script.\
AFP3 UNIX Privs & checkbox & $\checkmark$ & Set to enable Unix
privileges supported by Mac OS X 10.5 and higher. Do not enable if the
network has Mac OS X 10.4 or lower clients. Those systems do not support
this feature.\
Default file permissions & checkboxes & $\checkmark$ & Only works with
Unix ACLs. New files created on the share are set with the selected
permissions.\
Default directory permissions & checkboxes & $\checkmark$ & Only works
with Unix ACLs. New directories created on the share are set with the
selected permissions.\
Default umask & integer & $\checkmark$ & Umask is used for newly created
files. Default is (anyone can read, write, and execute).\
Hosts Allow & string & $\checkmark$ & Enter a list of allowed hostnames
or IP addresses. Separate entries with a comma, space, or tab. Please
see the () for more information.\
Hosts Deny & string & $\checkmark$ & Enter a list of denied hostnames or
IP addresses. Separate entries with a comma, space, or tab. Please see
the () for more information.\
Auxiliary Parameters & string & $\checkmark$ & Enter any additional
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=afp.conf)
parameters not covered by other option fields.\

<span>note</span><span>Note:</span> If neither or contains an entry,
then AFP share access is allowed for any host.

If there is a list but no list, then only allow hosts on the list.

If there is a list but no list, then allow all hosts that are not on the
list.

If there is both a and list, then allow all hosts that are on the list.
If there is a host not on the and not on the list, then allow it.

### Creating AFP Guest Shares {#\detokenize{sharing:creating-afp-guest-shares}}

\[\] AFP supports guest logins, meaning that macOS users can access the
AFP share without requiring their user accounts to first be created on
or imported into the FreeNAS$^{\text{®}}$ system.

<span>note</span><span>Note:</span> When a guest share is created along
with a share that requires authentication, AFP only maps users who log
in as to the guest share. If a user logs in to the share that requires
authentication, permissions on the guest share can prevent that user
from writing to the guest share. The only way to allow both guest and
authenticated users to write to a guest share is to set the permissions
on the guest share to or to add the authenticated users to a guest group
and set the permissions to .

Before creating a guest share, go to and click the sliding button to
turn on the service. Click (Configure) to open the screen shown in . For
, use the dropdown to select , set , and click .

\[\]

Next, create a dataset for the guest share. Refer to () for more
information about dataset creation.

After creating the dataset for the guest share, go to , click the
(Options) button for the dataset, then click . Complete the fields shown
in .

1.  Use the dropdown to select .

2.  Click .

\[\]

To create a guest AFP share:

1.  Go to and click .

2.  to the dataset created for the guest share.

3.  Fill out the other required fields, then press .

macOS users can use Finder to connect to the guest AFP share by clicking
. In the example shown in , the user entered followed by the IP address
of the FreeNAS$^{\text{®}}$ system.

Click the button. Once connected, Finder opens automatically. The name
of the AFP share is displayed in the SHARED section in the left frame
and the contents of any data saved in the share is displayed in the
right frame.

\[\]

To disconnect from the pool, click the button in the sidebar.

Block (iSCSI) {#\detokenize{sharing:block-iscsi}}
-------------

\[\]\[\] iSCSI is a protocol standard for the consolidation of storage
data. iSCSI allows FreeNAS$^{\text{®}}$ to act like a storage area
network (SAN) over an existing Ethernet network. Specifically, it
exports disk devices over an Ethernet network that iSCSI clients (called
initiators) can attach to and mount. Traditional SANs operate over fibre
channel networks which require a fibre channel infrastructure such as
fibre channel HBAs, fibre channel switches, and discrete cabling. iSCSI
can be used over an existing Ethernet network, although dedicated
networks can be built for iSCSI traffic in an effort to boost
performance. iSCSI also provides an advantage in an environment that
uses Windows shell programs; these programs tend to filter “Network
Location” but iSCSI mounts are not filtered.

Before configuring the iSCSI service, be familiar with this iSCSI
terminology:

an authentication method which uses a shared secret and threeway
authentication to determine if a system is authorized to access the
storage device and to periodically confirm that the session has not been
hijacked by another system. In iSCSI, the initiator (client) performs
the CHAP authentication.

a superset of CHAP in that both ends of the communication authenticate
to each other.

a client which has authorized access to the storage data on the
FreeNAS$^{\text{®}}$ system. The client requires initiator software to
initiate the connection to the iSCSI share.

a storage resource on the FreeNAS$^{\text{®}}$ system. Every target has
a unique name known as an iSCSI Qualified Name (IQN).

protocol for the automated discovery of iSCSI devices on a TCP/IP
network.

the storage unit to be shared. It can either be a file or a device.

indicates which IP addresses and ports to listen on for connection
requests.

representing a logical SCSI device. An initiator negotiates with a
target to establish connectivity to a LUN. The result is an iSCSI
connection that emulates a connection to a SCSI hard disk. Initiators
treat iSCSI LUNs as if they were a raw SCSI or SATA hard drive. Rather
than mounting remote directories, initiators format and directly manage
filesystems on iSCSI LUNs. When configuring multiple iSCSI LUNs, create
a new target for each LUN. Since iSCSI multiplexes a target with
multiple LUNs over the same TCP connection, there can be TCP contention
when more than one target accesses the same LUN. FreeNAS$^{\text{®}}$
supports up to 1024 LUNs.

In FreeNAS$^{\text{®}}$, iSCSI is built into the kernel. This version of
iSCSI supports <span> </span>
(https://docs.microsoft.com/enus/previousversions/windows/itpro/windowsserver2012R2and2012/hh831628(v=ws.11)),
meaning that file copies happen locally, rather than over the network.
It also supports the () (vStorage APIs for Array Integration) primitives
for efficient operation of storage tasks directly on the NAS. To take
advantage of the VAAI primitives, () and use it to ().

### iSCSI Wizard {#\detokenize{sharing:iscsi-wizard}}

\[\] To configure iSCSI, click and follow each step:

1.  :

    -   : Enter a name for the block device. Keeping the name short
        is recommended. Using a name longer than 63 characters can
        prevent access to the block device.

    -   : Select or as the type of block device. provides virtual
        storage access to zvols, zvol snapshots, or physical devices.
        provides virtual storage access to an individual file.

    -   : Select the unformatted disk, controller, zvol, or
        zvol snapshot. Select for options to create a new zvol. If is
        selected, use the browser to select an existing pool or dataset
        to store the new zvol. Enter the desired size of the zvol in .
        Only displayed when is set to .

    -   : Browse to an existing file. Create a new file by browsing to a
        dataset and appending the file name to the path. When the file
        already exists, enter a size of to use the actual file size. For
        new files, enter the size of the file to create. Only displayed
        when is set to .

    -   : Choose the platform that will use this share. The associated
        options are applied to this share.

2.  -   : Select an existing portal or choose to configure a new portal.

    -   : allows anonymous discovery while and require authentication.

    -   : Choose an existing () group ID or create a new authorized
        access. This is required when the is set to or .

    -   : Select IP addresses to be listened on by the portal. Click to
        add IP addresses with a different network port. The address can
        be selected to listen on all IPv4 addresses, or to listen on all
        IPv6 addresses.

    -   : TCP port used to access the iSCSI target. Default is .

3.  -   : Leave blank to allow all or enter a list of initiator
        hostnames separated by spaces.

    -   : Network addresses allowed to use this initiator. Leave blank
        to allow all networks or list network addresses with a
        CIDR mask. Separate multiple addresses with a space: .

4.  -   Review the configuration and click to set up the iSCSI share.

The rest of this section describes iSCSI configuration in more detail.

### Target Global Configuration {#\detokenize{sharing:target-global-configuration}}

\[\] contains settings that apply to all iSCSI shares. describes each
option.

Some builtin values affect iSNS usage. Fetching of allowed initiators
from iSNS is not implemented, so target ACLs must be configured
manually. To make iSNS registration useful, iSCSI targets should have
explicitly configured port IP addresses. This avoids initiators
attempting to discover unconfigured target portal addresses like .

The iSNS registration period is seconds. Registered Network Entities not
updated during this period are unregistered. The timeout for iSNS
requests is seconds.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Base Name & string & Lowercase alphanumeric characters plus dot (.),
dash (), and colon (:) are allowed. See the “Constructing iSCSI names
using the iqn. format” section of <span> </span>
(https://tools.ietf.org/html/rfc3721.html).\
ISNS Servers & string & Enter the hostnames or IP addresses of ISNS
servers to be registered with iSCSI targets and portals of the system.
Separate each entry with a space.\
Pool Available Space Threshold & integer & Enter the percentage of free
space to remain in the pool. When this percentage is reached, the system
issues an alert, but only if zvols are used. See () Threshold Warning
for more information.\

### Portals {#\detokenize{sharing:portals}}

\[\] A portal specifies the IP address and port number to be used for
iSCSI connections. Go to and click to display the screen shown in .

summarizes the settings that can be configured when adding a portal.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Description & string & Optional description. Portals are automatically
assigned a numeric group.\
Discovery Auth Method & dropdown menu & () supports multiple
authentication methods that are used by the target to discover valid
devices. allows anonymous discovery while and both require
authentication.\
Discovery Auth Group & dropdown menu & Select a Group ID created in if
the is set to or .\
IP address & dropdown menu & Select IP addresses to be listened on by
the portal. Click to add IP addresses with a different network port. The
address can be selected to listen on all IPv4 addresses, or to listen on
all IPv6 addresses.\
Port & integer & TCP port used to access the iSCSI target. Default is .\

FreeNAS$^{\text{®}}$ systems with multiple IP addresses or interfaces
can use a portal to provide services on different interfaces or subnets.
This can be used to configure multipath I/O (MPIO). MPIO is more
efficient than a link aggregation.

If the FreeNAS$^{\text{®}}$ system has multiple configured interfaces,
portals can also be used to provide network access control. For example,
consider a system with four interfaces configured with these addresses:

192.168.1.1/24

192.168.2.1/24

192.168.3.1/24

192.168.4.1/24

A portal containing the first two IP addresses (group ID 1) and a portal
containing the remaining two IP addresses (group ID 2) could be created.
Then, a target named A with a Portal Group ID of 1 and a second target
named B with a Portal Group ID of 2 could be created. In this scenario,
the iSCSI service would listen on all four interfaces, but connections
to target A would be limited to the first two networks and connections
to target B would be limited to the last two networks.

Another scenario would be to create a portal which includes every IP
address for the one used by a management interface. This would prevent
iSCSI connections to the management interface.

### Initiators {#\detokenize{sharing:initiators}}

\[\] The next step is to configure authorized initiators, or the systems
which are allowed to connect to the iSCSI targets on the
FreeNAS$^{\text{®}}$ system. To configure which systems can connect, go
to and click as shown in .

\[\]

summarizes the settings that can be configured when adding an initiator.

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Allow All Initiators & checkbox & Accept all detected initiators. When
set, all other initiator fields are disabled.\
Connected Initiators & string & Initiators currently connected to the
system. Shown in IQN format with an IP address. Set initiators and click
an  to add the initiators to either the or lists. Clicking updates the
list.\
Allowed Initiators (IQN) & string & Initiators allowed access to this
system. Enter an <span> </span>
(https://tools.ietf.org/html/rfc3720\#section3.2.6) and click to add it
to the list. Example:\
Authorized Networks & string & Network addresses allowed to use this
initiator. Each address can include an optional <span> </span>
(https://en.wikipedia.org/wiki/Classless\_InterDomain\_Routing) netmask.
Click to add the network address to the list. Example:\
Description & string & Any notes about initiators.\

Click (Options) on an initiator entry for options to or it.

### Authorized Access {#\detokenize{sharing:authorized-access}}

\[\] When using CHAP or mutual CHAP to provide authentication, creating
authorized access is recommended. Do this by going to and clicking . The
screen is shown in .

<span>note</span><span>Note:</span> This screen sets login
authentication. This is different from discovery authentication which is
set in ().

\[\]

summarizes the settings that can be configured when adding an authorized
access:

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.16-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Group ID & integer & Allow different groups to be configured with
different authentication profiles. Example: enter for all users in Group
to inherit the Group authentication profile. Group IDs that are already
configured with authorized access cannot be reused.\
User & string & User account to create for CHAP authentication with the
user on the remote system. Many initiators use the initiator name as the
user name.\
Secret & string & password. Must be at least and no more than characters
long.\
Peer User & string & Only entered when configuring mutual CHAP. Usually
the same value as .\
Peer Secret & string & Mutual secret password. Required when is set.
Must be different than the . Must be at least and no more than
characters long.\

<span>note</span><span>Note:</span> CHAP does not work with GlobalSAN
initiators on macOS.

New authorized accesses are visible from the menu. In the example shown
in , three users (, , and ) and two groups ( and ) have been created,
with group 1 consisting of one CHAP user and group 2 consisting of one
mutual CHAP user and one CHAP user. Click an authorized access entry to
display its and buttons.

\[\]

### Targets {#\detokenize{sharing:targets}}

\[\] Next, create a Target by going to and clicking as shown in . A
target combines a portal ID, allowed initiator ID, and an authentication
method. summarizes the settings that can be configured when creating a
Target.

<span>note</span><span>Note:</span> An iSCSI target creates a block
device that may be accessible to multiple initiators. A clustered
filesystem is required on the block device, such as VMFS used by VMware
ESX/ESXi, in order for multiple initiators to mount the block device
read/write. If a traditional filesystem such as EXT, XFS, FAT, NTFS,
UFS, or ZFS is placed on the block device, care must be taken that only
one initiator at a time has read/write access or the result will be
filesystem corruption. If multiple clients need access to the same data
on a nonclustered filesystem, use SMB or NFS instead of iSCSI, or create
multiple iSCSI targets (one per client).

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Target Name & string & Required. The base name is automatically
prepended if the target name does not start with . Lowercase
alphanumeric characters plus dot (.), dash (), and colon (:) are
allowed. See the “Constructing iSCSI names using the iqn. format”
section of <span> </span> (https://tools.ietf.org/html/rfc3721.html).\
Target Alias & string & Enter an optional userfriendly name.\
Portal Group ID & dropdown menu & Leave empty or select number of
existing portal to use.\
Initiator Group ID & dropdown menu & Select which existing initiator
group has access to the target.\
Auth Method & dropdown menu & , , , or .\
Authentication Group number & dropdown menu & Select or an integer. This
number represents the number of existing authorized accesses.\

### Extents {#\detokenize{sharing:extents}}

\[\] iSCSI targets provide virtual access to resources on the
FreeNAS$^{\text{®}}$ system. are used to define resources to share with
clients. There are two types of extents: and .

provide virtual storage access to zvols, zvol snapshots, or physical
devices like a disk, an SSD, or a hardware RAID volume.

provide virtual storage access to an individual file.

<span>tip</span><span>Tip:</span> For other applications, device extents
sharing a raw device can be appropriate. File extents do not have the
performance or features of device extents, but do allow creating
multiple extents on a single filesystem.

Virtualized zvols support all the FreeNAS$^{\text{®}}$ () primitives and
are recommended for use with virtualization software as the iSCSI
initiator.

The ATS, WRITE SAME, XCOPY and STUN, primitives are supported by both
file and device extents. The UNMAP primitive is supported by zvols and
raw SSDs. The threshold warnings primitive is fully supported by zvols
and partially supported by file extents.

Virtualizing a raw device like a single disk or hardware RAID volume
limits performance to the abilities of the device. Because this bypasses
ZFS, such devices do not benefit from ZFS caching or provide features
like block checksums or snapshots.

Virtualizing a zvol adds the benefits of ZFS, such as read and write
cache. Even if the client formats a device extent with a different
filesystem, the data still resides on a ZFS pool and benefits from ZFS
features like block checksums and snapshots.

<span>warning</span><span>Warning:</span> For performance reasons and to
avoid excessive fragmentation, keep the used space of the pool below 80%
when using iSCSI. The capacity of an existing extent can be increased as
shown in ().

To add an extent, go to and click . In the example shown in , the device
extent is using the zvol that was previously created from the pool.

summarizes the settings that can be configured when creating an extent.
Note that

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Extent name & string & Enter the extent name. If the is not , it cannot
be an existing file within the pool or dataset.\
Extent type & dropdown menu & shares the contents of an individual file.
shares an entire device.\
Path to the extent & browse button & Only appears when is selected.
Browse to an existing file. Create a new file by browsing to a dataset
and appending the file name to the path. Extents cannot be created
inside a jail root directory.\
Extent size & integer & Only appears when is selected. Entering uses the
actual file size and requires that the file already exists. Otherwise,
specify the file size for the new file.\
Device & dropdown menu & Only appears when is selected. Select the
unformatted disk, controller, zvol, or zvol snapshot.\
Logical block size & dropdown menu & Leave at the default of 512 unless
the initiator requires a different block size.\
Disable physical block size reporting & checkbox & Set if the initiator
does not support physical block size values over 4K (MS SQL). Setting
can also prevent <span> </span>
(https://www.virten.net/2016/12/thephysicalblocksizereportedbythedeviceisnotsupported/)
when using this share with ESXi.\
Available space threshold & string & Only appears if or a zvol is
selected. When the specified percentage of free space is reached, the
system issues an alert. See () Threshold Warning.\
Description & string & Notes about this extent.\
Enable TPC & checkbox & Set to allow an initiator to bypass normal
access control and access any scannable target. This allows <span>
</span>
(https://docs.microsoft.com/enus/previousversions/windows/itpro/windowsserver2012R2and2012/cc771254(v=ws.11))
operations which are otherwise blocked by access control.\
Xen initiator compat mode & checkbox & Set when using Xen as the iSCSI
initiator.\
LUN RPM & dropdown menu & Do change this setting when using Windows as
the initiator. Only needs to be changed in large environments where the
number of systems using a specific RPM is needed for accurate reporting
statistics.\
Readonly & checkbox & Set to prevent the initiator from initializing
this LUN.\
Enable & checkbox & Set to enable the iSCSI extent.\

New extents have been added to . The associated and Network Address
Authority () are shown along with the extent name.

### Associated Targets {#\detokenize{sharing:associated-targets}}

\[\] The last step is associating an extent to a target by going to and
clicking . The screen is shown in . Use the dropdown menus to select the
existing target and extent. Click to add an entry for the LUN.

\[\]

summarizes the settings that can be configured when associating targets
and extents.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Target & dropdown menu & Select an existing target.\
LUN ID & integer & Select or enter a value between and . Some initiators
expect a value less than . Leave this field blank to automatically
assign the next available ID.\
Extent & dropdown menu & Select an existing extent.\

Always associating extents to targets in a onetoone manner is
recommended, even though the web interface will allow multiple extents
to be associated with the same target.

<span>note</span><span>Note:</span> Each LUN entry has and buttons for
modifying the settings or deleting the LUN entirely. A verification
popup appears when the button is clicked. If an initiator has an active
connection to the LUN, it is indicated in red text. Clearing the
initiator connections to a LUN before deleting it is recommended.

After iSCSI has been configured, remember to start the service in by
clicking the (Power) button.

### Connecting to iSCSI {#\detokenize{sharing:connecting-to-iscsi}}

\[\] To access the iSCSI target, clients must use iSCSI initiator
software.

An iSCSI Initiator client is preinstalled with Windows 7. A detailed
howto for this client can be found <span> </span>
(http://techgenix.com/ConnectingWindows7iSCSISAN/). A client for Windows
2000, XP, and 2003 can be found <span> </span>
(http://www.microsoft.com/enus/download/details.aspx?id=18986). This
<span> </span>
(https://www.pluralsight.com/blog/softwaredevelopment/freenas8iscsitargetwindows7)
shows how to create an iSCSI target for a Windows 7 system.

macOS does not include an initiator. <span> </span>
(http://www.studionetworksolutions.com/globalsaniscsiinitiator/) is a
commercial, easytouse Mac initiator.

BSD systems provide command line initiators: <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=iscontrol) comes with FreeBSD
versions 9.x and lower, <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=iscsictl) comes with FreeBSD
versions 10.0 and higher, <span> </span>
(http://netbsd.gw.com/cgibin/mancgi?iscsiinitiator++NetBSDcurrent) comes
with NetBSD, and <span> </span>
(http://man.openbsd.org/cgibin/man.cgi/OpenBSDcurrent/man8/iscsid.8?query=iscsid)
comes with OpenBSD.

Some Linux distros provide the command line utility from <span> </span>
(http://www.openiscsi.com/). Use a web search to see if a package exists
for the distribution should the command not exist on the Linux system.

If a LUN is added while is already connected, it will not see the new
LUN until rescanned with . Alternately, use to find the new LUN and to
log into the LUN.

Instructions for connecting from a VMware ESXi Server can be found at
<span> </span>
(https://www.vladan.fr/howtoconfigurefreenas8foriscsiandconnecttoesxi/).
Note that the requirements for booting vSphere 4.x off iSCSI differ
between ESX and ESXi. ESX requires a hardware iSCSI adapter while ESXi
requires specific iSCSI boot firmware support. The magic is on the
booting host side, meaning that there is no difference to the
FreeNAS$^{\text{®}}$ configuration. See the <span> </span>
(https://www.vmware.com/pdf/vsphere4/r41/vsp\_41\_iscsi\_san\_cfg.pdf)
for details.

The VMware firewall only allows iSCSI connections on port by default. If
a different port has been selected, outgoing connections to that port
must be manually added to the firewall before those connections will
work.

If the target can be seen but does not connect, check the settings in .

If the LUN is not discovered by ESXi, make sure that promiscuous mode is
set to in the vSwitch.

### Growing LUNs {#\detokenize{sharing:growing-luns}}

\[\] The method used to grow the size of an existing iSCSI LUN depends
on whether the LUN is backed by a file extent or a zvol. Both methods
are described in this section.

Enlarging a LUN with one of the methods below gives it more unallocated
space, but does not automatically resize filesystems or other data on
the LUN. This is the same as binarycopying a smaller disk onto a larger
one. More space is available on the new disk, but the partitions and
filesystems on it must be expanded to use this new space. Resizing
virtual disk images is usually done from virtual machine management
software. Application software to resize filesystems is dependent on the
type of filesystem and client, but is often run from within the virtual
machine. For instance, consider a Windows VM with the last partition on
the disk holding an NTFS filesystem. The LUN is expanded and the
partition table edited to add the new space to the last partition. The
Windows disk manager must still be used to resize the NTFS filesystem on
that last partition to use the new space.

#### Zvol Based LUN {#\detokenize{sharing:zvol-based-lun}}

\[\] To grow a zvolbased LUN, go to , click (Options) on the zvol to be
grown, then click . In the example shown in , the current size of the
zvol named is 4 GiB.

\[\]

Enter the new size for the zvol in the field and click . The new size
for the zvol is immediately shown in the column of the table.

<span>note</span><span>Note:</span> The web interface does not allow
reducing the size of the zvol, as doing so could result in loss of data.
It also does not allow increasing the size of the zvol past 80% of the
pool size.

#### File Extent Based LUN {#\detokenize{sharing:file-extent-based-lun}}

\[\] To grow a file extentbased LUN:

Go to . Click (Options), then . Ensure the is set to file and enter the
. Open the () to grow the file extent. This example grows by 2 GiB:

\[commandchars=\
{}\] truncate s +2g /mnt/pool1/data

Return to , click (Options) on the desired file extent, then click . Set
the size to as this causes the iSCSI target to use the new size of the
file.

Unix (NFS) Shares {#\detokenize{sharing:unix-nfs-shares}}
-----------------

\[\]\[\] FreeNAS$^{\text{®}}$ supports sharing pools, datasets, and
directories over the Network File System (NFS). Clients use the command
to mount the share. Mounted NFS shares appear as another directory on
the client system. Some Linux distros require the installation of
additional software to mount an NFS share. Windows systems must enable
Services for NFS in the Ultimate or Enterprise editions or install an
NFS client application.

<span>note</span><span>Note:</span> For performance reasons, iSCSI is
preferred to NFS shares when FreeNAS$^{\text{®}}$ is installed on ESXi.
When considering creating NFS shares on ESXi, read through the
performance analysis presented in <span> </span>
(https://tinyurl.com/archivezfsovernfsvmware).

Create an NFS share by going to and clicking . shows an example of
creating an NFS share.

\[\]

Remember these points when creating NFS shares:

1.  Clients specify the when mounting the share.

2.  The and options cannot both be enabled. The options supersede
    the options. To restrict only the user permissions, set the option.
    To restrict permissions of all users, set the options.

3.  Each pool or dataset is considered to be a unique filesystem.
    Individual NFS shares cannot cross filesystem boundaries. Adding
    paths to share more directories only works if those directories are
    within the same filesystem.

4.  The network and host must be unique to both each created share and
    the filesystem or directory included in that share. Because is not
    an access control list (ACL), the rules contained in become
    undefined with overlapping networks or when using the same share
    with multiple hosts.

5.  The option can only be used once per share per filesystem.

To better understand these restrictions, consider scenarios where there
are:

-   two networks, and

-   a ZFS pool named with a dataset named

-   contains directories named , , and

Because of restriction \#3, an error is shown when trying to create one
NFS share like this:

-   set to

-   set to the dataset . An additional path to directory is added.

The correct method to configure this share is to set the to and set the
box. This allows the client to also mount when is mounted.

Additional paths are used to define specific directories to be shared.
For example, has three directories. To share only and , create paths for
and within the share. This excludes from the share.

Restricting a specific directory to a single network is done by creating
a share for the volume or dataset and a share for the directory within
that volume or dataset. Define the authorized networks for both shares.

First NFS share:

-   set to

-   set to

Second NFS share:

-   set to

-   set to

This requires the creation of two shares. It cannot be done with only
one share.

summarizes the available configuration options in the screen. Click to
see all settings.

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.14-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.54-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Path & browse button && Browse to the dataset or directory to be shared.
Click to specify multiple paths.\
Comment & string && Text describing the share. Typically used to name
the share. If left empty, this shows the entries of the share.\
All dirs & checkbox && Allow the client to also mount any subdirectories
of the selected pool or dataset.\
Read only & checkbox && Prohibit writing to the share.\
Quiet & checkbox & $\checkmark$ & Restrict some syslog diagnostics to
avoid some error messages. See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=exports) for examples.\
Authorized networks & string & $\checkmark$ & Spacedelimited list of
allowed networks in network/mask CIDR notation. Example: . Leave empty
to allow all.\
Authorized Hosts and IP addresses & string & $\checkmark$ &
Spacedelimited list of allowed IP addresses or hostnames. Leave empty to
allow all.\
Maproot User & dropdown menu & $\checkmark$ & When a user is selected,
the user is limited to permissions of that user.\
Maproot Group & dropdown menu & $\checkmark$ & When a group is selected,
the user is also limited to permissions of that group.\
Mapall User & dropdown menu & $\checkmark$ & FreeNAS$^{\text{®}}$ user
or user imported with (). The specified permissions of that user are
used by all clients.\
Mapall Group & dropdown menu & $\checkmark$ & FreeNAS$^{\text{®}}$ group
or group imported with (). The specified permissions of that group are
used by all clients.\
Security & selection & $\checkmark$ & Only appears if is enabled in .
Choices are or these Kerberos options: (authentication only),
(authentication and integrity), or (authentication and privacy). If
multiple security mechanisms are added to the column using the arrows,
use the or buttons to list in order of preference.\

Go to and click (Options) and to edit an existing share. shows the
configuration screen for the existing share. Options are the same as
described in ().

\[\]

### Example Configuration {#\detokenize{sharing:example-configuration}}

\[\] By default, the fields are not set. This means that when a user
connects to the NFS share, the user has the permissions associated with
their user account. This is a security risk if a user is able to connect
as as they will have complete access to the share.

A better option is to do this:

1.  Specify the builtin account to be used for NFS access.

2.  In the screen of the pool or dataset that is being shared, change
    the owner and group to and set the permissions according to the
    desired requirements.

3.  Select in the and dropdown menus for the share in .

With this configuration, it does not matter which user account connects
to the NFS share, as it will be mapped to the user account and will only
have the permissions that were specified on the pool or dataset. For
example, even if the user is able to connect, it will not gain access to
the share.

### Connecting to the Share {#\detokenize{sharing:connecting-to-the-share}}

\[\] The following examples share this configuration:

1.  The FreeNAS$^{\text{®}}$ system is at IP address .

2.  A dataset named is created and the permissions set to the user
    account and the group.

3.  An NFS share is created with these attributes:

    -   :

    -   :

    -   option is enabled

    -   is set to

    -   is set to

#### From BSD or Linux {#\detokenize{sharing:from-bsd-or-linux}}

\[\] NFS shares are mounted on BSD or Linux clients with this command
executed as the superuser () or with :

\[commandchars=\
{}\] mount t nfs 192.168.2.2:/mnt/pool1/nfsshare1 /mnt

-   specifies the filesystem type of the share

-   is the IP address of the FreeNAS$^{\text{®}}$ system

-   is the name of the directory to be shared, a dataset in this case

-   is the mountpoint on the client system. This must be an
    existing, directory. The data in the NFS share appears in this
    directory on the client computer.

Successfully mounting the share returns to the command prompt without
any status or error messages.

<span>note</span><span>Note:</span> If this command fails on a Linux
system, make sure that the <span> </span>
(https://sourceforge.net/projects/nfs/files/nfsutils/) package is
installed.

This configuration allows users on the client system to copy files to
and from (the mount point). All files are owned by . Changes to any
files or directories in write to the FreeNAS$^{\text{®}}$ system
dataset.

NFS share settings cannot be changed when the share is mounted on a
client computer. The command is used to unmount the share on BSD and
Linux clients. Run it as the superuser or with on each client computer:

\[commandchars=\
{}\] umount /mnt

#### From Microsoft {#\detokenize{sharing:from-microsoft}}

\[\] Windows NFS client support varies with versions and releases. For
best results, use ().

#### From macOS {#\detokenize{sharing:from-macos}}

\[\] A macOS client uses Finder to mount the NFS volume. Go to . In the
field, enter followed by the IP address of the FreeNAS$^{\text{®}}$
system, and the name of the pool or dataset being shared by NFS. The
example shown in continues with the example of .

Finder opens automatically after connecting. The IP address of the
FreeNAS$^{\text{®}}$ system displays in the SHARED section of the left
frame and the contents of the share display in the right frame. shows an
example where has one folder named . The user can now copy files to and
from the share.

\[\]

\[\]

### Troubleshooting NFS {#\detokenize{sharing:troubleshooting-nfs}}

\[\] Some NFS clients do not support the NLM (Network Lock Manager)
protocol used by NFS. This is the case if the client receives an error
that all or part of the file may be locked when a file transfer is
attempted. To resolve this error, add the option when running the
command on the client to allow write access to the NFS share.

If a “time out giving up” error is shown when trying to mount the share
from a Linux system, make sure that the portmapper service is running on
the Linux client. If portmapper is running and timeouts are still shown,
force the use of TCP by including in the command.

If a error is shown, upgrade to the latest version of
FreeNAS$^{\text{®}}$ and restart the NFS service after the upgrade to
clear the NFS cache.

If clients see “reverse DNS” errors, add the FreeNAS$^{\text{®}}$ IP
address in the field of .

If clients receive timeout errors when trying to mount the share, add
the client IP address and hostname to the field in .

Some older versions of NFS clients default to UDP instead of TCP and do
not autonegotiate for TCP. By default, FreeNAS$^{\text{®}}$ uses TCP. To
support UDP connections, go to and enable the option.

The or commands can be helpful to detect problems from the (). A high
proportion of retries and timeouts compared to reads usually indicates
network problems.

WebDAV Shares {#\detokenize{sharing:webdav-shares}}
-------------

\[\]\[\] In FreeNAS$^{\text{®}}$, WebDAV shares can be created so that
authenticated users can browse the contents of the specified pool,
dataset, or directory from a web browser.

Configuring WebDAV shares is a two step process. First, create the
WebDAV shares to specify which data can be accessed. Then, configure the
WebDAV service by specifying the port, authentication type, and
authentication password. Once the configuration is complete, the share
can be accessed using a URL in the format:

\[commandchars=\
{}\] protocol://IPaddress:portnumber/sharename

where:

-   is either or , depending upon the configured in .

-   is the IP address or hostname of the FreeNAS$^{\text{®}}$ system.
    Take care when configuring a public IP address to ensure that the
    network firewall only allows access to authorized systems.

-   is configured in . If the FreeNAS$^{\text{®}}$ system is to be
    accessed using a public IP address, consider changing the default
    port number and ensure that the network firewall only allows access
    to authorized systems.

-   is configured by clicking , then .

Entering the URL in a web browser brings up an authentication popup
message. Enter a username of and the password configured in .

<span>warning</span><span>Warning:</span> At this time, only the user is
supported. For this reason, it is important to set a good password for
this account and to only give the password to users which should have
access to the WebDAV share.

To create a WebDAV share, go to and click , which will open the screen
shown in .

\[\]

summarizes the available options.

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.16-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Share Path Name & string & Enter a name for the share.\
Comment & string & Optional.\
Path & browse button & Enter the path or to the pool or dataset to
share. Appending a new name to the path creates a new dataset. Example:
.\
Read Only & checkbox & Set to prohibit users from writing to the share.\
Change User & Group & checkbox & Ownership of all files in the share
will be changed to user and group . Existing permissions will not be
changed, but the ownership change might make files inaccesible to their
original owners. This operation cannot be undone! If unset, ownership of
files to be accessed through WebDAV must be manually set to the or
user/group.\

Click to create the share. Then, go to and click the (Power) button to
turn on the service.

After the service starts, review the settings in as they are used to
determine which URL is used to access the WebDAV share and whether or
not authentication is required to access the share. These settings are
described in ().

Windows (SMB) Shares {#\detokenize{sharing:windows-smb-shares}}
--------------------

\[\]\[\] FreeNAS$^{\text{®}}$ uses <span> </span>
(https://www.samba.org/) to share pools using Microsoft’s SMB protocol.
SMB is built into the Windows and macOS operating systems and most Linux
and BSD systems preinstall the Samba client in order to provide support
for SMB. If the distro did not, install the Samba client using the
distro software repository.

The SMB protocol supports many different types of configuration
scenarios, ranging from the simple to complex. The complexity of the
scenario depends upon the types and versions of the client operating
systems that will connect to the share, whether the network has a
Windows server, and whether Active Directory is being used. Depending on
the authentication requirements, it might be necessary to create or
import users and groups.

Samba supports serverside copy of files on the same share with clients
from Windows 8 and higher. Copying between two different shares is not
serverside. Windows 7 clients support serverside copying with <span>
</span>
(https://docs.microsoft.com/enus/previousversions/windows/itpro/windowsserver2012R2and2012/cc733145(v=ws.11)).

This chapter starts by summarizing the available configuration options.
It demonstrates some common configuration scenarios as well as offering
some troubleshooting tips. Reading through this entire chapter before
creating any SMB shares is recommended to gain a better understanding of
the configuration scenario that meets the specific network requirements.

<span> </span>
(https://forums.freenas.org/index.php?resources/smbtipsandtricks.15/)
shows helpful hints for configuring and managing SMB networking. The
<span> </span> (https://www.youtube.com/watch?v=RxggaE935PM) and <span>
</span> (https://www.youtube.com/watch?v=QhwOyLtArw0) videos clarify
setting up permissions on SMB shares. Another helpful reference is
<span> </span>
(https://forums.freenas.org/index.php?threads/methodsforfinetuningsambapermissions.50739/).

<span>warning</span><span>Warning:</span> <span> </span>
(https://www.ixsystems.com/blog/library/donotusesmb1/). If necessary,
SMB1 can be enabled in .

shows the configuration screen that appears after clicking , then .

\[\]

summarizes the options available when creating a SMB share. Some
settings are only configurable after clicking the button. For simple
sharing scenarios, options are not needed. For more complex sharing
scenarios, only change an option after fully understanding the function
of that option. <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=smb.conf) provides more
details for each configurable option.

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.14-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.54-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Path & browse button && Select the pool, dataset, or directory to share.
The same path can be used by more than one share.\
Name & string && Name the new share. Each share name must be unique. The
names , , and are reserved and cannot be used.\
Use as home share & checkbox && Set to allow this share to hold user
home directories. Only one share can be the home share. Note that lower
case names for user home directories are strongly recommended, as Samba
maps usernames to all lower case. For example, the username John will be
mapped to a home directory named . If the to the home share includes an
upper case username, delete the existing user and recreate it in with an
all lower case . Return to to create the home share, and select the that
contains the new lower case username.\
Description & string && Description of the share or notes on how it is
used.\
Time Machine & checkbox && Enable <span> </span>
(https://developer.apple.com/library/archive/releasenotes/NetworkingInternetWeb/Time\_Machine\_SMB\_Spec/\#//apple\_ref/doc/uid/TP40017496CH1SW1)
backups for this share. The process to configure a Time Machine backup
is shown in (). Changing this setting on an existing share requres an ()
service restart.\
Export Read Only & checkbox & $\checkmark$ & Prohibit write access to
this share.\
Browsable to Network Clients & checkbox & $\checkmark$ & Determine
whether this share name is included when browsing shares. Home shares
are only visible to the owner regardless of this setting.\
Export Recycle Bin & checkbox & $\checkmark$ & Files that are deleted
from the same dataset are moved to the Recycle Bin and do not take any
additional space. This is only applies over the SMB protocol. . When the
files are in a different dataset or a child dataset, they are copied to
the dataset where the Recycle Bin is located. To prevent excessive space
usage, files larger than 20 MiB are deleted rather than moved. Adjust
the setting to allow larger files. For example, allows moves of files up
to 50 MiB in size. The recylce bin has readwrite functionality. This
means files can be permanently deleted or moved from the recylce bin.
().\
Show Hidden Files & checkbox & $\checkmark$ & Disable the Windows
attribute on a new Unix hidden file. Unix hidden filenames start with a
dot: . Existing files are not affected.\
Allow Guest Access & checkbox && Privileges are the same as the guest
account. Guest access is disabled by default in Windows 10 version 1709
and Windows Server version 1903. Additional clientside configuration is
required to provide guest access to these clients.

MacOS clients: Attempting to connect as a user that does not exist in
FreeNAS$^{\text{®}}$ automatically connect as the guest account. The
option must be specifically chosen in MacOS to log in as the guest
account. See the <span> </span>
(https://support.apple.com/guide/machelp/connectmacsharedcomputersserversmchlp1140/)
for more details.\
Only Allow Guest Access & checkbox & $\checkmark$ & Requires to also be
enabled. Forces guest access for all connections.\
Access Based Share Enumeration & checkbox & $\checkmark$ & Restrict
share visibility to users with a current Windows Share ACL access of
read or write. Use Windows administration tools to adjust the share
permissions. See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=smb.conf).\
Hosts Allow & string & $\checkmark$ & Enter a list of allowed hostnames
or IP addresses. Separate entries with a comma (), space, or tab. Please
see the () for more information.\
Hosts Deny & string & $\checkmark$ & Enter a list of denied hostnames or
IP addresses. Specify and list any hosts from to have those hosts take
precedence. Separate entries with a comma (), space, or tab. Please see
the () for more information.\
VFS Objects & selection & $\checkmark$ & Add virtual file system objects
to enhance functionality. summarizes the available objects.\
Enable Shadow Copies & checkbox && Expose ZFS snapshots as <span>
</span>
(https://docs.microsoft.com/enus/windows/desktop/vss/shadowcopiesandshadowcopysets).\
Auxiliary Parameters & string & $\checkmark$ & Additional <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=smb.conf) parameters not
covered by other option fields.\

<span>note</span><span>Note:</span> If neither or contains an entry,
then SMB share access is allowed for any host.

If there is a list but no list, then only allow hosts on the list.

If there is a list but no list, then allow all hosts that are not on the
list.

If there is both a and list, then allow all hosts that are on the list.
If there is a host not on the and not on the list, then allow it.

Here are some notes about settings:

-   Hostname lookups add some time to accessing the SMB share. If only
    using IP addresses, unset the setting in (Configure).

-   When the option is selected, the share is visible through Windows
    File Explorer or through . When the option is selected, deselecting
    the option hides the share named so that only the dynamically
    generated share containing the authenticated user home directory
    will be visible. By default, the share and the user home directory
    are both visible. Users are not automatically granted read or write
    permissions on browsable shares. This option provides no real
    security because shares that are not visible in Windows File
    Explorer can still be accessed with a path.

-   If some files on a shared pool should be hidden and inaccessible to
    users, put a line in the field. The syntax for the option and some
    examples can be found in the <span>
    </span> (https://www.freebsd.org/cgi/man.cgi?query=smb.conf).

Samba disables NTLMv1 authentication by default for security. Standard
configurations of Windows XP and some configurations of later clients
like Windows 7 will not be able to connect with NTLMv1 disabled. <span>
</span>
(https://support.microsoft.com/enus/help/2793313/securityguidanceforntlmv1andlmnetworkauthentication)
has information about the security implications and ways to enable
NTLMv2 on those clients. If changing the client configuration is not
possible, NTLMv1 authentication can be enabled by selecting the option
in (Configure).

provides an overview of the available VFS objects. Be sure to research
each object adding or deleting it from the column of the field of the
share. Some objects need additional configuration after they are added.
Refer to <span> </span>
(https://www.samba.org/samba/docs/old/Samba3HOWTO/VFS.html) and the
<span> </span> (https://www.samba.org/samba/docs/current/manhtml/) for
more details.

\[\]\[\]

\[t\]<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.47-2</span>|</span>
Value & Description\
audit & Log share access, connects/disconnects, directory
opens/creates/removes, and file opens/closes/renames/unlinks/chmods to
syslog.\
catia & Improve Mac interoperability by translating characters that are
unsupported by Windows.\
crossrename & Allow server side rename operations even if source and
target are on different physical devices. Required for the recycle bin
to work across dataset boundaries. Automatically added when is enabled.\
dirsort & Sort directory entries alphabetically before sending them to
the client.\
fruit & Enhance macOS support by providing the SMB2 AAPL extension and
Netatalk interoperability. Automatically loads and , but see the ()
below.\
full\_audit & Record selected client operations to the system log.\
ixnas & Improves ACL compatibility with Windows, stores DOS attributes
as file flags, optimizes share case sensitivity to improve performance,
and enables () from Windows. Enabled by default. Several are available
with .

Userspace Quota Settings:

-   sets a ZFS user quota on every user that connects to the share.
    Example: sets the quota to .

-   enables support for userspace quotas. Choices are or . Default is .
    Example: .

Home Dataset Settings:

-   changes the owner of a created home dataset to the currently
    authenticated user. must be set to . Choices are or . Example: .

-   sets a quota on new ZFS datasets. must be set to . Example: sets the
    quota to .

-   creates new ZFS datasets for users connecting to home shares instead
    of folders. Choices are or . Default is . Example: .

\
media\_harmony & Allow Avid editing workstations to share a network
drive.\
noacl & Disable NT ACL support. If an extended ACL is present in the
share connection path, all access to this share will be denied. When the
<span> </span>
(https://www.oreilly.com/openbook/samba/book/ch05\_03.html) is set, all
write bits are removed. Disabling the attribute adds the write bits back
to the share, up to (). Adding requires adding the object. is
incompatible with the VFS object.\
offline & Mark all files in the share with the DOS attribute. This can
prevent Windows Explorer from reading files just to make thumbnail
images.\
preopen & Useful for video streaming applications that want to read one
file per frame.\
shell\_snap & Provide shellscript callouts for snapshot creation and
deletion operations issued by remote clients using the File Server
Remote VSS Protocol (FSRVP).\
streams\_xattr & Enable storing NTFS alternate data streams in the file
system. Enabled by default.\
winmsa & Emulate the Microsoft registry option. Moving files or
directories sets the ACL for file and directory hierarchies to inherit
from the destination directory.\
zfs\_space & Correctly calculate ZFS space used by the share, including
space used by ZFS snapshots, quotas, and resevations.\
zfsacl & Provide ACL extensions for proper integration with ZFS.\

\[\]

<span>warning</span><span>Warning:</span> Be careful when using multiple
SMB shares, some with and some without . macOS clients negotiate SMB2
AAPL protocol extensions on the first connection to the server, so
mixing shares with and without fruit will globally disable AAPL if the
first connection occurs without fruit. To resolve this, all macOS
clients need to disconnect from all SMB shares and the first
reconnection to the server has to be to a fruitenabled share.

These VFS objects do not appear in the dropdown menu:

-   moves deleted files to the recycle directory instead of
    deleting them. Controlled by in the ().

Creating or editing an SMB share on a dataset with a <span> </span>
(https://www.ixsystems.com/community/threads/methodsforfinetuningsambapermissions.50739/)
prompts to () for the dataset.

To view all active SMB connections and users, enter in the (). To log
more details for clients that are attempting to authenticate to an SMB
share, open the options and add to the .

### Configuring Unauthenticated Access {#\detokenize{sharing:configuring-unauthenticated-access}}

\[\] SMB supports guest logins, meaning that users can access the SMB
share without needing to provide a username or password. This type of
share is convenient as it is easy to configure, easy to access, and does
not require any users to be configured on the FreeNAS$^{\text{®}}$
system. This type of configuration is also the least secure as anyone on
the network can access the contents of the share. Additionally, since
all access is as the guest user, even if the user inputs a username or
password, there is no way to differentiate which users accessed or
modified the data on the share. This type of configuration is best
suited for small networks where quick and easy access to the share is
more important than the security of the data on the share.

<span>note</span><span>Note:</span> Windows 10, Windows Server 2016
version 1709, and Windows Server 2019 disable SMB2 guest access. Read
the <span> </span>
(https://support.microsoft.com/enhk/help/4046019/guestaccessinsmb2disabledbydefaultinwindows10andwindowsser)
for details about security vulnerabilities with SMB2 guest access and
instructions to reenable guest logins on these Microsoft systems.

To configure an unauthenticated SMB share:

1.  Go to and click .

2.  Fill out the the fields as shown in .

3.  Enable .

4.  Press .

<span>note</span><span>Note:</span> If a dataset for the share has not
been created, refer to () to find out more about dataset creation.

\[\]

The new share appears in .

By default, users that access the share from an SMB client will not be
prompted for a username or password. For example, to access the share
from a Windows system, open Explorer and click on . In this example, a
system named appears with a share named . The user can copy data to and
from this share.

The guest account can be changed by opening the options and selecting a
different account from the dropdown.

The guest account can also have an () that governs the permissions of
the guest account to access the different pools and datasets on the
system. To change the guest account permissions, edit the dataset Access
Control List (ACL) and add a new item with the set to and set to the
account used for guest access ( by default). The ACE can then be
adjusted to define the access level required for guest sessions. See ()
for more details about each available setting.

Changing the Guest Account permissions will not grant access for
anonymous sessions. This is best accomplished by creating or editing the
ACE in the dataset ACL. Note that anonymous sessions also do not have
the guest SID in the security token.

### Configuring Authenticated Access With Local Users {#\detokenize{sharing:configuring-authenticated-access-with-local-users}}

\[\] Most configuration scenarios require each user to have their own
user account and to authenticate before accessing the share. This allows
the administrator to control access to data, provide appropriate
permissions to that data, and to determine who accesses and modifies
stored data. A Windows domain controller is not needed for authenticated
SMB shares, which means that additional licensing costs are not
required. However, because there is no domain controller to provide
authentication for the network, each user account must be created on the
FreeNAS$^{\text{®}}$ system. This type of configuration scenario is
often used in home and small networks as it does not scale well if many
user accounts are needed.

To configure authenticated access for an SMB share, first create a ()
for all the SMB user accounts in FreeNAS$^{\text{®}}$. Go to and click .
Use a descriptive name for the group like .

Configure the SMB share dataset with permissions for this new group.
When (), set the to . After the dataset is created, open the dataset ()
and add a new entry. Set to and select the SMB group for the . Finish ()
for the SMB group. Any () now have access to the dataset.

\[\]

Determine which users need authenticated access to the dataset and () in
FreeNAS$^{\text{®}}$. It is recommended to use the same username and
password from the client system for the associated FreeNAS$^{\text{®}}$
user account. Add the SMB group to the list during account creation.

Finally, (). Make sure the is pointed to the dataset that has defined
permissions for the SMB group and that the () service is active.

The authenticated share can be tested from any SMB client. For example,
to test an authenticated share from a Windows system with network
discovery enabled, open Explorer and click on . If network discovery is
disabled, open Explorer and enter in the address bar, where is the IP
address or hostname of the share system. This example shows a system
named with a share named .

After clicking , a Windows Security dialog prompts for the username and
password of the user associated with . After authenticating, the user
can copy data to and from the SMB share.

Map the share as a network drive to prevent Windows Explorer from
hanging when accessing the share. Rightclick the share and select .
Choose a drive letter from the dropdown menu and click .

Windows caches user account credentials with the authenticated share.
This sometimes prevents connection to a share, even when the correct
username and password are provided. Logging out of Windows clears the
cache. The authentication dialog reappears the next time the user
connects to an authenticated share.

### User Quota Administration {#\detokenize{sharing:user-quota-administration}}

\[\] File Explorer can manage quotas on SMB shares connected to an ()
server. Both the share and dataset being shared must be configured to
allow this feature:

-   Create an authenticated share with as both the user and group name
    in .

-   Edit the SMB share and add to the list of selected ().

-   In Windows Explorer, connect to and map the share with a user
    account which is a member of the group. The tab becomes active.

### Configuring Shadow Copies {#\detokenize{sharing:configuring-shadow-copies}}

\[\]\[\] <span> </span> (https://en.wikipedia.org/wiki/Shadow\_copy),
also known as the Volume Shadow Copy Service (VSS) or Previous Versions,
is a Microsoft service for creating volume snapshots. Shadow copies can
be used to restore previous versions of files from within Windows
Explorer. Shadow Copy support is built into Vista and Windows 7. Windows
XP or 2000 users need to install the <span> </span>
(http://www.microsoft.com/enus/download/details.aspx?displaylang=en&id=16220).

When a periodic snapshot task is created on a ZFS pool that is
configured as a SMB share in FreeNAS$^{\text{®}}$, it is automatically
configured to support shadow copies.

Before using shadow copies with FreeNAS$^{\text{®}}$, be aware of the
following caveats:

-   If the Windows system is not fully patched to the latest service
    pack, Shadow Copies may not work. If no previous versions of files
    to restore are visible, use Windows Update to ensure the system is
    fully uptodate.

-   Shadow copy support only works for ZFS pools or datasets. This means
    that the SMB share must be configured on a pool or dataset, not on
    a directory.

-   Datasets are filesystems and shadow copies cannot
    traverse filesystems. To see the shadow copies in the child
    datasets, create separate shares for them.

-   Shadow copies will not work with a manual snapshot. Creating a
    periodic snapshot task for the pool or dataset being shared by SMB
    or a recursive task for a parent dataset is recommended.

-   The periodic snapshot task should be created and at least one
    snapshot should exist creating the SMB share. If the SMB share was
    created first, restart the SMB service in .

-   Appropriate permissions must be configured on the pool or dataset
    being shared by SMB.

-   Users cannot delete shadow copies on the Windows system due to the
    way Samba works. Instead, the administrator can remove snapshots
    from the FreeNAS$^{\text{®}}$ web interface. The only way to disable
    shadow copies completely is to remove the periodic snapshot task and
    delete all snapshots associated with the SMB share.

To configure shadow copy support, use the instructions in () to create
the desired number of shares.

To enable shadow copies, check the setting when creating an ().

Creating Authenticated and Time Machine Shares {#\detokenize{sharing:creating-authenticated-and-time-machine-shares}}
----------------------------------------------

\[\]\[\] macOS includes the <span> </span>
(https://support.apple.com/enus/HT201250) feature which performs
automatic backups. FreeNAS$^{\text{®}}$ supports Time Machine backups
for both () and () shares. The process for creating an authenticated
share for a user is the same as creating a Time Machine share for that
user.

Create Time Machine or authenticated shares on a ().

Change permissions on the new dataset by going to . Select the dataset,
click (Options), .

Enter these settings:

1.  Use the dropdown to select the desired user account. If the user
    does not yet exist on the FreeNAS$^{\text{®}}$ system, create one
    with . See () for more information.

2.  Select the desired group name. If the group does not yet exist on
    the FreeNAS$^{\text{®}}$ system, create one with . See () for
    more information.

3.  Click .

Create the authenticated or Time Machine share:

1.  Go to or and click . Apple <span> </span>
    (https://support.apple.com/enus/HT207828) and recommends using SMB.

2.  to the dataset created for the share.

3.  When creating a Time Machine share, set the option.

4.  Fill out the other required fields.

5.  Click .

When creating multiple authenticated or Time Machine shares, repeat this
process for each user. shows creating a Time Machine Share in .

\[\]

Configuring a quota for each Time Machine share helps prevent backups
from using all available space on the FreeNAS$^{\text{®}}$ system. Time
Machine waits two minutes before creating a full backup. It then creates
ongoing hourly, daily, weekly, and monthly backups. Note that a default
installation of macOS is over 20 GiB.

Configure a global quota using the instructions in <span> </span>
(https://forums.freenas.org/index.php?threads/howtosetuptimemachineformultiplemachineswithosxserverstylequotas.47173/)
or create individual share quotas.

### Setting SMB and AFP Share Quotas {#\detokenize{sharing:setting-smb-and-afp-share-quotas}}

Go to , click (Options) on the Time Machine share, and . Click and enter
a <span> </span>
(https://www.samba.org/samba/docs/current/manhtml/vfs\_fruit.8.html)
parameter in the . Time Machine quotas use the parameter. For example,
to set a quota of , enter .

Go to , click (Options) on the Time Machine share, and . In the example
shown in , the Time Machine share name is . Enter a value in the field,
and click . In this example, the Time Machine share is restricted to 200
GiB.

\[\]

### Client Time Machine Configuration {#\detokenize{sharing:client-time-machine-configuration}}

<span>note</span><span>Note:</span> The example shown here is intended
to show the general process of adding a FreeNAS$^{\text{®}}$ share in
Time Machine. The example might not reflect the exact process to
configure Time Machine on a specific version of macOS. See the <span>
</span> (https://support.apple.com/enus/HT201250) for detailed Time
Machine configuration instructions.

To configure Time Machine on the macOS client, go to , and click in the
left panel.

\[\]

Click in the right panel to find the FreeNAS$^{\text{®}}$ system with
the share. Highlight the share and click . A connection dialog prompts
to log in to the FreeNAS$^{\text{®}}$ system.

If is shown when backing up to the FreeNAS$^{\text{®}}$ system, a
sparsebundle image must be created using <span> </span>
(https://community.netgear.com/t5/StoraLegacy/SolutiontoquotTimeMachinecouldnotcompletethebackup/tdp/294697).

If is shown, follow the instructions in <span> </span>
(http://www.garth.org/archives/2011,08,27,169,fixtimemachinesparsebundlenasbasedbackuperrors.html)
to avoid making another backup or losing past backups.

Services {#\detokenize{services:services}}
========

\[\]\[\]\[\] Services that ship with FreeNAS$^{\text{®}}$ are
configured, started, or stopped in . FreeNAS$^{\text{®}}$ includes these
builtin services:

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

This section demonstrates starting a FreeNAS$^{\text{®}}$ service and
the available configuration options for each FreeNAS$^{\text{®}}$
service.

Configure Services {#\detokenize{services:configure-services}}
------------------

\[\]\[\] The page, shown in , lists all services. The list has options
to activate the service, set a service to at system boot, and configure
a service. The S.M.A.R.T. service is enabled by default, but only runs
if the storage devices support <span> </span>
(https://en.wikipedia.org/wiki/S.M.A.R.T.). Other services default to
until started.

\[\]

Stopped services show the sliding button on the left. Active services
show the sliding button on the right. Click the slider to start or stop
a service. Stopping a service shows a confirmation dialog.

<span>tip</span><span>Tip:</span> Using a proxy server can prevent the
list of services from being displayed. If a proxy server is used, do not
configure it to proxy local network or websocket connections. VPN
software can also cause problems. If the list of services is displayed
when connecting on the local network but not when connecting through the
VPN, check the VPN software configuration.

Services are configured by clicking (Configure).

If a service does not start, go to and enable . Console messages appear
at the bottom of the browser. Clicking the console message area makes it
into a popup window, allowing scrolling through or copying the messages.
Watch these messages for errors when stopping or starting the
problematic service.

To read the system logs for more information about a service failure,
open () and type .

AFP {#\detokenize{services:afp}}
---

\[\]\[\] The settings that are configured when creating AFP shares in
are specific to each configured AFP share. An AFP share is created by
navigating to , and clicking . In contrast, global settings which apply
to all AFP shares are configured in .

shows the available global AFP configuration options which are described
in .

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Guest Account & dropdown menu & Select an account to use for guest
access. The account must have permissions to the pool or dataset being
shared.\
Guest Access & checkbox & If enabled, clients are not prompted to
authenticate before accessing AFP shares.\
Max. Connections & integer & Maximum number of simultaneous connections
permited via AFP. The default limit is 50.\
Database Path & browse button & Sets the database information to be
stored in the path. Default is the root of the pool. The path must be
writable even if the pool is read only.\
Chmod Request & dropdown menu & Set how ACLs are handled. Choices are: ,
, or .\
Map ACLs & dropdown menu & Choose mapping of effective permissions for
authenticated users: (default, Unixstyle permissions), (ACLs), or .\
Bind Interfaces & selection & Specify the IP addresses to listen for FTP
connections. Select the desired IP addresses in the list to add them to
the list.\
Global auxiliary parameters & string & Additional <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=afp.conf) parameters not
covered elsewhere in this screen.\

### Troubleshooting AFP {#\detokenize{services:troubleshooting-afp}}

\[\] Check for error messages in .

Determine which users are connected to an AFP share by typing .

If is shown, run this command from (), replacing the path to the
problematic AFP share:

\[commandchars=\
{}\] dbd rf /path/to/share

This command can take some time, depending upon the size of the pool or
dataset being shared. The CNID database is wiped and rebuilt from the
CNIDs stored in the AppleDouble files.

Dynamic DNS {#\detokenize{services:dynamic-dns}}
-----------

\[\]\[\] Dynamic DNS (DDNS) is useful if the FreeNAS$^{\text{®}}$ system
is connected to an ISP that periodically changes the IP address of the
system. With dynamic DNS, the system can automatically associate its
current IP address with a domain name, allowing access to the
FreeNAS$^{\text{®}}$ system even if the IP address changes. DDNS
requires registration with a DDNS service such as <span> </span>
(https://dyn.com/dns/).

shows the DDNS configuration screen and summarizes the configuration
options. The values for these fields are provided by the DDNS provider.
After configuring DDNS, remember to start the DDNS service in .

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Provider & dropdown menu & Several providers are supported. If a
specific provider is not listed, select and enter the information in the
and fields.\
CheckIP Server SSL & checkbox & Use HTTPS for the connection to the .\
CheckIP Server & string & Name and port of the server that reports the
external IP address. For example, entering uses <span> </span>
(https://help.dyn.com/remoteaccessapi/checkiptool/) to discover the
remote socket IP address.\
CheckIP Path & string & Path to the . For example, uses a of and of .\
SSL & checkbox & Use HTTPS for the connection to the server that updates
the DNS record.\
Custom Server & string & DDNS server name. For example, denotes a server
similar to dyndns.org.\
Custom Path & string & DDNS server path. Path syntax varies by provider
and must be obtained from that provider. For example, is a simple path
for the . The hostname is automatically appended by default. More
examples are in the <span> </span>
(https://github.com/troglobit/inadyn\#customddnsproviders).\
Domain name & string & Fully qualified domain name of the host with the
dynamic IP addess. Separate multiple domains with a space, comma (), or
semicolon (). Example:\
Username & string & Username for logging in to the provider and updating
the record.\
Password & string & Password for logging in to the provider and updating
the record.\
Update period & integer & How often the IP is checked in seconds.\

When using the , enter the domain name for and enter the DDNS key
generated for that domain’s A entry at the <span> </span>
(https://he.net) website for .

FTP {#\detokenize{services:ftp}}
---

\[\]\[\] FreeNAS$^{\text{®}}$ uses the <span> </span>
(http://www.proftpd.org/) FTP server to provide FTP services. Once the
FTP service is configured and started, clients can browse and download
data using a web browser or FTP client software. The advantage of FTP is
that easytouse crossplatform utilities are available to manage uploads
to and downloads from the FreeNAS$^{\text{®}}$ system. The disadvantage
of FTP is that it is considered to be an insecure protocol, meaning that
it should not be used to transfer sensitive files. If concerned about
sensitive data, see ().

This section provides an overview of the FTP configuration options. It
then provides examples for configuring anonymous FTP, specified user
access within a chroot environment, encrypting FTP connections, and
troubleshooting tips.

shows the configuration screen for . Some settings are only available in
. To see these settings, either click the button or configure the system
to always display these settings by setting the option in .

\[\]

summarizes the available options when configuring the FTP server.

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.14-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.54-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Port & integer && Set the port the FTP service listens on.\
Clients & integer && Maximum number of simultaneous clients.\
Connections & integer && Set the maximum number of connections per IP
address. means unlimited.\
Login Attempts & integer && Enter the maximum number of attempts before
the client is disconnected. Increase this if users are prone to typos.\
Timeout & integer && Maximum client idle time in seconds before client
is disconnected.\
Allow Root Login & checkbox && Setting this option is discouraged as it
increases security risk.\
Allow Anonymous Login & checkbox && Allow anonymous FTP logins with
access to the directory specified in the .\
Path & browse button && Set the root directory for anonymous FTP
connections.\
Allow Local User Login & checkbox && Allow any local user to log in. By
default, only members of the group are allowed to log in.\
Display Login & string && Specify the message displayed to local login
users after authentication. Not displayed to anonymous login users.\
Allow Transfer Resumption & checkbox && Set to allow FTP clients to
resume interrupted transfers.\
Always Chroot & checkbox && When set a local user is only allowed access
to their home directory when they are a member of the group.\
Perform Reverse DNS Lookups & checkbox && Set to perform reverse DNS
lookups on client IPs. Can cause long delays if reverse DNS is not
configured.\
Masquerade address & string && Public IP address or hostname. Set if FTP
clients cannot connect through a NAT device.\
Certificate & dropdown menu && Select the SSL certificate to be used for
TLS FTP connections. Go to to create a certificate.\
TLS No Certificate Request & checkbox && Set if the client cannot
connect, and it is suspected the client is not properly handling server
certificate requests.\
File Permission & checkboxes & $\checkmark$ & Sets default permissions
for newly created files.\
Directory Permission & checkboxes & $\checkmark$ & Sets default
permissions for newly created directories.\
Enable <span> </span>
(https://en.wikipedia.org/wiki/File\_eXchange\_Protocol) & checkbox &
$\checkmark$ & Set to enable the File eXchange Protocol. This is
discouraged as it makes the server vulnerable to FTP bounce attacks.\
Require IDENT Authentication & checkbox & $\checkmark$ & Setting this
option results in timeouts if is not running on the client.\
Minimum Passive Port & integer & $\checkmark$ & Used by clients in PASV
mode, default of means any port above 1023.\
Maximum Passive Port & integer & $\checkmark$ & Used by clients in PASV
mode, default of means any port above 1023.\
Local User Upload Bandwidth & integer & $\checkmark$ & Defined in KiB/s,
default of means unlimited.\
Local User Download Bandwidth & integer & $\checkmark$ & Defined in
KiB/s, default of means unlimited.\
Anonymous User Upload Bandwidth & integer & $\checkmark$ & Defined in
KiB/s, default of means unlimited.\
Anonymous User Download Bandwidth & integer & $\checkmark$ & Defined in
KiB/s, default of means unlimited.\
Enable TLS & checkbox & $\checkmark$ & Set to enable encrypted
connections. Requires a certificate to be created or imported using ().\
TLS Policy & dropdown menu & $\checkmark$ & The selected policy defines
whether the control channel, data channel, both channels, or neither
channel of an FTP session must occur over SSL/TLS. The policies are
described <span> </span>
(http://www.proftpd.org/docs/directives/linked/config\_ref\_TLSRequired.html).\
TLS Allow Client Renegotiations & checkbox & $\checkmark$ & Setting this
option is recommended as it breaks several security measures. For this
and the rest of the TLS fields, refer to <span> </span>
(http://www.proftpd.org/docs/contrib/mod\_tls.html) for more details.\
TLS Allow Dot Login & checkbox & $\checkmark$ & If set, the user home
directory is checked for a file which contains one or more PEMencoded
certificates. If not found, the user is prompted for password
authentication.\
TLS Allow Per User & checkbox & $\checkmark$ & If set, the user password
may be sent unencrypted.\
TLS Common Name Required & checkbox & $\checkmark$ & When set, the
common name in the certificate must match the FQDN of the host.\
TLS Enable Diagnostics & checkbox & $\checkmark$ & If set when
troubleshooting a connection, logs more verbosely.\
TLS Export Certificate Data & checkbox & $\checkmark$ & If set, exports
the certificate environment variables.\
TLS No Certificate Request & checkbox & $\checkmark$ & Set if the client
cannot connect and it is suspected the client is poorly handling the
server certificate request.\
TLS No Empty Fragments & checkbox & $\checkmark$ & Setting this option
is recommended as it bypasses a security mechanism.\
TLS No Session Reuse Required & checkbox & $\checkmark$ & Setting this
option reduces the security of the connection. Only use if the client
does not understand reused SSL sessions.\
TLS Export Standard Vars & checkbox & $\checkmark$ & If enabled, sets
several environment variables.\
TLS DNS Name Required & checkbox & $\checkmark$ & If set, the client DNS
name must resolve to its IP address and the cert must contain the same
DNS name.\
TLS IP Address Required & checkbox & $\checkmark$ & If set, the client
certificate must contain the IP address that matches the IP address of
the client.\
Auxiliary Parameters & string & $\checkmark$ & Used to add <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=proftpd) parameters
not covered elsewhere in this screen.\

This example demonstrates the auxiliary parameters that prevent all
users from performing the FTP DELETE command:

\[commandchars=\
{}\] Limit DELE DenyAll /Limit

### Anonymous FTP {#\detokenize{services:anonymous-ftp}}

\[\] Anonymous FTP may be appropriate for a small network where the
FreeNAS$^{\text{®}}$ system is not accessible from the Internet and
everyone in the internal network needs easy access to the stored data.
Anonymous FTP does not require a user account for every user. In
addition, passwords are not required so it is not necessary to manage
changed passwords on the FreeNAS$^{\text{®}}$ system.

To configure anonymous FTP:

1.  Give the builtin ftp user account permissions to the pool or dataset
    to be shared in :

    -   : select the builtin user from the dropdown menu

    -   : select the builtin group from the dropdown menu

    -   : review that the permissions are appropriate for the share

    <span>note</span><span>Note:</span> For FTP, the type of client does
    not matter when it comes to the type of ACL. This means that Unix
    ACLs are used even if Windows clients are accessing
    FreeNAS$^{\text{®}}$ via FTP.

2.  Configure anonymous FTP in by setting these attributes:

    -   : set this option

    -   : browse to the pool/dataset/directory to be shared

3.  Start the FTP service in . Click the sliding button on the row. The
    FTP service takes a second or so to start. The sliding button moves
    to the right when the service is running.

4.  Test the connection from a client using a utility such as <span>
    </span> (https://filezillaproject.org/).

In the example shown in , The user has entered this information into the
Filezilla client:

-   IP address of the FreeNAS$^{\text{®}}$ server:

-   :

-   : the email address of the user

\[\]

The messages within the client indicate the FTP connection is
successful. The user can now navigate the contents of the root folder on
the remote site. This is the pool or dataset specified in the FTP
service configuration. The user can also transfer files between the
local site (their system) and the remote site (the FreeNAS$^{\text{®}}$
system).

### FTP in chroot {#\detokenize{services:ftp-in-chroot}}

\[\] If users are required to authenticate before accessing the data on
the FreeNAS$^{\text{®}}$ system, either create a user account for each
user or import existing user accounts using () or (). Create a ZFS
dataset for user, then chroot each user so they are limited to the
contents of their own home directory. Datasets provide the added benefit
of configuring a quota so that the size of a user home directory is
limited to the size of the quota.

To configure this scenario:

1.  Create a ZFS dataset for each user in . Click the (Options) button,
    then . Set an appropriate quota for each dataset. Repeat this
    process to create a dataset for every user that needs access to the
    FTP service.

2.  When () or () are not being used, create a user account for each
    user by navigating to , and clicking . For each user, browse to the
    dataset created for that user in the field. Repeat this process to
    create a user account for every user that needs access to the FTP
    service, making sure to assign each user their own dataset.

3.  Set the permissions for each dataset by navigating to , and clicking
    the (Options) on the desired dataset. Click the button, then assign
    a user account as of that dataset. Set the desired permissions for
    that user. Repeat for each dataset.

    <span>note</span><span>Note:</span> For FTP, the type of client does
    not matter when it comes to the type of ACL. This means Unix ACLs
    are always used, even if Windows clients will be accessing
    FreeNAS$^{\text{®}}$ via FTP.

4.  Configure FTP in with these attributes:

    -   : browse to the parent pool containing the datasets.

    -   Make sure the options for and are .

    -   Select the option to enable it.

    -   Select the option to enable it.

5.  Start the FTP service in . Click the sliding button on the row. The
    FTP service takes a second or so to start. The sliding button moves
    to the right to show the service is running.

6.  Test the connection from a client using a utility such as Filezilla.

To test this configuration in Filezilla, use the of the
FreeNAS$^{\text{®}}$ system, the of a user that is associated with a
dataset, and the for that user. The messages will indicate the
authorization and the FTP connection are successful. The user can now
navigate the contents of the root folder on the remote site. This time
it is not the entire pool but the dataset created for that user. The
user can transfer files between the local site (their system) and the
remote site (their dataset on the FreeNAS$^{\text{®}}$ system).

### Encrypting FTP {#\detokenize{services:encrypting-ftp}}

\[\] To configure any FTP scenario to use encrypted connections:

1.  Import or create a certificate authority using the instructions
    in (). Then, import or create the certificate to use for encrypted
    connections using the instructions in ().

2.  In , click , choose the certificate in , and set the option.

3.  Specify secure FTP when accessing the FreeNAS$^{\text{®}}$ system.
    For example, in Filezilla enter (for an implicit connection) or (for
    an explicit connection) as the Host when connecting. The first time
    a user connects, they will be presented with the certificate of the
    FreeNAS$^{\text{®}}$ system. Click to accept the certificate and
    negotiate an encrypted connection.

4.  To force encrypted connections, select for the .

### Troubleshooting FTP {#\detokenize{services:troubleshooting-ftp}}

\[\] The FTP service will not start if it cannot resolve the system
hostname to an IP address with DNS. To see if the FTP service is
running, open () and issue the command:

\[commandchars=\
{}\] sockstat 4p 21

If there is nothing listening on port 21, the FTP service is not
running. To see the error message that occurs when FreeNAS$^{\text{®}}$
tries to start the FTP service, go to , enable , and click . Go to and
switch the FTP service off, then back on. Watch the console messages at
the bottom of the browser for errors.

If the error refers to DNS, either create an entry in the local DNS
server with the FreeNAS$^{\text{®}}$ system hostname and IP address, or
add an entry for the IP address of the FreeNAS$^{\text{®}}$ system in
the field.

iSCSI {#\detokenize{services:iscsi}}
-----

\[\] Refer to () for instructions on configuring iSCSI. Start the iSCSI
service in by clicking the sliding button in the row.

<span>note</span><span>Note:</span> A warning message is shown the iSCSI
service stops when initiators are connected. Open the () and type to
determine the names of the connected initiators.

LLDP {#\detokenize{services:lldp}}
----

\[\]\[\] The Link Layer Discovery Protocol (LLDP) is used by network
devices to advertise their identity, capabilities, and neighbors on an
Ethernet network. FreeNAS$^{\text{®}}$ uses the <span> </span>
(https://github.com/sspans/ladvd) LLDP implementation. If the network
contains managed switches, configuring and starting the LLDP service
will tell the FreeNAS$^{\text{®}}$ system to advertise itself on the
network.

shows the LLDP configuration screen and summarizes the configuration
options for the LLDP service.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Interface Description & checkbox & Set to enable receive mode and to
save and received peer information in interface descriptions.\
Country Code & string & Required for LLDP location support. Enter a
twoletter ISO 3166 country code.\
Location & string & Optional. Specify the physical location of the
host.\

NFS {#\detokenize{services:nfs}}
---

\[\]\[\] The settings that are configured when creating NFS shares in
are specific to each configured NFS share. An NFS share is created by
going to and clicking . Global settings which apply to all NFS shares
are configured in .

shows the configuration screen and summarizes the configuration options
for the NFS service.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Number of servers & integer & Specify how many servers to create.
Increase if NFS client responses are slow. To limit CPU context
switching, keep this number less than or equal to the number of CPUs
reported by .\
Serve UDP NFS clients & checkbox & Set if NFS clients need to use UDP.\
Bind IP Addresses & dropdown & Select IP addresses to listen on for NFS
requests. When all options are unset, NFS listens on all available
addresses.\
Allow nonroot mount & checkbox & Set only if required by the NFS
client.\
Enable NFSv4 & checkbox & Set to switch from NFSv3 to NFSv4. The default
is NFSv3.\
NFSv3 ownership model for NFSv4 & checkbox & Grayed out unless is
selected and, in turn, grays out which is incompatible. Set this option
if NFSv4 ACL support is needed without requiring the client and the
server to sync users and groups.\
Require Kerberos for NFSv4 & checkbox & Set to force NFS shares to fail
if the Kerberos ticket is unavailable. Disabling this option allows
using either default NFS or Kerberos authentication.\
mountd(8) bind port & integer & Optional. Specify the port that <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=mountd) binds to.\
rpc.statd(8) bind port & integer & Optional. Specify the port that
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=rpc.statd)
binds to.\
rpc.lockd(8) bind port & integer & Optional. Specify the port that
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=rpc.lockd)
binds to.\
Support &gt;16 groups & checkbox & Set this option if any users are
members of more than 16 groups (useful in AD environments). Note this
assumes group membership is configured correctly on the NFS server.\
Log mountd(8) requests & checkbox & Enable logging of <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=mountd) requests by syslog.\
Log rpc.statd(8) and rpc.lockd(8) & checkbox & Enable logging of <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=rpc.statd) and <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=rpc.lockd) requests
by syslog.\

<span>note</span><span>Note:</span> NFSv4 sets all ownership to if user
and group do not match on client and server.

Rsync {#\detokenize{services:rsync}}
-----

\[\]\[\] is used to configure an rsync server when using rsync module
mode. Refer to () for a configuration example.

This section describes the configurable options for the service and
rsync modules.

### Configure Rsyncd {#\detokenize{services:configure-rsyncd}}

\[\] To configure the server, go to and click for the service.

\[\]

summarizes the configuration options for the rsync daemon:

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

TCP Port & integer & listens on this port. The default is .\
Auxiliary parameters & string & Enter any additional parameters from
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=rsyncd.conf).\

### Rsync Modules {#\detokenize{services:rsync-modules}}

\[\] To add a new Rsync module, go to , click for the service, select
the tab, and click .

\[\]

summarizes the configuration options available when creating a rsync
module.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Name & string & Module name that matches the name requested by the rsync
client.\
Comment & string & Describe this module.\
Path & file browser & Browse to the pool or dataset to store received
data.\
Access Mode & dropdown menu & Choose permissions for this rsync module.\
Maximum connections & integer & Maximum connections to this module. is
unlimited.\
User & dropdown menu & User to run as during file transfers to and from
this module.\
Group & dropdown menu & Group to run as during file transfers to and
from this module.\
Hosts Allow & string & From <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=rsyncd.conf). A list of
patterns to match with the hostname and IP address of a connecting
client. The connection is rejected if no patterns match. Separate
patterns with whitespace or a comma.\
Hosts Deny & string & From <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=rsyncd.conf). A list of
patterns to match with the hostname and IP address of a connecting
client. The connection is rejected when the patterns match. Separate
patterns with whitespace or a comma.\
Auxiliary parameters & string & Enter any additional parameters from
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=rsyncd.conf).\

S3 {#\detokenize{services:s3}}
--

\[\]\[\] S3 is a distributed or clustered filesystem protocol compatible
with Amazon S3 cloud storage. The FreeNAS$^{\text{®}}$ S3 service uses
<span> </span> (https://minio.io/) to provide S3 storage hosted on the
FreeNAS$^{\text{®}}$ system itself. Minio also provides features beyond
the limits of the basic Amazon S3 specifications.

shows the S3 service configuration screen and summarizes the
configuration options. After configuring the S3 service, start it in .

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

IP Address & dropdown menu & Enter the IP address to run the S3 service.
sets the server to listen on all addresses.\
Port & string & Enter the TCP port on which to provide the S3 service.
Default is .\
Access Key & string & Enter the S3 access ID. See <span> </span>
(https://docs.aws.amazon.com/general/latest/gr/awsseccredtypes.html\#accesskeysandsecretaccesskeys)
for more information.\
Secret Key & string & Enter the S3 secret access key. See <span> </span>
(https://docs.aws.amazon.com/general/latest/gr/awsseccredtypes.html\#accesskeysandsecretaccesskeys)
for more information.\
Confirm Secret Key & string & Reenter the S3 password to confirm.\
Disk & browse & Directory where the S3 filesystem will be mounted.
Ownership of this directory and all subdirectories is set to . () for
Minio to avoid issues with conflicting directory permissions or
ownership.\
Enable Browser & checkbox & Set to enable the web user interface for the
S3 service. Access the minio web interface by entering the IP address
and port number separated by a colon in the browser address bar.\
Certificate & dropdown menu & Add the () to be used for secure S3
connections.\

S.M.A.R.T. {#\detokenize{services:s-m-a-r-t}}
----------

\[\]\[\] <span> </span> (https://en.wikipedia.org/wiki/S.M.A.R.T.), is
an industry standard for disk monitoring and testing. Drives can be
monitored for status and problems, and several types of selftests can be
run to check the drive health.

Tests run internally on the drive. Most tests can run at the same time
as normal disk usage. However, a running test can greatly reduce drive
performance, so they should be scheduled at times when the system is not
busy or in normal use. It is very important to avoid scheduling
diskintensive tests at the same time. For example, do not schedule
S.M.A.R.T. tests to run at the same time, or preferably, even on the
same days as ().

Of particular interest in a NAS environment are the and S.M.A.R.T.
tests. Details vary between drive manufacturers, but a test generally
does some basic tests of a drive that takes a few minutes. The test
scans the entire disk surface, and can take several hours on larger
drives.

FreeNAS$^{\text{®}}$ uses the <span> </span>
(https://www.smartmontools.org/browser/trunk/smartmontools/smartd.8.in)
service to monitor S.M.A.R.T. information, including disk temperature. A
complete configuration consists of:

1.  Scheduling when S.M.A.R.T. tests are run. S.M.A.R.T tests are
    created by navigating to , and clicking .

2.  Enabling or disabling S.M.A.R.T. for each disk member of a pool in .
    This setting is enabled by default for disks that support S.M.A.R.T.

3.  Checking the configuration of the S.M.A.R.T. service as described in
    this section.

4.  Starting the S.M.A.R.T. service in .

shows the configuration screen that appears after going to and clicking
(Configure).

\[\]

<span>note</span><span>Note:</span> wakes up at the configured . It
checks the times configured in to see if a test must begin. Since the
smallest time increment for a test is an hour, it does not make sense to
set a value higher than 60 minutes. For example, if the is set to
minutes and the smart test to every hour, the test will only be run
every two hours because only activates every two hours.

summarizes the options in the S.M.A.R.T configuration screen.

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Check Interval & integer & Define in minutes how often activates to
check if any tests are configured to run.\
Power Mode & dropdown menu & Tests are only performed when is selected.
Choices are: , , , or .\
Difference & integer in degrees Celsius & Enter number of degrees in
Celsius. S.M.A.R.T reports if the temperature of a drive has changed by
N degrees Celsius since the last report. Default of disables this
option.\
Informational & integer in degrees Celsius & Enter a threshold
temperature in Celsius. S.M.A.R.T will message with a log level of
LOG\_INFO if the temperature is higher than the threshold. Default of
disables this option.\
Critical & integer in degrees Celsius & Enter a threshold temperature in
Celsius. S.M.A.R.T will message with a log level of LOG\_CRIT and send
an email if the temperature is higher than the threshold. Default of
disables this option.\

SMB {#\detokenize{services:smb}}
---

\[\]\[\]

<span>note</span><span>Note:</span> After starting the SMB service, it
can take several minutes for the <span> </span>
(https://www.samba.org/samba/docs/old/Samba3HOWTO/NetworkBrowsing.html\#id2581357)
to occur and for the FreeNAS$^{\text{®}}$ system to become available in
Windows Explorer.

shows the global configuration options which apply to all SMB shares.
This configuration screen displays the configurable options from <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=smb4.conf).

These options are described in .

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

NetBIOS Name & string & Automatically populated with the original
hostname of the system. Limited to 15 characters. It be different from
the name.\
NetBIOS Alias & string & Enter any aliases, separated by spaces. Each
alias cannot be longer than 15 characters.\
Workgroup & string & Must match the Windows workgroup name. This setting
is ignored if the () or () service is running.\
Description & string & Enter a server description. Optional.\
Enable SMB1 support & checkbox & Allow legacy SMB clients to connect to
the server. SMB1 is not secure and has been deprecated by Microsoft. See
<span> </span> (https://www.ixsystems.com/blog/library/donotusesmb1/).\
UNIX Charset & dropdown menu & Default is which supports all characters
in all languages.\
Log Level & dropdown menu & Choices are , , or .\
Use syslog only & checkbox & Set to log authentication failures in
instead of the default of .\
Local Master & checkbox & Set to determine if the system participates in
a browser election. Disable when network contains an AD or LDAP server
or Vista or Windows 7 machines are present.\
Guest Account & dropdown menu & Account to be used for guest access.
Default is . The chosen account is required to have permissions to the
shared pool or dataset. To adjust permissions, edit the dataset Access
Control List (ACL), add a new entry for the chosen guest account, and
configure the permissions in that entry. If the selected is deleted the
field resets to .\
Administrators Group & dropdown menu & Members of this group are local
admins and automatically have privileges to take ownership of any file
in an SMB share, reset permissions, and administer the SMB server
through the Computer Management MMC snapin.\
Auxiliary Parameters & string & Enter additional <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=smb.conf) options. See the
<span> </span>
(http://www.oreilly.com/openbook/samba/book/appb\_02.html) for more
information on the available settings.

To log more details when a client attempts to authenticate to the share,
add .\
Zeroconf share discovery & checkbox & Enable if Mac clients will be
connecting to the SMB share.\
NTLMv1 Auth & checkbox & Set to allow NTLMv1 authentication. Required by
Windows XP clients and sometimes by clients in later versions of
Windows.\
Bind IP Addresses & checkboxes & Static IP addresses which SMB listens
on for connections. Leaving all unselected defaults to listening on all
active interfaces.\
Range Low & integer &\
Range High & integer &\

Changes to SMB settings take effect immediately. Changes to share
settings only take effect after the client and server negotiate a new
session.

<span>note</span><span>Note:</span> Do not set the as an . Due to
differences in how Linux and BSD handle file descriptors, directory name
caching is disabled on BSD systems to improve performance.

<span>note</span><span>Note:</span> () cannot be disabled while () is
enabled.

### Troubleshooting SMB {#\detokenize{services:troubleshooting-smb}}

\[\] Connecting to SMB shares as , and adding the root user in the SMB
user database is not recommended.

Samba is single threaded, so CPU speed makes a big difference in SMB
performance. A typical 2.5Ghz Intel quad core or greater should be
capable of handling speeds in excess of Gb LAN while low power CPUs such
as Intel Atoms and AMD C30sE350E450 will not be able to achieve more
than about 3040MB/sec typically. Remember that other loads such as ZFS
will also require CPU resources and may cause Samba performance to be
less than optimal.

Windows automatically caches file sharing information. If changes are
made to an SMB share or to the permissions of a pool or dataset being
shared by SMB and the share becomes inaccessible, log out and back in to
the Windows system. Alternately, users can type from the command line to
clear their SMB sessions.

Windows also automatically caches login information. To require users to
log in every time they access the system, reduce the cache settings on
the client computers.

Where possible, avoid using a mix of case in filenames as this can cause
confusion for Windows users. <span> </span>
(https://www.oreilly.com/openbook/samba/book/ch05\_04.html) explains in
more detail.

If the SMB service will not start, run this command from () to see if
there is an error in the configuration:

\[commandchars=\
{}\] testparm /usr/local/etc/smb4.conf

Using a dataset for SMB sharing is recommended. When creating the
dataset, make sure that the is set to .

use to attempt to fix the permissions on a SMB share as it destroys the
Windows ACLs. The correct way to manage permissions on a SMB share is to
use the ().

The Samba <span> </span>
(https://wiki.samba.org/index.php/Performance\_Tuning) page describes
options to improve performance.

Directory listing speed in folders with a large number of files is
sometimes a problem. A few specific changes can help improve the
performance. However, changing these settings can affect other usage. In
general, the defaults are adequate.

-   can also have a performance penalty. When not needed, it can be
    disabled or reduced in the ().

-   Create as SMBstyle dataset and enable the auxiliary parameter

-   Disable as many as possible in the (). Many have
    performance overhead.

SNMP {#\detokenize{services:snmp}}
----

\[\]\[\] SNMP (Simple Network Management Protocol) is used to monitor
networkattached devices for conditions that warrant administrative
attention. FreeNAS$^{\text{®}}$ uses <span> </span>
(http://netsnmp.sourceforge.net/) to provide SNMP. When starting the
SNMP service, this port will be enabled on the FreeNAS$^{\text{®}}$
system:

-   UDP 161 (listens here for SNMP requests)

Available MIBS are located in .

shows the screen. summarizes the configuration options.

\[\]

<span>|&gt;p<span>0.16-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Location & string & Enter the location of the system.\
Contact & string & Enter an email address to receive messages from the
SNMP service.\
Community & string & Change from to increase system security. Can only
contain alphanumeric characters, underscores, dashes, periods, and
spaces. This can be left empty for SNMPv3 networks.\
SNMP v3 Support & checkbox & Set to enable support for <span> </span>
(https://tools.ietf.org/html/rfc3410). See <span> </span>
(http://netsnmp.sourceforge.net/docs/man/snmpd.conf.html) for more
information about configuring this and the , , , and fields.\
Username & string & Only applies if is set. Enter a username to register
with this service.\
Authentication Type & dropdown menu & Only applies if is enabled.
Choices are or .\
Password & string & Only applies if is enabled. Enter and confirm a
password of at least eight characters.\
Privacy Protocol & dropdown menu & Only applies if is enabled. Choices
are or .\
Privacy Passphrase & string & Enter a separate privacy passphrase. is
used when this is left empty.\
Auxiliary Parameters & string & Enter additional <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=snmpd.conf) options. Add one
option for each line.\
Expose zilstat via SNMP & checkbox & Enabling this option may have pool
performance implications.\
Log Level & dropdown menu & Choose how many log entries to create.
Choices range from the least log entries (Emergency) to the most
(Debug).\

<span> </span> (https://www.zenoss.com/) provides a seamless monitoring
service through SNMP for FreeNAS$^{\text{®}}$ called <span> </span>
(https://www.zenoss.com/product/zenpacks/truenas).

SSH {#\detokenize{services:ssh}}
---

\[\]\[\] Secure Shell (SSH) is used to transfer files securely over an
encrypted network. When a FreeNAS$^{\text{®}}$ system is used as an SSH
server, the users in the network must use <span> </span>
(https://en.wikipedia.org/wiki/Comparison\_of\_SSH\_clients) to transfer
files with SSH.

This section shows the FreeNAS$^{\text{®}}$ SSH configuration options,
demonstrates an example configuration that restricts users to their home
directory, and provides some troubleshooting tips.

shows the screen.

<span>note</span><span>Note:</span> After configuring SSH, remember to
start it in by clicking the sliding button in the row. The sliding
button moves to the right when the service is running.

\[\]

summarizes the configuration options. Some settings are only available
in . To see these settings, either click the button, or configure the
system to always display these settings by enabling the option in .

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.14-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.54-2</span>|</span>

\[\]\
Setting & Value & Advanced Mode & Description\

\
Setting & Value & Advanced Mode & Description\

\

Bind interfaces & selection & $\checkmark$ & By default, SSH listens on
all interfaces unless specific interfaces are selected in this dropdown
menu.\
TCP port & integer && Port to open for SSH connection requests. by
default.\
Log in as root with password & checkbox && If enabled, password must be
set for the user in .\
Allow password authentication & checkbox && Unset to require keybased
authentication for all users. This requires <span> </span>
(http://the.earth.li/\~sgtatham/putty/0.55/htmldoc/Chapter8.html) on
both the SSH client and server.\
Allow kerberos authentication & checkbox & $\checkmark$ & Ensure () and
() are configured and FreeNAS$^{\text{®}}$ can communicate with the
Kerberos Domain Controller (KDC) before enabling this option.\
Allow TCP port forwarding & checkbox && Set to allow users to bypass
firewall restrictions using the SSH <span> </span>
(https://www.symantec.com/connect/articles/sshportforwarding).\
Compress connections & checkbox && Set to attempt to reduce latency over
slow networks.\
SFTP log level & dropdown menu & $\checkmark$ & Select the <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=syslog) level of the
SFTP server.\
SFTP log facility & dropdown menu & $\checkmark$ & Select the <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=syslog) facility of
the SFTP server.\
Extra options & string & $\checkmark$ & Add any additional <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=sshd\_config) options
not covered in this screen, one per line. These options are
casesensitive and misspellings can prevent the SSH service from
starting.\

Here are some recommendations for the :

-   Add to disable the insecure cipher.

-   Increase the if SSH connections tend to drop.

-   defaults to . Increase this value when more concurrent SSH
    connections are required.

### SCP Only {#\detokenize{services:scp-only}}

\[\]\[\] When SSH is configured, authenticated users with a user account
can use to log into the FreeNAS$^{\text{®}}$ system over the network.
User accounts are created by navigating to , and clicking . The user
home directory is the pool or dataset specified in the field of the
FreeNAS$^{\text{®}}$ account for that user. While the SSH login defaults
to the user home directory, users are able to navigate outside their
home directory, which can pose a security risk.

It is possible to allow users to use and to transfer files between their
local computer and their home directory on the FreeNAS$^{\text{®}}$
system, while restricting them from logging into the system using . To
configure this scenario, go to , click (Options) for the user, and then
. Change the to . Repeat for each user that needs restricted SSH access.

Test the configuration from another system by running the , , and
commands as the user. and will work but will fail.

<span>note</span><span>Note:</span> Some utilities like WinSCP and
Filezilla can bypass the scponly shell. This section assumes users are
accessing the system using the command line versions of and .

### Troubleshooting SSH {#\detokenize{services:troubleshooting-ssh}}

\[\] Keywords listed in <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=sshd\_config) are case
sensitive. This is important to remember when adding any . The
configuration will not function as intended if the upper and lowercase
letters of the keyword are not an exact match.

If clients are receiving “reverse DNS” or timeout errors, add an entry
for the IP address of the FreeNAS$^{\text{®}}$ system in the field of .

When configuring SSH, always test the configuration as an SSH user
account to ensure the user is limited by the configuration and they have
permission to transfer files within the intended directories. If the
user account is experiencing problems, the SSH error messages are
specific in describing the problem. Type this command within () to read
these messages as they occur:

\[commandchars=\
{}\] tail f /var/log/messages

Additional messages regarding authentication errors are found in .

TFTP {#\detokenize{services:tftp}}
----

\[\]\[\] Trivial File Transfer Protocol (TFTP) is a lightweight version
of FTP typically used to transfer configuration or boot files between
machines, such as routers, in a local environment. TFTP provides an
extremely limited set of commands and provides no authentication.

If the FreeNAS$^{\text{®}}$ system will be used to store images and
configuration files for network devices, configure and start the TFTP
service. Starting the TFTP service opens UDP port 69.

shows the TFTP configuration screen and summarizes the available
options.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Directory & Browse button & Browse to an directory to be used for
storage. Some devices require a specific directory name, refer to the
device documentation for details.\
Allow New Files & checkbox & Set when network devices need to send files
to the system. For example, to back up their configuration.\
Host & IP address & The default host to use for TFTP transfers. Enter an
IP address. Example: .\
Port & integer & The UDP port number that listens for TFTP requests.
Example: .\
Username & dropdown menu & Select the account to use for TFTP requests.
This account must have permission to the .\
File Permissions & checkboxes & Set permissions for newly created files.
The default is everyone can read and only the owner can write. Some
devices require less strict permissions.\
Extra options & string & Add more options from <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=tftpd) Add one option on each
line.\

UPS {#\detokenize{services:ups}}
---

\[\]\[\] FreeNAS$^{\text{®}}$ uses <span> </span>
(https://networkupstools.org/) (Network UPS Tools) to provide UPS
support. If the FreeNAS$^{\text{®}}$ system is connected to a UPS
device, configure the UPS service in .

shows the UPS configuration screen:

\[\]

summarizes the options in the UPS Configuration screen.

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

UPS Mode & dropdown menu & Select if the UPS is plugged directly into
the system serial port. The UPS will remain the last item to shut down.
Select to have the system shut down before .\
Identifier & string & Required. Describe the UPS device. Can contain
alphanumeric, period, comma, hyphen, and underscore characters.\
Driver / Remote Host & combobox & Required. For a list of supported
devices, see the <span> </span>
(https://networkupstools.org/stablehcl.html). The field suggests drivers
based on the text entered. To search for a specific driver, begin typing
the name of the driver. The search is case sensitive.

The field changes to when is set to . Enter the IP address of the system
configured as the UPS system. See this <span> </span>
(https://forums.freenas.org/index.php?resources/configuringupssupportforsingleormultiplefreenasservers.30/)
for more details about configuring multiple systems with a single UPS.\
Port or Hostname & dropdown menu & Serial or USB port connected to the
UPS. To automatically detect and manage the USB port settings, open the
dropdown menu and select . If the specific USB port must be chosen, see
this () about identifing the USB port used by the UPS.

When an SNMP driver is selected, enter the IP address or hostname of the
SNMP UPS device.

becomes when the is set to . Enter the open network port number of the
UPS system. The default port is .\
Auxiliary Parameters (ups.conf) & string & Enter any additional options
from <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=ups.conf).\
Auxiliary Parameters (upsd.conf) & string & Enter any additional options
from <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=upsd.conf).\
Description & string & Optional. Describe the UPS service.\
Shutdown Mode & dropdown menu & Choose when the UPS initiates shutdown.
Choices are and .\
Shutdown Timer & integer & Select a value in seconds for the UPS to wait
before initiating shutdown. Shutdown will not occur if the power is
restored while the timer is counting down. This value only applies when
is set to .\
Shutdown Command & string & Enter the command to run to shut down the
computer when battery power is low or shutdown timer runs out.\
No Communication Warning Time & string & Enter a value in seconds to
wait before alerting that the service cannot reach any UPS. Warnings
continue until the situation is fixed.\
Monitor User & string & Required. Enter a user to associate with this
service. The recommended default user is .\
Monitor Password & string & Required. Default is the known value .
Change this to enhance system security. Cannot contain a space or .\
Extra Users & string & Enter accounts that have administrative access.
See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=upsd.users) for examples.\
Remote Monitor & checkbox & Set for the default configuration to listen
on all interfaces using the known values of user: and password: .\
Send Email Status Updates & checkbox & Set to enables the
FreeNAS$^{\text{®}}$ system to send email updates to the configured
field.\
Email & email address & Enter any email addresses to receive status
updates. Separate multiple addresses with a semicolon ().\
Email Subject & string & Enter a subject line for email status updates.\
Power Off UPS & checkbox & Set for the UPS to power off after shutting
down the FreeNAS$^{\text{®}}$ system.\
Host Sync & integer & Enter a time in seconds for <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=upsmon) to wait in master
mode for the slaves to disconnect during a shutdown.\

\[\]

<span>note</span><span>Note:</span> For USB devices, the easiest way to
determine the correct device name is to enable the option in . Plug in
the USB device and look for a or device name in the console messages.

Some UPS models might be unresponsive with the default polling
frequency. This can show in FreeNAS$^{\text{®}}$ logs as a recurring
error like: .

If this error occurs, decrease the polling frequency by adding an entry
to : . The default polling frequency is two seconds.

<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=upsc) can be
used to get status variables from the UPS daemon such as the current
charge and input voltage. It can be run from () using this syntax:

\[commandchars=\
{}\] upsc ups@localhost

The <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=upsc) man
page gives some other usage examples.

<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=upscmd) can be
used to send commands directly to the UPS, assuming the hardware
supports the command being sent. Only users with administrative rights
can use this command. These users are created in the field.

### Multiple Computers with One UPS {#\detokenize{services:multiple-computers-with-one-ups}}

\[\] A UPS with adequate capacity can power multiple computers. One
computer is connected to the UPS data port with a serial or USB cable.
This makes UPS status available on the network for other computers.
These computers are powered by the UPS, but receive UPS status data from
the master computer. See the <span> </span>
(https://networkupstools.org/docs/usermanual.chunked/index.html) and
<span> </span>
(https://networkupstools.org/docs/man/index.html\#User\_man).

WebDAV {#\detokenize{services:webdav}}
------

\[\]\[\] The WebDAV service can be configured to provide a file browser
over a web connection. Before starting this service, at least one WebDAV
share must be created by navigating to , and clicking . Refer to () for
instructions on how to create a share and connect to it after the
service is configured and started.

The settings in the WebDAV service apply to all WebDAV shares. shows the
WebDAV configuration screen. summarizes the available options.

\[\]

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.63-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Protocol & dropdown menu & keeps the connection unencrypted. encrypts
the connection. allows both types of connections.\
HTTP Port & string & Specify a port for unencrypted connections. The
default port is recommended. use a port number already being used by
another service.\
HTTPS Port & string & Specify a port for encrypted connections. The
default port is recommended. use a port number already being used by
another service.\
Webdav SSL Certificate & dropdown menu & Select the SSL certificate to
be used for encrypted connections. To create a certificate, use .\
HTTP Authentication & dropdown menu & Choices are , (unencrypted) or
(encrypted).\
Webdav Password & string & Default is . Change this password as it is a
known value.\

Plugins {#\detokenize{plugins:plugins}}
=======

\[\]\[\]\[\] FreeNAS$^{\text{®}}$ provides the ability to extend the
builtin NAS services by providing two methods for installing additional
software.

() allow the user to browse, install, and configure prepackaged software
from the web interface. This method is easy to use, but provides a
limited amount of available software. Each plugin is automatically
installed into its own limited <span> </span>
(https://en.wikipedia.org/wiki/Freebsd\_jail) that cannot install
additional software.

() provide more control over software installation, but requires working
from the command line and a good understanding of networking basics and
software installation on FreeBSDbased systems.

Look through the () and () sections to become familiar with the features
and limitations of each. Choose the method that best meets the needs of
the application.

<span>note</span><span>Note:</span> () must be configured before plugins
are available on FreeNAS$^{\text{®}}$. This means having a suitable ()
created to store plugins.

Installing Plugins {#\detokenize{plugins:installing-plugins}}
------------------

\[\] A plugin is a selfcontained application installer designed to
integrate into the FreeNAS$^{\text{®}}$ web interface. A plugin offers
several advantages:

-   the FreeNAS$^{\text{®}}$ web interface provides a browser for
    viewing the list of available plugins

-   the FreeNAS$^{\text{®}}$ web interface provides buttons for
    installing, starting, managing, and uninstalling plugins

-   if the plugin has configuration options, a management screen is
    added to the FreeNAS$^{\text{®}}$ web interface for these options to
    be configured

View available plugins by clicking .

shows some of the available plugins.

\[\]

<span>note</span><span>Note:</span> If the list of available plugins is
not displayed, open () and verify that the FreeNAS$^{\text{®}}$ system
can an address on the Internet. If it cannot, add a default gateway
address and DNS server address in .

Click to toggle the plugins list between <span> </span>
(https://www.freenas.org/plugins/), which receive updates every few
weeks, and <span> </span>
(https://github.com/ixpluginhub/iocagepluginindex).

Click to refresh the current list of plugins.

Click a plugin icon to see the description, whether it is an Official or
Community plugin, the version available, and the number of installed
instances.

To install the selected plugin, click .

\[\]

<span>note</span><span>Note:</span> A warning will display when an
unofficial plugin is selected for installation.

Enter a . A unique name is required, since multiple installations of the
same plugin are supported. Names can contain letters, numbers, periods
(), dashes (), and underscores ().

Most plugins default to . This setting is recommended as it does not
require manual configuration of multiple available IP addresses and
prevents addressing conflicts on the network.

Some plugins default to as their management utility conflicts with .
Keep these plugins set to unless manually configuring an IP address is
preferred.

If both and are unset, an IPv4 or IPv6 address can be manually entered.
If desired, an IPv4 or IPv6 interface can be selected. If no interface
is selected the jail IP address uses the current active interface. The
IPv4 or IPv6 address must be in the range of the local network.

Click to show all options for the plugin jail. The options are described
in ().

To start the installation, click .

Depending on the size of the application, the installation can take
several minutes to download and install. A confirmation message is shown
when the installation completes, along with any postinstallation notes.

Installed plugins appear on the page as shown in .

<span>note</span><span>Note:</span> Plugins are also added to as a jail.
This type of jail is editable like a standard jail, but the cannot be
altered. See () for more details about modifying jails.

\[\]

Plugins are immediately started after installation. By default, all
plugins are started when the system boots. Unsetting means the plugin
will not start when the system boots and must be started manually.

In addition to the name, the menu can be used to display more
information about installed Plugins.

More information such as and is shown by clicking (Expand). Options to ,
, , , and the plugin are also displayed. If an installed plugin has
notes, the notes can be viewed by clicking .

Plugins with additional documentation also have a button which opens the
README in the plugin repository.

The plugin must be started before the installed application is
available. Click (Expand) and . The plugin changes to when it starts
successfully.

Stop and immediately start an plugin by clicking (Expand) and .

Click (Expand) and to open a management or configuration screen for the
application. Plugins with a management interface show the IP address and
port to that page in the column.

<span>note</span><span>Note:</span> Not all plugins have a functional
management option. See () for more instructions about interacting with a
plugin jail with the shell.

Some plugins have options that need to be set before their service will
successfully start. Check the website of the application to see what
documentation is available. If there are any difficulties using a
plugin, refer to the official documentation for that application.

If the application requires access to the data stored on the
FreeNAS$^{\text{®}}$ system, click the entry for the associated jail in
the page and add storage as described in ().

Click (Options) and for the plugin jail in the page. This will give
access to the shell of the jail containing the application to complete
or test the configuration.

If a plugin jail fails to start, open the plugin jail shell from the
page and type to see if any errors were logged.

Updating Plugins {#\detokenize{plugins:updating-plugins}}
----------------

\[\] When a newer version of a plugin or release becomes available in
the official repository, click (Expand) and . Updating a plugin updates
the operating system and version of the plugin.

\[\]

Updating a plugin also restarts that plugin. To update or upgrade the
plugin jail operating system, see ().

Uninstalling Plugins {#\detokenize{plugins:uninstalling-plugins}}
--------------------

\[\] Installing a plugin creates an associated jail. Uninstalling a
plugin deletes the jail because it is no longer required. This means all
Make sure to back up any important data from the plugin uninstalling it.

shows an example of uninstalling a plugin by expanding the plugin’s
entry and clicking . A twostep dialog opens to confirm the action. Enter
the plugin name, set the checkbox, and click to remove the plugin and
the associated jail, dataset, and snapshots.

\[\]

Create a Plugin {#\detokenize{plugins:create-a-plugin}}
---------------

\[\] If an application is not available as a plugin, it is possible to
create a new plugin for FreeNAS$^{\text{®}}$ in a few steps. This
requires an existing <span> </span> (https://github.com) account.

<span> </span> (https://github.com).

Refer to for the files to add to the artifact repository.

<span>|&gt;p<span>0.33-2</span> |&gt;p<span>0.67-2</span>|</span>

\[\]\
Directory/File & Description\

\
Directory/File & Description\

\

& This script is run the jail after it is created and any packages
installed. Enable services in that need to start with the jail and apply
any configuration customizations with this this script.\
& JSON file that accepts the key or value options. For example:

designates the webinterface of the plugin.\
& Directory of files overlaid on the jail after install. For example, is
placed in the location of the jail. Can be used to supply custom files
and configuration data, scripts, and any other type of customized files
to the plugin jail.\
& JSON file that manages the settings interface of the plugin. Required
fields include:

-   

Command to run when restarting the plugin service after changing
settings.

-   

Command used to get values for plugin configuration. Provided by the
plugin creator. The command accepts two arguments for key or value pair.

-   

This subsection contains arrays of elements, starting with the “key”
name and required arguments for that particular type of setting.

See () below.\

This example file is used for the plugin. It is also available online in
the <span> </span>
(https://github.com/freenas/iocagepluginquassel/blob/master/settings.json).

\[commandchars=\
{}\]

Clone the <span> </span>
(https://github.com/ixpluginhub/iocagepluginindex) GitHub repository.

<span>tip</span><span>Tip:</span> Full tutorials and documentation for
GitHub and commands are available on <span> </span>
(https://guides.github.com/).

On the local copy of , create a new JSON file for the
FreeNAS$^{\text{®}}$ plugin. The JSON file describes the plugin, the
packages it requires for operation, and other installation details. This
file is named . For example, the <span> </span>
(https://github.com/ixpluginhub/iocagepluginindex/blob/master/madsonic.json)
plugin is named .

The fields of the file are described in .

<span>|&gt;p<span>0.33-2</span> |&gt;p<span>0.67-2</span>|</span>

\[\]\
Data Field & Description\

\
Data Field & Description\

\

& Name of the plugin.\
& Optional. Enter if simplified postinstall information has been
supplied in . After specifying , echo the information to be presented to
the user in inside the file. See the () and () examples.\
& FreeBSD RELEASE to use for the plugin jail.\
& URL of the plugin artifact repository.\
& The FreeBSD packages required by the plugin.\
& Content Delivery Network (CDN) used by the plugin jail. Default for
the TrueOS CDN is .\
&

Default is .

The pkg fingerprint for the artifact repository. Default is\
& Define whether this is an official iXsystemssupported plugin. Enter or
.\

\[commandchars=\
{},numbers=left,firstnumber=1,stepnumber=1\]

\[commandchars=\
{},numbers=left,firstnumber=1,stepnumber=1\]

sysrc f /etc/rc.conf service rslsync start /dev/null

/root/PLUGININFO /root/PLUGININFO

Here is reproduced as an example:

\[commandchars=\
{}\]

The correct directory and package name of the plugin application must be
used for the value. Find the package name and directory by searching
<span> </span> (https://www.freshports.org/) and checking the “To
install the port:” line. For example, the plugin uses the directory and
package name .

Now edit . Add an entry for the new plugin that includes these fields:

-   Add the name of the newly created file here.

-   Use the same name used within the file.

-   Most plugins will have a specific icon. Search the web and save the
    icon to the directory as a . The naming convention is . For example,
    the plugin has the icon file .

-   Describe the plugin in a single sentence.

-   Specify if the plugin is supported by iXsystems. Enter .

See the <span> </span>
(https://github.com/ixpluginhub/iocagepluginindex/blob/master/INDEX) for
examples of entries.

Open a pull request for the <span> </span>
(https://github.com/ixpluginhub/iocagepluginindex). Make sure the pull
request contains:

-   the new file.

-   the plugin icon added to the directory.

-   an update to the file with an entry for the new plugin.

-   a link to the artifact repository populated with all required
    plugin files.

### Test a Plugin {#\detokenize{plugins:test-a-plugin}}

\[\]

<span>warning</span><span>Warning:</span> Installing experimental
plugins is not recommended for general use of FreeNAS$^{\text{®}}$. This
feature is meant to help plugin creators test their work before it
becomes generally available on FreeNAS$^{\text{®}}$.

Plugin pull requests are merged into the branch of the <span> </span>
(https://github.com/ixpluginhub/iocagepluginindex) repository. These
plugins are not available in the web interface until they are tested and
added to a branch of the repository. It is possible to test an
indevelopment plugin by using this command:

This will install the plugin, configure it with any chosen properties,
and specifically use the branch of the repository to download the
plugin.

Here is an example of downloading and configuring an experimental plugin
with the FreeNAS$^{\text{®}}$ :

\[commandchars=\
{}\] \[root@freenas \] iocage fetch P name mineos
ip4addr=em0|10.231.1.37/24 branch master Plugin: mineos Official Plugin:
False Using RELEASE: 11.2RELEASE Using Branch: master Postinstall
Artifact: https://github.com/jseqaert/iocagepluginmineos.git These pkgs
will be installed: ...

... Running postinstall.sh Command output: ...

... Admin Portal: http://10.231.1.37:8443 \[root@freenas \]

This plugin appears in the and screens as and can be tested with the
FreeNAS$^{\text{®}}$ system.

Asigra Plugin {#\detokenize{plugins:asigra-plugin}}
-------------

\[\]\[\] The Asigra plugin connects FreeNAS$^{\text{®}}$ to a third
party service and is subject to licensing. Please read the <span>
</span> (https://www.asigra.com/legal/softwarelicenseagreement) before
using this plugin.

To begin using Asigra services after installing the plugin, open the
plugin options and click . A new browser tab opens to <span> </span>
(https://licenseportal.asigra.com/licenseportal/userregistration.do).

The FreeNAS$^{\text{®}}$ system must have a public static IP address for
Asigra services to function.

Refer to the Asigra documentation for details about using the Asigra
platform:

-   <span>
    ttps://s3.amazonaws.com/asigra-documentation/Help/v14.1/DS-System%20Help/index.html</span><span>DSOperator
    Management Guide</span>
    (https://s3.amazonaws.com/asigradocumentation/Help/v14.1/DSSystem%20Help/index.html):
    Using the interface to manage the plugin service. Click in the
    plugin options to open the interface.

-   <span>
    ttps://s3.amazonaws.com/asigra-documentation/Guides/Cloud%20Backup/v14.1/Client\_Software\_Installation\_Guide.pdf</span><span>DSClient
    Installation Guide</span>
    (https://s3.amazonaws.com/asigradocumentation/Guides/Cloud%20Backup/v14.1/Client\_Software\_Installation\_Guide.pdf):
    How to install the system. aggregates backup content from endpoints
    and transmits it to the .

-   <span>
    ttps://s3.amazonaws.com/asigra-documentation/Help/v14.1/DS-Client%20Help/index.html</span><span>DSClient
    Management Guide</span>
    (https://s3.amazonaws.com/asigradocumentation/Help/v14.1/DSClient%20Help/index.html):
    Managing the system after it has been successfully installed at one
    or more locations.

Jails {#\detokenize{jails:jails}}
=====

\[\]\[\]\[\] Jails are a lightweight, operatingsystemlevel
virtualization. One or multiple services can run in a jail, isolating
those services from the host FreeNAS$^{\text{®}}$ system.
FreeNAS$^{\text{®}}$ uses <span> </span>
(https://github.com/iocage/iocage) for jail and () management. The main
differences between a usercreated jail and a plugin are that plugins are
preconfigured and usually provide only a single service.

By default, jails run the <span> </span> (https://www.freebsd.org/)
operating system. These jails are independent instances of FreeBSD. The
jail uses the host hardware and runs on the host kernel, avoiding most
of the overhead usually associated with virtualization. The jail
installs FreeBSD software management utilities so FreeBSD packages or
ports can be installed from the jail command line. This allows for
FreeBSD ports to be compiled and FreeBSD packages to be installed from
the command line of the jail.

It is important to understand that users, groups, installed software,
and configurations within a jail are isolated from both the
FreeNAS$^{\text{®}}$ host operating system and any other jails running
on that system.

The ability to create multiple jails offers flexibility regarding
software management. For example, an administrator can choose to provide
application separation by installing different applications in each
jail, to create one jail for all installed applications, or to mix and
match how software is installed into each jail.

Jail Storage {#\detokenize{jails:jail-storage}}
------------

\[\]\[\] A () must be created before using jails or (). Make sure the
pool has enough storage for all the intended jails and plugins. The
screen displays a message and button to if no pools exist on the
FreeNAS$^{\text{®}}$ system.

If pools exist, but none have been chosen for use with jails or plugins,
a dialog appears to choose a pool. Select a pool and click .

To select a different pool for jail and plugin storage, click
(Settings). A dialog shows the active pool. A different pool can be
selected from the dropdown.

Jails and downloaded FreeBSD release files are stored in a dataset named
.

Notes about the dataset:

-   At least 10 GiB of free space is recommended.

-   Cannot be located on a ().

-   <span> </span> (http://iocage.readthedocs.io/en/latest/index.html)
    automatically uses the first pool that is not a root pool for the
    FreeNAS$^{\text{®}}$ system.

-   A file contains default settings used when a new jail is created.
    The file is created automatically if not already present. If the
    file is present but corrupted, shows a warning and uses default
    settings from memory.

-   Each new jail installs into a new child dataset of . For example,
    with the dataset in , a new jail called installs into a new dataset
    named .

-   FreeBSD releases are fetched as a child dataset into the dataset.
    This datset is then extracted into the dataset to be used in
    jail creation. The dataset in can then be removed without affecting
    the availability of fetched releases or an existing jail.

-   datasets on activated pools are independent of each other and do
    share any data.

<span>note</span><span>Note:</span> iocage jail configs are stored in .
When iocage is updated, the configuration file is backed up as . The
backup file can be renamed to to restore previous jail settings.

Creating Jails {#\detokenize{jails:creating-jails}}
--------------

\[\]\[\] FreeNAS$^{\text{®}}$ has two options to create a jail. The
makes it easy to quickly create a jail. is an alternate method, where
every possible jail option is configurable. There are numerous options
spread across four different primary sections. This form is recommended
for advanced users with very specific requirements for a jail.

### Jail Wizard {#\detokenize{jails:jail-wizard}}

\[\]\[\] New jails can be created quickly by going to . This opens the
wizard screen shown in .

\[\]

The wizard provides the simplest process to create and configure a new
jail.

Enter a . Names can contain letters, numbers, periods (), dashes (), and
underscores ().

Choose a : or . Clone jails are clones of the specified FreeBSD RELEASE.
They are linked to that RELEASE, even if they are upgraded. Basejails
mount the specified RELEASE directories as nullfs mounts over the jail
directories. Basejails are not linked to the original RELEASE when
upgraded.

Jails can run FreeBSD versions up to the same version as the host
FreeNAS$^{\text{®}}$ system. Newer releases are not shown.

<span>tip</span><span>Tip:</span> Versions of FreeBSD are downloaded the
first time they are used in a jail. Additional jails created with the
same version of FreeBSD are created faster because the download has
already been completed.

Click to see a simplified list of networking options.

\[\] Jails support several different networking solutions:

-   can be set to add a virtual network interface to the jail. This
    interface can be used to set NAT, DHCP, or static jail
    network configurations. Since provides the jail with an independent
    networking stack, it can broadcast an IP address, which is required
    by some applications.

-   The jail can use <span> </span>
    (https://en.wikipedia.org/wiki/Network\_address\_translation), which
    uses the FreeNAS$^{\text{®}}$ IP address and sets a unique port for
    the jail to use. is required when is selected.

-   Configure the jail to receive its IP address from a DHCP server by
    setting .

-   Networking can be manually configured by entering values for the
    or fields. Any combination of these fields can be configured.
    Multiple interfaces are supported for IPv4 and IPv6 addresses. To
    add more interfaces and addresses, click . Setting the and fields to
    automatically configures these values. must be set to enable
    the field. If no interface is selected when manually configuring IP
    addresses, FreeNAS$^{\text{®}}$ automatically assigns the given IP
    address of the jail to the current active interface of the
    host system.

-   Leaving all checkboxes unset and fields empty initializes the jail
    without any networking abilities. Networking can be added to the
    jail after creation by going to (Expand) .

Setting a proxy in the FreeNAS$^{\text{®}}$ () also configures new jails
to use the proxy settings, except when performing DNS lookups. Make sure
a firewall is properly configured to maximize system security.

When pairing the jail with a physical interface, edit the () and set .
This prevents a network interface reset when the jail starts.

\[\]

Click to view a summary screen of the chosen jail options. Click to
create the new jail. After a few moments, the new jail is added to the
primary jails list.

### Advanced Jail Creation {#\detokenize{jails:advanced-jail-creation}}

\[\]\[\] The advanced jail creation form is opened by clicking then .
The screen in is shown.

\[\]

A usable jail can be quickly created by setting only the required
values, the and . Additional settings are in the , , and sections. shows
the available options of the of a new jail.

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.15-2</span>
|&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

Name & string & Required. Can contain letters, numbers, periods (),
dashes (), and underscores ().\
Jail Type & dropdown & are clones of the specified RELEASE. They are
linked to that RELEASE, even if they are upgraded. mount the specified
RELEASE directories as nullfs mounts over the jail directories.
Basejails are not linked to the original RELEASE when upgraded.\
Release & dropdown menu & Required. Jails can run FreeBSD versions up to
the same version as the host FreeNAS$^{\text{®}}$ system. Newer releases
are not shown.\
DHCP Autoconfigure IPv4 & checkbox & Automatically configure IPv4
networking with an independent VNET stack. and must also be checked. If
not set, ensure the defined address in does not conflict with an
existing address.\
NAT & checkbox & Network Address Translation (NAT). When set, the jail
is given an internal IP address and connections are forwarded from the
host to the jail. When NAT is set, cannot be set. Adds the options to
the jail ().\
VNET & checkbox & Use VNET to emulate network devices for this jail and
a create a fully virtualized perjail network stack. See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=vnet) for more details.\
Berkeley Packet Filter & checkbox & Use the Berkeley Packet Filter to
data link layers in a protocol independent fashion. Unset by default to
avoid security vulnerabilities. See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=bpf) for more details. Cannot
be set when is set.\
vnet\_default\_interface & dropdown & Set the default VNET interface.
Only takes effect when is set. Choose a specific interface, or set to to
use the interface that has the default route. Choose to not set a
default VNET interface.\
IPv4 Interface & dropdown menu & Choose a network interface to use for
this IPv4 connection. See () to add more.\
IPv4 Address & string & This and the other IPv4 settings are grayed out
if is set. Configures the interface to use for network or internet
access for the jail.

Enter an IPv4 address for this IP jail. Example: . See () to add more.\
IPv4 Netmask & dropdown menu & Choose a subnet mask for this IPv4
Address.\
IPv4 Default Router & string & Type or a valid IP address. Setting this
property to anything other than configures a default route inside a VNET
jail.\
Auto Configure IPv6 & checkbox & Set to use SLAAC (Stateless Address
Auto Configuration) to autoconfigure IPv6 in the jail.\
IPv6 Interface & dropdown menu & Choose a network interface to use for
this IPv6 connection. See () to add more.\
IPv6 Address & string & Configures network or internet access for the
jail.

Type the IPv6 address for VNET and shared IP jails. Example: . See () to
add more.\
IPv6 Prefix & dropdown menu & Choose a prefix for this IPv6 Address.\
IPv6 Default Router & string & Type or a valid IP address. Setting this
property to anything other than configures a default route inside a VNET
jail.\
Notes & string & Enter any notes or comments about the jail.\
Autostart & checkbox & Start the jail at system startup.\

<span>note</span><span>Note:</span> For static configurations not using
DHCP or NAT, multiple IPv4 and IPv6 addresses and interfaces can be
added to the jail by clicking .

Similar to the (), configuring the basic properties, then clicking is
often all that is needed to quickly create a new jail. To continue
configuring more settings, click to proceed to the section of the form.
describes each of these options.

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.15-2</span>
|&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

devfs\_ruleset & integer & Number of the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=devfs) ruleset to enforce
when mounting in the jail. The default value of means no ruleset is
enforced. Mounting inside a jail is only possible when the and
permissions are enabled and is set to a value lower than .\
exec.start & string & Commands to run in the jail environment when a
jail is created. Example: . See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=jail) for more details.\
exec.stop & string & Commands to run in the jail environment before a
jail is removed and after any commands are complete. Example: .\
exec\_prestart & string & Commands to run in the system environment
before a jail is started.\
exec\_poststart & string & Commands to run in the system environment
after a jail is started and after any commands are finished.\
exec\_prestop & string & Commands to run in the system environment
before a jail is stopped.\
exec\_poststop & string & Commands to run in the system environment
after a jail is started and after any commands are finished.\
exec\_clean & checkbox & Run commands in a clean environment. The
current environment is discarded except for \$HOME, \$SHELL, \$TERM and
\$USER.

\$HOME and \$SHELL are set to the target login. \$USER is set to the
target login. \$TERM is imported from the current environment. The
environment variables from the login class capability database for the
target login are also set.\
exec\_timeout & integer & The maximum amount of time in seconds to wait
for a command to complete. If a command is still running after the
allotted time, the jail is terminated.\
stop\_timeout & integer & The maximum amount of time in seconds to wait
for the jail processes to exit after sending a SIGTERM signal. This
happens after any commands are complete. After the specified time, the
jail is removed, killing any remaining processes. If set to , no SIGTERM
is sent and the jail is immeadility removed.\
exec\_jail\_user & string & Enter either or a valid . Inside the jail,
commands run as this user.\
exec\_system\_jail\_user & string & Set to to look for the in the system
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=passwd) file
of the jail .\
exec\_system\_user & string & Run commands in the jail as this user. By
default, commands are run as the current user.\
mount\_devfs & checkbox & Mount a <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=devfs) filesystem on the
chrooted directory and apply the ruleset in the parameter to restrict
the devices visible inside the jail.\
mount\_fdescfs & checkbox & Mount an <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=fdescfs) filesystem in the
jail directory.\
enforce\_statfs & dropdown & Determine which information processes in a
jail are able to obtain about mount points. The behavior of multiple
syscalls is affected: <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=statfs), <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=statfs), <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=getfsstat), <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=fhstatfs), and other similar
compatibility syscalls.

All mount points are available without any restrictions if this is set
to . Only mount points below the jail chroot directory are available if
this is set to . Set to , the default option only mount points where the
jail chroot directory is located are available.\
children\_max & integer & Number of child jails allowed to be created by
the jail or other jails under this jail. A limit of restricts the jail
from creating child jails. in the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=jail) man page explains the
finer details.\
login\_flags & string & Flags to pass to <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=login) when logging in to the
jail using the function.\
securelevel & integer & Value of the jail <span> </span>
(https://www.freebsd.org/doc/faq/security.html) sysctl. A jail never has
a lower securelevel than the host system. Setting this parameter allows
a higher securelevel. If the host system securelevel is changed, jail
securelevel will be at least as secure. Securelevel options are: , , , ,
and .\
sysvmsg & dropdown & Allow or deny access to SYSV IPC message
primitives. Set to : All IPC objects on the system are visible to the
jail. Set to : Only objects the jail created using the private key
namespace are visible. The system and parent jails have access to the
jail objects but not private keys. Set to : The jail cannot perform any
sysvmsg related system calls.\
sysvsem & dropdown & Allow or deny access to SYSV IPC semaphore
primitives. Set to : All IPC objects on the system are visible to the
jail. Set to : Only objects the jail creates using the private key
namespace are visible. The system and parent jails have access to the
jail objects but not private keys. Set to : The jail cannot perform any
related system calls.\
sysvshm & dropdown & Allow or deny access to SYSV IPC shared memory
primitives. Set to : All IPC objects on the system are visible to the
jail. Set to : Only objects the jail creates using the private key
namespace are visible. The system and parent jails have access to the
jail objects but not private keys. Set to : The jail cannot perform any
sysvshm related system calls.\
allow\_set\_hostname & checkbox & Allow the jail hostname to be changed
with <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=hostname)
or <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=sethostname).\
allow\_sysvipc & checkbox & Choose whether a process in the jail has
access to System V IPC primitives. Equivalent to setting , , and to .

Use , ,and instead.\
allow\_raw\_sockets & checkbox & Allow the jail to use <span> </span>
(https://en.wikipedia.org/wiki/Network\_socket\#Raw\_socket). When set,
the jail has access to lowerlevel network layers. This allows utilities
like <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=ping) and
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=traceroute) to
work in the jail, but has security implications and should only be used
on jails running trusted software.\
allow\_chflags & checkbox & Treat jail users as privileged and allow the
manipulation of system file flags. constraints are still enforced.\
allow\_mlock & checkbox & Allow jail to run services that use <span>
</span> (https://www.freebsd.org/cgi/man.cgi?query=mlock) to lock
physical pages in memory.\
allow\_mount & checkbox & Allow privileged users inside the jail to
mount and unmount filesystem types marked as jailfriendly.\
allow\_mount\_devfs & checkbox & Allow privileged users inside the jail
to mount and unmount the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=devfs). This permission is
only effective when is set and is set to a value lower than .\
allout\_mount\_fusefs & checkbox & Allow privileged users inside the
jail to mount and unmount fusefs. The jail must have FreeBSD 12.0 or
newer installed. This permission is only effective when is set and is
set to a value lower than 2.\
allow\_mount\_nullfs & checkbox & Allow privileged users inside the jail
to mount and unmount the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=nullfs). This permission is
only effective when is set and is set to a value lower than .\
allow\_mount\_procfs & checkbox & Allow privileged users inside the jail
to mount and unmount the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=procfs). This permission is
only effective when is set and is set to a value lower than .\
allow\_mount\_tmpfs & checkbox & Allow privileged users inside the jail
to mount and unmount the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=tmpfs). This permission is
only effective when is set and is set to a value lower than .\
allow\_mount\_zfs & checkbox & Allow privileged users inside the jail to
mount and unmount the ZFS file system. This permission is only effective
when is set and is set to a value lower than . The <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=zfs) man page has information
on how to configure the ZFS filesystem to operate from within a jail.\
allow\_vmm & checkbox & Grants the jail access to the Bhyve Virtual
Machine Monitor (VMM). The jail must have FreeBSD 12.0 or newer
installed with the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=vmm) kernel module loaded.\
allow\_quotas & checkbox & Allow the jail root to administer quotas on
the jail filesystems. This includes filesystems the jail shares with
other jails or with nonjailed parts of the system.\
allow\_socket\_af & checkbox & Allow access to other protocol stacks
beyond IPv4, IPv6, local (UNIX), and route. : jail functionality does
not exist for all protocal stacks.\
vnet\_interfaces & string & Spacedelimited list of network interfaces to
attach to a VNETenabled jail after it is created. Interfaces are
automatically released when the jail is removed.\

Click to view all jail . These are shown in :

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.15-2</span>
|&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

interfaces & string & Enter up to four interface configurations in the
format , separated by a comma (). The left value is the virtual VNET
interface name and the right value is the bridge name where the virtual
interface is attached.\
host\_domainname & string & Enter an <span> </span>
(https://www.freebsd.org/doc/handbook/networknis.html) for the jail.\
host\_hostname & string & Enter a hostname for the jail. By default, the
system uses the jail NAME/UUID.\
exec\_fib & integer & Enter a number to define the routing table (FIB)
to set when running commands inside the jail.\
ip4.saddrsel & checkbox & Disables IPv4 source address selection for the
jail in favor of the primary IPv4 address of the jail. Only available
when the jail is not configured to use VNET.\
ip4 & dropdown & Control the availability of IPv4 addresses. Set to :
allow unrestricted access to all system addresses. Set to : restrict
addresses with . Set to : stop the jail from using IPv4 entirely.\
ip6.saddrsel & string & Disable IPv6 source address selection for the
jail in favor of the primary IPv6 address of the jail. Only available
when the jail is not configured to use VNET.\
ip6 & dropdown & Control the availability of IPv6 addresses. Set to :
allow unrestricted access to all system addresses. Set to : restrict
addresses with . Set to : stop the jail from using IPv6 entirely.\
resolver & string & Add lines to in file. Example: . Fields must be
delimited with a semicolon (), this is translated as new lines in .
Enter to inherit from the host.\
mac\_prefix & string & Optional. Enter a valid MAC address vendor
prefix. Example:\
vnet0\_mac & string & Leave this blank to generate random MAC addresses
for the host and jail. To assign fixed MAC addresses, enter the host MAC
address and the jail MAC address separated by a space.\
vnet1\_mac & string & Leave this blank to generate random MAC addresses
for the host and jail. To assign fixed MAC addresses, enter the host MAC
address and the jail MAC address separated by a space.\
vnet2\_mac & string & Leave this blank to generate random MAC addresses
for the host and jail. To assign fixed MAC addresses, enter the host MAC
address and the jail MAC address separated by a space.\
vnet3\_mac & string & Leave this blank to generate random MAC addresses
for the host and jail. To assign fixed MAC addresses, enter the host MAC
address and the jail MAC address separated by a space.\

The final set of jail properties are contained in the section. describes
these options.

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.15-2</span>
|&gt;p<span>0.60-2</span>|</span>

\[\]\
Setting & Value & Description\

\
Setting & Value & Description\

\

owner & string & The owner of the jail. Can be any string.\
priority & integer & The numeric start priority for the jail at boot
time. values mean a priority. At system shutdown, the priority is .
Example: 99\
hostid & string & A new a jail hostid, if necessary. Example hostid: .\
hostid\_strict\_check & checkbox & Check the jail property. Prevents the
jail from starting if the does not match the host.\
comment & string & Comments about the jail.\
depends & string & Specify any jails the jail depends on. Child jails
must already exist before the parent jail can be created.\
mount\_procfs & checkbox & Allow mounting of a <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=procfs) filesystems in the
jail directory.\
mount\_linprocfs & checkbox & Allow mounting of a <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=linprocfs) filesystem in the
jail.\
template & checkbox & Convert the jail into a template. Template jails
can be used to quickly create jails with the same configuration.\
host\_time & checkbox & Synchronize the time between jail and host.\
jail\_zfs & checkbox & Enable automatic ZFS jailing inside the jail. The
assigned ZFS dataset is fully controlled by the jail.

Note: , , and must all be set for ZFS management inside the jail to work
correctly.\
jail\_zfs\_dataset & string & Define the dataset to be jailed and fully
handed over to a jail. Enter a ZFS filesystem name without a pool name.
must be set for this option to work.\
jail\_zfs\_mountpoint & string & The mountpoint for the . Example:\
allow\_tun & checkbox & Expose host <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=tun) devices in the jail.
Allow the jail to create tun devices.\
Autoconfigure IPv6 with rtsold & checkbox & Use <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=rtsold) as part of IPv6
autoconfiguration. Send ICMPv6 Router Solicitation messages to
interfaces to discover new routers.\
ip\_hostname & checkbox & Use DNS records during jail IP configuration
to search the resolver and apply the first open IPv4 and IPv6 addresses.
See <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=jail).\
assign\_localhost & checkbox & Add network interface to the jail and
assign it the first available localhost address, starting with . cannot
be set. Jails using configure a localhost as part of their virtualized
network stack.\

Click when the desired jail properties have been set. New jails are
added to the primary list in the menu.

#### Creating Template Jails {#\detokenize{jails:creating-template-jails}}

\[\]\[\] Template jails are basejails that can be used as a template to
efficiently create jails with the same configuration. These steps create
a template jail:

1.  Go to .

2.  Select as the . Configure the jail with desired options.

3.  Set in the tab.

4.  Click .

5.  Click .

6.  Enter a name for the template jail. Leave as . Set to , where is the
    name of the base jail created earlier.

7.  Complete the jail creation wizard.

Managing Jails {#\detokenize{jails:managing-jails}}
--------------

\[\]\[\] Clicking shows a list of installed jails. An example is shown
in .

\[\]

Operations can be applied to multiple jails by selecting those jails
with the checkboxes on the left. After selecting one or more jails,
icons appear which can be used to (Start), (Stop), (Update), or (Delete)
those jails.

More information such as , , of jail, and whether it is a jail or can be
shown by clicking (Expand). Additional options for that jail are also
displayed. These are described in .

shows the menu that appears.

\[\]

<span>warning</span><span>Warning:</span> Modify the IP address
information for a jail by clicking (Expand) instead of issuing the
networking commands directly from the command line of the jail. This
ensures the changes are saved and will survive a jail or
FreeNAS$^{\text{®}}$ reboot.

<span>|&gt;p<span>0.25-2</span> |&gt;p<span>0.75-2</span>|</span>

\[\]\
Option & Description\

\
Option & Description\

\

EDIT & Used to modify the settings described in (). A jail cannot be
edited while it is running. The settings can be viewed, but are read
only.\
MOUNT POINTS & Select an existing mount point to or click to create a
mount point for the jail. A mount point gives a jail access to storage
located elsewhere on the system. A jail must be stopped before adding,
editing, or deleting a mount point. See () for more details.\
RESTART & Stop and immediately start an jail.\
START & Start a jail that has a current of .\
STOP & Stop a jail that has a current of .\
UPDATE & Runs <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=freebsdupdate) to update the
jail to the latest patch level of the installed FreeBSD release.\
SHELL & Access a command prompt to interact with a jail directly from
the command line. Type to leave the command prompt.\
DELETE & Caution: deleting the jail also deletes all of the jail
contents and all associated (). Back up the jail data, configuration,
and programs first. There is no way to recover the contents of a jail
after deletion!\

<span>note</span><span>Note:</span> Menu entries change depending on the
jail state. For example, a stopped jail does not have a or option.

Jail status messages and command output are stored in .

### Jail Updates and Upgrades {#\detokenize{jails:jail-updates-and-upgrades}}

\[\]\[\] Click (Expand) to update a jail to the most current patch level
of the installed FreeBSD release. This does change the release. For
example, a jail installed with can update to or the latest patch of
11.2, but not an 11.3RELEASEp\# version of FreeBSD.

A jail replaces the jail FreeBSD operating system with a new release of
FreeBSD, such as taking a jail from FreeBSD 11.2RELEASE to 11.3RELEASE.
Upgrade a jail by stopping it, opening the () and entering , where is
the plugin jail name and is the desired release to upgrade to.

<span>tip</span><span>Tip:</span> It is possible to () unused releases
from the dataset after upgrading a jail. The release not be in use by
any jail on the system!

### Accessing a Jail Using SSH {#\detokenize{jails:accessing-a-jail-using-ssh}}

\[\]\[\] The ssh daemon <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=sshd) must be enabled in a
jail to allow SSH access to that jail from another system.

The jail must be before the option is available. If the jail is not up,
start it by clicking (Expand) for the desired jail. Click (Expand) to
open a shell in the jail. A jail root shell is shown in this example:

\[commandchars=\
{}\] Last login: Fri Apr 6 07:57:04 on pts/12 FreeBSD 11.1STABLE
(FreeNAS.amd64) 0 0ale9f753(freenas/11stable): FriApr 6 04:46:31 UTC
2018

Welcome to FreeBSD!

Release Notes, Errata: https://www.FreeBSD.org/releases/ Security
Advisories: https://www.FreeBSD.org/security/ FreeBSD Handbook:
https://www.FreeBSD.org/handbook/ FreeBSD FAQ:
https://www.FreeBSD.org/faq/ Questions List:
https://lists.FreeBSD.org/mailman/listinfo/freebsdquestions/ FreeBSD
Forums: https://forums.FreeBSD.org/

Documents installed with the system are in the
/usr/local/share/doc/freebsd/ directory, or can be installed later with:
pkg install enfreebsddoc For other languages, replace en with a language
code like de or fr.

Show the version of FreeBSD installed: freebsdversion ; uname a Please
include that output and any error messages when posting questions.
Introduction to manual pages: man man FreeBSD directory layout: man hier

Edit /etc/motd to change this login announcement. root@jailexamp:

<span>tip</span><span>Tip:</span> A root shell can also be opened for a
jail using the FreeNAS$^{\text{®}}$ UI . Open the , then type .

Enable sshd:

\[commandchars=\
{}\] sysrc sshdenable=YES sshdenable: NO YES

<span>tip</span><span>Tip:</span> Using to enable sshd verifies that
sshd is enabled.

Start the SSH daemon:

The first time the service runs, the jail RSA key pair is generated and
the key fingerprint is displayed.

Add a user account with . Follow the prompts, will accept the default
value offered. Users that require access must also be a member of the
group. Enter when prompted to

\[commandchars=\
{}\] root@jailexamp: adduser Username: jailuser Full name: Jail User Uid
(Leave empty for default): Login group \[jailuser\]: Login group is
jailuser. Invite jailuser into other groups? \[\]: wheel Login class
\[default\]: Shell (sh csh tcsh gitshell zsh rzsh nologin) \[sh\]: csh
Home directory \[/home/jailuser\]: Home directory permissions (Leave
empty for default): Use passwordbased authentication? \[yes\]: Use an
empty password? (yes/no) \[no\]: Use a random password? (yes/no) \[no\]:
Enter password: Enter password again: Lock out the account after
creation? \[no\]: Username : jailuser Password : \*\*\*\*\* Full Name :
Jail User Uid : 1002 Class : Groups : jailuser wheel Home :
/home/jailuser Home Mode : Shell : /bin/csh Locked : no OK? (yes/no):
yes adduser: INFO: Successfully added (jailuser) to the user database.
Add another user? (yes/no): no Goodbye! root@jailexamp:

After creating the user, set the jail password to allow users to use to
gain superuser privileges. To set the jail password, use . Nothing is
echoed back when using

\[commandchars=\
{}\] root@jailexamp: passwd Changing local password for root New
Password: Retype New Password: root@jailexamp:

Finally, test that the user can successfully into the jail from another
system and gain superuser privileges. In the example, a user named uses
to access the jail at 192.168.2.3. The host RSA key fingerprint must be
verified the first time a user logs in.

\[commandchars=\
{}\] ssh jailuser@192.168.2.3 The authenticity of host 192.168.2.3
(192.168.2.3) cant be established. RSA key fingerprint is
6f:93:e5:36:4f:54:ed:4b:9c:c8:c2:71:89:c1:58:f0. Are you sure you want
to continue connecting (yes/no)? yes Warning: Permanently added
192.168.2.3 (RSA) to the list of known hosts. Password:

<span>note</span><span>Note:</span> Every jail has its own user accounts
and service configuration. These steps must be repeated for each jail
that requires SSH access.

### Additional Storage {#\detokenize{jails:additional-storage}}

\[\]\[\] Jails can be given access to an area of storage outside of the
jail that is configured on the FreeNAS$^{\text{®}}$ system. It is
possible to give a FreeBSD jail access to an area of storage on the
FreeNAS$^{\text{®}}$ system. This is useful for applications or plugins
that store large amounts of data or if an application in a jail needs
access to data stored on the FreeNAS$^{\text{®}}$ system. For example,
Transmission is a plugin that stores data using BitTorrent. The
FreeNAS$^{\text{®}}$ external storage is added using the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=mount\_nullfs) mechanism,
which links data that resides outside of the jail as a storage area
within a jail.

(Expand) shows any added storage and allows adding more storage.

<span>note</span><span>Note:</span> A jail must have a of before adding
a new mount point. Click (Expand) and for a jail to change the jail to .

Storage can be added by clicking (Expand) for the desired jail. The
section is a list of all of the currently defined mount points.

Go to to add storage to a jail. This opens the screen shown in .

\[\]

to the and , where:

-   : is the directory or dataset on the FreeNAS$^{\text{®}}$ system
    which will be accessed by the jail. FreeNAS$^{\text{®}}$ creates the
    directory if it does not exist. This directory must reside outside
    of the pool or dataset being used by the jail. This is why it is
    recommended to create a separate dataset to store jails, so the
    dataset holding the jails is always separate from any datasets used
    for storage on the FreeNAS$^{\text{®}}$ system.

-   : Browse to an existing and directory within the jail to link to the
    storage area. It is also possible to add and a name to the end of
    the path and FreeNAS$^{\text{®}}$ automatically creates a
    new directory. New directories created must be the jail
    directory structure. Example: .

Storage is typically added because the user and group account associated
with an application installed inside of a jail needs to access data
stored on the FreeNAS$^{\text{®}}$ system. Before selecting the , it is
important to first ensure that the permissions of the selected directory
or dataset grant permission to the user/group account inside of the
jail. This is not the default, as the users and groups created inside of
a jail are totally separate from the users and groups of the
FreeNAS$^{\text{®}}$ system.

The workflow for adding storage usually goes like this:

1.  Determine the name of the user and group account used by
    the application. For example, the installation of the transmission
    application automatically creates a user account named and a group
    account also named . When in doubt, check the files (to find the
    user account) and (to find the group account) inside the jail.
    Typically, the user and group names are similar to the
    application name. Also, the UID and GID are usually the same as the
    port number used by the service.

    A user and group (GID 8675309) are part of the base system. Having
    applications run as this group or user makes it possible to share
    storage between multiple applications in a single jail, between
    multiple jails, or even between the host and jails.

2.  On the FreeNAS$^{\text{®}}$ system, create a user account and group
    account that match the user and group names used by the application
    in the jail.

3.  Decide whether the jail will be given access to existing data or a
    new storage area will be allocated.

4.  If the jail accesses existing data, edit the permissions of the pool
    or dataset so the user and group accounts have the desired read and
    write access. If multiple applications or jails are to have access
    to the same data, create a new group and add each needed user
    account to that group.

5.  If an area of storage is being set aside for that jail or individual
    application, create a dataset. Edit the permissions of that dataset
    so the user and group account has the desired read and write access.

6.  Use the jail (Expand) to select the of the data and the where it
    will be mounted in the jail.

To prevent writes to the storage, click .

After storage has been added or created, it appears in the for that
jail. In the example shown in , a dataset named has been chosen as the
as it contains the files stored on the FreeNAS$^{\text{®}}$ system. The
user entered as the directory to be mounted in the field. To users
inside the jail, this data appears in the directory.

\[\]

Storage is automatically mounted as it is created.

<span>note</span><span>Note:</span> Mounting a dataset does not
automatically mount any child datasets inside it. Each dataset is a
separate filesystem, so child datasets must each have separate mount
points.

Click (Options) to delete the storage.

<span>warning</span><span>Warning:</span> Remember that added storage is
just a pointer to the selected storage directory on the
FreeNAS$^{\text{®}}$ system. It does copy that data to the jail.
FreeNAS$^{\text{®}}$ However, removing the jail storage entry only
removes the pointer. This leaves the data intact but not accessible from
the jail.

Jail Software {#\detokenize{jails:jail-software}}
-------------

\[\] A jail is created with no software aside from the core packages
installed as part of the selected version of FreeBSD. To install more
software, start the jail and click SHELL.

### Installing FreeBSD Packages {#\detokenize{jails:installing-freebsd-packages}}

\[\] The quickest and easiest way to install software inside the jail is
to install a FreeBSD package. FreeBSD packages are precompiled and
contain all the binaries and a list of dependencies required for the
software to run on a FreeBSD system.

A huge amount of software has been ported to FreeBSD. Most of that
software is available as packages. One way to find FreeBSD software is
to use the search bar at <span> </span> (https://www.freshports.org/).

After finding the name of the desired package, use the command to
install it. For example, to install the audiotag package, use the
command

When prompted, press to complete the installation. Messages will show
the download and installation status.

A successful installation can be confirmed by querying the package
database:

\[commandchars=\
{}\] pkg info f audiotag audiotag0.191 Name: audiotag Version: 0.191
Installed on: Fri Nov 21 10:10:34 PST 2014 Origin: audio/audiotag
Architecture: freebsd:9:x86:64 Prefix: /usr/local Categories: multimedia
audio Licenses: GPLv2 Maintainer: ports@FreeBSD.org WWW:
http://github.com/Daenyth/audiotag Comment: Commandline tool for mass
tagging/renaming of audio files Options: DOCS: on FLAC: on ID3: on MP4:
on VORBIS: on Annotations: repotype: binary repository: FreeBSD Flat
size: 62.8KiB Description: Audiotag is a commandline tool for mass
tagging/renaming of audio files it supports the vorbis comment, id3
tags, and MP4 tags. WWW: http://github.com/Daenyth/audiotag

To show what was installed by the package:

\[commandchars=\
{}\] pkg info l audiotag audiotag0.191: /usr/local/bin/audiotag
/usr/local/share/doc/audiotag/COPYING
/usr/local/share/doc/audiotag/ChangeLog
/usr/local/share/doc/audiotag/README
/usr/local/share/licenses/audiotag0.191/GPLv2
/usr/local/share/licenses/audiotag0.191/LICENSE
/usr/local/share/licenses/audiotag0.191/catalog.mk

In FreeBSD, thirdparty software is always stored in to differentiate it
from the software that came with the operating system. Binaries are
almost always located in a subdirectory called or and configuration
files in a subdirectory called .

### Compiling FreeBSD Ports {#\detokenize{jails:compiling-freebsd-ports}}

\[\] Compiling a port is another option. Compiling ports offer these
advantages:

-   Not every port has an available package. This is usually due to
    licensing restrictions or known, unaddressed
    security vulnerabilities.

-   Sometimes the package is outofdate and a feature is needed that only
    became available in the newer version.

-   Some ports provide compile options that are not available in the
    precompiled package. These options are used to add or remove
    features or options.

Compiling a port has these disadvantages:

-   It takes time. Depending upon the size of the application, the
    amount of dependencies, the speed of the CPU, the amount of RAM
    available, and the current load on the FreeNAS$^{\text{®}}$ system,
    the time needed can range from a few minutes to a few hours or even
    to a few days.

<span>note</span><span>Note:</span> If the port does not provide any
compile options, it saves time and preserves the FreeNAS$^{\text{®}}$
system resources to use the command instead.

The <span> </span> (https://www.freshports.org/) listing shows whether a
port has any configurable compile options. shows the for , a utility for
renaming multiple audio files.

\[\]

Packages are built with default options. Ports let the user select
options.

The Ports Collection must be installed in the jail before ports can be
compiled. Inside the jail, use the utility. This command downloads the
ports collection and extracts it to the directory of the jail:

\[commandchars=\
{}\] portsnap fetch extract

<span>note</span><span>Note:</span> To install additional software at a
later date, make sure the ports collection is updated with .

To compile a port, into a subdirectory of . The entry for the port at
FreshPorts provides the location to into and the command to run. This
example compiles and installs the audiotag port:

\[commandchars=\
{}\] cd /usr/ports/audio/audiotag make install clean

The first time this command is run, the configure screen shown in is
displayed:

\[\]

This port has several configurable options: , , , , and . Selected
options are shown with a .

Use the arrow keys to select an option and press to toggle the value.
Press when satisfied with the options. The port begins to compile and
install.

<span>note</span><span>Note:</span> After options have been set, the
configuration screen is normally not shown again. Use to display the
screen and change options before rebuilding the port with .

Many ports depend on other ports. Those other ports also have
configuration screens that are shown before compiling begins. It is a
good idea to watch the compile until it finishes and the command prompt
returns.

Installed ports are registered in the same package database that manages
packages. The can be used to determine which ports were installed.

### Starting Installed Software {#\detokenize{jails:starting-installed-software}}

\[\] After packages or ports are installed, they must be configured and
started. Configuration files are usually in or a subdirectory of it.
Many FreeBSD packages contain a sample configuration file as a
reference. Take some time to read the software documentation to learn
which configuration options are available and which configuration files
require editing.

Most FreeBSD packages that contain a startable service include a startup
script which is automatically installed to . After the configuration is
complete, test starting the service by running the script with the
option. For example, with openvpn installed in the jail, these commands
are run to verify that the service started:

\[commandchars=\
{}\] /usr/local/etc/rc.d/openvpn onestart Starting openvpn.

/usr/local/etc/rc.d/openvpn onestatus openvpn is running as pid 45560.

sockstat 4 USER COMMAND PID FD PROTO LOCAL ADDRESS FOREIGN ADDRESS root
openvpn 48386 4 udp4 \*:54789 \*:\*

If it produces an error:

\[commandchars=\
{}\] /usr/local/etc/rc.d/openvpn onestart Starting openvpn.
/usr/local/etc/rc.d/openvpn: WARNING: failed to start openvpn

Run to see any error messages if an issue is found. Most startup
failures are related to a misconfiguration in a configuration file.

After verifying that the service starts and is working as intended, add
a line to to start the service automatically when the jail is started.
The line to start a service always ends in and typically starts with the
name of the software. For example, this is the entry for the openvpn
service:

\[commandchars=\
{}\] openvpnenable=YES

When in doubt, the startup script shows the line to put in . This is the
description in :

\[commandchars=\
{}\] This script supports running multiple instances of openvpn. To run
additional instances link this script to something like ln s openvpn
openvpnfoo

and define additional openvpnfoo\* variables in one of /etc/rc.conf,
/etc/rc.conf.local or /etc/rc.conf.d /openvpnfoo

Below NAME should be substituted with the name of this script. By
default it is openvpn, so read as openvpnenable. If you linked the
script to openvpnfoo, then read as openvpnfooenable etc. The following
variables are supported (defaults are shown). You can place them in any
of /etc/rc.conf, /etc/rc.conf.local or /etc/rc.conf.d/NAME NAMEenable=NO
set to YES to enable openvpn

The startup script also indicates if any additional parameters are
available:

\[commandchars=\
{}\] NAMEif= driver(s) to load, set to tun, tap or tun tap it is OK to
specify the if prefix. optional: NAMEflags= additional command line
arguments NAMEconfigfile=/usr/local/etc/openvpn/NAME.conf config file
NAMEdir=/usr/local/etc/openvpn cd directory

Reporting {#\detokenize{reporting:reporting}}
=========

\[\]\[\]\[\] Reporting displays several graphs, as seen in . Choose a
category from the dropdown menu to view those graphs. There are also
options to change the graph view and number of graphs on each page.

\[\]

FreeNAS$^{\text{®}}$ uses <span> </span> (https://collectd.org/) to
provide reporting statistics. For a clearer picture, hover over a point
in the graph to show exact numbers for that point in time. Use the
magnifier buttons next to each graph to increase or decrease the
displayed time increment from 10 minutes, hourly, daily, weekly, or
monthly. The and buttons scroll through the output.

<span>note</span><span>Note:</span> Reporting graphs do not appear if
there is no related data.

Graphs are grouped by category on the Reporting page:

-   -   <span> </span> (https://collectd.org/wiki/index.php/Plugin:CPU)
        shows the amount of time spent by the CPU in various states such
        as executing user code, executing system code, and being idle.
        Graphs of short, mid, and longterm load are shown, along with
        CPU temperature graphs.

-   -   <span> </span> (https://collectd.org/wiki/index.php/Plugin:Disk)
        shows read and write statistics on I/O, percent busy, latency,
        operations per second, pending I/O requests, and disk
        temperature. Choose the and to view the selected metrics for the
        chosen devices.

    <span>note</span><span>Note:</span> Temperature monitoring for the
    disk is disabled if is enabled in ().

-   -   <span> </span>
        (https://collectd.org/wiki/index.php/Plugin:Memory) displays
        memory usage.

    -   <span> </span> (https://collectd.org/wiki/index.php/Plugin:Swap)
        displays the amount of free and used swap space.

-   -   <span> </span>
        (https://collectd.org/wiki/index.php/Plugin:Interface) shows
        received and transmitted traffic in megabytes per second for
        each configured interface.

-   -   <span> </span> (https://collectd.org/wiki/index.php/Plugin:NFS)
        shows information about the number of procedure calls for each
        procedure and whether the system is a server or client.

-   -   <span> </span> (https://collectd.org/wiki/index.php/Plugin:DF)
        displays free, used, and reserved space for each pool and
        dataset. However, the disk space used by an individual zvol is
        not displayed as it is a block device.

-   -   <span> </span>
        (https://collectd.org/wiki/index.php/Plugin:Processes) displays
        the number of processes. It is grouped by state.

-   -   Target shows bandwidth statistics for iSCSI ports.

-   -   <span> </span> (https://collectd.org/wiki/index.php/Plugin:NUT)
        displays statistics about an uninterruptible power supply (UPS)
        using <span> </span> (https://networkupstools.org/). Statistics
        include voltages, currents, power, frequencies, load,
        and temperatures.

-   -   <span> </span>
        (https://collectd.org/wiki/index.php/Plugin:ZFS\_ARC) shows
        compressed physical ARC size, hit ratio, demand data, demand
        metadata, and prefetch data.

Reporting data is saved to permit viewing and monitoring usage trends
over time. This data is preserved across system upgrades and restarts.

Data files are saved in .

<span>warning</span><span>Warning:</span> Reporting data is frequently
written and should not be stored on the boot pool or operating system
device.

Virtual Machines {#\detokenize{virtualmachines:virtual-machines}}
================

\[\]\[\]\[\] A Virtual Machine () is an environment on a host computer
that can be used as if it were a separate physical computer. VMs can be
used to run multiple operating systems simultaneously on a single
computer. Operating systems running inside a VM see emulated virtual
hardware rather than the actual hardware of the host computer. This
provides more isolation than (), although there is additional overhead.
A portion of system RAM is assigned to each VM, and each VM uses a ()
for storage. While a VM is running, these resources are not available to
the host computer or other VMs.

FreeNAS$^{\text{®}}$ VMs use the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=bhyve) virtual machine
software. This type of virtualization requires an Intel processor with
Extended Page Tables (EPT) or an AMD processor with Rapid Virtualization
Indexing (RVI) or Nested Page Tables (NPT). VMs cannot be created unless
the host system supports these features.

To verify that an Intel processor has the required features, use () to
run . If the and features are shown, this processor can be used with .

To verify that an AMD processor has the required features, use () to run
. If the output shows the POPCNT feature, this processor can be used
with .

<span>note</span><span>Note:</span> AMD K10 “Kuma” processors include
POPCNT but do not support NRIPS, which is required for use with bhyve.
Production of these processors ceased in 2012 or 2013.

By default, new VMs have the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=bhyve) option set. This
causes the virtual CPU thread to yield when a HLT instruction is
detected and prevents idle VMs from consuming all of the host CPU.

shows a list of installed virtual machines and available memory. The
available memory changes depending on what the system is doing,
including which virtual machines are running.

A log file for each VM is written to .

, , and are displayed on the page. Click (Expand) to view additional
options for controlling and modifying VMs:

-   boots a VM. VMs can also be started by clicking the slide toggle on
    the desired VM.

    If there is insufficient memory to start the VM, a dialog will
    prompt to . Memory overcommitment allows the VM to launch even
    though there is insufficient free memory. Proceeding with the
    overcommitment option should be used with caution.

    To start a VM when the host system boots, set . If is set and the VM
    is in an encrypted, locked pool, the VM starts when the pool
    is unlocked.

-   changes VM settings.

-   removes the VM. () used in () and image files used in () devices are
    removed when a VM is deleted. These resources can be removed
    manually in after it is determined that the data in them has been
    backed up or is no longer needed.

-   is used to add, remove, or edit devices attached to a
    virtual machine.

-   copies the VM. A new name for the clone can be specified. If a
    custom name is not entered, the name assigned is , where is the
    orignal VM name and is the clone number. Each clones is given a new
    VNC port.

These additional options in (Expand) are available when a VM is running:

-   immediately halts the VM. This is equivalent to unplugging the power
    cord from a computer.

-   shuts down the VM.

-   shuts down and immediately starts the VM.

-   VMs with set show a button. VNC connections permit remote graphical
    access to the VM.

-   opens a connection to a virtual serial port on the VM. is assigned
    to the first VM, is assigned to the second VM, and so on. These
    virtual serial ports allow connections to the VM console from
    the ().

    <span>tip</span><span>Tip:</span> The <span> </span>
    (https://www.freebsd.org/cgi/man.cgi?query=nmdm) device is
    dynamically created. The actual name varies on each VM.

    To connect to the first VM, type in the (). See <span> </span>
    (https://www.freebsd.org/cgi/man.cgi?query=cu) for more information.

Creating VMs {#\detokenize{virtualmachines:creating-vms}}
------------

\[\]\[\] Click to open the wizard in :

\[\]

The configuration options for a Virtual Machine (VM) type are described
in .

<span>|&gt;p<span>0.08-2</span> |&gt;p<span>0.20-2</span>
|&gt;p<span>0.12-2</span> |&gt;p<span>0.60-2</span>|</span>

\[\]\
Screen \# & Setting & Value & Description\

\
Screen \# & Setting & Value & Description\

\

1 & Guest Operating System & dropdown menu & Choose the VM operating
system type. Choices are: , , or . See <span> </span>
(https://github.com/FreeBSDUPB/freebsd/wiki/HowtolaunchdifferentguestOS)
for detailed instructions about using a different guest OS.\
1 & Name & string & Name of the VM. Alphanumeric characters and are
allowed. The name must be unique.\
1 & Description & string & Description (optional).\
1 & System Clock & dropdown menu & Virtual Machine system time. Options
are and . is default.\
1 & Boot Method & dropdown menu & Choices are , , and . Select for newer
operating systems, or (Compatibility Support Mode) for older operating
systems that only understand . is not supported by guest operating
systems.\
1 & Start on Boot & checkbox & Set to start the VM when the system
boots.\
1 & Enable VNC & checkbox & Add a VNC remote connection. Requires
booting.\
1 & Delay VM Boot Until VNC Connects & checkbox & Wait to start VM until
VNC client connects. Only appears when is set.\
1 & Bind & dropdown menu & VNC network interface IP address. The primary
interface IP address is the default. A different interface IP address
can be chosen.\
2 & Virtual CPUs & integer & Number of virtual CPUs to allocate to the
VM. The maximum is 16 unless limited by the host CPU. The VM operating
system might also have operational or licensing restrictions on the
number of CPUs.\
2 & Memory Size & integer & Set the amount of RAM for the VM. Allocating
too much memory can slow the system or prevent VMs from running. This is
a ().\
3 & Disk image & check option with custom fields & Select to create a
new zvol on an existing dataset. This is used as a virtual hard drive
for the VM. Select and choose an existing zvol from the dropdown.\
3 & Select Disk Type & dropdown menu & Select the disk type. Choices are
and . Refer to () for more information about these disk types.\
3 & Size (Examples: 500 KiB, 500M, 2TB) && Allocate the amount of
storage for the zvol. This is a (). Numbers without unit letters are
interpreted as megabytes. For example, sets the zvol size to 500
megabytes.\
3 & Zvol Location && When is chosen, select a pool or dataset for the
new zvol.\
3 & Select existing zvol & dropdown menu & When is chosen, select an
existing zvol for the VM.\
4 & Adapter Type & dropdown menu & emulates the same Intel Ethernet
card. This provides compatibility with most operating systems. provides
better performance when the operating system installed in the VM
supports VirtIO paravirtualized network drivers.\
4 & MAC Address & string & Enter the desired MAC address to override the
autogenerated randomized MAC address.\
4 & Attach NIC & dropdown menu & Select the physical interface to
associate with the VM.\
5 & Optional: Choose installation media image & browse button & Click
(Browse) to select an installer ISO or image file on the
FreeNAS$^{\text{®}}$ system.\
5 & Upload ISO & checkbox and & Set to upload an installer ISO or image
file to the FreeNAS$^{\text{®}}$ system.\

The final screen of the Wizard displays the chosen options for the new
Virtual Machine (VM) type. Click to create the VM or to change any
settings.

After the VM has been installed, remove the install media device. Go to
(Options) . Remove the device by clicking (Options) . This prevents the
virtual machine from trying to boot with the installation media after it
has already been installed.

This example creates a FreeBSD VM:

1.  is set to . is set to . Other options are left at defaults.

2.  is set to and is set to .

3.  is selected. The zvol size is set to GiB and stored on the pool
    named .

4.  Network settings are left at default values.

5.  A FreeBSD ISO installation image has been selected and uploaded to
    the FreeNAS$^{\text{®}}$ system. The field is populated when the
    upload completes.

6.  After verifying the is correct, is clicked.

shows the confirmation step and basic settings for the new virtual
machine:

\[\]

Installing Docker {#\detokenize{virtualmachines:installing-docker}}
-----------------

\[\] <span> </span> (https://www.docker.com/) can be used on
FreeNAS$^{\text{®}}$ by installing it on a Linux virtual machine.

Choose a Linux distro and install it on FreeNAS$^{\text{®}}$ by
following the steps in (). Using <span> </span> (https://ubuntu.com/) is
recommended.

After the Linux operating system has been installed, start the VM.
Connect to it by clicking (Expand) . Follow the <span> </span>
(https://docs.docker.com/) for Docker installation and usage.

Adding Devices to a VM {#\detokenize{virtualmachines:adding-devices-to-a-vm}}
----------------------

\[\]\[\] Go to , (Options) , and click to add a new VM device.

Select the new device from the field. These devices are available:

-   ()

-   ()

-   ()

-   ()

-   () (only available on virtual machines with set to )

(Options) is also used to edit or delete existing devices. Click
(Options) for a device to display , , , and options:

-   modifies a device.

-   removes the device from the VM.

-   sets the priority number for booting this device. Smaller numbers
    are higher in boot priority.

-   shows additional information about the specific device. This
    includes the physical interface and MAC address in a device, the
    path to the zvol in a device, and the path to an or other file for
    a device.

### CDROM Devices {#\detokenize{virtualmachines:cd-rom-devices}}

\[\] Adding a CDROM device makes it possible to boot the VM from a CDROM
image, typically an installation CD. The image must be present on an
accessible portion of the FreeNAS$^{\text{®}}$ storage. In this example,
a FreeBSD installation image is shown:

<span>note</span><span>Note:</span> VMs from other virtual machine
systems can be recreated for use in FreeNAS$^{\text{®}}$. Back up the
original VM, then create a new FreeNAS$^{\text{®}}$ VM with virtual
hardware as close as possible to the original VM. Binarycopy the disk
image data into the () created for the FreeNAS$^{\text{®}}$ VM with a
tool that operates at the level of disk blocks, like <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=dd). For some VM systems, it
is best to back up data, install the operating system from scratch in a
new FreeNAS$^{\text{®}}$ VM, and restore the data into the new VM.

### NIC (Network Interfaces) {#\detokenize{virtualmachines:nic-network-interfaces}}

\[\] shows the fields that appear after going to (Options) , clicking ,
and selecting as the .

\[\]

The can emulate an Intel e82545 (e1000) Ethernet card for compatibility
with most operating systems. can provide better performance when the
operating system installed in the VM supports VirtIO paravirtualized
network drivers.

By default, the VM receives an autogenerated random MAC address. To
override the default with a custom value, enter the desired address in .
Click to automatically populate with a new randomized MAC address.

If the system has multiple physical network interface cards, use the
dropdown menu to specify which physical interface to associate with the
VM. To prevent a network interface reset when the VM starts, edit the ()
and set .

Set a number to determine the boot order of this device. A lower number
means a higher boot priority.

<span>tip</span><span>Tip:</span> To check which interface is attached
to a VM, start the VM and go to the (). Type and find the <span> </span>
(https://en.wikipedia.org/wiki/TUN/TAP) interface that shows the name of
the VM in the description.

### Disk Devices {#\detokenize{virtualmachines:disk-devices}}

\[\] () are typically used as virtual hard drives. After (), associate
it with the VM by clicking (Options) , clicking , and selecting as the .

Open the dropdown menu to select a created , then set the disk :

-   emulates an AHCI hard disk for best software compatibility. This is
    recommended for Windows VMs.

-   uses paravirtualized drivers and can provide better performance, but
    requires the operating system installed in the VM to support VirtIO
    disk devices.

If a specific sector size is required, enter the number of bytes in .
The default of uses an autotune script to determine the best sector size
for the zvol.

Set a number to determine the boot order of this device. A lower number
means a higher boot priority.

### Raw Files {#\detokenize{virtualmachines:raw-files}}

\[\] are similar to () disk devices, but the disk image comes from a
file. These are typically used with existing readonly binary images of
drives, like an installer disk image file meant to be copied onto a USB
stick.

After obtaining and copying the image file to the FreeNAS$^{\text{®}}$
system, click (Options) , click , then set the to .

Click (Browse) to select the image file. If a specific sector size is
required, choose it from . The value automatically selects a preferred
sector size for the file.

Setting disk to emulates an AHCI hard disk for best software
compatibility. uses paravirtualized drivers and can provide better
performance, but requires the operating system installed in the VM to
support VirtIO disk devices.

Set a number to determine the boot order of this device. A lower number
means a higher boot priority.

Set the size of the file in GiB.

### VNC Interface {#\detokenize{virtualmachines:vnc-interface}}

\[\] VMs set to booting are also given a VNC (Virtual Network Computing)
remote connection. A standard <span> </span>
(https://en.wikipedia.org/wiki/Virtual\_Network\_Computing) client can
connect to the VM to provide screen output and keyboard and mouse input.

Each VM can have a single VNC device. An existing VNC interface can be
changed by clicking (Options) and .

<span>note</span><span>Note:</span> Using a nonUS keyboard with VNC is
not yet supported. As a workaround, select the US keymap on the system
running the VNC client, then configure the operating system running in
the VM to use a keymap that matches the physical keyboard. This will
enable passthrough of all keys regardless of the keyboard layout.

shows the fields that appear after going to (Options) , and clicking
(Options) for VNC.

\[\]

Setting to automatically assigns a port when the VM is started. If a
fixed, preferred port number is needed, enter it here.

Set to wait to start the VM until a VNC client connects.

sets the default screen resolution used for the VNC session.

Use to select the IP address for VNC connections.

To automatically pass the VNC password, enter it into the field. Note
that the password is limited to 8 characters.

To use the VNC web interface, set .

<span>tip</span><span>Tip:</span> If a RealVNC 5.X Client shows the
error , disable the option and move the slider to . On later versions of
RealVNC, select , click , , then select 4.1 from the dropdown menu.

Set a number to determine the boot order of this device. A lower number
means a higher boot priority.

Display System Processes {#\detokenize{displaysystemprocesses:display-system-processes}}
========================

\[\]\[\] Clicking opens a screen showing the output of <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=top). An example is shown in
.

\[\]

The display automatically refreshes itself. The display is readonly.

Shell {#\detokenize{shell:shell}}
=====

\[\]\[\]\[\] The FreeNAS$^{\text{®}}$ web interface provides a web
shell, making it convenient to run command line tools from the web
browser as the user.

\[\]

The prompt shows that the current user is , the hostname is , and the
current working directory is , the home directory of the loggedin user.

<span>note</span><span>Note:</span> The default shell for a new install
of FreeNAS$^{\text{®}}$ is <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=zsh). FreeNAS$^{\text{®}}$
systems which have been upgraded from an earlier version will continue
to use as the default shell.

The default shell can be changed in . Click (Options) and for the user.
Choose the desired shell from the dropdown and click .

The slider adjusts the size of text displayed in the Shell. Click to
reset the shell font and size.

A history of previous commands is available. Use the up and down arrow
keys to scroll through previously entered commands. Edit the command if
desired, then press to reenter the command.

, , and keys are supported. Tab completion is also available. Type a few
letters and press to complete a command name or filename in the current
directory. Right clicking in the terminal window displays a reminder
about using and or and for copy and paste operations in the
FreeNAS$^{\text{®}}$ shell.

Type to leave the session.

Clicking other web interface menus closes the shell session and stops
commands running in the shell. () provides the ability to detach shell
sessions and then reattach to them later. Commands continue to run in a
detached session.

<span>note</span><span>Note:</span> Not all shell features render
correctly in Chrome. Firefox is the recommended browser when using the
shell.

Most FreeBSD () are available in the , including additional
troubleshooting applications for FreeNAS$^{\text{®}}$.

Log Out, Restart, or Shut Down {#\detokenize{power:log-out-restart-or-shut-down}}
==============================

\[\]\[\]\[\] The (Power) button is used to log out of the web interface
or restart or shut down the FreeNAS$^{\text{®}}$ system.

Log Out {#\detokenize{power:log-out}}
-------

\[\]\[\] To log out, click (Power), then . After logging out, the login
screen is displayed.

Restart {#\detokenize{power:restart}}
-------

\[\]\[\] To restart the system, click (Power), then . A confirmation
screen asks for verification of the restart. . Click to confirm the
action, then click to restart the system.

\[\]

An additional warning message appears when a restart is attempted when a
scrub or resilver is in progress. When that warning appears, the
recommended steps are to the restart request and to periodically run
from () until it shows that the scrub or verify has completed. Then the
restart request can be entered again.

To complete the restart request, click the checkbox and then the button.
Restarting the system disconnects all clients, including the web
administration interface. Wait a few minutes for the system to boot,
then use the back button in the browser to return to the IP address of
the FreeNAS$^{\text{®}}$ system. The login screen appears after a
successful reboot. If the login screen does not appear, using a monitor
and keyboard to physically access the FreeNAS$^{\text{®}}$ system is
required to determine the issue preventing the system from resuming
normal operation.

Shut Down {#\detokenize{power:shut-down}}
---------

\[\]\[\] Click (Power) and to shut down the system. The warning message
shown in is displayed.

\[\]

Click and then to shut down the system. Shutting down the system
disconnects all clients, including the web interface. Physical access to
the FreeNAS$^{\text{®}}$ system is required to turn it back on.

Alert {#\detokenize{alert:alert}}
=====

\[\]\[\]\[\] The FreeNAS$^{\text{®}}$ alert system provides a visual
warning of any conditions that require administrative attention. The
icon in the upper right corner has a notification badge that displays
the total number of unread alerts. In the example alert shown in , the
system is warning that a pool is degraded.

\[\]

shows the icons that indicate notification, warning, critical, and
oneshot critical alerts. Critical messages are also emailed to the root
account. Oneshot critical alerts must be dismissed by the user.

<span>|&gt;p<span>0.20-2</span> |&gt;p<span>0.15-2</span>|</span>

\[\]\
Alert Level & Icon\

\
Alert Level & Icon\

\

Notification &\
Warning &\
Critical &\
Oneshot Critical &\

Close an alert message by clicking . There is also an option to .
Dismissing all alerts removes the notification badge from the alerts
icon. Dismissed alerts can be reopened by clicking .

Behind the scenes, an alert daemon checks for various alert conditions,
such as pool and disk status, and writes the current conditions to the
system RAM. These messages are flushed to the SQLite database
periodically and then published to the user interface.

Current alerts are viewed from the Shell option of the Console Setup
Menu () or the Web Shell () by running .

Notifications for specific alerts are adjusted in the () menu. An alert
message can be set to publish , , , or .

Some of the conditions that trigger an alert include:

-   used space on a pool, dataset, or zvol goes over 80%; the alert goes
    red at 95%

-   new () are available for the pool; this alert can be adjusted in ()
    if a pool upgrade is not desired at present

-   a new update is available

-   hardware events detected by an attached () controller

-   an error with the () connection

-   ZFS pool status changes from

-   a S.M.A.R.T. error occurs

-   the system is unable to bind to the set in

-   the system can not find an IP address configured on an iSCSI portal

-   the NTP server cannot be contacted

-   <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=syslogng)
    is not running

-   a periodic snapshot or replication task fails

-   a VMware login or a () task fails

-   a () fails

-   deleting a VMware snapshot fails

-   a Certificate Authority or certificate is invalid or malformed

-   an update failed, or the system needs to reboot to complete a
    successful update

-   a rekey operation fails on an encrypted pool

-   an Active Directory domain goes offline; by default the winbindd
    connection manager will try to reconnect every 30 seconds and will
    clear the alert when the domain comes back online

-   LDAP failed to bind to the domain

-   any member interfaces of a lagg interface are not active

-   a device is slowing pool I/O

-   () status

-   the status of an Avago MegaRAID SAS controller has changed; <span>
    </span> (https://www.freebsd.org/cgi/man.cgi?query=mfiutil) is
    included for managing these devices

-   a scrub has been paused for more than eight hours

-   a connected Uninterruptible Power Supply (UPS) switches to battery
    power, switches to line power, communication with the UPS is lost or
    established, the battery is low, or the battery needs to be replaced

Task Manager {#\detokenize{taskmanager:task-manager}}
============

\[\]\[\] The task manager shows a list of tasks performed by the
FreeNAS$^{\text{®}}$ system starting with the most recent. Click a task
name to display its start time, progress, finish time, and whether the
task succeeded. If a task failed, the error status is shown.

Tasks with log file output have a button to show the log files.

The task manager can be opened by clicking (Task Manager). Close the
task manager by clicking , clicking anywhere outside the task manager
dialog, or by pressing .

Support Resources {#\detokenize{support:support-resources}}
=================

\[\]\[\] FreeNAS$^{\text{®}}$ has a large installation base and an
active user community. This means that many usage questions have already
been answered and the details are available on the Internet. If an issue
occurs while using FreeNAS$^{\text{®}}$, it can be helpful to spend a
few minutes searching the Internet for the word with some keywords that
describe the error message or function that is being implemented.

The section discusses resources available to FreeNAS$^{\text{®}}$ users:

-   ()

-   ()

-   ()

-   ()

-   ()

-   ()

User Guide {#\detokenize{support:user-guide}}
----------

\[\]\[\] The FreeNAS$^{\text{®}}$ User Guide with complete configuration
instructions is available either by clicking in the FreeNAS$^{\text{®}}$
user interface or going to

Website and Social Media {#\detokenize{support:website-and-social-media}}
------------------------

\[\] The <span> </span> (http://www.freenas.org/) contains links to all
of the available documentation, support, and social media resources.
Major announcements are also posted to the main page.

Users are welcome to network on the FreeNAS$^{\text{®}}$ social media
sites:

-   <span> </span> (https://www.linkedin.com/groups/3903140/profile)

-   <span> </span> (https://www.facebook.com/freenascommunity)

-   <span> </span> (https://www.facebook.com/groups/1707686686200221)

-   <span> </span> (https://mobile.twitter.com/freenas)

Forums {#\detokenize{support:forums}}
------

\[\]\[\] The <span> </span> (https://forums.freenas.org/index.php) are
an active online resource where people can ask questions, receive help,
and share findings with other FreeNAS$^{\text{®}}$ users. New users are
encouraged to post a brief message about themselves and how they use
FreeNAS$^{\text{®}}$ in the <span> </span>
(https://forums.freenas.org/index.php?forums/introductions.25/) forum.

The <span> </span> (https://forums.freenas.org/index.php?resources/)
section contains categorized, usercontributed guides on many aspects of
building and using FreeNAS$^{\text{®}}$ systems.

Languagespecific categories are available under .

-   <span>
    ttps://forums.freenas.org/index.php?forums/chinese-%E4%B8%AD%E6%96%87.60/</span><span>Chinese</span> (https://forums.freenas.org/index.php?forums/chinese%E4%B8%AD%E6%96%87.60/)

-   <span>
    </span> (https://forums.freenas.org/index.php?forums/dutchnederlands.35/)

-   <span>
    </span> (https://forums.freenas.org/index.php?forums/frenchfrancais.29/)

-   <span>
    </span> (https://forums.freenas.org/index.php?forums/germandeutsch.31/)

-   <span>
    </span> (https://forums.freenas.org/index.php?forums/italianitaliano.30/)

-   <span>
    ttps://forums.freenas.org/index.php?forums/portuguese-portugu%C3%AAs.44/</span><span>Portuguese
    Português</span> (https://forums.freenas.org/index.php?forums/portugueseportugu%C3%AAs.44/)

-   <span>
    ttps://forums.freenas.org/index.php?forums/romanian-rom%C3%A2n%C4%83.53/</span><span>Romanian
    Română</span> (https://forums.freenas.org/index.php?forums/romanianrom%C3%A2n%C4%83.53/)

-   <span>
    ttps://forums.freenas.org/index.php?forums/russian-%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9.38/</span><span>Russian
    Русский</span> (https://forums.freenas.org/index.php?forums/russian%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9.38/)

-   <span>
    ttps://forums.freenas.org/index.php?forums/spanish-espa%C3%B1ol.33/</span><span>Spanish
    Español</span> (https://forums.freenas.org/index.php?forums/spanishespa%C3%B1ol.33/)

-   <span>
    </span> (https://forums.freenas.org/index.php?forums/swedishsvenske.51/)

-   <span>
    ttps://forums.freenas.org/index.php?forums/turkish-t%C3%BCrk%C3%A7e.36/</span><span>Turkish
    Türkçe</span> (https://forums.freenas.org/index.php?forums/turkisht%C3%BCrk%C3%A7e.36/)

To join the forums, create an account with the link.

Before asking a question on the forums, check the <span> </span>
(https://forums.freenas.org/index.php?resources/) to see if the
information is already there. See the <span> </span>
(https://forums.freenas.org/index.php?threads/updatedforumrules41117.45124/)
for guidelines on posting your hardware information and how to ask a
questions that will get a response.

IRC {#\detokenize{support:irc}}
---

\[\]\[\] To ask a question in real time, use the channel on IRC <span>
</span> (http://freenode.net/). Depending on the time of day and the
time zone, FreeNAS$^{\text{®}}$ developers or other users may be
available to provide assistance. If no one answers right away, remain on
the channel, as other users tend to read the channel history to answer
questions as time permits.

Typically, an IRC <span> </span>
(https://en.wikipedia.org/wiki/Comparison\_of\_Internet\_Relay\_Chat\_clients)
is used to access the IRC channel. Alternately, use <span> </span>
(http://webchat.freenode.net/?channels=freenas) from a web browser.

To get the most out of the IRC channel, keep these points in mind:

-   Do not ask “Can anyone help me?”. Just ask the question.

-   Do not ask a question and leave. Users who know the answer cannot
    help you if you disappear.

-   If no one answers, the question may be difficult to answer or it has
    been asked before. Research other resources while waiting for the
    question to be answered.

-   Do not post error messages in the channel. Instead, use a pasting
    service such as <span> </span> (https://pastebin.com/) and paste the
    resulting URL into the IRC discussion.

Videos {#\detokenize{support:videos}}
------

\[\] A series of instructional videos are available for
FreeNAS$^{\text{®}}$:

-   <span> </span> (https://www.youtube.com/watch?v=aAeZRNfarJc)

-   <span> </span> (https://www.youtube.com/watch?v=OT1Le5VQIc0)

-   <span> </span> (https://www.youtube.com/watch?v=2nvb90AhgL8)

-   <span> </span> (https://www.youtube.com/watch?v=wqSH\_uQSArQ)

-   <span> </span> (https://www.youtube.com/watch?v=RxggaE935PM)

-   <span> </span> (https://www.youtube.com/watch?v=uJ\_7eG88zk)

-   <span> </span> (https://www.youtube.com/watch?v=R3fSr6yc4)

Professional Support {#\detokenize{support:professional-support}}
--------------------

\[\]\[\] In addition to free community resources, support might be
available in your area through thirdparty consultants. Submit a support
inquiry using the form at .

Contributing to FreeNAS$^{\text{®}}$ {#\detokenize{contribute:contributing-to-freenas}}
====================================

\[\]\[\] FreeNAS$^{\text{®}}$ is an open source community, relying on
the input and expertise of users to grow and improve. When users take
time to assist the community, their contributions benefit everyone.

This section describes how to participate and contribute to
FreeNAS$^{\text{®}}$. It is by no means an exhaustive list. If you have
an idea that will benefit the community, bring it up on one of the
resources mentioned in ().

This section demonstrates how to:

-   ()

Translation {#\detokenize{contribute:translation}}
-----------

\[\]\[\] FreeNAS$^{\text{®}}$ is developed and documented in English.
Having complete translations of the user interface into other languages
helps make FreeNAS$^{\text{®}}$ much more useful to communities around
the world.

FreeNAS$^{\text{®}}$ uses files stored in the <span> </span>
(https://github.com/freenas/webui/tree/master/src/assets/i18n) to manage
the translation of text shown in the FreeNAS$^{\text{®}}$ graphical
administrative interface. GitHub provides an easy to use webbased
editor, making it possible for individuals to assist with translation or
comment on existing translations.

To view translation files, open the directory of the
FreeNAS$^{\text{®}}$ <span> </span>
(https://github.com/freenas/webui/tree/master/src/assets/i18n), as shown
in .

\[\]

To assist with translating FreeNAS$^{\text{®}}$, first create an account
with <span> </span> (https://github.com/) and the <span> </span>
(https://github.com/freenas/webui) repository.

There are two methods for committing translations:

1.  Use the GitHub website to edit the files.

OR

1.  Make a local copy of the forked repository and use a text editor
    for translations.

### Translate with GitHub {#\detokenize{contribute:translate-with-github}}

Open a browser and go to your GitHub profile. Select the tab and open
your fork of the repository. Click to open the translations directory.
Click on the desired language file to begin translating.

<span>tip</span><span>Tip:</span> Here is a list of <span> </span>
(https://www.abbreviations.com/acronyms/LANGUAGES2L)

Click the icon in the upper right area to open the online file editor.
shows the page that appears:

\[\]

There are numerous and entries in the file. Read the text and enter the
translation between the quotes.

Scroll to the bottom of the page when finished entering translations.
Enter a descriptive title and summary of changes for the edits and click
.

### Download and Translate Offline {#\detokenize{contribute:download-and-translate-offline}}

<span> </span>
(https://gitscm.com/book/en/v2/GettingStartedInstallingGit). There are
numerous examples in these instructions of using , but full
documentation for is <span> </span> (https://gitscm.com/doc).

These instructions show using the Command Line Interface (CLI) with ,
but many graphical utilities are available.

Create a suitable directory to store the local copy of the forked
repository. Download the repository with :

The download can take several minutes, depending on connection speed.

Use to go to the directory:

Use a editor to add translations to the desired language file. Any
capable editor will work, but <span> </span> (https://poedit.net/) and
<span> </span> (https://wiki.gnome.org/Apps/Gtranslator) are two common
options.

Commit any file changes with :

Enter a descriptive message about the changes and save the commit.

When finished making commits to the branch, use to send your changes to
the online fork repository.

### Translation Pull Requests {#\detokenize{contribute:translation-pull-requests}}

When ready to merge translations in the original repository, open a web
browser and go to your forked repository on GitHub. Select the tab and
click . Set the dropdown to and the to your fork. Click , write a title
and summary of your proposed changes, and click again to submit your
translations to the repository.

The FreeNAS$^{\text{®}}$ project automatically tests pull requests for
compatibility. If there any issues with a pull request, either the
automated system will update the request or a FreeNAS$^{\text{®}}$ team
member will leave a message in the comment section of the request.

All assistance with translations helps to benefit the
FreeNAS$^{\text{®}}$ community. Thank you!

Command Line Utilities {#\detokenize{cli:command-line-utilities}}
======================

\[\]\[\] Several command line utilities which are provided with
FreeNAS$^{\text{®}}$ are demonstrated in this section.

The following utilities can be used for benchmarking and performance
testing:

-   (): used for measuring maximum TCP and UDP bandwidth performance

-   (): a tool for measuring network performance

-   (): filesystem benchmark utility used to perform a broad filesystem
    analysis

-   (): used to gather ZFS ARC statistics

The following utilities are specific to RAID controllers:

-   ():\_used to monitor and maintain 3ware RAID controllers

-   (): used to configure and manage Broadcom MegaRAID SAS family of
    RAID controllers

This section also describes these utilities:

-   (): the backend used to dump FreeNAS$^{\text{®}}$ debugging
    information

-   (): a terminal multiplexer similar to GNU screen

-   (): reports information about system hardware as described in the
    system’s BIOS

Iperf {#\detokenize{cli:iperf}}
-----

\[\]\[\] Iperf is a utility for measuring maximum TCP and UDP bandwidth
performance. It can be used to chart network throughput over time. For
example, it is used to test the speed of different types of shares to
determine which type performs best on the network.

FreeNAS$^{\text{®}}$ includes the iperf server. To perform network
testing, install an iperf client on a desktop system that has network
access to the FreeNAS$^{\text{®}}$ system. This section demonstrates how
to use the <span> </span>
(https://code.google.com/archive/p/xjperf/downloads) as it works on
Windows, macOS, Linux, and BSD systems.

Since this client is Javabased, the appropriate <span> </span>
(http://www.oracle.com/technetwork/java/javase/downloads/index.html)
must be installed on the client computer.

Linux and BSD users will need to install the iperf package using the
package management system for their operating system.

To start xjperf on Windows: unzip the downloaded file, start Command
Prompt in Run as administrator mode, to the unzipped folder, and run .

To start xjperf on macOS, Linux, or BSD, unzip the downloaded file, to
the unzipped directory, type , and run .

Start the iperf server on FreeNAS$^{\text{®}}$ when the client is ready.

<span>note</span><span>Note:</span> Beginning with FreeNAS$^{\text{®}}$
version 11.1, both <span> </span>
(https://sourceforge.net/projects/iperf2/) and <span> </span>
(http://software.es.net/iperf/) are preinstalled. To use iperf2, use .
To use iperf3, instead type . The examples below are for iperf2.

To see the available server options, open Shell and type:

\[commandchars=\
{}\] iperf help | more

or:

\[commandchars=\
{}\] iperf3 help | more

For example, to perform a TCP test and start the server in daemon mode
(to get the prompt back), type:

\[commandchars=\
{}\] iperf sD Server listening on TCP port 5001 TCP window size: 64.0
KByte (default) Running Iperf Server as a daemon The Iperf daemon
process ID: 4842

<span>note</span><span>Note:</span> The daemon process stops when ()
closes. Set up the environment with shares configured and started
starting the Iperf process.

From the desktop, open the client. Enter the IP of address of the
FreeNAS$^{\text{®}}$ system, specify the running time for the test under
(the default test time is 10 seconds), and click the button. shows an
example of the client running on a Windows system while an SFTP transfer
is occurring on the network.

\[\]

Check the type of traffic before testing UPD or TCP. The iperf server is
used to get additional details for services using TCP or UDP . The
startup message indicates when the server is listening for TCP or UDP.
The command gives an overview of the services running on the
FreeNAS$^{\text{®}}$ system. This helps to determine if the traffic to
test is UDP or TCP.

\[commandchars=\
{}\] sockstat 4 | more USER COMMAND PID FD PROTO LOCAL ADDRESS FOREIGN
ADDRESS root iperf 4870 6 udp4 \*:5001 \*:\* root iperf 4842 6 tcp4
\*:5001 \*:\* www nginx 4827 3 tcp4 127.0.0.1:15956 127.0.0.1:9042 www
nginx 4827 5 tcp4 192.168.2.11:80 192.168.2.26:56964 www nginx 4827 7
tcp4 \*:80 \*:\* root sshd 3852 5 tcp4 \*:22 \*:\* root python 2503 5
udp4 \*:\* \*:\* root mountd 2363 7 udp4 \*:812 \*:\* root mountd 2363 8
tcp4 \*:812 \*:\* root rpcbind 2359 9 udp4 \*:111 \*:\* root rpcbind
2359 10 udp4 \*:886 \*:\* root rpcbind 2359 11 tcp4 \*:111 \*:\* root
nginx 2044 7 tcp4 \*:80 \*:\* root python 2029 3 udp4 \*:\* \*:\* root
python 2029 4 tcp4 127.0.0.1:9042 \*:\* root python 2029 7 tcp4
127.0.0.1:9042 127.0.0.1:15956 root ntpd 1548 20 udp4 \*:123 \*:\* root
ntpd 1548 22 udp4 192.168.2.11:123\*:\* root ntpd 1548 25 udp4
127.0.0.1:123 \*:\* root syslogd 1089 6 udp4 127.0.0.1:514 \*:\*

When testing is finished, either type or close Shell to terminate the
iperf server process.

Netperf {#\detokenize{cli:netperf}}
-------

\[\]\[\] Netperf is a benchmarking utility that can be used to measure
the performance of unidirectional throughput and endtoend latency.

Before using the command, start its server process with this command:

\[commandchars=\
{}\] netserver Starting netserver with host IN(6)ADDRANY port 12865 and
family AFUNSPEC

The following command displays the available options for performing
tests with the command. The <span> </span>
(https://hewlettpackard.github.io/netperf/) describes each option in
more detail and explains how to perform many types of tests. It is the
best reference for understanding how each test works and how to
interpret the results. When testing is finished, type to stop the server
process.

\[commandchars=\
{}\] netperf h |more Usage: netperf \[global options\] \[test options\]
Global options: a send,recv Set the local send,recv buffer alignment A
send,recv Set the remote send,recv buffer alignment B brandstr Specify a
string to be emitted with brief output c \[cpurate\] Report local CPU
usage C \[cpurate\] Report remote CPU usage d Increase debugging output
D \[secs,units\] \* Display interim results at least every secs seconds
using units as the initial guess for units per second f G|M|K|g|m|k Set
the output units F fillfile Prefill buffers with data from fillfile h
Display this text H name|ip,fam \* Specify the target machine and/or
local ip and family i max,min Specify the max and min number of
iterations (15,1) I lvl\[,intvl\] Specify confidence level (95 or 99)
(99) and confidence interval in percentage (10) j Keep additional timing
statistics l testlen Specify test duration (0 secs) (0 bytes|trans) L
name|ip,fam \* Specify the local ip|name and address family o send,recv
Set the local send,recv buffer offsets O send,recv Set the remote
send,recv buffer offset n numcpu Set the number of processors for CPU
util N Establish no control connection, do send side only p port,lport\*
Specify netserver port number and/or local port P 0|1 Dont/Do display
test headers r Allow confidence to be hit on result only s seconds Wait
seconds between test setup and test start S Set SOKEEPALIVE on the data
connection t testname Specify test to perform T lcpu,rcpu Request
netperf/netserver be bound to local/remote cpu v verbosity Specify the
verbosity level W send,recv Set the number of send,recv buffers v level
Set the verbosity level (default 1, min 0) V Display the netperf version
and exit

For those options taking two parms, at least one must be specified.
Specifying one value without a comma will set both parms to that value,
specifying a value with a leading comma will set just the second parm,
and specifying a value with a trailing comma will set the first. To set
each parm to unique values, specify both and separate them with a comma.

For these options taking two parms, specifying one value with no comma
will only set the first parms and will leave the second at the default
value. To set the second value it must be preceded with a comma or be a
commaseparated pair. This is to retain previous netperf behavior.

IOzone {#\detokenize{cli:iozone}}
------

\[\]\[\] IOzone is a disk and filesystem benchmarking tool. It can be
used to test file I/O performance for the following operations: read,
write, reread, rewrite, read backwards, read strided, fread, fwrite,
random read, pread, mmap, aio\_read, and aio\_write.

FreeNAS$^{\text{®}}$ ships with IOzone so it can be run from Shell. When
using IOzone on FreeNAS$^{\text{®}}$, to a directory in a pool that you
have permission to write to, otherwise an error about being unable to
write the temporary file will occur.

Before using IOzone, read through the <span> </span>
(http://www.iozone.org/docs/IOzone\_msword\_98.pdf) as it describes the
tests, the many command line switches, and how to interpret the results.

These resources provide good starting points on which tests to run, when
to run them, and how to interpret the results:

-   <span>
    </span> (https://www.cyberciti.biz/tips/linuxfilesystembenchmarkingwithiozone.html)

-   <span>
    </span> (http://www.iozone.org/docs/NFSClientPerf\_revised.pdf)

-   <span>
    </span> (https://www.thegeekstuff.com/2011/05/iozoneexamples/)

Type the following command to receive a summary of the available
switches. IOzone is comprehensive so it may take some time to learn how
to use the tests effectively.

Starting with version 9.2.1, FreeNAS$^{\text{®}}$ enables compression on
newly created ZFS pools by default. Since IOzone creates test data that
is compressible, this can skew test results. To configure IOzone to
generate incompressible test data, include the options .

Alternatively, consider temporarily disabling compression on the ZFS
pool or dataset when running IOzone benchmarks.

<span>note</span><span>Note:</span> If a visual representation of the
collected data is preferred, scripts are available to render IOzone’s
output in <span> </span> (http://www.gnuplot.info/).

\[commandchars=\
{}\]

arcstat {#\detokenize{cli:arcstat}}
-------

\[\]\[\] Arcstat is a script that prints out ZFS <span> </span>
(https://en.wikipedia.org/wiki/Adaptive\_replacement\_cache) statistics.
Originally it was a perl script created by Sun. That perl script was
ported to FreeBSD and then ported as a Python script for use on
FreeNAS$^{\text{®}}$.

Watching ARC hits/misses and percentages shows how well the ZFS pool is
fetching from the ARC rather than using disk I/O. Ideally, there will be
as many things fetching from cache as possible. Keep the load in mind
while reviewing the stats. For random reads, expect a miss and having to
go to disk to fetch the data. For cached reads, expect it to pull out of
the cache and have a hit.

Like all cache systems, the ARC takes time to fill with data. This means
that it will have a lot of misses until the pool has been in use for a
while. If there continues to be lots of misses and high disk I/O on
cached reads, there is cause to investigate further and tune the system.

The <span> </span> (https://wiki.freebsd.org/ZFSTuningGuide) provides
some suggestions for commonly tuned values. It should be noted that
performance tuning is more of an art than a science and that any changes
made will probably require several iterations of tune and test. Be aware
that what needs to be tuned will vary depending upon the type of
workload and that what works for one one network may not benefit
another.

In particular, the value of prefetching depends upon the amount of
memory and the type of workload, as seen in <span> </span>
(http://cuddletech.com/?page\_id=834&id=1040)

FreeNAS$^{\text{®}}$ provides two command line scripts which can be
manually run from ():

-   : provides a summary of the statistics

-   : used to watch the statistics in real time

The advantage of these scripts is that they provide real time
information, whereas the current web interface reporting mechanism is
designed to only provide graphs charted over time.

This <span> </span>
(https://forums.freenas.org/index.php?threads/benchmarkingzfs.7928/)
demonstrates some examples of using these scripts with hints on how to
interpret the results.

To view the help for arcstat.py:

\[commandchars=\
{}\] arcstat.py h \[havxp\] \[f fields\] \[o file\] \[s string\]
\[interval \[count\]\]

h : Print this help message a : Print all possible stats v : List all
possible field headers and definitions x : Print extended stats f :
Specify specific fields to print (see v) o : Redirect output to the
specified file s : Override default field separator with custom
character or string p : Disable autoscaling of numerical fields

Examples: arcstat o /tmp/a.log 2 10 arcstat s , o /tmp/a.log 2 10
arcstat v arcstat f time,hit,dh,ph,mh 1

To view ARC statistics in real time, specify an interval and a count.
This command will display every 1 second for a count of five.

\[commandchars=\
{}\] arcstat.py 1 5 time read miss miss dmis dm pmis pm mmis mm arcsz c
06:19:03 7 0 0 0 0 0 0 0 0 153M 6.6G 06:19:04 257 0 0 0 0 0 0 0 0 153M
6.6G 06:19:05 193 0 0 0 0 0 0 0 0 153M 6.6G 06:19:06 193 0 0 0 0 0 0 0 0
153M 6.6G 06:19:07 255 0 0 0 0 0 0 0 0 153M 6.6G

briefly describes the columns in the output.

<span>|&gt;p<span>0.12-2</span> |&gt;p<span>0.33-2</span>|</span>

\[\]\
Column & Description\

\
Column & Description\

\

read & total ARC accesses/second\
miss & ARC misses/second\
miss% & ARC miss percentage\
dmis & demand data misses/second\
dm% & demand data miss percentage\
pmis & prefetch misses per second\
pm% & prefetch miss percentage\
mmis & metadata misses/second\
mm% & metadata miss percentage\
arcsz & arc size\
c & arc target size\

To receive a summary of statistics, use:

\[commandchars=\
{}\] arcsummary.py System Memory: 2.36 93.40 MiB Active, 8.95 353.43 MiB
Inact 8.38 330.89 MiB Wired, 0.15 5.90 MiB Cache 80.16 3.09 GiB Free,
0.00 0 Bytes Gap Real Installed: 4.00 GiB Real Available: 99.31 3.97 GiB
Real Managed: 97.10 3.86 GiB Logical Total: 4.00 GiB Logical Used: 13.93
570.77 MiB Logical Free: 86.07 3.44 GiB Kernel Memory: 87.62 MiB Data:
69.91 61.25 MiB Text: 30.09 26.37 MiB Kernel Memory Map: 3.86 GiB Size:
5.11 201.70 MiB Free: 94.89 3.66 GiB ARC Summary: (HEALTHY) Storage pool
Version: 5000 Filesystem Version: 5 Memory Throttle Count: 0 ARC Misc:
Deleted: 8 Mutex Misses: 0 Evict Skips: 0 ARC Size: 5.83 170.45 MiB
Target Size: (Adaptive) 100.00 2.86 GiB Min Size (Hard Limit): 12.50
365.69 MiB Max Size (High Water): 8:1 2.86 GiB ARC Size Breakdown:
Recently Used Cache Size: 50.00 1.43 GiB Frequently Used Cache Size:
50.00 1.43 GiB ARC Hash Breakdown: Elements Max: 5.90k Elements Current:
100.00 5.90k Collisions: 72 Chain Max: 1 Chains: 23 ARC Total accesses:
954.06k Cache Hit Ratio: 99.18 946.25k Cache Miss Ratio: 0.82 7.81k
Actual Hit Ratio: 98.84 943.00k Data Demand Efficiency: 99.20 458.77k
CACHE HITS BY CACHE LIST: Anonymously Used: 0.34 3.25k Most Recently
Used: 3.73 35.33k Most Frequently Used: 95.92 907.67k Most Recently Used
Ghost: 0.00 0 Most Frequently Used Ghost: 0.00 0 CACHE HITS BY DATA
TYPE: Demand Data: 48.10 455.10k Prefetch Data: 0.00 0 Demand Metadata:
51.56 487.90k Prefetch Metadata: 0.34 3.25k CACHE MISSES BY DATA TYPE:
Demand Data: 46.93 3.66k Prefetch Data: 0.00 0 Demand Metadata: 49.76
3.88k Prefetch Metadata: 3.30 258 ZFS Tunable (sysctl): kern.maxusers
590 vm.kmemsize 4141375488 vm.kmemsizescale 1 vm.kmemsizemin 0
vm.kmemsizemax 1319413950874 vfs.zfs.vol.unmapenabled 1 vfs.zfs.vol.mode
2 vfs.zfs.syncpassrewrite 2 vfs.zfs.syncpassdontcompress 5
vfs.zfs.syncpassdeferredfree 2 vfs.zfs.zio.excludemetadata 0
vfs.zfs.zio.useuma 1 vfs.zfs.cacheflushdisable 0
vfs.zfs.zilreplaydisable 0 vfs.zfs.version.zpl 5 vfs.zfs.version.spa
5000 vfs.zfs.version.acl 1 vfs.zfs.version.ioctl 5 vfs.zfs.debug 0
vfs.zfs.superowner 0 vfs.zfs.minautoashift 9 vfs.zfs.maxautoashift 13
vfs.zfs.vdev.writegaplimit 4096 vfs.zfs.vdev.readgaplimit 32768
vfs.zfs.vdev.aggregationlimit 131072 vfs.zfs.vdev.trimmaxactive 64
vfs.zfs.vdev.trimminactive 1 vfs.zfs.vdev.scrubmaxactive 2
vfs.zfs.vdev.scrubminactive 1 vfs.zfs.vdev.asyncwritemaxactive 10
vfs.zfs.vdev.asyncwriteminactive 1 vfs.zfs.vdev.asyncreadmaxactive 3
vfs.zfs.vdev.asyncreadminactive 1 vfs.zfs.vdev.syncwritemaxactive 10
vfs.zfs.vdev.syncwriteminactive 10 vfs.zfs.vdev.syncreadmaxactive 10
vfs.zfs.vdev.syncreadminactive 10 vfs.zfs.vdev.maxactive 1000
vfs.zfs.vdev.asyncwriteactivemaxdirtypercent60
vfs.zfs.vdev.asyncwriteactivemindirtypercent30
vfs.zfs.vdev.mirror.nonrotatingseekinc1
vfs.zfs.vdev.mirror.nonrotatinginc 0
vfs.zfs.vdev.mirror.rotatingseekoffset1048576
vfs.zfs.vdev.mirror.rotatingseekinc 5 vfs.zfs.vdev.mirror.rotatinginc 0
vfs.zfs.vdev.trimoninit 1 vfs.zfs.vdev.largerashiftminimal 0
vfs.zfs.vdev.biodeletedisable 0 vfs.zfs.vdev.bioflushdisable 0
vfs.zfs.vdev.cache.bshift 16 vfs.zfs.vdev.cache.size 0
vfs.zfs.vdev.cache.max 16384 vfs.zfs.vdev.metaslabspervdev 200
vfs.zfs.vdev.trimmaxpending 10000 vfs.zfs.txg.timeout 5
vfs.zfs.trim.enabled 1 vfs.zfs.trim.maxinterval 1 vfs.zfs.trim.timeout
30 vfs.zfs.trim.txgdelay 32 vfs.zfs.spacemapblksz 4096
vfs.zfs.spaslopshift 5 vfs.zfs.spaasizeinflation 24
vfs.zfs.deadmanenabled 1 vfs.zfs.deadmanchecktimems 5000
vfs.zfs.deadmansynctimems 1000000 vfs.zfs.recover 0
vfs.zfs.spaloadverifydata 1 vfs.zfs.spaloadverifymetadata 1
vfs.zfs.spaloadverifymaxinflight 10000 vfs.zfs.checkhostid 1
vfs.zfs.mgfragmentationthreshold 85 vfs.zfs.mgnoallocthreshold 0
vfs.zfs.condensepct 200 vfs.zfs.metaslab.biasenabled 1
vfs.zfs.metaslab.lbaweightingenabled 1
vfs.zfs.metaslab.fragmentationfactorenabled1
vfs.zfs.metaslab.preloadenabled 1 vfs.zfs.metaslab.preloadlimit 3
vfs.zfs.metaslab.unloaddelay 8 vfs.zfs.metaslab.loadpct 50
vfs.zfs.metaslab.minallocsize 33554432 vfs.zfs.metaslab.dffreepct 4
vfs.zfs.metaslab.dfallocthreshold 131072 vfs.zfs.metaslab.debugunload 0
vfs.zfs.metaslab.debugload 0 vfs.zfs.metaslab.fragmentationthreshold70
vfs.zfs.metaslab.gangbang 16777217 vfs.zfs.freebpobjenabled 1
vfs.zfs.freemaxblocks 18446744073709551615 vfs.zfs.noscrubprefetch 0
vfs.zfs.noscrubio 0 vfs.zfs.resilvermintimems 3000 vfs.zfs.freemintimems
1000 vfs.zfs.scanmintimems 1000 vfs.zfs.scanidle 50 vfs.zfs.scrubdelay 4
vfs.zfs.resilverdelay 2 vfs.zfs.topmaxinflight 32 vfs.zfs.delayscale
500000 vfs.zfs.delaymindirtypercent 60 vfs.zfs.dirtydatasync 67108864
vfs.zfs.dirtydatamaxpercent 10 vfs.zfs.dirtydatamaxmax 4294967296
vfs.zfs.dirtydatamax 426512793 vfs.zfs.maxrecordsize 1048576
vfs.zfs.zfetch.arrayrdsz 1048576 vfs.zfs.zfetch.maxdistance 8388608
vfs.zfs.zfetch.minsecreap 2 vfs.zfs.zfetch.maxstreams 8
vfs.zfs.prefetchdisable 1 vfs.zfs.mdcompdisable 0
vfs.zfs.nopwriteenabled 1 vfs.zfs.dedup.prefetch 1 vfs.zfs.l2conlysize 0
vfs.zfs.mfughostdatalsize 0 vfs.zfs.mfughostmetadatalsize 0
vfs.zfs.mfughostsize 0 vfs.zfs.mfudatalsize 26300416
vfs.zfs.mfumetadatalsize 1780736 vfs.zfs.mfusize 29428736
vfs.zfs.mrughostdatalsize 0 vfs.zfs.mrughostmetadatalsize 0
vfs.zfs.mrughostsize 0 vfs.zfs.mrudatalsize 122090496
vfs.zfs.mrumetadatalsize 2235904 vfs.zfs.mrusize 139389440
vfs.zfs.anondatalsize 0 vfs.zfs.anonmetadatalsize 0 vfs.zfs.anonsize
163840 vfs.zfs.l2arcnorw 1 vfs.zfs.l2arcfeedagain 1
vfs.zfs.l2arcnoprefetch 1 vfs.zfs.l2arcfeedminms 200
vfs.zfs.l2arcfeedsecs 1 vfs.zfs.l2archeadroom 2 vfs.zfs.l2arcwriteboost
8388608 vfs.zfs.l2arcwritemax 8388608 vfs.zfs.arcmetalimit 766908416
vfs.zfs.arcfreetarget 7062 vfs.zfs.arcshrinkshift 7
vfs.zfs.arcaverageblocksize 8192 vfs.zfs.arcmin 383454208 vfs.zfs.arcmax
3067633664

When reading the tunable values, 0 means no, 1 typically means yes, and
any other number represents a value. To receive a brief description of a
“sysctl” value, use . For example:

\[commandchars=\
{}\] sysctl d vfs.zfs.zio.useuma vfs.zfs.zio.useuma: Use uma(9) for ZIO
allocations

The ZFS tunables require a fair understanding of how ZFS works, meaning
that reading man pages and searching for the meaning of unfamiliar
acronyms is required. If the tunable takes a numeric value (rather than
0 for no or 1 for yes), do not make one up. Instead, research examples
of beneficial values that match the workload.

If any of the ZFS tunables are changed, continue to monitor the system
to determine the effect of the change. It is recommended that the
changes are tested first at the command line using . For example, to
disable prefetch (i.e. change disable to or yes):

\[commandchars=\
{}\] sysctl vfs.zfs.prefetchdisable=1 vfs.zfs.prefetchdisable: 0 1

The output will indicate the old value followed by the new value. If the
change is not beneficial, change it back to the original value. If the
change turns out to be beneficial, it can be made permanent by creating
a using the instructions in ().

tw\_cli {#\detokenize{cli:tw-cli}}
-------

\[\]\[\] FreeNAS$^{\text{®}}$ includes the command line utility for
providing controller, logical unit, and drive management for AMCC/3ware
ATA RAID Controllers. The supported models are listed in the man pages
for the <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=twe)
and <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=twa)
drivers.

Before using this command, read its <span> </span>
(https://www.cyberciti.biz/files/tw\_cli.8.html) as it describes the
terminology and provides some usage examples.

When in Shell is entered, the prompt will change, indicating that
interactive mode is enabled where all sorts of maintenance commands on
the controller and its arrays can be run.

Alternately, one command can be specified to run. For example, to view
the disks in the array:

\[commandchars=\
{}\] twcli /c0 show Unit UnitType Status RCmpl V/I/M Stripe Size(GB)
Cache AVrfy u0 RAID6 OK 256K 5587.88 RiW ON u1 SPARE OK 931.505 OFF u2
RAID10 OK 256K 1862.62 RiW ON

VPort Status Unit Size Type Phy EnclSlot Model p8 OK u0 931.51 GB SAS
/c0/e0/slt0 SEAGATE ST31000640SS p9 OK u0 931.51 GB SAS /c0/e0/slt1
SEAGATE ST31000640SS p10 OK u0 931.51 GB SAS /c0/e0/slt2 SEAGATE
ST31000640SS p11 OK u0 931.51 GB SAS /c0/e0/slt3 SEAGATE ST31000640SS
p12 OK u0 931.51 GB SAS /c0/e0/slt4 SEAGATE ST31000640SS p13 OK u0
931.51 GB SAS /c0/e0/slt5 SEAGATE ST31000640SS p14 OK u0 931.51 GB SAS
/c0/e0/slt6 SEAGATE ST31000640SS p15 OK u0 931.51 GB SAS /c0/e0/slt7
SEAGATE ST31000640SS p16 OK u1 931.51 GB SAS /c0/e0/slt8 SEAGATE
ST31000640SS p17 OK u2 931.51 GB SATA /c0/e0/slt9 ST31000340NS p18 OK u2
931.51 GB SATA /c0/e0/slt10 ST31000340NS p19 OK u2 931.51 GB SATA
/c0/e0/slt11 ST31000340NS p20 OK u2 931.51 GB SATA /c0/e0/slt15
ST31000340NS

Name OnlineState BBUReady Status Volt Temp Hours LastCapTest bbu On Yes
OK OK OK 212 03Jan2012

Or, to review the event log:

\[commandchars=\
{}\] twcli /c0 show events Ctl Date Severity AEN Message c0 \[Thu Feb 23
2012 14:01:15\] INFO Battery charging started c0 \[Thu Feb 23 2012
14:03:02\] INFO Battery charging completed c0 \[Sat Feb 25 2012
00:02:18\] INFO Verify started: unit=0 c0 \[Sat Feb 25 2012 00:02:18\]
INFO Verify started: unit=2,subunit=0 c0 \[Sat Feb 25 2012 00:02:18\]
INFO Verify started: unit=2,subunit=1 c0 \[Sat Feb 25 2012 03:49:35\]
INFO Verify completed: unit=2,subunit=0 c0 \[Sat Feb 25 2012 03:51:39\]
INFO Verify completed: unit=2,subunit=1 c0 \[Sat Feb 25 2012 21:55:59\]
INFO Verify completed: unit=0 c0 \[Thu Mar 01 2012 13:51:09\] INFO
Battery health check started c0 \[Thu Mar 01 2012 13:51:09\] INFO
Battery health check completed c0 \[Thu Mar 01 2012 13:51:09\] INFO
Battery charging started c0 \[Thu Mar 01 2012 13:53:03\] INFO Battery
charging completed c0 \[Sat Mar 03 2012 00:01:24\] INFO Verify started:
unit=0 c0 \[Sat Mar 03 2012 00:01:24\] INFO Verify started:
unit=2,subunit=0 c0 \[Sat Mar 03 2012 00:01:24\] INFO Verify started:
unit=2,subunit=1 c0 \[Sat Mar 03 2012 04:04:27\] INFO Verify completed:
unit=2,subunit=0 c0 \[Sat Mar 03 2012 04:06:25\] INFO Verify completed:
unit=2,subunit=1 c0 \[Sat Mar 03 2012 16:22:05\] INFO Verify completed:
unit=0 c0 \[Thu Mar 08 2012 13:41:39\] INFO Battery charging started c0
\[Thu Mar 08 2012 13:43:42\] INFO Battery charging completed c0 \[Sat
Mar 10 2012 00:01:30\] INFO Verify started: unit=0 c0 \[Sat Mar 10 2012
00:01:30\] INFO Verify started: unit=2,subunit=0 c0 \[Sat Mar 10 2012
00:01:30\] INFO Verify started: unit=2,subunit=1 c0 \[Sat Mar 10 2012
05:06:38\] INFO Verify completed: unit=2,subunit=0 c0 \[Sat Mar 10 2012
05:08:57\] INFO Verify completed: unit=2,subunit=1 c0 \[Sat Mar 10 2012
15:58:15\] INFO Verify completed: unit=0

If the disks added to the array do not appear in the web interface, try
running this command:

\[commandchars=\
{}\] twcli /c0 rescan

Use the drives to create units and export them to the operating system.
When finished, run to make them available in the FreeNAS$^{\text{®}}$
web interface.

This <span> </span>
(https://forums.freenas.org/index.php?threads/3waredrivemonitoring.13835/)
contains a handy wrapper script that will give error notifications.

MegaCli {#\detokenize{cli:megacli}}
-------

\[\]\[\] is the command line interface for the Broadcom :MegaRAID SAS
family of RAID controllers. FreeNAS$^{\text{®}}$ also includes the
<span> </span> (https://www.freebsd.org/cgi/man.cgi?query=mfiutil)
utility which can be used to configure and manage connected storage
devices.

The command is quite complex with several dozen options. The commands
demonstrated in the <span> </span>
(http://tools.rapidsoft.de/perc/perccheatsheet.html) can get you
started.

freenasdebug {#\detokenize{cli:freenas-debug}}
------------

\[\]\[\] The FreeNAS$^{\text{®}}$ web interface provides an option to
save debugging information to a text file using . This debugging
information is created by the command line utility and a copy of the
information is saved to .

This command can be run manually from () to gather specific debugging
information. To see a usage explanation listing all options, run the
command without any options:

\[commandchars=\
{}\] freenasdebug Usage: /usr/local/bin/freenasdebug options Where
options are:

A Dump all debug information B Dump System Configuration Database C Dump
SMB Configuration I Dump IPMI Configuration M Dump SATA DOMs Information
N Dump NFS Configuration S Dump SMART Information T Loader Configuration
Information Z Remove old debug information a Dump Active Directory
Configuration c Dump (AD|LDAP) Cache e Email debug log to this
commadelimited list of email addresses f Dump AFP Configuration g Dump
GEOM Configuration h Dump Hardware Configuration i Dump iSCSI
Configuration j Dump Jail Information l Dump LDAP Configuration n Dump
Network Configuration s Dump SSL Configuration t Dump System Information
v Dump Boot System File Verification Status and Inconsistencies y Dump
Sysctl Configuration z Dump ZFS Configuration

Individual tests can be run alone. For example, when troubleshooting an
Active Directory configuration, use:

\[commandchars=\
{}\] freenasdebug a

To collect the output of every module, use :

\[commandchars=\
{}\] freenasdebug A

For collecting debug information about a single pool, use with followed
by the name of the pool:

\[commandchars=\
{}\] zdb U /data/zfs/zpool.cache pool1

See the <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=zdb)
for more information.

tmux {#\detokenize{cli:tmux}}
----

\[\]\[\] is a terminal multiplexer which enables a number of :terminals
to be created, accessed, and controlled from a single :screen. is an
alternative to GNU . Similar to screen, can be detached from a screen
and continue running in the background, then later reattached. Unlike
(), provides access to a command prompt while still giving access to the
graphical administration screens.

To start a session, simply type . As seen in , a new session with a
single window opens with a status line at the bottom of the screen. This
line shows information on the current session and is used to enter
interactive commands.

\[\]

To create a second window, press then . To close a window, type within
the window.

<span> </span>
(http://man.openbsd.org/cgibin/man.cgi/OpenBSDcurrent/man1/tmux.1?query=tmux)
lists all of the key bindings and commands for interacting with windows
and sessions.

If () is closed while is running, it will detach its session. The next
time Shell is open, run to return to the previous session. To leave the
session entirely, type . If multiple windows are running, it is required
to out of each first.

These resources provide more information about using :

-   <span> </span> (https://robots.thoughtbot.com/atmuxcrashcourse)

-   <span>
    </span> (http://blog.hawkhost.com/2010/06/28/tmuxtheterminalmultiplexer/)

Dmidecode {#\detokenize{cli:dmidecode}}
---------

\[\]\[\] Dmidecode reports hardware information as reported by the
system BIOS. Dmidecode does not scan the hardware, it only reports what
the BIOS told it to. A sample output can be seen <span> </span>
(http://www.nongnu.org/dmidecode/sample/dmidecode.txt).

To view the BIOS report, type the command with no arguments:

\[commandchars=\
{}\] dmidecode | more

<span> </span> (https://linux.die.net/man/8/dmidecode) describes the
supported strings and types.

Midnight Commander {#\detokenize{cli:midnight-commander}}
------------------

\[\]\[\] Midnight Commander is a program used to manage files from the
shell. Open the application by running . The arrow keys are used to
navigate and select files. Function keys are used to perform operations
such as renaming, editing, and copying files. These resources provide
more information about using Midnight Commander:

-   <span> </span> (https://en.wikipedia.org/wiki/Midnight\_Commander)

-   <span> </span> (https://midnightcommander.org/)

-   <span> </span> (https://www.freebsd.org/cgi/man.cgi?query=mc)

-   <span> </span> (http://linuxcommand.org/lc3\_adv\_mc.php)

ZFS Primer {#\detokenize{zfsprimer:zfs-primer}}
==========

\[\]\[\] ZFS is an advanced, modern filesystem that was specifically
designed to provide features not available in traditional UNIX
filesystems. It was originally developed at Sun with the intent to open
source the filesystem so that it could be ported to other operating
systems. After the Oracle acquisition of Sun, some of the original ZFS
engineers founded <span> </span> (http://openzfs.org/wiki/Main\_Page) to
provide continued, collaborative development of the open source version.

Here is an overview of the features provided by ZFS:

<span> </span>
(https://en.wikipedia.org/wiki/ZFS\#Copyonwrite\_transactional\_model)
filesystem. For each write request, a copy is made of the associated
disk blocks and all changes are made to the copy rather than to the
original blocks. When the write is complete, all block pointers are
changed to point to the new copy. This means that ZFS always writes to
free space, most writes are sequential, and old versions of files are
not unlinked until a complete new version has been written successfully.
ZFS has direct access to disks and bundles multiple read and write
requests into transactions. Most filesystems cannot do this, as they
only have access to disk blocks. A transaction either completes or
fails, meaning there will never be a <span> </span>
(https://blogs.oracle.com/bonwick/raidz) and a filesystem checker
utility is not necessary. Because of the transactional design, as
additional storage capacity is added, it becomes immediately available
for writes. To rebalance the data, one can copy it to rewrite the
existing data across all available disks. As a 128bit filesystem, the
maximum filesystem or file size is 16 exabytes.

. As ZFS writes data, it creates a checksum for each disk block it
writes. As ZFS reads data, it validates the checksum for each disk block
it reads. Media errors or “bit rot” can cause data to change, and the
checksum no longer matches. When ZFS identifies a disk block checksum
error on a pool that is mirrored or uses RAIDZ, it replaces the
corrupted data with the correct data. Since some disk blocks are rarely
read, regular scrubs should be scheduled so that ZFS can read all of the
data blocks to validate their checksums and correct any corrupted
blocks. While multiple disks are required in order to provide redundancy
and data correction, ZFS will still provide data corruption detection to
a system with one disk. FreeNAS$^{\text{®}}$ automatically schedules a
monthly scrub for each ZFS pool and the results of the scrub are
displayed by selecting the (), clicking (Settings), then the button.
Checking scrub results can provide an early indication of potential disk
problems.

Unlike traditional UNIX filesystems, . Instead, a group of disks, known
as a , are built into a ZFS . Filesystems are created from the pool as
needed. As more capacity is needed, identical vdevs can be striped into
the pool. In FreeNAS$^{\text{®}}$, () is used to create or extend pools.
After a pool is created, it can be divided into dynamicallysized
datasets or fixedsize zvols as needed. Datasets can be used to optimize
storage for the type of data being stored as permissions and properties
such as quotas and compression can be set on a perdataset level. A zvol
is essentially a raw, virtual block device which can be used for
applications that need rawdevice semantics such as iSCSI device extents.

. Compression happens when a block is written to disk, but only if the
written data will benefit from compression. When a compressed block is
accessed, it is automatically decompressed. Since compression happens at
the block level, not the file level, it is transparent to any
applications accessing the compressed data. ZFS pools created on
FreeNAS$^{\text{®}}$ version 9.2.1 or later use the recommended LZ4
compression algorithm.

of the specified pool, dataset, or zvol. Due to COW, snapshots initially
take no additional space. The size of a snapshot increases over time as
changes to the files in the snapshot are written to disk. Snapshots can
be used to provide a copy of data at the point in time the snapshot was
created. When a file is deleted, its disk blocks are added to the free
list; however, the blocks for that file in any existing snapshots are
not added to the free list until all referencing snapshots are removed.
This makes snapshots a clever way to keep a history of files, useful for
recovering an older copy of a file or a deleted file. For this reason,
many administrators take snapshots often, store them for a period of
time, and store them on another system. Such a strategy allows the
administrator to roll the system back to a specific time. If there is a
catastrophic loss, an offsite snapshot can restore the system up to the
last snapshot interval, within 15 minutes of the data loss, for example.
Snapshots are stored locally but can also be replicated to a remote ZFS
pool. During replication, ZFS does not do a byteforbyte copy but instead
converts a snapshot into a stream of data. This design means that the
ZFS pool on the receiving end does not need to be identical and can use
a different RAIDZ level, pool size, or compression settings.

. In FreeNAS$^{\text{®}}$, a snapshot of the dataset the operating
system resides on is automatically taken before an upgrade or a system
update. This saved boot environment is automatically added to the GRUB
boot loader. Should the upgrade or configuration change fail, simply
reboot and select the previous boot environment from the boot menu.
Users can also create their own boot environments in as needed, for
example before making configuration changes. This way, the system can be
rebooted into a snapshot of the system that did not include the new
configuration changes.

in RAM as well as a ZFS Intent Log (ZIL). The ZIL is a storage area that
<span> </span>
(https://pthree.org/2013/04/19/zfsadministrationappendixavisualizingthezfsintentlog/).
Adding a fast (lowlatency), powerprotected SSD as a SLOG () device
permits much higher performance. This is a necessity for NFS over ESXi,
and highly recommended for database servers or other applications that
depend on synchronous writes. More detail on SLOG benefits and usage is
available in these blog and forum posts:

-   <span>
    </span> (http://www.freenas.org/blog/zfszilandslogdemystified/)

-   <span>
    </span> (https://forums.freenas.org/index.php?threads/someinsightsintoslogzilwithzfsonfreenas.13633/)

-   <span> </span> (http://nex7.blogspot.com/2013/04/zfsintentlog.html)

Synchronous writes are relatively rare with SMB, AFP, and iSCSI, and
adding a SLOG to improve performance of these protocols only makes sense
in special cases. The utility can be run from () to determine if the
system will benefit from a SLOG. See <span> </span>
(http://www.richardelling.com/Home/scriptsandprograms1/zilstat) for
usage information.

ZFS currently uses 16 GiB of space for SLOG. Larger SSDs can be
installed, but the extra space will not be used. SLOG devices cannot be
shared between pools. Each pool requires a separate SLOG device.
Bandwidth and throughput limitations require that a SLOG device must
only be used for this single purpose. Do not attempt to add other
caching functions on the same SSD, or performance will suffer.

In missioncritical systems, a mirrored SLOG device is highly
recommended. Mirrored SLOG devices are for ZFS pools at ZFS version 19
or earlier. The ZFS pool version is checked from the () with . A version
value of means the ZFS pool is version 5000 (also known as ) or later.

in RAM, known as the ARC, which reduces read latency.
FreeNAS$^{\text{®}}$ adds ARC stats to <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=top) and includes the and
tools for monitoring the efficiency of the ARC. If an SSD is dedicated
as a cache device, it is known as an <span> </span>
(http://www.brendangregg.com/blog/20080722/zfsl2arc.html). Additional
read data is cached here, which can increase random read performance.
L2ARC does reduce the need for sufficient RAM. In fact, L2ARC needs RAM
to function. If there is not enough RAM for a adequatelysized ARC,
adding an L2ARC will not increase performance. Performance actually
decreases in most cases, potentially causing system instability. RAM is
always faster than disks, so always add as much RAM as possible before
considering whether the system can benefit from an L2ARC device.

When applications perform large amounts of reads on a dataset small
enough to fit into L2ARC, read performance can be increased by adding a
dedicated cache device. SSD cache devices only help if the active data
is larger than system RAM but small enough that a significant percentage
fits on the SSD. As a general rule, L2ARC should not be added to a
system with less than 32 GiB of RAM, and the size of an L2ARC should not
exceed ten times the amount of RAM. In some cases, it may be more
efficient to have two separate pools: one on SSDs for active data, and
another on hard drives for rarely used content. After adding an L2ARC
device, monitor its effectiveness using tools such as . To increase the
size of an existing L2ARC, stripe another cache device with it. The web
interface will always stripe L2ARC, not mirror it, as the contents of
L2ARC are recreated at boot. Failure of an individual SSD from an L2ARC
pool will not affect the integrity of the pool, but may have an impact
on read performance, depending on the workload and the ratio of dataset
size to cache size. Note that dedicated L2ARC devices cannot be shared
between ZFS pools.

such as the writehole and corrupt data written over time before the
hardware controller provides an alert. ZFS provides three levels of
redundancy, known as , where the number after the indicates how many
disks per vdev can be lost without losing data. ZFS also supports
mirrors, with no restrictions on the number of disks in the mirror. ZFS
was designed for commodity disks so no RAID controller is needed. While
ZFS can also be used with a RAID controller, it is recommended that the
controller be put into JBOD mode so that ZFS has full control of the
disks.

When determining the type of ZFS redundancy to use, consider whether the
goal is to maximize disk space or performance:

-   RAIDZ1 maximizes disk space and generally performs well when data is
    written and read in large chunks (128K or more).

-   RAIDZ2 offers better data availability and significantly better mean
    time to data loss (MTTDL) than RAIDZ1.

-   A mirror consumes more disk space but generally performs better with
    small random reads. For better performance, a mirror is strongly
    favored over any RAIDZ, particularly for large, uncacheable, random
    read loads.

-   Using more than 12 disks per vdev is not recommended. The
    recommended number of disks per vdev is between 3 and 9. With more
    disks, use multiple vdevs.

-   Some older ZFS documentation recommends that a certain number of
    disks is needed for each type of RAIDZ in order to achieve
    optimal performance. On systems using LZ4 compression, which is the
    default for FreeNAS$^{\text{®}}$ 9.2.1 and higher, this is no
    longer true. See <span> </span>
    (https://www.delphix.com/blog/delphixengineering/zfsraidzstripewidthorhowilearnedstopworryingandloveraidz)
    for details.

These resources can also help determine the RAID configuration best
suited to the specific storage requirements:

-   <span>
    </span> (https://forums.freenas.org/index.php?threads/gettingthemostoutofzfspools.16/)

-   <span>
    </span> (https://constantin.glez.de/2010/06/04/acloserlookzfsvdevsandperformance/)

<span>warning</span><span>Warning:</span> RAID AND DISK REDUNDANCY ARE
NOT A SUBSTITUTE FOR A RELIABLE BACKUP STRATEGY. BAD THINGS HAPPEN AND A
GOOD BACKUP STRATEGY IS STILL REQUIRED TO PROTECT VALUABLE DATA. See ()
and () to use replicated ZFS snapshots as part of a backup strategy.

. When an individual drive in a mirror or RAIDZ fails and is replaced by
the user, ZFS adds the replacement device to the vdev and copies
redundant data to it in a process called . Hardware RAID controllers
usually have no way of knowing which blocks were in use and must copy
every block to the new device. ZFS only copies blocks that are in use,
reducing the time it takes to rebuild the vdev. Resilvering is also
interruptable. After an interruption, resilvering resumes where it left
off rather than starting from the beginning.

While ZFS provides many benefits, there are some caveats:

-   At 90% capacity, ZFS switches from performance to spacebased
    optimization, which has massive performance implications. For
    maximum write performance and to prevent problems with drive
    replacement, add more capacity before a pool reaches 80%.

-   When considering the number of disks to use per vdev, consider the
    size of the disks and the amount of time required for resilvering,
    which is the process of rebuilding the vdev. The larger the size of
    the vdev, the longer the resilvering time. When replacing a disk in
    a RAIDZ, it is possible that another disk will fail before the
    resilvering process completes. If the number of failed disks exceeds
    the number allowed per vdev for the type of RAIDZ, the data in the
    pool will be lost. For this reason, RAIDZ1 is not recommended for
    drives over 1 TiB in size.

-   Using drives of equal sizes is recommended when creating a vdev.
    While ZFS can create a vdev using disks of differing sizes, its
    capacity will be limited by the size of the smallest disk.

For those new to ZFS, the <span> </span>
(https://en.wikipedia.org/wiki/Zfs) provides an excellent starting point
to learn more about its features. These resources are also useful for
reference:

-   <span> </span> (https://wiki.freebsd.org/ZFSTuningGuide)

-   <span>
    </span> (https://docs.oracle.com/cd/E1925301/8195461/index.html)

-   <span> </span> (https://www.youtube.com/watch?v=tPsV\_8kaVU) and
    <span> </span> (https://www.youtube.com/watch?v=wy6cJRVHiYU)

-   <span>
    </span> (https://www.freebsd.org/doc/en\_US.ISO88591/books/handbook/zfs.html)

-   <span>
    </span> (https://www.youtube.com/watch?v=aTXKxpL\_0OI&list=PL5AD0E43959919807)

-   <span> </span> (https://www.youtube.com/watch?v=ptY6K78McY)

ZFS Feature Flags {#\detokenize{zfsprimer:zfs-feature-flags}}
-----------------

\[\]\[\] To differentiate itself from Oracle ZFS version numbers,
OpenZFS uses feature flags. Feature flags are used to tag features with
unique names to provide portability between OpenZFS implementations
running on different platforms, as long as all of the feature flags
enabled on the ZFS pool are supported by both platforms.
FreeNAS$^{\text{®}}$ uses OpenZFS and each new version of
FreeNAS$^{\text{®}}$ keeps uptodate with the latest feature flags and
OpenZFS bug fixes.

See <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=zpoolfeatures) for a complete
listing of all OpenZFS feature flags available on FreeBSD.

OpenStack Cinder Driver {#\detokenize{cinder:openstack-cinder-driver}}
=======================

\[\]\[\]\[\] An open source, communitysupported FreeNAS$^{\text{®}}$
driver for OpenStack is available at .

VMware Recommendations {#\detokenize{vmware:vmware-recommendations}}
======================

\[\]\[\]\[\] This section offers FreeNAS$^{\text{®}}$ configuration
recommendations and troubleshooting tips when using FreeNAS$^{\text{®}}$
with a <span> </span> (https://www.vmware.com/) hypervisor.

FreeNAS$^{\text{®}}$ as a VMware Guest {#\detokenize{vmware:freenas-as-a-vmware-guest}}
--------------------------------------

\[\] This section has recommendations for configuring
FreeNAS$^{\text{®}}$ when it is installed as a Virtual Machine (VM) in
VMware.

To create a new FreeNAS$^{\text{®}}$ Virtual Machine in VMware, see the
() section of this guide.

Configure and use the <span> </span>
(https://www.freebsd.org/cgi/man.cgi?query=vmx) drivers for the
FreeNAS$^{\text{®}}$ system.

Network connection errors for plugins or jails inside the
FreeNAS$^{\text{®}}$ VM can be caused by a misconfigured <span>
ttps://pubs.vmware.com/vsphere-51/index.jsp?topic=%2Fcom.vmware.wssdk.pg.doc%2FPG\_Networking.11.4.html</span><span>virtual
switch</span>
(https://pubs.vmware.com/vsphere51/index.jsp?topic=%2Fcom.vmware.wssdk.pg.doc%2FPG\_Networking.11.4.html)
or <span> </span>
(https://pubs.vmware.com/vsphere4esxvcenter/index.jsp?topic=/com.vmware.vsphere.server\_configclassic.doc\_40/esx\_server\_config/networking/c\_port\_groups.html).
Make sure MAC spoofing and promiscuous mode are enabled on the switch
first, and then the port group the VM is using.

Hosting VMware Storage with FreeNAS$^{\text{®}}$ {#\detokenize{vmware:hosting-vmware-storage-with-freenas}}
------------------------------------------------

\[\] This section has recommendations for configuring
FreeNAS$^{\text{®}}$ when the system is being used as a VMware
datastore.

Make sure guest VMs have the latest version of installed. VMware
provides instructions to <span> </span>
(https://www.vmware.com/support/ws5/doc/new\_guest\_tools\_ws.html) on
different guest operating systems.

Increase the VM disk timeouts to better survive long disk operations.
Set the timeout to a minimum of . See the guest operating system
documentation for setting disk timeouts. VMware provides instructions
for setting disk timeouts on some specific guest operating systems:

-   Windows guest operating system:

-   Linux guests running kernel version :

When FreeNAS$^{\text{®}}$ is used as a VMware datastore, () can be used.

VAAI for iSCSI {#\detokenize{vmware:vaai-for-iscsi}}
--------------

\[\]\[\] VMware’s vStorage APIs for Array Integration, or , allows
storage tasks such as large data moves to be offloaded from the
virtualization hardware to the storage array. These operations are
performed locally on the NAS without transferring bulk data over the
network.

VAAI for iSCSI supports these operations:

-   () allows multiple initiators to synchronize LUN access in a
    finegrained manner rather than locking the whole LUN and preventing
    other hosts from accessing the same LUN simultaneously.

-   () copies disk blocks on the NAS. Copies occur locally rather than
    over the network. This operation is similar to <span>
    </span> (https://docs.microsoft.com/enus/previousversions/windows/itpro/windowsserver2012R2and2012/hh831628(v=ws.11)).

-   allows a hypervisor to query the NAS to determine whether a LUN is
    using thin provisioning.

-   pauses virtual machines when a pool runs out of space. The space
    issue can then be fixed and the virtual machines can continue rather
    than reporting write errors.

-   the system reports a warning when a configurable capacity
    is reached. In FreeNAS$^{\text{®}}$, this threshold is configured at
    the pool level when using zvols (see ) or at the extent level (see )
    for both file and device based extents. Typically, the warning is
    set at the pool level, unless file extents are used, in which case
    it must be set at the extent level.

-   informs FreeNAS$^{\text{®}}$ that the space occupied by deleted
    files should be freed. Without unmap, the NAS is unaware of freed
    space created when the initiator deletes files. For this feature to
    work, the initiator must support the unmap command.

-   or zeros out disk regions. When allocating virtual machines with
    thick provisioning, the zero write is done locally, rather than over
    the network. This makes virtual machine creation and any other
    zeroing of disk regions much quicker.

Using the API {#\detokenize{api:using-the-api}}
=============

\[\]\[\]\[\] A <span> </span>
(https://en.wikipedia.org/wiki/Representational\_state\_transfer) API is
provided to be used as an alternate mechanism for remotely controlling a
FreeNAS$^{\text{®}}$ system.

REST provides an easytoread, HTTP implementation of functions, known as
resources, which are available beneath a specified base URL. Each
resource is manipulated using the HTTP methods defined in <span> </span>
(https://tools.ietf.org/html/rfc2616.html), such as GET, PUT, POST, or
DELETE.

As shown in , an online version of the API is available at <span>
</span> (https://api.ixsystems.com/freenas/).

\[\]

The rest of this section shows code examples to illustrate the use of
the API.

<span>note</span><span>Note:</span> A new API was released with
FreeNAS$^{\text{®}}$ 11.1. The previous API is still present and in use
because it is featurecomplete. Documentation for the new API is
available on the FreeNAS$^{\text{®}}$ system at the URL. For example, if
the FreeNAS$^{\text{®}}$ system is at IP address 192.168.1.119, enter in
a browser to see the API documentation. Work is under way to make the
new API featurecomplete. The new APIv2 uses <span> </span>
(https://developer.mozilla.org/enUS/docs/Web/API/WebSockets\_API). This
advanced technology makes it possible to open interactive communication
sessions between web browsers and servers, allowing eventdriven
responses without the need to poll the server for a reply. When APIv2 is
featurecomplete, the FreeNAS$^{\text{®}}$ documentation will include
relevant examples that make use of the new API.

A Simple API Example {#\detokenize{api:a-simple-api-example}}
--------------------

\[\] The <span> </span>
(https://github.com/freenas/freenas/tree/master/examples/api) contains
some API usage examples. This section provides a walkthrough of the
script, shown below, as it provides a simple example that creates a
user.

A FreeNAS$^{\text{®}}$ system running at least version 9.2.0 is required
when creating a customized script based on this example. To test the
scripts directly on the FreeNAS$^{\text{®}}$ system, create a user
account and select an existing pool or dataset for the user . After
creating the user, start the SSH service in . That user will now be able
to to the IP address of the FreeNAS$^{\text{®}}$ system to create and
run scripts. Alternately, scripts can be tested on any system with the
required software installed as shown in the previous section.

To customize this script, copy the contents of this example into a
filename that ends in . The text that is highlighted in red below can be
modified in the new version to match the needs of the user being
created. Do not change the text in black. After saving changes, run the
script by typing . The new user account will appear in in the
FreeNAS$^{\text{®}}$ web interface.

Here is the example script with an explanation of the line numbers below
it.

\[commandchars=\
{},numbers=left,firstnumber=1,stepnumber=1\]

Where:

import the Python modules used to make HTTP requests and handle data in
JSON format.

replace with the value in . Note that the script will fail if the
machine running it is unable to resolve that hostname. Go to and set the
to .

replace with the password used to access the FreeNAS$^{\text{®}}$
system.

to force validation of the SSL certificate while using HTTPS, change to
.

set the values for the user being created. The user section at <span>
</span> (https://api.ixsystems.com/freenas/) describes this in more
detail. Allowed parameters are listed in the JSON Parameters section of
that resource. Since this resource creates a FreeBSD user, the values
entered must be valid for a FreeBSD user account. summarizes acceptable
values. This resource uses JSON, so the boolean values are or .

<span>|&gt;p<span>0.24-2</span> |&gt;p<span>0.12-2</span>
|&gt;p<span>0.64-2</span>|</span>

\[\]\
JSON Parameter & Type & Description\

\
JSON Parameter & Type & Description\

\

bsdusr\_username & string & Maximum 32 characters, though a maximum of 8
is recommended for interoperability. Can include numerals but cannot
include a space.\
bsdusr\_full\_name & string & May contain spaces and uppercase
characters.\
bsdusr\_password & string & Can include a mix of upper and lowercase
letters, characters, and numbers.\
bsdusr\_uid & integer & By convention, user accounts have an ID greater
than 1000 with a maximum allowable value of 65,535.\
bsdusr\_group & integer & If is set to , specify the numeric ID of the
group to create.\
bsdusr\_creategroup & boolean & Set to automatically create a primary
group with the same numeric ID as .\
bsdusr\_mode & string & Sets default numeric UNIX permissions of a user
home directory.\
bsdusr\_shell & string & Specify the full path to a UNIX shell that is
installed on the system.\
bsdusr\_password\_disabled & boolean & Set to to disable user login.\
bsdusr\_locked & boolean & Set to to disable user login.\
bsdusr\_sudo & boolean & Set to to enable for the user.\
bsdusr\_sshpubkey & string & Contents of SSH authorized keys file.\

<span>note</span><span>Note:</span> When using boolean values, JSON
returns raw lowercase values but Python uses uppercase values. So use or
in Python scripts even though the example JSON responses in the API
documentation are displayed as or .

A More Complex Example {#\detokenize{api:a-more-complex-example}}
----------------------

\[\] This section provides a walkthrough of a more complex example found
in the script. Use the search bar within the API documentation to
quickly locate the JSON parameters used here. This example defines a
class and several methods to create a ZFS pool, create a ZFS dataset,
share the dataset over CIFS, and enable the CIFS service. Responses from
some methods are used as parameters in other methods. In addition to the
import lines seen in the previous example, two Python modules are
imported to provide parsing functions for command line arguments:

\[commandchars=\
{}\]

It then creates a class which is started with the hostname, username,
and password provided by the user through the command line:

\[commandchars=\
{},numbers=left,firstnumber=1,stepnumber=1\]

A method is defined to get all the disks in the system as a response.
The method uses this information to create a ZFS pool named which is
created as a stripe. The and JSON parameters are described in the
resource of the API documentation.:

\[commandchars=\
{},numbers=left,firstnumber=1,stepnumber=1\]

The method is defined which creates a dataset named :

\[commandchars=\
{},numbers=left,firstnumber=1,stepnumber=1\]

The method is used to share with guestonly access enabled. The , , JSON
parameters, as well as the other allowable parameters, are described in
the resource of the API documentation.:

\[commandchars=\
{},numbers=left,firstnumber=1,stepnumber=1\]

Finally, the method enables the CIFS service. The JSON parameter is
described in the Services resource.

\[commandchars=\
{},numbers=left,firstnumber=1,stepnumber=1\]
