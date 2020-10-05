Sharing
=======

Shares provide and control access to an area of storage. Consider
factors like operating system, security, transfer speed, and user access
before creating a new share. This information can help determine the
type of share, if multiple datasets are needed to divide the storage
into areas with different access and permissions, and the complexity of
setting up permissions.

Note that shares are only used to provide access to data. Deleting a
share configuration does not affect the data that was being shared.

These types of shares and services are available:

-   `AFP <Apple (AFP) Shares>`: Apple Filing Protocol shares are used
    when the client computers all run macOS. Apple has deprecated AFP in
    favor of `SMB <Windows (SMB) Shares>`. Using AFP in modern networks
    is no longer recommended.
-   `Unix (NFS) <Unix (NFS) Shares>`: Network File System shares are
    accessible from macOS, Linux, BSD, and the professional and
    enterprise versions (but not the home editions) of Windows. This can
    be a good choice when the client computers do not all run the same
    operating system but NFS client software is available for all of
    them.
-   `WebDAV <WebDAV Shares>`: WebDAV shares are accessible using an
    authenticated web browser (read-only) or [WebDAV client][] running
    on any operating system.
-   `SMB <Windows (SMB) Shares>`: Server Message Block shares, also
    known as Common Internet File System (CIFS) shares, are accessible
    by Windows, macOS, Linux, and BSD computers. Access is slower than
    an NFS share due to the single-threaded design of Samba. SMB
    provides more configuration options than NFS and is a good choice on
    a network for Windows or Mac systems. However, it is a poor choice
    if the CPU on the FreeNAS<sup>®</sup> system is limited. If it is
    maxed out, upgrade the CPU or consider a different type of share.
-   `Block (iSCSI)`: Block or iSCSI shares appear as an unformatted disk
    to clients running iSCSI initiator software or a virtualization
    solution such as VMware. These are usually used as virtual drives.

  [WebDAV client]: https://en.wikipedia.org/wiki/WebDAV#Client_support

Fast access from any operating system can be obtained by configuring the
`FTP` service instead of a share and using a cross-platform FTP file
manager application such as [Filezilla][]. Secure FTP can be configured
if the data needs to be encrypted.

  [Filezilla]: https://filezilla-project.org/

When data security is a concern and the network users are familiar with
SSH command line utilities or [WinSCP][], consider using the `SSH`
service instead of a share. It is slower than unencrypted FTP due to the
encryption overhead, but the data passing through the network is
encrypted.

  [WinSCP]: https://winscp.net/eng/index.php

<div class="note">

<div class="title">

Note

</div>

It is generally a mistake to share a pool or dataset with more than one
share type or access method. Different types of shares and services use
different file locking methods. For example, if the same pool is
configured to use both NFS and FTP, NFS will lock a file for editing by
an NFS user, but an FTP user can simultaneously edit or delete that
file. This results in lost edits and confused users. Another example: if
a pool is configured for both AFP and SMB, Windows users can be confused
by the "extra" filenames used by Mac files and delete them. This
corrupts the files on the AFP share. Pick the one type of share or
service that makes the most sense for the types of clients accessing
that pool, and use that single type of share or service. To support
multiple types of shares, divide the pool into datasets and use one
dataset per share.

</div>

This section demonstrates configuration and fine-tuning of AFP, NFS,
SMB, WebDAV, and iSCSI shares. FTP and SSH configurations are described
in `Services`.

<div class="index">

AFP, Apple Filing Protocol

</div>

Apple (AFP) Shares
------------------

FreeNAS<sup>®</sup> uses the [Netatalk][] AFP server to share data with
Apple systems. This section describes the configuration screen for
fine-tuning AFP shares. It then provides configuration examples for
configuring Time Machine to back up to a dataset on the
FreeNAS<sup>®</sup> system and for connecting to the share from a macOS
client.

  [Netatalk]: http://netatalk.sourceforge.net/

Create a share by clicking `Sharing --> Apple (AFP)`, then .

New AFP shares are visible in the `Sharing --> Apple (AFP)` menu.

The configuration options shown in `Figure %s <creating_afp_share_fig>`
appear after clicking on an existing share, and selecting the `Edit`
option. The values showing for these options will vary, depending upon
the information given when the share was created.

<div id="creating_afp_share_fig">

![Creating an AFP Share][]

</div>

  [Creating an AFP Share]: images/sharing-apple-afp-add.png

<div class="note">

<div class="title">

Note

</div>

`Table %s <afp_share_config_opts_tab>` summarizes the options available
to fine-tune an AFP share. Leaving these options at the default settings
is recommended as changing them can cause unexpected behavior. Most
settings are only available with `Advanced Mode`. Do **not** change an
advanced option without fully understanding the function of that option.
Refer to [Setting up Netatalk][] for a more detailed explanation of
these options.

</div>

  [Setting up Netatalk]: http://netatalk.sourceforge.net/2.2/htmldocs/configuration.html

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.14linewidth-2tabcolsep}
&gt;{RaggedRight}p{dimexpr 0.54linewidth-2tabcolsep}\|

</div>

<div id="afp_share_config_opts_tab">

| Setting                       | Value         | Advanced Mode | Description                                                                                                                                                                                                                                                        |
|-------------------------------|---------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Path                          | browse button |               | Browse to the pool or dataset to share. Do not nest additional pools, datasets, or symbolic links beneath this path because Netatalk does not fully support that.                                                                                                  |
| Name                          | string        |               | Enter the pool name that appears in macOS after selecting `Go --> Connect to server` in the Finder menu. Limited to 27 characters and cannot contain a period.                                                                                                     |
| Comment                       | string        | ✓             | Optional comment.                                                                                                                                                                                                                                                  |
| Allow list                    | string        | ✓             | Comma-delimited list of allowed users and/or groups where groupname begins with a `@`. Note that adding an entry will deny any user/group that is not specified.                                                                                                   |
| Deny list                     | string        | ✓             | Comma-delimited list of denied users and/or groups where groupname begins with a `@`. Note that adding an entry will allow all users/groups that are not specified.                                                                                                |
| Read Only Access              | string        | ✓             | Comma-delimited list of users and/or groups who only have read access where groupname begins with a `@`.                                                                                                                                                           |
| Read/Write Access             | string        | ✓             | Comma-delimited list of users and/or groups who have read and write access where groupname begins with a `@`.                                                                                                                                                      |
| Time Machine                  | checkbox      |               | Set to advertise FreeNAS<sup>®</sup> as a Time Machine disk so it can be found by Macs. Setting multiple shares for Time Machine use is not recommended. When multiple Macs share the same pool, low diskspace issues and intermittently failed backups can occur. |
| Time Machine Quota            | integer       |               | Appears when `Time Machine` is set. Enter a storage quota for each Time Machine backup on this share. The share must be remounted for any changes to this value to take effect.                                                                                    |
| Use as home share             | checkbox      |               | Allows the share to host user home directories. Each user is given a personal home directory when connecting to the share which is not accessible by other users. This allows for a personal, dynamic share. Only one share can be used as the home share.         |
| Zero Device Numbers           | checkbox      | ✓             | Enable when the device number is not constant across a reboot.                                                                                                                                                                                                     |
| No Stat                       | checkbox      | ✓             | If set, AFP does not stat the pool path when enumerating the pools list. Useful for automounting or pools created by a preexec script.                                                                                                                             |
| AFP3 UNIX Privs               | checkbox      | ✓             | Set to enable Unix privileges supported by Mac OS X 10.5 and higher. Do not enable if the network has Mac OS X 10.4 or lower clients. Those systems do not support this feature.                                                                                   |
| Default file permissions      | checkboxes    | ✓             | Only works with Unix ACLs. New files created on the share are set with the selected permissions.                                                                                                                                                                   |
| Default directory permissions | checkboxes    | ✓             | Only works with Unix ACLs. New directories created on the share are set with the selected permissions.                                                                                                                                                             |
| Default umask                 | integer       | ✓             | Umask is used for newly created files. Default is *000* (anyone can read, write, and execute).                                                                                                                                                                     |
| Hosts Allow                   | string        | ✓             | Enter a list of allowed hostnames or IP addresses. Separate entries with a comma, space, or tab. Please see the `note <afp allow/deny note>` for more information.                                                                                                 |
| Hosts Deny                    | string        | ✓             | Enter a list of denied hostnames or IP addresses. Separate entries with a comma, space, or tab. Please see the `note <afp allow/deny note>` for more information.                                                                                                  |
| Auxiliary Parameters          | string        | ✓             | Enter any additional [afp.conf][] parameters not covered by other option fields.                                                                                                                                                                                   |

AFP Share Configuration Options

</div>

  [afp.conf]: https://www.freebsd.org/cgi/man.cgi?query=afp.conf

<div id="afp allow/deny note" class="note">

<div class="title">

Note

</div>

If neither *Hosts Allow* or *Hosts Deny* contains an entry, then AFP
share access is allowed for any host.

If there is a *Hosts Allow* list but no *Hosts Deny* list, then only
allow hosts on the *Hosts Allow* list.

If there is a *Hosts Deny* list but no *Hosts Allow* list, then allow
all hosts that are not on the *Hosts Deny* list.

If there is both a *Hosts Allow* and *Hosts Deny* list, then allow all
hosts that are on the *Hosts Allow* list. If there is a host not on the
*Hosts Allow* and not on the *Hosts Deny* list, then allow it.

</div>

### Creating AFP Guest Shares

AFP supports guest logins, meaning that macOS users can access the AFP
share without requiring their user accounts to first be created on or
imported into the FreeNAS<sup>®</sup> system.

<div class="note">

<div class="title">

Note

</div>

When a guest share is created along with a share that requires
authentication, AFP only maps users who log in as *guest* to the guest
share. If a user logs in to the share that requires authentication,
permissions on the guest share can prevent that user from writing to the
guest share. The only way to allow both guest and authenticated users to
write to a guest share is to set the permissions on the guest share to
*777* or to add the authenticated users to a guest group and set the
permissions to *77x*.

</div>

Before creating a guest share, go to `Services --> AFP` and click the
sliding button to turn on the service. Click to open the screen shown in
`Figure %s <creating_guest_afp_share_fig>`. For `Guest Account`, use the
drop-down to select `Nobody`, set `Guest Access`, and click `SAVE`.

<div id="creating_guest_afp_share_fig">

![Creating a Guest AFP Share][]

</div>

  [Creating a Guest AFP Share]: images/services-afp-guest.png

Next, create a dataset for the guest share. Refer to `Adding Datasets`
for more information about dataset creation.

After creating the dataset for the guest share, go to
`Storage --> Pools`, click the button for the dataset, then click
`Edit Permissions`. Complete the fields shown in
`Figure %s <creating_guest_afp_dataset_fig>`.

1.  **User:** Use the drop-down to select `Nobody`.
2.  Click `SAVE`.

<div id="creating_guest_afp_dataset_fig">

![Editing Dataset Permissions for Guest AFP Share][]

</div>

  [Editing Dataset Permissions for Guest AFP Share]: images/sharing-afp-dataset-permissions.png

To create a guest AFP share:

1.  Go to `Sharing --> Apple (AFP) Shares` and click .
2.  `Browse` to the dataset created for the guest share.
3.  Fill out the other required fields, then press `SAVE`.

macOS users can use Finder to connect to the guest AFP share by clicking
`Go --> Connect to Server`. In the example shown in
`Figure %s <afp_connect_server_fig>`, the user entered `afp://` followed
by the IP address of the FreeNAS<sup>®</sup> system.

Click the `Connect` button. Once connected, Finder opens automatically.
The name of the AFP share is displayed in the SHARED section in the left
frame and the contents of any data saved in the share is displayed in
the right frame.

<div id="afp_connect_server_fig">

![Connect to Server Dialog][]

</div>

  [Connect to Server Dialog]: images/external/sharing-afp-connect-server.png

To disconnect from the pool, click the `eject` button in the `Shared`
sidebar.

<div class="index">

iSCSI, Internet Small Computer System Interface

</div>

Block (iSCSI)
-------------

iSCSI is a protocol standard for the consolidation of storage data.
iSCSI allows FreeNAS<sup>®</sup> to act like a storage area network
(SAN) over an existing Ethernet network. Specifically, it exports disk
devices over an Ethernet network that iSCSI clients (called initiators)
can attach to and mount. Traditional SANs operate over fibre channel
networks which require a fibre channel infrastructure such as fibre
channel HBAs, fibre channel switches, and discrete cabling. iSCSI can be
used over an existing Ethernet network, although dedicated networks can
be built for iSCSI traffic in an effort to boost performance. iSCSI also
provides an advantage in an environment that uses Windows shell
programs; these programs tend to filter "Network Location" but iSCSI
mounts are not filtered.

Before configuring the iSCSI service, be familiar with this iSCSI
terminology:

**CHAP:** an authentication method which uses a shared secret and
three-way authentication to determine if a system is authorized to
access the storage device and to periodically confirm that the session
has not been hijacked by another system. In iSCSI, the initiator
(client) performs the CHAP authentication.

**Mutual CHAP:** a superset of CHAP in that both ends of the
communication authenticate to each other.

**Initiator:** a client which has authorized access to the storage data
on the FreeNAS<sup>®</sup> system. The client requires initiator
software to initiate the connection to the iSCSI share.

**Target:** a storage resource on the FreeNAS<sup>®</sup> system. Every
target has a unique name known as an iSCSI Qualified Name (IQN).

**Internet Storage Name Service (iSNS):** protocol for the automated
discovery of iSCSI devices on a TCP/IP network.

**Extent:** the storage unit to be shared. It can either be a file or a
device.

**Portal:** indicates which IP addresses and ports to listen on for
connection requests.

**LUN:** *Logical Unit Number* representing a logical SCSI device. An
initiator negotiates with a target to establish connectivity to a LUN.
The result is an iSCSI connection that emulates a connection to a SCSI
hard disk. Initiators treat iSCSI LUNs as if they were a raw SCSI or
SATA hard drive. Rather than mounting remote directories, initiators
format and directly manage filesystems on iSCSI LUNs. When configuring
multiple iSCSI LUNs, create a new target for each LUN. Since iSCSI
multiplexes a target with multiple LUNs over the same TCP connection,
there can be TCP contention when more than one target accesses the same
LUN. FreeNAS<sup>®</sup> supports up to 1024 LUNs.

In FreeNAS<sup>®</sup>, iSCSI is built into the kernel. This version of
iSCSI supports [Microsoft Offloaded Data Transfer (ODX)][], meaning that
file copies happen locally, rather than over the network. It also
supports the `VAAI <VAAI_for_iSCSI>` (vStorage APIs for Array
Integration) primitives for efficient operation of storage tasks
directly on the NAS. To take advantage of the VAAI primitives,
`create a zvol <Adding Zvols>` and use it to
`create a device extent <Extents>`.

  [Microsoft Offloaded Data Transfer (ODX)]: https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831628(v=ws.11)

### iSCSI Wizard

To configure iSCSI, click `WIZARD` and follow each step:

1.  **Create or Choose Block Device**:
    -   `Name`: Enter a name for the block device. Keeping the name
        short is recommended. Using a name longer than 63 characters can
        prevent access to the block device.
    -   `Type`: Select *File* or *Device* as the type of block device.
        *Device* provides virtual storage access to zvols, zvol
        snapshots, or physical devices. *File* provides virtual storage
        access to an individual file.
    -   `Device`: Select the unformatted disk, controller, zvol, or zvol
        snapshot. Select *Create New* for options to create a new zvol.
        If *Create New* is selected, use the browser to select an
        existing pool or dataset to store the new zvol. Enter the
        desired size of the zvol in `Size`. Only displayed when `Type`
        is set to *Device*.
    -   `File`: Browse to an existing file. Create a new file by
        browsing to a dataset and appending the file name to the path.
        When the file already exists, enter a size of *0* to use the
        actual file size. For new files, enter the size of the file to
        create. Only displayed when `Type` is set to *File*.
    -   `What are you using this for`: Choose the platform that will use
        this share. The associated options are applied to this share.
2.  **Portal**
    -   `Portal`: Select an existing portal or choose *Create New* to
        configure a new portal.
    -   `Discovery Auth Method`: *NONE* allows anonymous discovery while
        *CHAP* and *Mutual CHAP* require authentication.
    -   `Discovery Auth Group`: Choose an existing `Authorized Access`
        group ID or create a new authorized access. This is required
        when the `Discovery Auth Method` is set to *CHAP* or *Mutual
        CHAP*.
    -   `IP`: Select IP addresses to be listened on by the portal. Click
        `ADD` to add IP addresses with a different network port. The
        address `0.0.0.0` can be selected to listen on all IPv4
        addresses, or `::` to listen on all IPv6 addresses.
    -   `Port`: TCP port used to access the iSCSI target. Default is
        *3260*.
3.  **Initiator**
    -   `Initiators`: Leave blank to allow all or enter a list of
        initiator hostnames separated by spaces.
    -   `Authorized Networks`: Network addresses allowed to use this
        initiator. Leave blank to allow all networks or list network
        addresses with a CIDR mask. Separate multiple addresses with a
        space: `192.168.2.0/24 192.168.2.1/12`.
4.  **Confirm Options**
    -   Review the configuration and click `SUBMIT` to set up the iSCSI
        share.

The rest of this section describes iSCSI configuration in more detail.

### Target Global Configuration

`Sharing --> Block (iSCSI) --> Target Global Configuration` contains
settings that apply to all iSCSI shares.
`Table %s <iscsi_targ_global_config_tab>` describes each option.

Some built-in values affect iSNS usage. Fetching of allowed initiators
from iSNS is not implemented, so target ACLs must be configured
manually. To make iSNS registration useful, iSCSI targets should have
explicitly configured port IP addresses. This avoids initiators
attempting to discover unconfigured target portal addresses like
*0.0.0.0*.

The iSNS registration period is *900* seconds. Registered Network
Entities not updated during this period are unregistered. The timeout
for iSNS requests is *5* seconds.

> iSCSI Target Global Configuration Variables

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.12linewidth-2tabcolsep}

</div>

<div id="iscsi_targ_global_config_tab">

| Setting                        | Value   | Description                                                                                                                                                                                                            |
|--------------------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Base Name                      | string  | Lowercase alphanumeric characters plus dot (.), dash (-), and colon (:) are allowed. See the "Constructing iSCSI names using the iqn. format" section of `3721`.                                                       |
| ISNS Servers                   | string  | Enter the hostnames or IP addresses of ISNS servers to be registered with iSCSI targets and portals of the system. Separate each entry with a space.                                                                   |
| Pool Available Space Threshold | integer | Enter the percentage of free space to remain in the pool. When this percentage is reached, the system issues an alert, but only if zvols are used. See `VAAI <VAAI_for_iSCSI>` Threshold Warning for more information. |

Target Global Configuration Settings

</div>

### Portals

A portal specifies the IP address and port number to be used for iSCSI
connections. Go to `Sharing --> Block (iSCSI) --> Portals` and click to
display the screen shown in `Figure %s <iscsi_add_portal_fig>`.

`Table %s <iscsi_add_portal_fig>` summarizes the settings that can be
configured when adding a portal.

<div id="iscsi_add_portal_fig">

![Adding an iSCSI Portal][]

</div>

  [Adding an iSCSI Portal]: images/sharing-block-iscsi-portals-add.png

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.12linewidth-2tabcolsep}

</div>

<div id="iscsi_portal_conf_tab">

| Setting               | Value          | Description                                                                                                                                                                                                                         |
|-----------------------|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description           | string         | Optional description. Portals are automatically assigned a numeric group.                                                                                                                                                           |
| Discovery Auth Method | drop-down menu | `iSCSI` supports multiple authentication methods that are used by the target to discover valid devices. *None* allows anonymous discovery while *CHAP* and *Mutual CHAP* both require authentication.                               |
| Discovery Auth Group  | drop-down menu | Select a Group ID created in `Authorized Access` if the `Discovery Auth Method` is set to *CHAP* or *Mutual CHAP*.                                                                                                                  |
| IP address            | drop-down menu | Select IP addresses to be listened on by the portal. Click `ADD` to add IP addresses with a different network port. The address `0.0.0.0` can be selected to listen on all IPv4 addresses, or `::` to listen on all IPv6 addresses. |
| Port                  | integer        | TCP port used to access the iSCSI target. Default is *3260*.                                                                                                                                                                        |

Portal Configuration Settings

</div>

FreeNAS<sup>®</sup> systems with multiple IP addresses or interfaces can
use a portal to provide services on different interfaces or subnets.
This can be used to configure multi-path I/O (MPIO). MPIO is more
efficient than a link aggregation.

If the FreeNAS<sup>®</sup> system has multiple configured interfaces,
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
address **except** for the one used by a management interface. This
would prevent iSCSI connections to the management interface.

### Initiators

The next step is to configure authorized initiators, or the systems
which are allowed to connect to the iSCSI targets on the
FreeNAS<sup>®</sup> system. To configure which systems can connect, go
to `Sharing --> Block (iSCSI) --> Initiators` and click as shown in
`Figure %s <iscsi_add_initiator_fig>`.

<div id="iscsi_add_initiator_fig">

![Adding an iSCSI Initiator][]

</div>

  [Adding an iSCSI Initiator]: images/sharing-block-iscsi-initiators-add.png

`Table %s <iscsi_initiator_conf_tab>` summarizes the settings that can
be configured when adding an initiator.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.12linewidth-2tabcolsep}

</div>

<div id="iscsi_initiator_conf_tab">

| Setting                  | Value    | Description                                                                                                                                                                                                                                                            |
|--------------------------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allow All Initiators     | checkbox | Accept all detected initiators. When set, all other initiator fields are disabled.                                                                                                                                                                                     |
| Connected Initiators     | string   | Initiators currently connected to the system. Shown in IQN format with an IP address. Set initiators and click an to add the initiators to either the `Allowed Initiators` or `Authorized Networks` lists. Clicking `REFRESH` updates the `Connected Initiators` list. |
| Allowed Initiators (IQN) | string   | Initiators allowed access to this system. Enter an [iSCSI Qualified Name (IQN)][] and click `+` to add it to the list. Example: `{iqn.1994-09.org.freebsd:freenas.local}`                                                                                              |
| Authorized Networks      | string   | Network addresses allowed to use this initiator. Each address can include an optional [CIDR][] netmask. Click `+` to add the network address to the list. Example: `{192.168.2.0/24}`                                                                                  |
| Description              | string   | Any notes about initiators.                                                                                                                                                                                                                                            |

Initiator Configuration Settings

</div>

  [iSCSI Qualified Name (IQN)]: https://tools.ietf.org/html/rfc3720#section-3.2.6
  [CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing

Click on an initiator entry for options to `Edit` or `Delete` it.

### Authorized Access

When using CHAP or mutual CHAP to provide authentication, creating
authorized access is recommended. Do this by going to
`Sharing --> Block (iSCSI) --> Authorized Access` and clicking . The
screen is shown in `Figure %s <iscsi_add_auth_access_fig>`.

<div class="note">

<div class="title">

Note

</div>

This screen sets login authentication. This is different from discovery
authentication which is set in `Global Configuration`.

</div>

<div id="iscsi_add_auth_access_fig">

![Adding an iSCSI Authorized Access][]

</div>

  [Adding an iSCSI Authorized Access]: images/sharing-block-iscsi-authorized-access-add.png

`Table %s <iscsi_auth_access_config_tab>` summarizes the settings that
can be configured when adding an authorized access:

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.16linewidth-2tabcolsep}

</div>

<div id="iscsi_auth_access_config_tab">

| Setting     | Value   | Description                                                                                                                                                                                                                                                   |
|-------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Group ID    | integer | Allow different groups to be configured with different authentication profiles. Example: enter *1* for all users in Group *1* to inherit the Group *1* authentication profile. Group IDs that are already configured with authorized access cannot be reused. |
| User        | string  | User account to create for CHAP authentication with the user on the remote system. Many initiators use the initiator name as the user name.                                                                                                                   |
| Secret      | string  | `User` password. Must be at least *12* and no more than *16* characters long.                                                                                                                                                                                 |
| Peer User   | string  | Only entered when configuring mutual CHAP. Usually the same value as `User`.                                                                                                                                                                                  |
| Peer Secret | string  | Mutual secret password. Required when `Peer User` is set. Must be different than the `Secret`. Must be at least *12* and no more than *16* characters long.                                                                                                   |

Authorized Access Configuration Settings

</div>

<div class="note">

<div class="title">

Note

</div>

CHAP does not work with GlobalSAN initiators on macOS.

</div>

New authorized accesses are visible from the
`Sharing --> Block (iSCSI) --> Authorized Access` menu. In the example
shown in `Figure %s <iscsi_view_auth_access_fig>`, three users (*test1*,
*test2*, and *test3*) and two groups (*1* and *2*) have been created,
with group 1 consisting of one CHAP user and group 2 consisting of one
mutual CHAP user and one CHAP user. Click an authorized access entry to
display its `Edit` and `Delete` buttons.

<div id="iscsi_view_auth_access_fig">

![Viewing Authorized Accesses][]

</div>

  [Viewing Authorized Accesses]: images/sharing-block-iscsi-authorized-access-example.png

### Targets

Next, create a Target by going to
`Sharing --> Block (iSCSI) --> Targets` and clicking as shown in
`Figure %s <iscsi_add_target_fig>`. A target combines a portal ID,
allowed initiator ID, and an authentication method.
`Table %s <iscsi_target_settings_tab>` summarizes the settings that can
be configured when creating a Target.

<div class="note">

<div class="title">

Note

</div>

An iSCSI target creates a block device that may be accessible to
multiple initiators. A clustered filesystem is required on the block
device, such as VMFS used by VMware ESX/ESXi, in order for multiple
initiators to mount the block device read/write. If a traditional
filesystem such as EXT, XFS, FAT, NTFS, UFS, or ZFS is placed on the
block device, care must be taken that only one initiator at a time has
read/write access or the result will be filesystem corruption. If
multiple clients need access to the same data on a non-clustered
filesystem, use SMB or NFS instead of iSCSI, or create multiple iSCSI
targets (one per client).

</div>

<div id="iscsi_add_target_fig">

![Adding an iSCSI Target][]

</div>

  [Adding an iSCSI Target]: images/sharing-block-iscsi-targets-add.png

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.12linewidth-2tabcolsep}

</div>

<div id="iscsi_target_settings_tab">

| Setting                     | Value          | Description                                                                                                                                                                                                                                                       |
|-----------------------------|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Target Name                 | string         | Required. The base name is automatically prepended if the target name does not start with *iqn*. Lowercase alphanumeric characters plus dot (.), dash (-), and colon (:) are allowed. See the "Constructing iSCSI names using the iqn. format" section of `3721`. |
| Target Alias                | string         | Enter an optional user-friendly name.                                                                                                                                                                                                                             |
| Portal Group ID             | drop-down menu | Leave empty or select number of existing portal to use.                                                                                                                                                                                                           |
| Initiator Group ID          | drop-down menu | Select which existing initiator group has access to the target.                                                                                                                                                                                                   |
| Auth Method                 | drop-down menu | *None*, *Auto*, *CHAP*, or *Mutual CHAP*.                                                                                                                                                                                                                         |
| Authentication Group number | drop-down menu | Select *None* or an integer. This number represents the number of existing authorized accesses.                                                                                                                                                                   |

Target Settings

</div>

### Extents

iSCSI targets provide virtual access to resources on the
FreeNAS<sup>®</sup> system. *Extents* are used to define resources to
share with clients. There are two types of extents: *device* and *file*.

**Device extents** provide virtual storage access to zvols, zvol
snapshots, or physical devices like a disk, an SSD, or a hardware RAID
volume.

**File extents** provide virtual storage access to an individual file.

<div class="tip">

<div class="title">

Tip

</div>

**For typical use as storage for virtual machines where the
virtualization software is the iSCSI initiator, device extents with
zvols provide the best performance and most features.** For other
applications, device extents sharing a raw device can be appropriate.
File extents do not have the performance or features of device extents,
but do allow creating multiple extents on a single filesystem.

</div>

Virtualized zvols support all the FreeNAS<sup>®</sup>
`VAAI <VAAI_for_iSCSI>` primitives and are recommended for use with
virtualization software as the iSCSI initiator.

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

<div class="warning">

<div class="title">

Warning

</div>

For performance reasons and to avoid excessive fragmentation, keep the
used space of the pool below 80% when using iSCSI. The capacity of an
existing extent can be increased as shown in `Growing LUNs`.

</div>

To add an extent, go to `Sharing --> Block (iSCSI) --> Extents` and
click . In the example shown in `Figure %s <iscsi_adding_extent_fig>`,
the device extent is using the `export` zvol that was previously created
from the `/mnt/pool1` pool.

`Table %s <iscsi_extent_conf_tab>` summarizes the settings that can be
configured when creating an extent. Note that **file extent creation
fails unless the name of the file to be created is appended to the pool
or dataset name.**

<div id="iscsi_adding_extent_fig">

![Adding an iSCSI Extent][]

</div>

  [Adding an iSCSI Extent]: images/sharing-block-iscsi-extents-add.png

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.12linewidth-2tabcolsep}

</div>

<div id="iscsi_extent_conf_tab">

| Setting                               | Value          | Description                                                                                                                                                                                                      |
|---------------------------------------|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Extent name                           | string         | Enter the extent name. If the `Extent size` is not *0*, it cannot be an existing file within the pool or dataset.                                                                                                |
| Extent type                           | drop-down menu | *File* shares the contents of an individual file. *Device* shares an entire device.                                                                                                                              |
| Path to the extent                    | browse button  | Only appears when *File* is selected. Browse to an existing file. Create a new file by browsing to a dataset and appending the file name to the path. Extents cannot be created inside a jail root directory.    |
| Extent size                           | integer        | Only appears when *File* is selected. Entering *0* uses the actual file size and requires that the file already exists. Otherwise, specify the file size for the new file.                                       |
| Device                                | drop-down menu | Only appears when *Device* is selected. Select the unformatted disk, controller, zvol, or zvol snapshot.                                                                                                         |
| Logical block size                    | drop-down menu | Leave at the default of 512 unless the initiator requires a different block size.                                                                                                                                |
| Disable physical block size reporting | checkbox       | Set if the initiator does not support physical block size values over 4K (MS SQL). Setting can also prevent [constant block size warnings][] when using this share with ESXi.                                    |
| Available space threshold             | string         | Only appears if *File* or a zvol is selected. When the specified percentage of free space is reached, the system issues an alert. See `VAAI <VAAI_for_iSCSI>` Threshold Warning.                                 |
| Description                           | string         | Notes about this extent.                                                                                                                                                                                         |
| Enable TPC                            | checkbox       | Set to allow an initiator to bypass normal access control and access any scannable target. This allows [xcopy][] operations which are otherwise blocked by access control.                                       |
| Xen initiator compat mode             | checkbox       | Set when using Xen as the iSCSI initiator.                                                                                                                                                                       |
| LUN RPM                               | drop-down menu | Do **NOT** change this setting when using Windows as the initiator. Only needs to be changed in large environments where the number of systems using a specific RPM is needed for accurate reporting statistics. |
| Read-only                             | checkbox       | Set to prevent the initiator from initializing this LUN.                                                                                                                                                         |
| Enable                                | checkbox       | Set to enable the iSCSI extent.                                                                                                                                                                                  |

Extent Configuration Settings

</div>

  [constant block size warnings]: https://www.virten.net/2016/12/the-physical-block-size-reported-by-the-device-is-not-supported/
  [xcopy]: https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc771254(v=ws.11)

New extents have been added to `Sharing --> Block (iSCSI) --> Extents`.
The associated `Serial` and Network Address Authority (`NAA`) are shown
along with the extent name.

### Associated Targets

The last step is associating an extent to a target by going to
`Sharing --> Block (iSCSI) --> Associated Targets` and clicking . The
screen is shown in `Figure %s <iscsi_target_extent_fig>`. Use the
drop-down menus to select the existing target and extent. Click `SAVE`
to add an entry for the LUN.

> Associating a Target With an Extent

`Table %s <iscsi_target_extent_config_tab>` summarizes the settings that
can be configured when associating targets and extents.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

| Setting | Value          | Description                                                                                                                                                           |
|---------|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Target  | drop-down menu | Select an existing target.                                                                                                                                            |
| LUN ID  | integer        | Select or enter a value between *0* and *1023*. Some initiators expect a value less than *256*. Leave this field blank to automatically assign the next available ID. |
| Extent  | drop-down menu | Select an existing extent.                                                                                                                                            |

</div>

Always associating extents to targets in a one-to-one manner is
recommended, even though the will allow multiple extents to be
associated with the same target.

<div class="note">

<div class="title">

Note

</div>

Each LUN entry has `Edit` and `Delete` buttons for modifying the
settings or deleting the LUN entirely. A verification popup appears when
the `Delete` button is clicked. If an initiator has an active connection
to the LUN, it is indicated in red text. Clearing the initiator
connections to a LUN before deleting it is recommended.

</div>

After iSCSI has been configured, remember to start the service in
`Services --> iSCSI` by clicking the button.

### Connecting to iSCSI

To access the iSCSI target, clients must use iSCSI initiator software.

An iSCSI Initiator client is pre-installed with Windows 7. A detailed
how-to for this client can be found [here][]. A client for Windows 2000,
XP, and 2003 can be found [here][1]. This [How-to][] shows how to create
an iSCSI target for a Windows 7 system.

  [here]: http://techgenix.com/Connecting-Windows-7-iSCSI-SAN/
  [1]: http://www.microsoft.com/en-us/download/details.aspx?id=18986
  [How-to]: https://www.pluralsight.com/blog/software-development/freenas-8-iscsi-target-windows-7

macOS does not include an initiator. [globalSAN][] is a commercial,
easy-to-use Mac initiator.

  [globalSAN]: http://www.studionetworksolutions.com/globalsan-iscsi-initiator/

BSD systems provide command line initiators: [iscontrol(8)][] comes with
FreeBSD versions 9.x and lower, [iscsictl(8)][] comes with FreeBSD
versions 10.0 and higher, [iscsi-initiator(8)][] comes with NetBSD, and
[iscsid(8)][] comes with OpenBSD.

  [iscontrol(8)]: https://www.freebsd.org/cgi/man.cgi?query=iscontrol
  [iscsictl(8)]: https://www.freebsd.org/cgi/man.cgi?query=iscsictl
  [iscsi-initiator(8)]: http://netbsd.gw.com/cgi-bin/man-cgi?iscsi-initiator++NetBSD-current
  [iscsid(8)]: http://man.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man8/iscsid.8?query=iscsid

Some Linux distros provide the command line utility `iscsiadm` from
[Open-iSCSI][]. Use a web search to see if a package exists for the
distribution should the command not exist on the Linux system.

  [Open-iSCSI]: http://www.open-iscsi.com/

If a LUN is added while `iscsiadm` is already connected, it will not see
the new LUN until rescanned with `iscsiadm -m node -R`. Alternately, use
`iscsiadm -m discovery -t st -p portal_IP` to find the new LUN and
`iscsiadm -m node -T LUN_Name -l` to log into the LUN.

Instructions for connecting from a VMware ESXi Server can be found at
[How to configure FreeNAS 8 for iSCSI and connect to ESX(i)][]. Note
that the requirements for booting vSphere 4.x off iSCSI differ between
ESX and ESXi. ESX requires a hardware iSCSI adapter while ESXi requires
specific iSCSI boot firmware support. The magic is on the booting host
side, meaning that there is no difference to the FreeNAS<sup>®</sup>
configuration. See the [iSCSI SAN Configuration Guide][] for details.

  [How to configure FreeNAS 8 for iSCSI and connect to ESX(i)]: https://www.vladan.fr/how-to-configure-freenas-8-for-iscsi-and-connect-to-esxi/
  [iSCSI SAN Configuration Guide]: https://www.vmware.com/pdf/vsphere4/r41/vsp_41_iscsi_san_cfg.pdf

The VMware firewall only allows iSCSI connections on port *3260* by
default. If a different port has been selected, outgoing connections to
that port must be manually added to the firewall before those
connections will work.

If the target can be seen but does not connect, check the
`Discovery Auth` settings in `Target Global Configuration`.

If the LUN is not discovered by ESXi, make sure that promiscuous mode is
set to `Accept` in the vSwitch.

### Growing LUNs

The method used to grow the size of an existing iSCSI LUN depends on
whether the LUN is backed by a file extent or a zvol. Both methods are
described in this section.

Enlarging a LUN with one of the methods below gives it more unallocated
space, but does not automatically resize filesystems or other data on
the LUN. This is the same as binary-copying a smaller disk onto a larger
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

#### Zvol Based LUN

To grow a zvol-based LUN, go to `Storage --> Pools`, click on the zvol
to be grown, then click `Edit zvol`. In the example shown in
`Figure %s <iscsi_zvol_lun_fig>`, the current size of the zvol named
*zvol1* is 4 GiB.

> Editing an Existing Zvol

Enter the new size for the zvol in the `Size for this zvol` field and
click `SAVE`. The new size for the zvol is immediately shown in the
`Used` column of the `Storage --> Pools` table.

<div class="note">

<div class="title">

Note

</div>

The does not allow reducing the size of the zvol, as doing so could
result in loss of data. It also does not allow increasing the size of
the zvol past 80% of the pool size.

</div>

#### File Extent Based LUN

To grow a file extent-based LUN:

Go to `Services --> iSCSI --> CONFIGURE --> Extents`. Click , then
`Edit`. Ensure the `Extent Type` is set to file and enter the
`Path to the extent`. Open the `Shell` to grow the file extent. This
example grows `/mnt/pool1/data` by 2 GiB:

    truncate -s +2g /mnt/pool1/data

Return to `Services --> iSCSI --> CONFIGURE --> Extents`, click on the
desired file extent, then click `Edit`. Set the size to *0* as this
causes the iSCSI target to use the new size of the file.

<div class="index">

NFS, Network File System

</div>

Unix (NFS) Shares
-----------------

FreeNAS<sup>®</sup> supports sharing pools, datasets, and directories
over the Network File System (NFS). Clients use the `mount` command to
mount the share. Mounted NFS shares appear as another directory on the
client system. Some Linux distros require the installation of additional
software to mount an NFS share. Windows systems must enable Services for
NFS in the Ultimate or Enterprise editions or install an NFS client
application.

<div class="note">

<div class="title">

Note

</div>

For performance reasons, iSCSI is preferred to NFS shares when
FreeNAS<sup>®</sup> is installed on ESXi. When considering creating NFS
shares on ESXi, read through the performance analysis presented in
[Running ZFS over NFS as a VMware Store][].

</div>

  [Running ZFS over NFS as a VMware Store]: https://tinyurl.com/archive-zfs-over-nfs-vmware

Create an NFS share by going to `Sharing --> Unix (NFS) Shares` and
clicking . `Figure %s <nfs_share_wiz_fig>` shows an example of creating
an NFS share.

<div id="nfs_share_wiz_fig">

![NFS Share Creation][]

</div>

  [NFS Share Creation]: images/sharing-unix-nfs-add.png

Remember these points when creating NFS shares:

1.  Clients specify the `Path` when mounting the share.
2.  The `Maproot` and `Mapall` options cannot both be enabled. The
    `Mapall` options supersede the `Maproot` options. To restrict only
    the *root* user permissions, set the `Maproot` option. To restrict
    permissions of all users, set the `Mapall` options.
3.  Each pool or dataset is considered to be a unique filesystem.
    Individual NFS shares cannot cross filesystem boundaries. Adding
    paths to share more directories only works if those directories are
    within the same filesystem.
4.  The network and host must be unique to both each created share and
    the filesystem or directory included in that share. Because
    `/etc/exports` is not an access control list (ACL), the rules
    contained in `/etc/exports` become undefined with overlapping
    networks or when using the same share with multiple hosts.
5.  The `All dirs` option can only be used once per share per
    filesystem.

To better understand these restrictions, consider scenarios where there
are:

-   two networks, *10.0.0.0/8* and *20.0.0.0/8*
-   a ZFS pool named `pool1` with a dataset named `dataset1`
-   `dataset1` contains directories named `directory1`, `directory2`,
    and `directory3`

Because of restriction \#3, an error is shown when trying to create one
NFS share like this:

-   `Authorized Networks` set to *10.0.0.0/8 20.0.0.0/8*
-   `Path` set to the dataset `/mnt/pool1/dataset1`. An additional path
    to directory `/mnt/pool1/dataset1/directory1` is added.

The correct method to configure this share is to set the `Path` to
`/mnt/pool1/dataset1` and set the `All dirs` box. This allows the client
to also mount `/mnt/pool1/dataset1/directory1` when
`/mnt/pool1/dataset1` is mounted.

Additional paths are used to define specific directories to be shared.
For example, `dataset1` has three directories. To share only
`/mnt/pool1/dataset1/directory1` and `/mnt/pool1/dataset1/directory2`,
create paths for `directory1` and `directory2` within the share. This
excludes `directory3` from the share.

Restricting a specific directory to a single network is done by creating
a share for the volume or dataset and a share for the directory within
that volume or dataset. Define the authorized networks for both shares.

First NFS share:

-   `Authorized Networks` set to *10.0.0.0/8*
-   `Path` set to `/mnt/pool1/dataset1`

Second NFS share:

-   `Authorized Networks` set to *20.0.0.0/8*
-   `Path` set to `/mnt/pool1/dataset1/directory1`

This requires the creation of two shares. It cannot be done with only
one share.

`Table %s <nfs_share_opts_tab>` summarizes the available configuration
options in the `Sharing/NFS/Add` screen. Click `ADVANCED MODE` to see
all settings.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.14linewidth-2tabcolsep}
&gt;{RaggedRight}p{dimexpr 0.54linewidth-2tabcolsep}\|

</div>

<div id="nfs_share_opts_tab">

| Setting                           | Value          | Advanced Mode | Description                                                                                                                                                                                                                                                                                                                                                                      |
|-----------------------------------|----------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Path                              | browse button  |               | Browse to the dataset or directory to be shared. Click `ADD` to specify multiple paths.                                                                                                                                                                                                                                                                                          |
| Comment                           | string         |               | Text describing the share. Typically used to name the share. If left empty, this shows the `Path` entries of the share.                                                                                                                                                                                                                                                          |
| All dirs                          | checkbox       |               | Allow the client to also mount any subdirectories of the selected pool or dataset.                                                                                                                                                                                                                                                                                               |
| Read only                         | checkbox       |               | Prohibit writing to the share.                                                                                                                                                                                                                                                                                                                                                   |
| Quiet                             | checkbox       | ✓             | Restrict some syslog diagnostics to avoid some error messages. See [exports(5)][] for examples.                                                                                                                                                                                                                                                                                  |
| Authorized networks               | string         | ✓             | Space-delimited list of allowed networks in network/mask CIDR notation. Example: *1.2.3.0/24*. Leave empty to allow all.                                                                                                                                                                                                                                                         |
| Authorized Hosts and IP addresses | string         | ✓             | Space-delimited list of allowed IP addresses or hostnames. Leave empty to allow all.                                                                                                                                                                                                                                                                                             |
| Maproot User                      | drop-down menu | ✓             | When a user is selected, the *root* user is limited to permissions of that user.                                                                                                                                                                                                                                                                                                 |
| Maproot Group                     | drop-down menu | ✓             | When a group is selected, the *root* user is also limited to permissions of that group.                                                                                                                                                                                                                                                                                          |
| Mapall User                       | drop-down menu | ✓             | FreeNAS<sup>®</sup> user or user imported with `Active Directory`. The specified permissions of that user are used by all clients.                                                                                                                                                                                                                                               |
| Mapall Group                      | drop-down menu | ✓             | FreeNAS<sup>®</sup> group or group imported with `Active Directory`. The specified permissions of that group are used by all clients.                                                                                                                                                                                                                                            |
| Security                          | selection      | ✓             | Only appears if `Enable NFSv4` is enabled in `Services --> NFS`. Choices are *sys* or these Kerberos options: *krb5* (authentication only), *krb5i* (authentication and integrity), or *krb5p* (authentication and privacy). If multiple security mechanisms are added to the `Selected` column using the arrows, use the `Up` or `Down` buttons to list in order of preference. |

NFS Share Options

</div>

  [exports(5)]: https://www.freebsd.org/cgi/man.cgi?query=exports

Go to `Sharing --> Unix (NFS)` and click and `Edit` to edit an existing
share. `Figure %s <nfs_share_settings_fig>` shows the configuration
screen for the existing *nfs\_share1* share. Options are the same as
described in `nfs_share_opts_tab`.

<div id="nfs_share_settings_fig">

![NFS Share Settings][]

</div>

  [NFS Share Settings]: images/sharing-unix-nfs-edit-example.png

### Example Configuration

By default, the `Mapall` fields are not set. This means that when a user
connects to the NFS share, the user has the permissions associated with
their user account. This is a security risk if a user is able to connect
as *root* as they will have complete access to the share.

A better option is to do this:

1.  Specify the built-in *nobody* account to be used for NFS access.
2.  In the `Change Permissions` screen of the pool or dataset that is
    being shared, change the owner and group to *nobody* and set the
    permissions according to the desired requirements.
3.  Select *nobody* in the `Mapall User` and `Mapall Group` drop-down
    menus for the share in `Sharing --> Unix (NFS) Shares`.

With this configuration, it does not matter which user account connects
to the NFS share, as it will be mapped to the *nobody* user account and
will only have the permissions that were specified on the pool or
dataset. For example, even if the *root* user is able to connect, it
will not gain *root* access to the share.

### Connecting to the Share

The following examples share this configuration:

1.  The FreeNAS<sup>®</sup> system is at IP address *192.168.2.2*.
2.  A dataset named `/mnt/pool1/nfs_share1` is created and the
    permissions set to the *nobody* user account and the *nobody* group.
3.  An NFS share is created with these attributes:
    -   `Path`: `/mnt/pool1/nfs_share1`
    -   `Authorized Networks`: *192.168.2.0/24*
    -   `All dirs` option is enabled
    -   `MapAll User` is set to *nobody*
    -   `MapAll Group` is set to *nobody*

#### From BSD or Linux

NFS shares are mounted on BSD or Linux clients with this command
executed as the superuser (*root*) or with `sudo`:

    mount -t nfs 192.168.2.2:/mnt/pool1/nfs_share1 /mnt

-   **-t nfs** specifies the filesystem type of the share
-   **192.168.2.2** is the IP address of the FreeNAS<sup>®</sup> system
-   **/mnt/pool/nfs\_share1** is the name of the directory to be shared,
    a dataset in this case
-   **/mnt** is the mountpoint on the client system. This must be an
    existing, *empty* directory. The data in the NFS share appears in
    this directory on the client computer.

Successfully mounting the share returns to the command prompt without
any status or error messages.

<div class="note">

<div class="title">

Note

</div>

If this command fails on a Linux system, make sure that the
[nfs-utils][] package is installed.

</div>

  [nfs-utils]: https://sourceforge.net/projects/nfs/files/nfs-utils/

This configuration allows users on the client system to copy files to
and from `/mnt` (the mount point). All files are owned by
*nobody:nobody*. Changes to any files or directories in `/mnt` write to
the FreeNAS<sup>®</sup> system `/mnt/pool1/nfs_share1` dataset.

NFS share settings cannot be changed when the share is mounted on a
client computer. The `umount` command is used to unmount the share on
BSD and Linux clients. Run it as the superuser or with `sudo` on each
client computer:

    umount /mnt

#### From Microsoft

Windows NFS client support varies with versions and releases. For best
results, use `Windows (SMB) Shares`.

#### From macOS

A macOS client uses Finder to mount the NFS volume. Go to
`Go --> Connect to Server`. In the `Server Address` field, enter
*nfs://* followed by the IP address of the FreeNAS<sup>®</sup> system,
and the name of the pool or dataset being shared by NFS. The example
shown in `Figure %s <mount_nfs_osx_fig>` continues with the example of
*192.168.2.2:/mnt/pool1/nfs\_share1*.

Finder opens automatically after connecting. The IP address of the
FreeNAS<sup>®</sup> system displays in the SHARED section of the left
frame and the contents of the share display in the right frame.
`Figure %s <view_nfs_finder_fig>` shows an example where `/mnt/data` has
one folder named `images`. The user can now copy files to and from the
share.

<div id="mount_nfs_osx_fig">

![Mounting the NFS Share from macOS][]

</div>

  [Mounting the NFS Share from macOS]: images/external/sharing-nfs-mac.png

<div id="view_nfs_finder_fig">

![Viewing the NFS Share in Finder][]

</div>

  [Viewing the NFS Share in Finder]: images/external/sharing-nfs-finder.png

### Troubleshooting NFS

Some NFS clients do not support the NLM (Network Lock Manager) protocol
used by NFS. This is the case if the client receives an error that all
or part of the file may be locked when a file transfer is attempted. To
resolve this error, add the option `-o nolock` when running the `mount`
command on the client to allow write access to the NFS share.

If a "time out giving up" error is shown when trying to mount the share
from a Linux system, make sure that the portmapper service is running on
the Linux client. If portmapper is running and timeouts are still shown,
force the use of TCP by including `-o tcp` in the `mount` command.

If a `RPC: Program not registered` error is shown, upgrade to the latest
version of FreeNAS<sup>®</sup> and restart the NFS service after the
upgrade to clear the NFS cache.

If clients see "reverse DNS" errors, add the FreeNAS<sup>®</sup> IP
address in the `Host name database` field of
`Network --> Global Configuration`.

If clients receive timeout errors when trying to mount the share, add
the client IP address and hostname to the `Host name database` field in
`Network --> Global Configuration`.

Some older versions of NFS clients default to UDP instead of TCP and do
not auto-negotiate for TCP. By default, FreeNAS<sup>®</sup> uses TCP. To
support UDP connections, go to `Services --> NFS --> Configure` and
enable the `Serve UDP NFS clients` option.

The `nfsstat -c` or `nfsstat -s` commands can be helpful to detect
problems from the `Shell`. A high proportion of retries and timeouts
compared to reads usually indicates network problems.

<div class="index">

WebDAV

</div>

WebDAV Shares
-------------

In FreeNAS<sup>®</sup>, WebDAV shares can be created so that
authenticated users can browse the contents of the specified pool,
dataset, or directory from a web browser.

Configuring WebDAV shares is a two step process. First, create the
WebDAV shares to specify which data can be accessed. Then, configure the
WebDAV service by specifying the port, authentication type, and
authentication password. Once the configuration is complete, the share
can be accessed using a URL in the format:

    protocol://IP_address:port_number/share_name

where:

-   **protocol:** is either *http* or *https*, depending upon the
    `Protocol` configured in `Services --> WebDAV --> CONFIGURE`.
-   **IP address:** is the IP address or hostname of the
    FreeNAS<sup>®</sup> system. Take care when configuring a public IP
    address to ensure that the network firewall only allows access to
    authorized systems.
-   **port\_number:** is configured in
    `Services --> WebDAV --> CONFIGURE`. If the FreeNAS<sup>®</sup>
    system is to be accessed using a public IP address, consider
    changing the default port number and ensure that the network
    firewall only allows access to authorized systems.
-   **share\_name:** is configured by clicking
    `Sharing --> WebDAV Shares`, then .

Entering the URL in a web browser brings up an authentication pop-up
message. Enter a username of *webdav* and the password configured in
`Services --> WebDAV --> CONFIGURE`.

<div class="warning">

<div class="title">

Warning

</div>

At this time, only the *webdav* user is supported. For this reason, it
is important to set a good password for this account and to only give
the password to users which should have access to the WebDAV share.

</div>

To create a WebDAV share, go to `Sharing --> WebDAV Shares` and click ,
which will open the screen shown in `Figure %s <add_webdav_share_fig>`.

<div id="add_webdav_share_fig">

![Adding a WebDAV Share][]

</div>

  [Adding a WebDAV Share]: images/sharing-webdav-add.png

`Table %s <webdav_share_opts_tab>` summarizes the available options.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.16linewidth-2tabcolsep}

</div>

<div id="webdav_share_opts_tab">

| Setting             | Value         | Description                                                                                                                                                                                                                                                                                                                                                              |
|---------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Share Path Name     | string        | Enter a name for the share.                                                                                                                                                                                                                                                                                                                                              |
| Comment             | string        | Optional.                                                                                                                                                                                                                                                                                                                                                                |
| Path                | browse button | Enter the path or `Browse` to the pool or dataset to share. Appending a new name to the path creates a new dataset. Example: */mnt/pool1/newdataset*.                                                                                                                                                                                                                    |
| Read Only           | checkbox      | Set to prohibit users from writing to the share.                                                                                                                                                                                                                                                                                                                         |
| Change User & Group | checkbox      | Ownership of all files in the share will be changed to user `webdav` and group `webdav`. Existing permissions will not be changed, but the ownership change might make files inaccesible to their original owners. This operation cannot be undone! If unset, ownership of files to be accessed through WebDAV must be manually set to the `webdav` or `www` user/group. |

WebDAV Share Options

</div>

Click `SAVE` to create the share. Then, go to `Services --> WebDAV` and
click the button to turn on the service.

After the service starts, review the settings in
`Services --> WebDAV --> CONFIGURE` as they are used to determine which
URL is used to access the WebDAV share and whether or not authentication
is required to access the share. These settings are described in
`WebDAV`.

<div class="index">

CIFS, Samba, Windows Shares, SMB

</div>

Windows (SMB) Shares
--------------------

FreeNAS<sup>®</sup> uses [Samba][] to share pools using Microsoft's SMB
protocol. SMB is built into the Windows and macOS operating systems and
most Linux and BSD systems pre-install the Samba client in order to
provide support for SMB. If the distro did not, install the Samba client
using the distro software repository.

  [Samba]: https://www.samba.org/

The SMB protocol supports many different types of configuration
scenarios, ranging from the simple to complex. The complexity of the
scenario depends upon the types and versions of the client operating
systems that will connect to the share, whether the network has a
Windows server, and whether Active Directory is being used. Depending on
the authentication requirements, it might be necessary to create or
import users and groups.

Samba supports server-side copy of files on the same share with clients
from Windows 8 and higher. Copying between two different shares is not
server-side. Windows 7 clients support server-side copying with
[Robocopy][].

  [Robocopy]: https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc733145(v=ws.11)

This chapter starts by summarizing the available configuration options.
It demonstrates some common configuration scenarios as well as offering
some troubleshooting tips. Reading through this entire chapter before
creating any SMB shares is recommended to gain a better understanding of
the configuration scenario that meets the specific network requirements.

[SMB Tips and Tricks][] shows helpful hints for configuring and managing
SMB networking. The [FreeNAS and Samba (CIFS) permissions][] and
[Advanced Samba (CIFS) permissions on FreeNAS][] videos clarify setting
up permissions on SMB shares. Another helpful reference is [Methods For
Fine-Tuning Samba Permissions][].

  [SMB Tips and Tricks]: https://forums.freenas.org/index.php?resources/smb-tips-and-tricks.15/
  [FreeNAS and Samba (CIFS) permissions]: https://www.youtube.com/watch?v=RxggaE935PM
  [Advanced Samba (CIFS) permissions on FreeNAS]: https://www.youtube.com/watch?v=QhwOyLtArw0
  [Methods For Fine-Tuning Samba Permissions]: https://forums.freenas.org/index.php?threads/methods-for-fine-tuning-samba-permissions.50739/

<div class="warning">

<div class="title">

Warning

</div>

[SMB1 is disabled by default for security][]. If necessary, SMB1 can be
enabled in `Services --> SMB Configure`.

</div>

  [SMB1 is disabled by default for security]: https://www.ixsystems.com/blog/library/do-not-use-smb1/

`Figure %s <adding_smb_share_fig>` shows the configuration screen that
appears after clicking `Sharing --> Windows (SMB Shares)`, then .

<div id="adding_smb_share_fig">

![Adding an SMB Share][]

</div>

  [Adding an SMB Share]: images/sharing-windows-smb-add.png

`Table %s <smb_share_opts_tab>` summarizes the options available when
creating a SMB share. Some settings are only configurable after clicking
the `ADVANCED MODE` button. For simple sharing scenarios,
`ADVANCED MODE` options are not needed. For more complex sharing
scenarios, only change an `ADVANCED MODE` option after fully
understanding the function of that option. [smb.conf(5)][] provides more
details for each configurable option.

  [smb.conf(5)]: https://www.freebsd.org/cgi/man.cgi?query=smb.conf

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.14linewidth-2tabcolsep}
&gt;{RaggedRight}p{dimexpr 0.54linewidth-2tabcolsep}\|

</div>

<div id="smb_share_opts_tab">

<table>
<caption>SMB Share Options</caption>
<colgroup>
<col style="width: 15%" />
<col style="width: 7%" />
<col style="width: 5%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Value</th>
<th>Advanced Mode</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Path</td>
<td>browse button</td>
<td></td>
<td>Select the pool, dataset, or directory to share. The same path can be used by more than one share.</td>
</tr>
<tr class="even">
<td>Name</td>
<td>string</td>
<td></td>
<td>Name the new share. Each share name must be unique. The names <em>global</em>, <em>homes</em>, and <em>printers</em> are reserved and cannot be used.</td>
</tr>
<tr class="odd">
<td>Use as home share</td>
<td>checkbox</td>
<td></td>
<td>Set to allow this share to hold user home directories. Only one share can be the home share. Note that lower case names for user home directories are strongly recommended, as Samba maps usernames to all lower case. For example, the username John will be mapped to a home directory named <code class="interpreted-text" role="file">john</code>. If the <code class="interpreted-text" role="guilabel">Path</code> to the home share includes an upper case username, delete the existing user and recreate it in <code class="interpreted-text" role="menuselection">Accounts --&gt; Users</code> with an all lower case <code class="interpreted-text" role="guilabel">Username</code>. Return to <code class="interpreted-text" role="menuselection">Sharing --&gt; SMB</code> to create the home share, and select the <code class="interpreted-text" role="guilabel">Path</code> that contains the new lower case username.</td>
</tr>
<tr class="even">
<td>Description</td>
<td>string</td>
<td></td>
<td>Description of the share or notes on how it is used.</td>
</tr>
<tr class="odd">
<td>Time Machine</td>
<td>checkbox</td>
<td></td>
<td>Enable <a href="https://developer.apple.com/library/archive/releasenotes/NetworkingInternetWeb/Time_Machine_SMB_Spec/#//apple_ref/doc/uid/TP40017496-CH1-SW1">Time Machine</a> backups for this share. The process to configure a Time Machine backup is shown in <code class="interpreted-text" role="ref">Creating Authenticated and Time Machine Shares</code>. Changing this setting on an existing share requres an <code class="interpreted-text" role="ref">SMB</code> service restart.</td>
</tr>
<tr class="even">
<td>Export Read Only</td>
<td>checkbox</td>
<td>✓</td>
<td>Prohibit write access to this share.</td>
</tr>
<tr class="odd">
<td>Browsable to Network Clients</td>
<td>checkbox</td>
<td>✓</td>
<td>Determine whether this share name is included when browsing shares. Home shares are only visible to the owner regardless of this setting.</td>
</tr>
<tr class="even">
<td>Export Recycle Bin</td>
<td>checkbox</td>
<td>✓</td>
<td>Files that are deleted from the same dataset are moved to the Recycle Bin and do not take any additional space. This is only applies over the SMB protocol. <strong>Deleting files over NFS will remove the files permanently</strong>. When the files are in a different dataset or a child dataset, they are copied to the dataset where the Recycle Bin is located. To prevent excessive space usage, files larger than 20 MiB are deleted rather than moved. Adjust the <code class="interpreted-text" role="guilabel">Auxiliary Parameter</code> <code class="interpreted-text" role="samp">crossrename:sizelimit=</code> setting to allow larger files. For example, <code class="interpreted-text" role="samp">crossrename:sizelimit={50}</code> allows moves of files up to 50 MiB in size. The recylce bin has read-write functionality. This means files can be permanently deleted or moved from the recylce bin. <strong>This is not a replacement for</strong> <code class="interpreted-text" role="ref">ZFS Snapshots &lt;Snapshots&gt;</code>.</td>
</tr>
<tr class="odd">
<td>Show Hidden Files</td>
<td>checkbox</td>
<td>✓</td>
<td>Disable the Windows <em>hidden</em> attribute on a new Unix hidden file. Unix hidden filenames start with a dot: <code class="interpreted-text" role="file">.foo</code>. Existing files are not affected.</td>
</tr>
<tr class="even">
<td>Allow Guest Access</td>
<td>checkbox</td>
<td></td>
<td><p>Privileges are the same as the guest account. Guest access is disabled by default in Windows 10 version 1709 and Windows Server version 1903. Additional client-side configuration is required to provide guest access to these clients.</p>
<p>MacOS clients: Attempting to connect as a user that does not exist in FreeNAS<sup>®</sup> <em>does not</em> automatically connect as the guest account. The <code class="interpreted-text" role="guilabel">Connect As:</code> <em>Guest</em> option must be specifically chosen in MacOS to log in as the guest account. See the <a href="https://support.apple.com/guide/mac-help/connect-mac-shared-computers-servers-mchlp1140/">Apple documentation</a> for more details.</p></td>
</tr>
<tr class="odd">
<td>Only Allow Guest Access</td>
<td>checkbox</td>
<td>✓</td>
<td>Requires <code class="interpreted-text" role="guilabel">Allow guest access</code> to also be enabled. Forces guest access for all connections.</td>
</tr>
<tr class="even">
<td>Access Based Share Enumeration</td>
<td>checkbox</td>
<td>✓</td>
<td>Restrict share visibility to users with a current Windows Share ACL access of read or write. Use Windows administration tools to adjust the share permissions. See <a href="https://www.freebsd.org/cgi/man.cgi?query=smb.conf">smb.conf(5)</a>.</td>
</tr>
<tr class="odd">
<td>Hosts Allow</td>
<td>string</td>
<td>✓</td>
<td>Enter a list of allowed hostnames or IP addresses. Separate entries with a comma (<code>,</code>), space, or tab. Please see the <code class="interpreted-text" role="ref">note &lt;smb allow/deny note&gt;</code> for more information.</td>
</tr>
<tr class="even">
<td>Hosts Deny</td>
<td>string</td>
<td>✓</td>
<td>Enter a list of denied hostnames or IP addresses. Specify <code>ALL</code> and list any hosts from <code class="interpreted-text" role="guilabel">Hosts Allow</code> to have those hosts take precedence. Separate entries with a comma (<code>,</code>), space, or tab. Please see the <code class="interpreted-text" role="ref">note &lt;smb allow/deny note&gt;</code> for more information.</td>
</tr>
<tr class="odd">
<td>VFS Objects</td>
<td>selection</td>
<td>✓</td>
<td>Add virtual file system objects to enhance functionality. <code class="interpreted-text" role="numref">Table %s &lt;avail_vfs_objects_tab&gt;</code> summarizes the available objects.</td>
</tr>
<tr class="even">
<td>Enable Shadow Copies</td>
<td>checkbox</td>
<td></td>
<td>Expose ZFS snapshots as <a href="https://docs.microsoft.com/en-us/windows/desktop/vss/shadow-copies-and-shadow-copy-sets">Windows Shadow Copies</a>.</td>
</tr>
<tr class="odd">
<td>Auxiliary Parameters</td>
<td>string</td>
<td>✓</td>
<td>Additional <a href="https://www.freebsd.org/cgi/man.cgi?query=smb.conf">smb4.conf</a> parameters not covered by other option fields.</td>
</tr>
</tbody>
</table>

SMB Share Options

</div>

<div id="smb allow/deny note" class="note">

<div class="title">

Note

</div>

If neither *Hosts Allow* or *Hosts Deny* contains an entry, then SMB
share access is allowed for any host.

If there is a *Hosts Allow* list but no *Hosts Deny* list, then only
allow hosts on the *Hosts Allow* list.

If there is a *Hosts Deny* list but no *Hosts Allow* list, then allow
all hosts that are not on the *Hosts Deny* list.

If there is both a *Hosts Allow* and *Hosts Deny* list, then allow all
hosts that are on the *Hosts Allow* list. If there is a host not on the
*Hosts Allow* and not on the *Hosts Deny* list, then allow it.

</div>

Here are some notes about `ADVANCED MODE` settings:

-   Hostname lookups add some time to accessing the SMB share. If only
    using IP addresses, unset the `Hostnames Lookups` setting in
    `Services --> SMB -->` .
-   When the `Browsable to Network Clients` option is selected, the
    share is visible through Windows File Explorer or through
    `net view`. When the `Use as home share` option is selected,
    deselecting the `Browsable to Network Clients` option hides the
    share named *homes* so that only the dynamically generated share
    containing the authenticated user home directory will be visible. By
    default, the *homes* share and the user home directory are both
    visible. Users are not automatically granted read or write
    permissions on browsable shares. This option provides no real
    security because shares that are not visible in Windows File
    Explorer can still be accessed with a *UNC* path.
-   If some files on a shared pool should be hidden and inaccessible to
    users, put a *veto files=* line in the `Auxiliary Parameters` field.
    The syntax for the `veto files` option and some examples can be
    found in the [smb.conf manual page][].

  [smb.conf manual page]: https://www.freebsd.org/cgi/man.cgi?query=smb.conf

Samba disables NTLMv1 authentication by default for security. Standard
configurations of Windows XP and some configurations of later clients
like Windows 7 will not be able to connect with NTLMv1 disabled.
[Security guidance for NTLMv1 and LM network authentication][] has
information about the security implications and ways to enable NTLMv2 on
those clients. If changing the client configuration is not possible,
NTLMv1 authentication can be enabled by selecting the `NTLMv1 auth`
option in `Services --> SMB -->` .

  [Security guidance for NTLMv1 and LM network authentication]: https://support.microsoft.com/en-us/help/2793313/security-guidance-for-ntlmv1-and-lm-network-authentication

`Table %s <avail_vfs_objects_tab>` provides an overview of the available
VFS objects. Be sure to research each object **before** adding or
deleting it from the `Selected` column of the `VFS Objects` field of the
share. Some objects need additional configuration after they are added.
Refer to [Stackable VFS modules][] and the [vfs\_\* man pages][] for
more details.

  [Stackable VFS modules]: https://www.samba.org/samba/docs/old/Samba3-HOWTO/VFS.html
  [vfs\_\* man pages]: https://www.samba.org/samba/docs/current/man-html/

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.47linewidth-2tabcolsep}\|

</div>

<div id="avail_vfs_objects_tab">

<table>
<caption>Available VFS Objects</caption>
<colgroup>
<col style="width: 15%" />
<col style="width: 84%" />
</colgroup>
<thead>
<tr class="header">
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>audit</td>
<td>Log share access, connects/disconnects, directory opens/creates/removes, and file opens/closes/renames/unlinks/chmods to syslog.</td>
</tr>
<tr class="even">
<td>catia</td>
<td>Improve Mac interoperability by translating characters that are unsupported by Windows.</td>
</tr>
<tr class="odd">
<td>crossrename</td>
<td>Allow server side rename operations even if source and target are on different physical devices. Required for the recycle bin to work across dataset boundaries. Automatically added when <code class="interpreted-text" role="guilabel">Export Recycle Bin</code> is enabled.</td>
</tr>
<tr class="even">
<td>dirsort</td>
<td>Sort directory entries alphabetically before sending them to the client.</td>
</tr>
<tr class="odd">
<td>fruit</td>
<td>Enhance macOS support by providing the SMB2 AAPL extension and Netatalk interoperability. Automatically loads <em>catia</em> and <em>streams_xattr</em>, but see the <code class="interpreted-text" role="ref">warning &lt;fruit-warning&gt;</code> below.</td>
</tr>
<tr class="even">
<td>full_audit</td>
<td>Record selected client operations to the system log.</td>
</tr>
<tr class="odd">
<td>ixnas</td>
<td><p>Improves ACL compatibility with Windows, stores DOS attributes as file flags, optimizes share case sensitivity to improve performance, and enables <code class="interpreted-text" role="ref">User Quota Administration</code> from Windows. Enabled by default. Several <code class="interpreted-text" role="guilabel">Auxiliary Parameters</code> are available with <em>ixnas</em>.</p>
<p>Userspace Quota Settings:</p>
<ul>
<li><em>ixnas:base_user_quota =</em> sets a ZFS user quota on every user that connects to the share. Example: <code class="interpreted-text" role="samp">ixnas:base_user_quota = 80G</code> sets the quota to <em>80 GiB</em>.</li>
<li><em>ixnas:zfs_quota_enabled =</em> enables support for userspace quotas. Choices are <em>True</em> or <em>False</em>. Default is <em>True</em>. Example: <code class="interpreted-text" role="samp">ixnas:zfs_quota_enabled = True</code>.</li>
</ul>
<p>Home Dataset Settings:</p>
<ul>
<li><em>ixnas:chown_homedir =</em> changes the owner of a created home dataset to the currently authenticated user. <em>ixnas:zfs_auto_homedir</em> must be set to <em>True</em>. Choices are <em>True</em> or <em>False</em>. Example: <code class="interpreted-text" role="samp">ixnas:chown_homedir = True</code>.</li>
<li><em>ixnas:homedir_quota =</em> sets a quota on new ZFS datasets. <em>ixnas:zfs_auto_homedir</em> must be set to <em>True</em>. Example: <code class="interpreted-text" role="samp">ixnas:homedir_quota = 20G</code> sets the quota to <em>20 GiB</em>.</li>
<li><em>ixnas:zfs_auto_homedir =</em> creates new ZFS datasets for users connecting to home shares instead of folders. Choices are <em>True</em> or <em>False</em>. Default is <em>False</em>. Example: <code class="interpreted-text" role="samp">ixnas:zfs_auto_homedir = False</code>.</li>
</ul></td>
</tr>
<tr class="even">
<td>media_harmony</td>
<td>Allow Avid editing workstations to share a network drive.</td>
</tr>
<tr class="odd">
<td>noacl</td>
<td>Disable NT ACL support. If an extended ACL is present in the share connection path, all access to this share will be denied. When the <a href="https://www.oreilly.com/openbook/samba/book/ch05_03.html">Read-only attribute</a> is set, all write bits are removed. Disabling the <em>Read-only</em> attribute adds the write bits back to the share, up to <em>create mask</em> (<em>umask</em>). Adding <em>noacl</em> requires adding the <em>zfsacl</em> object. <em>noacl</em> is incompatible with the <em>ixnas</em> VFS object.</td>
</tr>
<tr class="even">
<td>offline</td>
<td>Mark all files in the share with the DOS <em>offline</em> attribute. This can prevent Windows Explorer from reading files just to make thumbnail images.</td>
</tr>
<tr class="odd">
<td>preopen</td>
<td>Useful for video streaming applications that want to read one file per frame.</td>
</tr>
<tr class="even">
<td>shell_snap</td>
<td>Provide shell-script callouts for snapshot creation and deletion operations issued by remote clients using the File Server Remote VSS Protocol (FSRVP).</td>
</tr>
<tr class="odd">
<td>streams_xattr</td>
<td>Enable storing NTFS alternate data streams in the file system. Enabled by default.</td>
</tr>
<tr class="even">
<td>winmsa</td>
<td>Emulate the Microsoft <em>MoveSecurityAttributes=0</em> registry option. Moving files or directories sets the ACL for file and directory hierarchies to inherit from the destination directory.</td>
</tr>
<tr class="odd">
<td>zfs_space</td>
<td>Correctly calculate ZFS space used by the share, including space used by ZFS snapshots, quotas, and resevations.</td>
</tr>
<tr class="even">
<td>zfsacl</td>
<td>Provide ACL extensions for proper integration with ZFS.</td>
</tr>
</tbody>
</table>

Available VFS Objects

</div>

<div id="fruit-warning">

<div class="warning">

<div class="title">

Warning

</div>

Be careful when using multiple SMB shares, some with and some without
*fruit*. macOS clients negotiate SMB2 AAPL protocol extensions on the
first connection to the server, so mixing shares with and without fruit
will globally disable AAPL if the first connection occurs without fruit.
To resolve this, all macOS clients need to disconnect from all SMB
shares and the first reconnection to the server has to be to a
fruit-enabled share.

</div>

</div>

These VFS objects do not appear in the drop-down menu:

-   **recycle:** moves deleted files to the recycle directory instead of
    deleting them. Controlled by `Export Recycle Bin` in the
    `SMB share options <smb_share_opts_tab>`.

Creating or editing an SMB share on a dataset with a [trivial Access
Control List (ACL)][] prompts to `configure the ACL <ACL Management>`
for the dataset.

  [trivial Access Control List (ACL)]: https://www.ixsystems.com/community/threads/methods-for-fine-tuning-samba-permissions.50739/

To view all active SMB connections and users, enter `smbstatus` in the
`Shell`. To log more details for clients that are attempting to
authenticate to an SMB share, open the `Service --> SMB` options and add
`log level = 1, auth_audit:5` to the `Auxiliary Parameters`.

### Configuring Unauthenticated Access

SMB supports guest logins, meaning that users can access the SMB share
without needing to provide a username or password. This type of share is
convenient as it is easy to configure, easy to access, and does not
require any users to be configured on the FreeNAS<sup>®</sup> system.
This type of configuration is also the least secure as anyone on the
network can access the contents of the share. Additionally, since all
access is as the guest user, even if the user inputs a username or
password, there is no way to differentiate which users accessed or
modified the data on the share. This type of configuration is best
suited for small networks where quick and easy access to the share is
more important than the security of the data on the share.

<div class="note">

<div class="title">

Note

</div>

Windows 10, Windows Server 2016 version 1709, and Windows Server 2019
disable SMB2 guest access. Read the [Microsoft security notice][] for
details about security vulnerabilities with SMB2 guest access and
instructions to re-enable guest logins on these Microsoft systems.

</div>

  [Microsoft security notice]: https://support.microsoft.com/en-hk/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser

To configure an unauthenticated SMB share:

1.  Go to `Sharing --> Windows (SMB) Shares` and click .
2.  Fill out the the fields as shown in
    `Figure %s <create_unauth_smb_share_fig>`.
3.  Enable `Allow Guest Access`.
4.  Press `SAVE`.

<div class="note">

<div class="title">

Note

</div>

If a dataset for the share has not been created, refer to
`Adding Datasets` to find out more about dataset creation.

</div>

<div id="create_unauth_smb_share_fig">

![Creating an Unauthenticated SMB Share][]

</div>

  [Creating an Unauthenticated SMB Share]: images/sharing-windows-smb-guest-example.png

The new share appears in `Sharing --> Windows (SMB) Shares`.

By default, users that access the share from an SMB client will not be
prompted for a username or password. For example, to access the share
from a Windows system, open Explorer and click on `Network`. In this
example, a system named *FREENAS* appears with a share named
*p2ds2-smb*. The user can copy data to and from this share.

The guest account can be changed by opening the `Services --> SMB`
options and selecting a different account from the `Guest Account`
dropdown.

The guest account can also have an
`Access Control Entry (ACE) <ACL Management>` that governs the
permissions of the guest account to access the different pools and
datasets on the system. To change the guest account permissions, edit
the dataset Access Control List (ACL) and add a new item with the `Who`
set to *User* and `User` set to the account used for guest access
(*nobody* by default). The ACE can then be adjusted to define the access
level required for guest sessions. See `ACL Management` for more details
about each available setting.

Changing the Guest Account permissions will not grant access for
anonymous sessions. This is best accomplished by creating or editing the
`everyone@` ACE in the dataset ACL. Note that anonymous sessions also do
not have the guest SID in the security token.

### Configuring Authenticated Access With Local Users

Most configuration scenarios require each user to have their own user
account and to authenticate before accessing the share. This allows the
administrator to control access to data, provide appropriate permissions
to that data, and to determine who accesses and modifies stored data. A
Windows domain controller is not needed for authenticated SMB shares,
which means that additional licensing costs are not required. However,
because there is no domain controller to provide authentication for the
network, each user account must be created on the FreeNAS<sup>®</sup>
system. This type of configuration scenario is often used in home and
small networks as it does not scale well if many user accounts are
needed.

To configure authenticated access for an SMB share, first create a
`group <Groups>` for all the SMB user accounts in FreeNAS<sup>®</sup>.
Go to `Accounts --> Groups` and click . Use a descriptive name for the
group like `local_smb_users`.

Configure the SMB share dataset with permissions for this new group.
When `creating a new dataset <Adding Datasets>`, set the `Share Type` to
*SMB*. After the dataset is created, open the dataset
`Access Control List (ACL) <ACL Management>` and add a new entry. Set
`Who` to *Group* and select the SMB group for the `Group`. Finish
`defining the permissions <ACE Permissions>` for the SMB group. Any
`members of this group <Groups>` now have access to the dataset.

<div id="smb_auth_share_acl_fig">

![Defining Permissions for a Group][]

</div>

  [Defining Permissions for a Group]: images/sharing-windows-smb-dataset-acl.png

Determine which users need authenticated access to the dataset and
`create new accounts <Users>` in FreeNAS<sup>®</sup>. It is recommended
to use the same username and password from the client system for the
associated FreeNAS<sup>®</sup> user account. Add the SMB group to the
`Auxiliary Groups` list during account creation.

Finally, `create the SMB share <Windows (SMB) Shares>`. Make sure the
`Path` is pointed to the dataset that has defined permissions for the
SMB group and that the `SMB` service is active.

**Testing the Share**

The authenticated share can be tested from any SMB client. For example,
to test an authenticated share from a Windows system with network
discovery enabled, open Explorer and click on `Network`. If network
discovery is disabled, open Explorer and enter `\\{HOST}` in the address
bar, where *HOST* is the IP address or hostname of the share system.
This example shows a system named *FREENAS* with a share named
*smb\_share*.

After clicking *smb\_share*, a Windows Security dialog prompts for the
username and password of the user associated with *smb\_share*. After
authenticating, the user can copy data to and from the SMB share.

Map the share as a network drive to prevent Windows Explorer from
hanging when accessing the share. Right-click the share and select
`Map network drive...`. Choose a drive letter from the drop-down menu
and click `Finish`.

Windows caches user account credentials with the authenticated share.
This sometimes prevents connection to a share, even when the correct
username and password are provided. Logging out of Windows clears the
cache. The authentication dialog reappears the next time the user
connects to an authenticated share.

### User Quota Administration

File Explorer can manage quotas on SMB shares connected to an
`Active Directory` server. Both the share and dataset being shared must
be configured to allow this feature:

-   Create an authenticated share with `domain admins` as both the user
    and group name in `Ownership`.
-   Edit the SMB share and add *ixnas* to the list of selected
    `VFS Object <avail_vfs_objects_tab>`.
-   In Windows Explorer, connect to and map the share with a user
    account which is a member of the `domain admins` group. The `Quotas`
    tab becomes active.

<div class="index">

Shadow Copies

</div>

### Configuring Shadow Copies

[Shadow Copies][], also known as the Volume Shadow Copy Service (VSS) or
Previous Versions, is a Microsoft service for creating volume snapshots.
Shadow copies can be used to restore previous versions of files from
within Windows Explorer. Shadow Copy support is built into Vista and
Windows 7. Windows XP or 2000 users need to install the [Shadow Copy
client][].

  [Shadow Copies]: https://en.wikipedia.org/wiki/Shadow_copy
  [Shadow Copy client]: http://www.microsoft.com/en-us/download/details.aspx?displaylang=en&id=16220

When a periodic snapshot task is created on a ZFS pool that is
configured as a SMB share in FreeNAS<sup>®</sup>, it is automatically
configured to support shadow copies.

Before using shadow copies with FreeNAS<sup>®</sup>, be aware of the
following caveats:

-   If the Windows system is not fully patched to the latest service
    pack, Shadow Copies may not work. If no previous versions of files
    to restore are visible, use Windows Update to ensure the system is
    fully up-to-date.
-   Shadow copy support only works for ZFS pools or datasets. This means
    that the SMB share must be configured on a pool or dataset, not on a
    directory.
-   Datasets are filesystems and shadow copies cannot traverse
    filesystems. To see the shadow copies in the child datasets, create
    separate shares for them.
-   Shadow copies will not work with a manual snapshot. Creating a
    periodic snapshot task for the pool or dataset being shared by SMB
    or a recursive task for a parent dataset is recommended.
-   The periodic snapshot task should be created and at least one
    snapshot should exist **before** creating the SMB share. If the SMB
    share was created first, restart the SMB service in `Services`.
-   Appropriate permissions must be configured on the pool or dataset
    being shared by SMB.
-   Users cannot delete shadow copies on the Windows system due to the
    way Samba works. Instead, the administrator can remove snapshots
    from the FreeNAS<sup>®</sup> . The only way to disable shadow copies
    completely is to remove the periodic snapshot task and delete all
    snapshots associated with the SMB share.

To configure shadow copy support, use the instructions in
`Configuring Authenticated Access With Local Users` to create the
desired number of shares.

To enable shadow copies, check the `Enable Shadow Copies` setting when
creating an `smb share <Windows (SMB) Shares>`.

<div class="index">

Time Machine

</div>

Creating Authenticated and Time Machine Shares
----------------------------------------------

macOS includes the [Time Machine][] feature which performs automatic
backups. FreeNAS<sup>®</sup> supports Time Machine backups for both
`SMB <Windows (SMB) Shares>` and `AFP <Apple (AFP) Shares>` shares. The
process for creating an authenticated share for a user is the same as
creating a Time Machine share for that user.

  [Time Machine]: https://support.apple.com/en-us/HT201250

Create Time Machine or authenticated shares on a
`new dataset <Adding Datasets>`.

Change permissions on the new dataset by going to `Storage --> Pools`.
Select the dataset, click , `Change Permissions`.

Enter these settings:

1.  **User:** Use the drop-down to select the desired user account. If
    the user does not yet exist on the FreeNAS<sup>®</sup> system,
    create one with `Accounts --> Users`. See `users <Users>` for more
    information.
2.  **Group:** Select the desired group name. If the group does not yet
    exist on the FreeNAS<sup>®</sup> system, create one with
    `Accounts --> Groups`. See `groups <Groups>` for more information.
3.  Click `SAVE`.

Create the authenticated or Time Machine share:

1.  Go to `Sharing --> Windows (SMB) Shares` or
    `Sharing --> Apple (AFP) Shares` and click . Apple [deprecated the
    AFP protocol][] and recommends using SMB.
2.  `Browse` to the dataset created for the share.
3.  When creating a Time Machine share, set the `Time Machine` option.
4.  Fill out the other required fields.
5.  Click `SAVE`.

  [deprecated the AFP protocol]: https://support.apple.com/en-us/HT207828

When creating multiple authenticated or Time Machine shares, repeat this
process for each user. `Figure %s <creating_an_authenticated_share_fig>`
shows creating a Time Machine Share in `Sharing --> Apple (AFP) Shares`.

<div id="creating_an_authenticated_share_fig">

![Creating an Authenticated or Time Machine Share][]

</div>

  [Creating an Authenticated or Time Machine Share]: images/sharing-apple-afp-add-timemachine.png

Configuring a quota for each Time Machine share helps prevent backups
from using all available space on the FreeNAS<sup>®</sup> system. Time
Machine waits two minutes before creating a full backup. It then creates
ongoing hourly, daily, weekly, and monthly backups. **The oldest backups
are deleted when a Time Machine share fills up, so make sure that the
quota size is large enough to hold the desired number of backups.** Note
that a default installation of macOS is over 20 GiB.

Configure a global quota using the instructions in [Set up Time Machine
for multiple machines with OSX Server-Style Quotas][] or create
individual share quotas.

  [Set up Time Machine for multiple machines with OSX Server-Style Quotas]:
    https://forums.freenas.org/index.php?threads/how-to-set-up-time-machine-for-multiple-machines-with-osx-server-style-quotas.47173/

### Setting SMB and AFP Share Quotas

**SMB Quota**

Go to `Sharing --> Windows (SMB) Shares`, click on the Time Machine
share, and `Edit`. Click `Advanced Mode` and enter a [vfs\_fruit(8)][]
parameter in the `Auxiliary Parameters`. Time Machine quotas use the
`fruit:time machine max size` parameter. For example, to set a quota of
*500 GiB*, enter `fruit:time machine max size = 500 G`.

  [vfs\_fruit(8)]: https://www.samba.org/samba/docs/current/man-html/vfs_fruit.8.html

**AFP Quota**

Go to `Sharing --> Apple (AFP) Shares`, click on the Time Machine share,
and `Edit`. In the example shown in `Figure %s <set_quota_fig>`, the
Time Machine share name is *backup\_user1*. Enter a value in the
`Time Machine Quota` field, and click `SAVE`. In this example, the Time
Machine share is restricted to 200 GiB.

<div id="set_quota_fig">

![Setting an AFP Share Quota][]

</div>

  [Setting an AFP Share Quota]: images/sharing-apple-afp-add-example.png

### Client Time Machine Configuration

<div class="note">

<div class="title">

Note

</div>

The example shown here is intended to show the general process of adding
a FreeNAS<sup>®</sup> share in Time Machine. The example might not
reflect the exact process to configure Time Machine on a specific
version of macOS. See the [Apple documentation][] for detailed Time
Machine configuration instructions.

</div>

  [Apple documentation]: https://support.apple.com/en-us/HT201250

To configure Time Machine on the macOS client, go to
`System Preferences --> Time Machine`, and click `ON` in the left panel.

<div id="config_tm_osx">

![Configuring Time Machine on macOS][]

</div>

  [Configuring Time Machine on macOS]: images/external/sharing-afp-time-machine.png

Click `Select Disk...` in the right panel to find the
FreeNAS<sup>®</sup> system with the share. Highlight the share and click
`Use Backup Disk`. A connection dialog prompts to log in to the
FreeNAS<sup>®</sup> system.

If `Time Machine could not complete the backup. The backup disk
image could not be created (error 45)` is shown when backing up to the
FreeNAS<sup>®</sup> system, a sparsebundle image must be created using
[these instructions][].

  [these instructions]: https://community.netgear.com/t5/Stora-Legacy/Solution-to-quot-Time-Machine-could-not-complete-the-backup/td-p/294697

If `Time Machine completed a verification of your backups.
To improve reliability, Time Machine must create a new backup for you.`
is shown, follow the instructions in [this post][] to avoid making
another backup or losing past backups.

  [this post]: http://www.garth.org/archives/2011,08,27,169,fix-time-machine-sparsebundle-nas-based-backup-errors.html