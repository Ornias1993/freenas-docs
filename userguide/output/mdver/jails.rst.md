<div class="index">

Jails

</div>

Jails
=====

Jails are a lightweight, operating-system-level virtualization. One or
multiple services can run in a jail, isolating those services from the
host FreeNAS<sup>®</sup> system. FreeNAS<sup>®</sup> uses
[iocage](https://github.com/iocage/iocage) for jail and
`plugin <Plugins>` management. The main differences between a
user-created jail and a plugin are that plugins are preconfigured and
usually provide only a single service.

By default, jails run the [FreeBSD](https://www.freebsd.org/) operating
system. These jails are independent instances of FreeBSD. The jail uses
the host hardware and runs on the host kernel, avoiding most of the
overhead usually associated with virtualization. The jail installs
FreeBSD software management utilities so FreeBSD packages or ports can
be installed from the jail command line. This allows for FreeBSD ports
to be compiled and FreeBSD packages to be installed from the command
line of the jail.

It is important to understand that users, groups, installed software,
and configurations within a jail are isolated from both the
FreeNAS<sup>®</sup> host operating system and any other jails running on
that system.

The ability to create multiple jails offers flexibility regarding
software management. For example, an administrator can choose to provide
application separation by installing different applications in each
jail, to create one jail for all installed applications, or to mix and
match how software is installed into each jail.

<div class="index">

Jail Storage

</div>

Jail Storage
------------

A `pool <Creating Pools>` must be created before using jails or
`Plugins`. Make sure the pool has enough storage for all the intended
jails and plugins. The `Jails` screen displays a message and button to
`CREATE POOL` if no pools exist on the FreeNAS<sup>®</sup> system.

If pools exist, but none have been chosen for use with jails or plugins,
a dialog appears to choose a pool. Select a pool and click `CHOOSE`.

To select a different pool for jail and plugin storage, click . A dialog
shows the active pool. A different pool can be selected from the
drop-down.

Jails and downloaded FreeBSD release files are stored in a dataset named
`iocage/`.

Notes about the `iocage/` dataset:

-   At least 10 GiB of free space is recommended.
-   Cannot be located on a `Share <Sharing>`.
-   [iocage](http://iocage.readthedocs.io/en/latest/index.html)
    automatically uses the first pool that is not a root pool for the
    FreeNAS<sup>®</sup> system.
-   A `defaults.json` file contains default settings used when a new
    jail is created. The file is created automatically if not already
    present. If the file is present but corrupted, `iocage` shows a
    warning and uses default settings from memory.
-   Each new jail installs into a new child dataset of `iocage/`. For
    example, with the `iocage/jails` dataset in `pool1`, a new jail
    called *jail1* installs into a new dataset named
    `pool1/iocage/jails/jail1`.
-   FreeBSD releases are fetched as a child dataset into the
    `/iocage/download` dataset. This datset is then extracted into the
    `/iocage/releases` dataset to be used in jail creation. The dataset
    in `/iocage/download` can then be removed without affecting the
    availability of fetched releases or an existing jail.
-   `iocage/` datasets on activated pools are independent of each other
    and do **not** share any data.

<div class="note">

<div class="title">

Note

</div>

iocage jail configs are stored in
`/mnt/{poolname}/iocage/jails/{jailname}`. When iocage is updated, the
`config.json` configuration file is backed up as
`/mnt/{poolname}/iocage/jails/{jailname}/config_backup.json`. The backup
file can be renamed to `config.json` to restore previous jail settings.

</div>

<div class="index">

Add Jail, New Jail, Create Jail

</div>

Creating Jails
--------------

FreeNAS<sup>®</sup> has two options to create a jail. The `Jail Wizard`
makes it easy to quickly create a jail. `ADVANCED JAIL CREATION` is an
alternate method, where every possible jail option is configurable.
There are numerous options spread across four different primary
sections. This form is recommended for advanced users with very specific
requirements for a jail.

<div class="index">

Jail Wizard

</div>

### Jail Wizard

New jails can be created quickly by going to `Jails -->` . This opens
the wizard screen shown in `Figure %s <jail_wizard_fig>`.

<div id="jail_wizard_fig">

![Jail Creation Wizard](images/jails-add-wizard-name.png)

</div>

The wizard provides the simplest process to create and configure a new
jail.

Enter a `Jail Name`. Names can contain letters, numbers, periods (`.`),
dashes (`-`), and underscores (`_`).

Choose a `Jail Type`: *Default (Clone Jail)* or *Basejail*. Clone jails
are clones of the specified FreeBSD RELEASE. They are linked to that
RELEASE, even if they are upgraded. Basejails mount the specified
RELEASE directories as nullfs mounts over the jail directories.
Basejails are not linked to the original RELEASE when upgraded.

Jails can run FreeBSD versions up to the same version as the host
FreeNAS<sup>®</sup> system. Newer releases are not shown.

<div class="tip">

<div class="title">

Tip

</div>

Versions of FreeBSD are downloaded the first time they are used in a
jail. Additional jails created with the same version of FreeBSD are
created faster because the download has already been completed.

</div>

Click `NEXT` to see a simplified list of networking options.

<div id="Jail Networking">

Jails support several different networking solutions:

</div>

-   `VNET` can be set to add a virtual network interface to the jail.
    This interface can be used to set NAT, DHCP, or static jail network
    configurations. Since `VNET` provides the jail with an independent
    networking stack, it can broadcast an IP address, which is required
    by some applications.
-   The jail can use [Network Address Translation
    (NAT)](https://en.wikipedia.org/wiki/Network_address_translation),
    which uses the FreeNAS<sup>®</sup> IP address and sets a unique port
    for the jail to use. `VNET` is required when `NAT` is selected.
-   Configure the jail to receive its IP address from a DHCP server by
    setting `DHCP Autoconfigure IPv4`.
-   Networking can be manually configured by entering values for the
    `IPv4 Address` or `IPv6 Address` fields. Any combination of these
    fields can be configured. Multiple interfaces are supported for IPv4
    and IPv6 addresses. To add more interfaces and addresses, click
    `ADD`. Setting the `IPv4 Default Router` and `IPv6 Default Router`
    fields to *auto* automatically configures these values. `VNET` must
    be set to enable the `IPv4 Default Router` field. If no interface is
    selected when manually configuring IP addresses, FreeNAS<sup>®</sup>
    automatically assigns the given IP address of the jail to the
    current active interface of the host system.
-   Leaving all checkboxes unset and fields empty initializes the jail
    without any networking abilities. Networking can be added to the
    jail after creation by going to `Jails -->` `-->`
    `--> Basic Properties`.

Setting a proxy in the FreeNAS<sup>®</sup>
`network settings <Global Configuration>` also configures new jails to
use the proxy settings, except when performing DNS lookups. Make sure a
firewall is properly configured to maximize system security.

When pairing the jail with a physical interface, edit the
`network interface <Interfaces>` and set `Disable Hardware Offloading`.
This prevents a network interface reset when the jail starts.

<div id="jail_wizard_networking_fig">

![Configure Jail Networking](images/jails-add-wizard-networking.png)

</div>

Click `NEXT` to view a summary screen of the chosen jail options. Click
`SUBMIT` to create the new jail. After a few moments, the new jail is
added to the primary jails list.

<div class="index">

Advanced Jail Creation

</div>

### Advanced Jail Creation

The advanced jail creation form is opened by clicking `Jails -->` then
`Advanced Jail Creation`. The screen in `Figure %s <creating_jail_fig>`
is shown.

<div id="creating_jail_fig">

![Creating a Jail](images/jails-add-advanced.png)

</div>

A usable jail can be quickly created by setting only the required
values, the `Jail Name` and `Release`. Additional settings are in the
`Jail Properties`, `Network Properties`, and `Custom Properties`
sections. `Table %s <jail_basic_props_tab>` shows the available options
of the `Basic Properties` of a new jail.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.15linewidth-2tabcolsep}

</div>

<div id="jail_basic_props_tab">

<table>
<caption>Basic Properties</caption>
<colgroup>
<col style="width: 18%" />
<col style="width: 12%" />
<col style="width: 68%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Name</td>
<td>string</td>
<td>Required. Can contain letters, numbers, periods (<code>.</code>), dashes (<code>-</code>), and underscores (<code>_</code>).</td>
</tr>
<tr class="even">
<td>Jail Type</td>
<td>drop-down</td>
<td><em>Default (Clone Jail)</em> are clones of the specified RELEASE. They are linked to that RELEASE, even if they are upgraded. <em>Basejail</em> mount the specified RELEASE directories as nullfs mounts over the jail directories. Basejails are not linked to the original RELEASE when upgraded.</td>
</tr>
<tr class="odd">
<td>Release</td>
<td>drop-down menu</td>
<td>Required. Jails can run FreeBSD versions up to the same version as the host FreeNAS<sup>®</sup> system. Newer releases are not shown.</td>
</tr>
<tr class="even">
<td>DHCP Autoconfigure IPv4</td>
<td>checkbox</td>
<td>Automatically configure IPv4 networking with an independent VNET stack. <code class="interpreted-text" role="guilabel">VNET</code> and <code class="interpreted-text" role="guilabel">Berkeley Packet Filter</code> must also be checked. If not set, ensure the defined address in <code class="interpreted-text" role="guilabel">IPv4 Address</code> does not conflict with an existing address.</td>
</tr>
<tr class="odd">
<td>NAT</td>
<td>checkbox</td>
<td>Network Address Translation (NAT). When set, the jail is given an internal IP address and connections are forwarded from the host to the jail. When NAT is set, <code class="interpreted-text" role="guilabel">Berkeley Packet Filter</code> cannot be set. Adds the <code class="interpreted-text" role="guilabel">NAT Port Forwarding</code> options to the jail <code class="interpreted-text" role="ref">Network Properties &lt;jail_network_props_tab&gt;</code>.</td>
</tr>
<tr class="even">
<td>VNET</td>
<td>checkbox</td>
<td>Use VNET to emulate network devices for this jail and a create a fully virtualized per-jail network stack. See <a href="https://www.freebsd.org/cgi/man.cgi?query=vnet">VNET(9)</a> for more details.</td>
</tr>
<tr class="odd">
<td>Berkeley Packet Filter</td>
<td>checkbox</td>
<td>Use the Berkeley Packet Filter to data link layers in a protocol independent fashion. Unset by default to avoid security vulnerabilities. See <a href="https://www.freebsd.org/cgi/man.cgi?query=bpf">BPF(4)</a> for more details. Cannot be set when <code class="interpreted-text" role="guilabel">NAT</code> is set.</td>
</tr>
<tr class="even">
<td>vnet_default_interface</td>
<td>drop-down</td>
<td>Set the default VNET interface. Only takes effect when <code class="interpreted-text" role="guilabel">VNET</code> is set. Choose a specific interface, or set to <em>auto</em> to use the interface that has the default route. Choose <em>none</em> to not set a default VNET interface.</td>
</tr>
<tr class="odd">
<td>IPv4 Interface</td>
<td>drop-down menu</td>
<td>Choose a network interface to use for this IPv4 connection. See <code class="interpreted-text" role="ref">note &lt;additional interfaces&gt;</code> to add more.</td>
</tr>
<tr class="even">
<td>IPv4 Address</td>
<td>string</td>
<td><p>This and the other IPv4 settings are grayed out if <code class="interpreted-text" role="guilabel">DHCP autoconfigure IPv4</code> is set. Configures the interface to use for network or internet access for the jail.</p>
<p>Enter an IPv4 address for this IP jail. Example: <em>192.168.0.10</em>. See <code class="interpreted-text" role="ref">note &lt;additional interfaces&gt;</code> to add more.</p></td>
</tr>
<tr class="odd">
<td>IPv4 Netmask</td>
<td>drop-down menu</td>
<td>Choose a subnet mask for this IPv4 Address.</td>
</tr>
<tr class="even">
<td>IPv4 Default Router</td>
<td>string</td>
<td>Type <code>none</code> or a valid IP address. Setting this property to anything other than <em>none</em> configures a default route inside a VNET jail.</td>
</tr>
<tr class="odd">
<td>Auto Configure IPv6</td>
<td>checkbox</td>
<td>Set to use SLAAC (Stateless Address Auto Configuration) to autoconfigure IPv6 in the jail.</td>
</tr>
<tr class="even">
<td>IPv6 Interface</td>
<td>drop-down menu</td>
<td>Choose a network interface to use for this IPv6 connection. See <code class="interpreted-text" role="ref">note &lt;additional interfaces&gt;</code> to add more.</td>
</tr>
<tr class="odd">
<td>IPv6 Address</td>
<td>string</td>
<td><p>Configures network or internet access for the jail.</p>
<p>Type the IPv6 address for VNET and shared IP jails. Example: <em>2001:0db8:85a3:0000:0000:8a2e:0370:7334</em>. See <code class="interpreted-text" role="ref">note &lt;additional interfaces&gt;</code> to add more.</p></td>
</tr>
<tr class="even">
<td>IPv6 Prefix</td>
<td>drop-down menu</td>
<td>Choose a prefix for this IPv6 Address.</td>
</tr>
<tr class="odd">
<td>IPv6 Default Router</td>
<td>string</td>
<td>Type <code>none</code> or a valid IP address. Setting this property to anything other than <em>none</em> configures a default route inside a VNET jail.</td>
</tr>
<tr class="even">
<td>Notes</td>
<td>string</td>
<td>Enter any notes or comments about the jail.</td>
</tr>
<tr class="odd">
<td>Auto-start</td>
<td>checkbox</td>
<td>Start the jail at system startup.</td>
</tr>
</tbody>
</table>

Basic Properties

</div>

<div id="additional interfaces" class="note">

<div class="title">

Note

</div>

For static configurations not using DHCP or NAT, multiple IPv4 and IPv6
addresses and interfaces can be added to the jail by clicking `ADD`.

</div>

Similar to the `Jail Wizard`, configuring the basic properties, then
clicking `SAVE` is often all that is needed to quickly create a new
jail. To continue configuring more settings, click `NEXT` to proceed to
the `Jail Properties` section of the form.
`Table %s <jail_jail_props_tab>` describes each of these options.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.15linewidth-2tabcolsep}

</div>

<div id="jail_jail_props_tab">

<table>
<caption>Jail Properties</caption>
<colgroup>
<col style="width: 16%" />
<col style="width: 9%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>devfs_ruleset</td>
<td>integer</td>
<td>Number of the <a href="https://www.freebsd.org/cgi/man.cgi?query=devfs">devfs(8)</a> ruleset to enforce when mounting <em>devfs</em> in the jail. The default value of <em>0</em> means no ruleset is enforced. Mounting <em>devfs</em> inside a jail is only possible when the <code class="interpreted-text" role="guilabel">allow_mount</code> and <code class="interpreted-text" role="guilabel">allow_mount_devfs</code> permissions are enabled and <code class="interpreted-text" role="guilabel">enforce_statfs</code> is set to a value lower than <em>2</em>.</td>
</tr>
<tr class="even">
<td>exec.start</td>
<td>string</td>
<td>Commands to run in the jail environment when a jail is created. Example: <code class="interpreted-text" role="samp">sh /etc/rc</code>. See <a href="https://www.freebsd.org/cgi/man.cgi?query=jail">jail(8)</a> for more details.</td>
</tr>
<tr class="odd">
<td>exec.stop</td>
<td>string</td>
<td>Commands to run in the jail environment before a jail is removed and after any <code class="interpreted-text" role="guilabel">exec_prestop</code> commands are complete. Example: <code class="interpreted-text" role="samp">sh /etc/rc.shutdown</code>.</td>
</tr>
<tr class="even">
<td>exec_prestart</td>
<td>string</td>
<td>Commands to run in the system environment before a jail is started.</td>
</tr>
<tr class="odd">
<td>exec_poststart</td>
<td>string</td>
<td>Commands to run in the system environment after a jail is started and after any <code class="interpreted-text" role="guilabel">exec_start</code> commands are finished.</td>
</tr>
<tr class="even">
<td>exec_prestop</td>
<td>string</td>
<td>Commands to run in the system environment before a jail is stopped.</td>
</tr>
<tr class="odd">
<td>exec_poststop</td>
<td>string</td>
<td>Commands to run in the system environment after a jail is started and after any <code class="interpreted-text" role="guilabel">exec_start</code> commands are finished.</td>
</tr>
<tr class="even">
<td>exec_clean</td>
<td>checkbox</td>
<td><p>Run commands in a clean environment. The current environment is discarded except for $HOME, $SHELL, $TERM and $USER.</p>
<p>$HOME and $SHELL are set to the target login. $USER is set to the target login. $TERM is imported from the current environment. The environment variables from the login class capability database for the target login are also set.</p></td>
</tr>
<tr class="odd">
<td>exec_timeout</td>
<td>integer</td>
<td>The maximum amount of time in seconds to wait for a command to complete. If a command is still running after the allotted time, the jail is terminated.</td>
</tr>
<tr class="even">
<td>stop_timeout</td>
<td>integer</td>
<td>The maximum amount of time in seconds to wait for the jail processes to exit after sending a SIGTERM signal. This happens after any <code class="interpreted-text" role="guilabel">exec_stop</code> commands are complete. After the specified time, the jail is removed, killing any remaining processes. If set to <em>0</em>, no SIGTERM is sent and the jail is immeadility removed.</td>
</tr>
<tr class="odd">
<td>exec_jail_user</td>
<td>string</td>
<td>Enter either <code>root</code> or a valid <em>username</em>. Inside the jail, commands run as this user.</td>
</tr>
<tr class="even">
<td>exec_system_jail_user</td>
<td>string</td>
<td>Set to <em>True</em> to look for the <code class="interpreted-text" role="guilabel">exec.jail_user</code> in the system <a href="https://www.freebsd.org/cgi/man.cgi?query=passwd">passwd(5)</a> file <em>instead</em> of the jail <code class="interpreted-text" role="file">passwd</code>.</td>
</tr>
<tr class="odd">
<td>exec_system_user</td>
<td>string</td>
<td>Run commands in the jail as this user. By default, commands are run as the current user.</td>
</tr>
<tr class="even">
<td>mount_devfs</td>
<td>checkbox</td>
<td>Mount a <a href="https://www.freebsd.org/cgi/man.cgi?query=devfs">devfs(5)</a> filesystem on the chrooted <code class="interpreted-text" role="file">/dev</code> directory and apply the ruleset in the <code class="interpreted-text" role="guilabel">devfs_ruleset</code> parameter to restrict the devices visible inside the jail.</td>
</tr>
<tr class="odd">
<td>mount_fdescfs</td>
<td>checkbox</td>
<td>Mount an <a href="https://www.freebsd.org/cgi/man.cgi?query=fdescfs">fdescfs(5)</a> filesystem in the jail <code class="interpreted-text" role="file">/dev/fd</code> directory.</td>
</tr>
<tr class="even">
<td>enforce_statfs</td>
<td>drop-down</td>
<td><p>Determine which information processes in a jail are able to obtain about mount points. The behavior of multiple syscalls is affected: <a href="https://www.freebsd.org/cgi/man.cgi?query=statfs">statfs(2)</a>, <a href="https://www.freebsd.org/cgi/man.cgi?query=statfs">fstatfs(2)</a>, <a href="https://www.freebsd.org/cgi/man.cgi?query=getfsstat">getfsstat(2)</a>, <a href="https://www.freebsd.org/cgi/man.cgi?query=fhstatfs">fhstatfs(2)</a>, and other similar compatibility syscalls.</p>
<p>All mount points are available without any restrictions if this is set to <em>0</em>. Only mount points below the jail chroot directory are available if this is set to <em>1</em>. Set to <em>2</em>, the default option only mount points where the jail chroot directory is located are available.</p></td>
</tr>
<tr class="odd">
<td>children_max</td>
<td>integer</td>
<td>Number of child jails allowed to be created by the jail or other jails under this jail. A limit of <em>0</em> restricts the jail from creating child jails. <em>Hierarchical Jails</em> in the <a href="https://www.freebsd.org/cgi/man.cgi?query=jail">jail(8)</a> man page explains the finer details.</td>
</tr>
<tr class="even">
<td>login_flags</td>
<td>string</td>
<td>Flags to pass to <a href="https://www.freebsd.org/cgi/man.cgi?query=login">login(1)</a> when logging in to the jail using the <strong>console</strong> function.</td>
</tr>
<tr class="odd">
<td>securelevel</td>
<td>integer</td>
<td>Value of the jail <a href="https://www.freebsd.org/doc/faq/security.html">securelevel</a> sysctl. A jail never has a lower securelevel than the host system. Setting this parameter allows a higher securelevel. If the host system securelevel is changed, jail securelevel will be at least as secure. Securelevel options are: <em>3</em>, <em>2 (default)</em>, <em>1</em>, <em>0</em>, and <em>-1</em>.</td>
</tr>
<tr class="even">
<td>sysvmsg</td>
<td>drop-down</td>
<td>Allow or deny access to SYSV IPC message primitives. Set to <em>Inherit</em>: All IPC objects on the system are visible to the jail. Set to <em>New</em>: Only objects the jail created using the private key namespace are visible. The system and parent jails have access to the jail objects but not private keys. Set to <em>Disable</em>: The jail cannot perform any sysvmsg related system calls.</td>
</tr>
<tr class="odd">
<td>sysvsem</td>
<td>drop-down</td>
<td>Allow or deny access to SYSV IPC semaphore primitives. Set to <em>Inherit</em>: All IPC objects on the system are visible to the jail. Set to <em>New</em>: Only objects the jail creates using the private key namespace are visible. The system and parent jails have access to the jail objects but not private keys. Set to <em>Disable</em>: The jail cannot perform any <strong>sysvmem</strong> related system calls.</td>
</tr>
<tr class="even">
<td>sysvshm</td>
<td>drop-down</td>
<td>Allow or deny access to SYSV IPC shared memory primitives. Set to <em>Inherit</em>: All IPC objects on the system are visible to the jail. Set to <em>New</em>: Only objects the jail creates using the private key namespace are visible. The system and parent jails have access to the jail objects but not private keys. Set to <em>Disable</em>: The jail cannot perform any sysvshm related system calls.</td>
</tr>
<tr class="odd">
<td>allow_set_hostname</td>
<td>checkbox</td>
<td>Allow the jail hostname to be changed with <a href="https://www.freebsd.org/cgi/man.cgi?query=hostname">hostname(1)</a> or <a href="https://www.freebsd.org/cgi/man.cgi?query=sethostname">sethostname(3)</a>.</td>
</tr>
<tr class="even">
<td>allow_sysvipc</td>
<td>checkbox</td>
<td><p>Choose whether a process in the jail has access to System V IPC primitives. Equivalent to setting <code class="interpreted-text" role="guilabel">sysvmsg</code>, <code class="interpreted-text" role="guilabel">sysvsem</code>, and <code class="interpreted-text" role="guilabel">sysvshm</code> to <em>Inherit</em>.</p>
<p><em>Deprecated in FreeBSD 11.0 and later!</em> Use <code class="interpreted-text" role="guilabel">sysvmsg</code>, <code class="interpreted-text" role="guilabel">sysvsem</code>,and <code class="interpreted-text" role="guilabel">sysvshm</code> instead.</p></td>
</tr>
<tr class="odd">
<td>allow_raw_sockets</td>
<td>checkbox</td>
<td>Allow the jail to use <a href="https://en.wikipedia.org/wiki/Network_socket#Raw_socket">raw sockets</a>. When set, the jail has access to lower-level network layers. This allows utilities like <a href="https://www.freebsd.org/cgi/man.cgi?query=ping">ping(8)</a> and <a href="https://www.freebsd.org/cgi/man.cgi?query=traceroute">traceroute(8)</a> to work in the jail, but has security implications and should only be used on jails running trusted software.</td>
</tr>
<tr class="even">
<td>allow_chflags</td>
<td>checkbox</td>
<td>Treat jail users as privileged and allow the manipulation of system file flags. <em>securelevel</em> constraints are still enforced.</td>
</tr>
<tr class="odd">
<td>allow_mlock</td>
<td>checkbox</td>
<td>Allow jail to run services that use <a href="https://www.freebsd.org/cgi/man.cgi?query=mlock">mlock(2)</a> to lock physical pages in memory.</td>
</tr>
<tr class="even">
<td>allow_mount</td>
<td>checkbox</td>
<td>Allow privileged users inside the jail to mount and unmount filesystem types marked as jail-friendly.</td>
</tr>
<tr class="odd">
<td>allow_mount_devfs</td>
<td>checkbox</td>
<td>Allow privileged users inside the jail to mount and unmount the <a href="https://www.freebsd.org/cgi/man.cgi?query=devfs">devfs(5) device filesystem</a>. This permission is only effective when <code class="interpreted-text" role="guilabel">allow_mount</code> is set and <code class="interpreted-text" role="guilabel">enforce_statfs</code> is set to a value lower than <em>2</em>.</td>
</tr>
<tr class="even">
<td>allout_mount_fusefs</td>
<td>checkbox</td>
<td>Allow privileged users inside the jail to mount and unmount fusefs. The jail must have FreeBSD 12.0 or newer installed. This permission is only effective when <code class="interpreted-text" role="guilabel">allow_mount</code> is set and <code class="interpreted-text" role="guilabel">enforce_statfs</code> is set to a value lower than 2.</td>
</tr>
<tr class="odd">
<td>allow_mount_nullfs</td>
<td>checkbox</td>
<td>Allow privileged users inside the jail to mount and unmount the <a href="https://www.freebsd.org/cgi/man.cgi?query=nullfs">nullfs(5) file system</a>. This permission is only effective when <code class="interpreted-text" role="guilabel">allow_mount</code> is set and <code class="interpreted-text" role="guilabel">enforce_statfs</code> is set to a value lower than <em>2</em>.</td>
</tr>
<tr class="even">
<td>allow_mount_procfs</td>
<td>checkbox</td>
<td>Allow privileged users inside the jail to mount and unmount the <a href="https://www.freebsd.org/cgi/man.cgi?query=procfs">procfs(5) file system</a>. This permission is only effective when <code class="interpreted-text" role="guilabel">allow_mount</code> is set and <code class="interpreted-text" role="guilabel">enforce_statfs</code> is set to a value lower than <em>2</em>.</td>
</tr>
<tr class="odd">
<td>allow_mount_tmpfs</td>
<td>checkbox</td>
<td>Allow privileged users inside the jail to mount and unmount the <a href="https://www.freebsd.org/cgi/man.cgi?query=tmpfs">tmpfs(5) file system</a>. This permission is only effective when <code class="interpreted-text" role="guilabel">allow_mount</code> is set and <code class="interpreted-text" role="guilabel">enforce_statfs</code> is set to a value lower than <em>2</em>.</td>
</tr>
<tr class="even">
<td>allow_mount_zfs</td>
<td>checkbox</td>
<td>Allow privileged users inside the jail to mount and unmount the ZFS file system. This permission is only effective when <code class="interpreted-text" role="guilabel">allow_mount</code> is set and <code class="interpreted-text" role="guilabel">enforce_statfs</code> is set to a value lower than <em>2</em>. The <a href="https://www.freebsd.org/cgi/man.cgi?query=zfs">ZFS(8)</a> man page has information on how to configure the ZFS filesystem to operate from within a jail.</td>
</tr>
<tr class="odd">
<td>allow_vmm</td>
<td>checkbox</td>
<td>Grants the jail access to the Bhyve Virtual Machine Monitor (VMM). The jail must have FreeBSD 12.0 or newer installed with the <a href="https://www.freebsd.org/cgi/man.cgi?query=vmm">vmm(4)</a> kernel module loaded.</td>
</tr>
<tr class="even">
<td>allow_quotas</td>
<td>checkbox</td>
<td>Allow the jail root to administer quotas on the jail filesystems. This includes filesystems the jail shares with other jails or with non-jailed parts of the system.</td>
</tr>
<tr class="odd">
<td>allow_socket_af</td>
<td>checkbox</td>
<td>Allow access to other protocol stacks beyond IPv4, IPv6, local (UNIX), and route. <strong>Warning</strong>: jail functionality does not exist for all protocal stacks.</td>
</tr>
<tr class="even">
<td>vnet_interfaces</td>
<td>string</td>
<td>Space-delimited list of network interfaces to attach to a VNET-enabled jail after it is created. Interfaces are automatically released when the jail is removed.</td>
</tr>
</tbody>
</table>

Jail Properties

</div>

Click `NEXT` to view all jail `Network Properties`. These are shown in
`Table %s <jail_network_props_tab>`:

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.15linewidth-2tabcolsep}

</div>

<div id="jail_network_props_tab">

| Setting          | Value     | Description                                                                                                                                                                                                                               |
|------------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| interfaces       | string    | Enter up to four interface configurations in the format *interface:bridge*, separated by a comma (`,`). The left value is the virtual VNET interface name and the right value is the bridge name where the virtual interface is attached. |
| host\_domainname | string    | Enter an [NIS Domain name](https://www.freebsd.org/doc/handbook/network-nis.html) for the jail.                                                                                                                                           |
| host\_hostname   | string    | Enter a hostname for the jail. By default, the system uses the jail NAME/UUID.                                                                                                                                                            |
| exec\_fib        | integer   | Enter a number to define the routing table (FIB) to set when running commands inside the jail.                                                                                                                                            |
| ip4.saddrsel     | checkbox  | Disables IPv4 source address selection for the jail in favor of the primary IPv4 address of the jail. Only available when the jail is not configured to use VNET.                                                                         |
| ip4              | drop-down | Control the availability of IPv4 addresses. Set to *Inherit*: allow unrestricted access to all system addresses. Set to *New*: restrict addresses with `ip4_addr`. Set to *Disable*: stop the jail from using IPv4 entirely.              |
| ip6.saddrsel     | string    | Disable IPv6 source address selection for the jail in favor of the primary IPv6 address of the jail. Only available when the jail is not configured to use VNET.                                                                          |
| ip6              | drop-down | Control the availability of IPv6 addresses. Set to *Inherit*: allow unrestricted access to all system addresses. Set to *New*: restrict addresses with `ip6_addr`. Set to *Disable*: stop the jail from using IPv6 entirely.              |
| resolver         | string    | Add lines to `resolv.conf` in file. Example: *nameserver IP;search domain.local*. Fields must be delimited with a semicolon (`;`), this is translated as new lines in `resolv.conf`. Enter `none` to inherit `resolv.conf` from the host. |
| mac\_prefix      | string    | Optional. Enter a valid MAC address vendor prefix. Example: *E4F4C6*                                                                                                                                                                      |
| vnet0\_mac       | string    | Leave this blank to generate random MAC addresses for the host and jail. To assign fixed MAC addresses, enter the host MAC address and the jail MAC address separated by a space.                                                         |
| vnet1\_mac       | string    | Leave this blank to generate random MAC addresses for the host and jail. To assign fixed MAC addresses, enter the host MAC address and the jail MAC address separated by a space.                                                         |
| vnet2\_mac       | string    | Leave this blank to generate random MAC addresses for the host and jail. To assign fixed MAC addresses, enter the host MAC address and the jail MAC address separated by a space.                                                         |
| vnet3\_mac       | string    | Leave this blank to generate random MAC addresses for the host and jail. To assign fixed MAC addresses, enter the host MAC address and the jail MAC address separated by a space.                                                         |

Network Properties

</div>

The final set of jail properties are contained in the
`Custom Properties` section. `Table %s <jail_custom_props_tab>`
describes these options.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.15linewidth-2tabcolsep}

</div>

<div id="jail_custom_props_tab">

<table>
<caption>Custom Properties</caption>
<colgroup>
<col style="width: 17%" />
<col style="width: 9%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>owner</td>
<td>string</td>
<td>The owner of the jail. Can be any string.</td>
</tr>
<tr class="even">
<td>priority</td>
<td>integer</td>
<td>The numeric start priority for the jail at boot time. <strong>Smaller</strong> values mean a <strong>higher</strong> priority. At system shutdown, the priority is <em>reversed</em>. Example: 99</td>
</tr>
<tr class="odd">
<td>hostid</td>
<td>string</td>
<td>A new a jail hostid, if necessary. Example hostid: <em>1a2bc345-678d-90e1-23fa-4b56c78901de</em>.</td>
</tr>
<tr class="even">
<td>hostid_strict_check</td>
<td>checkbox</td>
<td>Check the jail <code class="interpreted-text" role="guilabel">hostid</code> property. Prevents the jail from starting if the <code class="interpreted-text" role="guilabel">hostid</code> does not match the host.</td>
</tr>
<tr class="odd">
<td>comment</td>
<td>string</td>
<td>Comments about the jail.</td>
</tr>
<tr class="even">
<td>depends</td>
<td>string</td>
<td>Specify any jails the jail depends on. Child jails must already exist before the parent jail can be created.</td>
</tr>
<tr class="odd">
<td>mount_procfs</td>
<td>checkbox</td>
<td>Allow mounting of a <a href="https://www.freebsd.org/cgi/man.cgi?query=procfs">procfs(5)</a> filesystems in the jail <code class="interpreted-text" role="file">/dev/proc</code> directory.</td>
</tr>
<tr class="even">
<td>mount_linprocfs</td>
<td>checkbox</td>
<td>Allow mounting of a <a href="https://www.freebsd.org/cgi/man.cgi?query=linprocfs">linprocfs(5)</a> filesystem in the jail.</td>
</tr>
<tr class="odd">
<td>template</td>
<td>checkbox</td>
<td>Convert the jail into a template. Template jails can be used to quickly create jails with the same configuration.</td>
</tr>
<tr class="even">
<td>host_time</td>
<td>checkbox</td>
<td>Synchronize the time between jail and host.</td>
</tr>
<tr class="odd">
<td>jail_zfs</td>
<td>checkbox</td>
<td><p>Enable automatic ZFS jailing inside the jail. The assigned ZFS dataset is fully controlled by the jail.</p>
<p>Note: <code class="interpreted-text" role="guilabel">allow_mount</code>, <code class="interpreted-text" role="guilabel">enforce_statfs</code>, and <code class="interpreted-text" role="guilabel">allow_mount_zfs</code> must all be set for ZFS management inside the jail to work correctly.</p></td>
</tr>
<tr class="even">
<td>jail_zfs_dataset</td>
<td>string</td>
<td>Define the dataset to be jailed and fully handed over to a jail. Enter a ZFS filesystem name without a pool name. <code class="interpreted-text" role="guilabel">jail_zfs</code> must be set for this option to work.</td>
</tr>
<tr class="odd">
<td>jail_zfs_mountpoint</td>
<td>string</td>
<td>The mountpoint for the <code class="interpreted-text" role="guilabel">jail_zfs_dataset</code>. Example: <em>/data/example-dataset-name</em></td>
</tr>
<tr class="even">
<td>allow_tun</td>
<td>checkbox</td>
<td>Expose host <a href="https://www.freebsd.org/cgi/man.cgi?query=tun">tun(4)</a> devices in the jail. Allow the jail to create tun devices.</td>
</tr>
<tr class="odd">
<td>Autoconfigure IPv6 with rtsold</td>
<td>checkbox</td>
<td>Use <a href="https://www.freebsd.org/cgi/man.cgi?query=rtsold">rtsold(8)</a> as part of IPv6 autoconfiguration. Send ICMPv6 Router Solicitation messages to interfaces to discover new routers.</td>
</tr>
<tr class="even">
<td>ip_hostname</td>
<td>checkbox</td>
<td>Use DNS records during jail IP configuration to search the resolver and apply the first open IPv4 and IPv6 addresses. See <a href="https://www.freebsd.org/cgi/man.cgi?query=jail">jail(8)</a>.</td>
</tr>
<tr class="odd">
<td>assign_localhost</td>
<td>checkbox</td>
<td>Add network interface <em>lo0</em> to the jail and assign it the first available localhost address, starting with <em>127.0.0.2</em>. <em>VNET</em> cannot be set. Jails using <em>VNET</em> configure a localhost as part of their virtualized network stack.</td>
</tr>
</tbody>
</table>

Custom Properties

</div>

Click `SAVE` when the desired jail properties have been set. New jails
are added to the primary list in the `Jails` menu.

<div class="index">

Creating Template Jails

</div>

#### Creating Template Jails

Template jails are basejails that can be used as a template to
efficiently create jails with the same configuration. These steps create
a template jail:

1.  Go to `Jails --> ADD --> ADVANCED JAIL CREATION`.
2.  Select *Basejail* as the `Jail Type`. Configure the jail with
    desired options.
3.  Set `template` in the `Custom Properties` tab.
4.  Click `Save`.
5.  Click `ADD`.
6.  Enter a name for the template jail. Leave `Jail Type` as *Default
    (Clone Jail)*. Set `Release` to `basejailname(template)`, where
    *basejailname* is the name of the base jail created earlier.
7.  Complete the jail creation wizard.

<div class="index">

Managing Jails

</div>

Managing Jails
--------------

Clicking `Jails` shows a list of installed jails. An example is shown in
`Figure %s <jail_overview_fig>`.

<div id="jail_overview_fig">

![Jail Overview Section](images/jails.png)

</div>

Operations can be applied to multiple jails by selecting those jails
with the checkboxes on the left. After selecting one or more jails,
icons appear which can be used to , , , or those jails.

More information such as *IPV4*, *IPV6*, *TYPE* of jail, and whether it
is a *TEMPLATE* jail or *BASEJAIL* can be shown by clicking . Additional
options for that jail are also displayed. These are described in
`Table %s <jail_option_menu_tab>`.

`Figure %s <jail_option_menu_fig>` shows the menu that appears.

<div id="jail_option_menu_fig">

![Jail Options Menu](images/jails-actions.png)

</div>

<div class="warning">

<div class="title">

Warning

</div>

Modify the IP address information for a jail by clicking `--> EDIT`
instead of issuing the networking commands directly from the command
line of the jail. This ensures the changes are saved and will survive a
jail or FreeNAS<sup>®</sup> reboot.

</div>

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.75linewidth-2tabcolsep}\|

</div>

<div id="jail_option_menu_tab">

| Option       | Description                                                                                                                                                                                                                                                                                                              |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| EDIT         | Used to modify the settings described in `Advanced Jail Creation`. A jail cannot be edited while it is running. The settings can be viewed, but are read only.                                                                                                                                                           |
| MOUNT POINTS | Select an existing mount point to `EDIT` or click `ACTIONS --> Add Mount Point` to create a mount point for the jail. A mount point gives a jail access to storage located elsewhere on the system. A jail must be stopped before adding, editing, or deleting a mount point. See `Additional Storage` for more details. |
| RESTART      | Stop and immediately start an `up` jail.                                                                                                                                                                                                                                                                                 |
| START        | Start a jail that has a current `STATE` of *down*.                                                                                                                                                                                                                                                                       |
| STOP         | Stop a jail that has a current `STATE` of *up*.                                                                                                                                                                                                                                                                          |
| UPDATE       | Runs [freebsd-update](https://www.freebsd.org/cgi/man.cgi?query=freebsd-update) to update the jail to the latest patch level of the installed FreeBSD release.                                                                                                                                                           |
| SHELL        | Access a *root* command prompt to interact with a jail directly from the command line. Type `exit` to leave the command prompt.                                                                                                                                                                                          |
| DELETE       | Caution: deleting the jail also deletes all of the jail contents and all associated `snapshots <Snapshots>`. Back up the jail data, configuration, and programs first. There is no way to recover the contents of a jail after deletion!                                                                                 |

Jail Option Menu Entry Descriptions

</div>

<div class="note">

<div class="title">

Note

</div>

Menu entries change depending on the jail state. For example, a stopped
jail does not have a `STOP` or `SHELL` option.

</div>

Jail status messages and command output are stored in
`/var/log/iocage.log`.

<div class="index">

Updating a Jail, Upgrading a Jail

</div>

### Jail Updates and Upgrades

Click `--> Update` to update a jail to the most current patch level of
the installed FreeBSD release. This does **not** change the release. For
example, a jail installed with *FreeBSD 11.2-RELEASE* can update to
*p15* or the latest patch of 11.2, but not an 11.3-RELEASE-p\# version
of FreeBSD.

A jail *upgrade* replaces the jail FreeBSD operating system with a new
release of FreeBSD, such as taking a jail from FreeBSD 11.2-RELEASE to
11.3-RELEASE. Upgrade a jail by stopping it, opening the `SHELL` and
entering `iocage upgrade {name} -r {release}`, where *name* is the
plugin jail name and *release* is the desired release to upgrade to.

<div class="tip">

<div class="title">

Tip

</div>

It is possible to `manually remove <storage dataset options>` unused
releases from the `/iocage/releases/` dataset after upgrading a jail.
The release **must** not be in use by any jail on the system!

</div>

<div class="index">

Accessing a Jail Using SSH, SSH

</div>

### Accessing a Jail Using SSH

The ssh daemon [sshd(8)](https://www.freebsd.org/cgi/man.cgi?query=sshd)
must be enabled in a jail to allow SSH access to that jail from another
system.

The jail `STATE` must be *up* before the `SHELL` option is available. If
the jail is not up, start it by clicking `Jails -->` `--> START` for the
desired jail. Click `--> SHELL` to open a shell in the jail. A jail root
shell is shown in this example:

    Last login: Fri Apr 6 07:57:04 on pts/12
    FreeBSD 11.1-STABLE (FreeNAS.amd64) #0 0ale9f753(freenas/11-stable): FriApr 6 04:46:31 UTC 2018

    Welcome to FreeBSD!

    Release Notes, Errata: https://www.FreeBSD.org/releases/
    Security Advisories:   https://www.FreeBSD.org/security/
    FreeBSD Handbook:      https://www.FreeBSD.org/handbook/
    FreeBSD FAQ:           https://www.FreeBSD.org/faq/
    Questions List: https://lists.FreeBSD.org/mailman/listinfo/freebsd-questions/
    FreeBSD Forums:        https://forums.FreeBSD.org/

    Documents installed with the system are in the /usr/local/share/doc/freebsd/
    directory, or can be installed later with: pkg install en-freebsd-doc
    For other languages, replace "en" with a language code like de or fr.

    Show the version of FreeBSD installed: freebsd-version ; uname -a
    Please include that output and any error messages when posting questions.
    Introduction to manual pages: man man
    FreeBSD directory layout:     man hier

    Edit /etc/motd to change this login announcement.
    root@jailexamp:~ #

<div class="tip">

<div class="title">

Tip

</div>

A root shell can also be opened for a jail using the FreeNAS<sup>®</sup>
UI `Shell`. Open the `Shell`, then type `iocage console {jailname}`.

</div>

Enable sshd:

    sysrc sshd_enable="YES"
    sshd_enable: NO -> YES

<div class="tip">

<div class="title">

Tip

</div>

Using `sysrc` to enable sshd verifies that sshd is enabled.

</div>

Start the SSH daemon: `service sshd start`

The first time the service runs, the jail RSA key pair is generated and
the key fingerprint is displayed.

Add a user account with `adduser`. Follow the prompts, `Enter` will
accept the default value offered. Users that require *root* access must
also be a member of the *wheel* group. Enter *wheel* when prompted to
*invite user into other groups? \[\]:*

    root@jailexamp:~ # adduser
    Username: jailuser
    Full name: Jail User
    Uid (Leave empty for default):
    Login group [jailuser]:
    Login group is jailuser. Invite jailuser into other groups? []: wheel
    Login class [default]:
    Shell (sh csh tcsh git-shell zsh rzsh nologin) [sh]: csh
    Home directory [/home/jailuser]:
    Home directory permissions (Leave empty for default):
    Use password-based authentication? [yes]:
    Use an empty password? (yes/no) [no]:
    Use a random password? (yes/no) [no]:
    Enter password:
    Enter password again:
    Lock out the account after creation? [no]:
    Username   : jailuser
    Password   : *****
    Full Name  : Jail User
    Uid        : 1002
    Class      :
    Groups     : jailuser wheel
    Home       : /home/jailuser
    Home Mode  :
    Shell      : /bin/csh
    Locked     : no
    OK? (yes/no): yes
    adduser: INFO: Successfully added (jailuser) to the user database.
    Add another user? (yes/no): no
    Goodbye!
    root@jailexamp:~

After creating the user, set the jail *root* password to allow users to
use `su` to gain superuser privileges. To set the jail *root* password,
use `passwd`. Nothing is echoed back when using *passwd*

    root@jailexamp:~ # passwd
    Changing local password for root
    New Password:
    Retype New Password:
    root@jailexamp:~ #

Finally, test that the user can successfully `ssh` into the jail from
another system and gain superuser privileges. In the example, a user
named *jailuser* uses `ssh` to access the jail at 192.168.2.3. The host
RSA key fingerprint must be verified the first time a user logs in.

    ssh jailuser@192.168.2.3
    The authenticity of host '192.168.2.3 (192.168.2.3)' can't be established.
    RSA key fingerprint is 6f:93:e5:36:4f:54:ed:4b:9c:c8:c2:71:89:c1:58:f0.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '192.168.2.3' (RSA) to the list of known hosts.
    Password:

<div class="note">

<div class="title">

Note

</div>

Every jail has its own user accounts and service configuration. These
steps must be repeated for each jail that requires SSH access.

</div>

<div class="index">

Additional Storage, Add Storage, Adding Storage

</div>

### Additional Storage

Jails can be given access to an area of storage outside of the jail that
is configured on the FreeNAS<sup>®</sup> system. It is possible to give
a FreeBSD jail access to an area of storage on the FreeNAS<sup>®</sup>
system. This is useful for applications or plugins that store large
amounts of data or if an application in a jail needs access to data
stored on the FreeNAS<sup>®</sup> system. For example, Transmission is a
plugin that stores data using BitTorrent. The FreeNAS<sup>®</sup>
external storage is added using the
[mount\_nullfs(8)](https://www.freebsd.org/cgi/man.cgi?query=mount_nullfs)
mechanism, which links data that resides outside of the jail as a
storage area within a jail.

`--> MOUNT POINTS` shows any added storage and allows adding more
storage.

<div class="note">

<div class="title">

Note

</div>

A jail must have a `STATE` of *down* before adding a new mount point.
Click and `STOP` for a jail to change the jail `STATE` to *down*.

</div>

Storage can be added by clicking `Jails -->` `--> MOUNT POINTS` for the
desired jail. The `MOUNT POINT` section is a list of all of the
currently defined mount points.

Go to `MOUNT POINTS --> ACTIONS --> Add Mount Point` to add storage to a
jail. This opens the screen shown in
`Figure %s <adding_storage_jail_fig>`.

<div id="adding_storage_jail_fig">

![Adding Storage to a Jail](images/jails-mountpoint-add.png)

</div>

*Browse* to the `Source` and `Destination`, where:

-   `Source`: is the directory or dataset on the FreeNAS<sup>®</sup>
    system which will be accessed by the jail. FreeNAS<sup>®</sup>
    creates the directory if it does not exist. This directory must
    reside outside of the pool or dataset being used by the jail. This
    is why it is recommended to create a separate dataset to store
    jails, so the dataset holding the jails is always separate from any
    datasets used for storage on the FreeNAS<sup>®</sup> system.
-   `Destination`: Browse to an existing and **empty** directory within
    the jail to link to the `Source` storage area. It is also possible
    to add `/` and a name to the end of the path and FreeNAS<sup>®</sup>
    automatically creates a new directory. New directories created must
    be **within** the jail directory structure. Example:
    `/mnt/iocage/jails/samplejail/root/new-destination-directory`.

Storage is typically added because the user and group account associated
with an application installed inside of a jail needs to access data
stored on the FreeNAS<sup>®</sup> system. Before selecting the `Source`,
it is important to first ensure that the permissions of the selected
directory or dataset grant permission to the user/group account inside
of the jail. This is not the default, as the users and groups created
inside of a jail are totally separate from the users and groups of the
FreeNAS<sup>®</sup> system.

The workflow for adding storage usually goes like this:

1.  Determine the name of the user and group account used by the
    application. For example, the installation of the transmission
    application automatically creates a user account named
    *transmission* and a group account also named *transmission*. When
    in doubt, check the files `/etc/passwd` (to find the user account)
    and `/etc/group` (to find the group account) inside the jail.
    Typically, the user and group names are similar to the application
    name. Also, the UID and GID are usually the same as the port number
    used by the service.

    A *media* user and group (GID 8675309) are part of the base system.
    Having applications run as this group or user makes it possible to
    share storage between multiple applications in a single jail,
    between multiple jails, or even between the host and jails.

2.  On the FreeNAS<sup>®</sup> system, create a user account and group
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

6.  Use the jail `--> MOUNT POINTS -->` `ACTIONS --> Add Mount Point` to
    select the `Source` of the data and the `Destination` where it will
    be mounted in the jail.

To prevent writes to the storage, click `Read-Only`.

After storage has been added or created, it appears in the
`MOUNT POINTS` for that jail. In the example shown in
`Figure %s <jail_example_storage_fig>`, a dataset named
`pool1/smb-backups` has been chosen as the `Source` as it contains the
files stored on the FreeNAS<sup>®</sup> system. The user entered
`/mnt/iocage/jails/jail1/root/mounted` as the directory to be mounted in
the `Destination` field. To users inside the jail, this data appears in
the `/root/mounted` directory.

<div id="jail_example_storage_fig">

![Example Storage](images/jails-mountpoint-example.png)

</div>

Storage is automatically mounted as it is created.

<div class="note">

<div class="title">

Note

</div>

Mounting a dataset does not automatically mount any child datasets
inside it. Each dataset is a separate filesystem, so child datasets must
each have separate mount points.

</div>

Click `--> Delete` to delete the storage.

<div class="warning">

<div class="title">

Warning

</div>

Remember that added storage is just a pointer to the selected storage
directory on the FreeNAS<sup>®</sup> system. It does **not** copy that
data to the jail. **Files that are deleted from the** `Destination`
**directory in the jail are really deleted from the** `Source`
**directory on the** FreeNAS<sup>®</sup> **system.** However, removing
the jail storage entry only removes the pointer. This leaves the data
intact but not accessible from the jail.

</div>

Jail Software
-------------

A jail is created with no software aside from the core packages
installed as part of the selected version of FreeBSD. To install more
software, start the jail and click .

<div id="jail_shell_example_fig">

![](images/jail-shell-example.png)

</div>

### Installing FreeBSD Packages

The quickest and easiest way to install software inside the jail is to
install a FreeBSD package. FreeBSD packages are precompiled and contain
all the binaries and a list of dependencies required for the software to
run on a FreeBSD system.

A huge amount of software has been ported to FreeBSD. Most of that
software is available as packages. One way to find FreeBSD software is
to use the search bar at [FreshPorts.org](https://www.freshports.org/).

After finding the name of the desired package, use the `pkg install`
command to install it. For example, to install the audiotag package, use
the command `pkg install audiotag`

When prompted, press `y` to complete the installation. Messages will
show the download and installation status.

A successful installation can be confirmed by querying the package
database:

    pkg info -f audiotag
    audiotag-0.19_1
    Name:       audiotag
    Version:    0.19_1
    Installed on:   Fri Nov 21 10:10:34 PST 2014
    Origin:     audio/audiotag
    Architecture:   freebsd:9:x86:64
    Prefix:     /usr/local
    Categories:     multimedia audio
    Licenses:   GPLv2
    Maintainer:     ports@FreeBSD.org
    WWW:        http://github.com/Daenyth/audiotag
    Comment:    Command-line tool for mass tagging/renaming of audio files
    Options:
      DOCS:     on
      FLAC:     on
      ID3:      on
      MP4:      on
      VORBIS:   on
    Annotations:
      repo_type:    binary
      repository:   FreeBSD
    Flat size:  62.8KiB
    Description:   Audiotag is a command-line tool for mass tagging/renaming of audio files
           it supports the vorbis comment, id3 tags, and MP4 tags.
    WWW:       http://github.com/Daenyth/audiotag

To show what was installed by the package:

    pkg info -l audiotag
    audiotag-0.19_1:
    /usr/local/bin/audiotag
    /usr/local/share/doc/audiotag/COPYING
    /usr/local/share/doc/audiotag/ChangeLog
    /usr/local/share/doc/audiotag/README
    /usr/local/share/licenses/audiotag-0.19_1/GPLv2
    /usr/local/share/licenses/audiotag-0.19_1/LICENSE
    /usr/local/share/licenses/audiotag-0.19_1/catalog.mk

In FreeBSD, third-party software is always stored in `/usr/local` to
differentiate it from the software that came with the operating system.
Binaries are almost always located in a subdirectory called `bin` or
`sbin` and configuration files in a subdirectory called `etc`.

### Compiling FreeBSD Ports

Compiling a port is another option. Compiling ports offer these
advantages:

-   Not every port has an available package. This is usually due to
    licensing restrictions or known, unaddressed security
    vulnerabilities.
-   Sometimes the package is out-of-date and a feature is needed that
    only became available in the newer version.
-   Some ports provide compile options that are not available in the
    pre-compiled package. These options are used to add or remove
    features or options.

Compiling a port has these disadvantages:

-   It takes time. Depending upon the size of the application, the
    amount of dependencies, the speed of the CPU, the amount of RAM
    available, and the current load on the FreeNAS<sup>®</sup> system,
    the time needed can range from a few minutes to a few hours or even
    to a few days.

<div class="note">

<div class="title">

Note

</div>

If the port does not provide any compile options, it saves time and
preserves the FreeNAS<sup>®</sup> system resources to use the
`pkg install` command instead.

</div>

The [FreshPorts.org](https://www.freshports.org/) listing shows whether
a port has any configurable compile options.
`Figure %s <config_opts_audiotag_fig>` shows the `Configuration Options`
for *audiotag*, a utility for renaming multiple audio files.

<div id="config_opts_audiotag_fig">

![Audiotag Port Information](images/external/jails-audio-tag.png)

</div>

Packages are built with default options. Ports let the user select
options.

The Ports Collection must be installed in the jail before ports can be
compiled. Inside the jail, use the `portsnap` utility. This command
downloads the ports collection and extracts it to the `/usr/ports/`
directory of the jail:

    portsnap fetch extract

<div class="note">

<div class="title">

Note

</div>

To install additional software at a later date, make sure the ports
collection is updated with `portsnap fetch update`.

</div>

To compile a port, `cd` into a subdirectory of `/usr/ports/`. The entry
for the port at FreshPorts provides the location to `cd` into and the
`make` command to run. This example compiles and installs the audiotag
port:

    cd /usr/ports/audio/audiotag
    make install clean

The first time this command is run, the configure screen shown in
`Figure %s <config_set_audiotag_fig>` is displayed:

<div id="config_set_audiotag_fig">

![Configuration Options for Audiotag
Port](images/console/jails-audio-tag-port.png)

</div>

This port has several configurable options: *DOCS*, *FLAC*, *ID3*,
*MP4*, and *VORBIS*. Selected options are shown with a `*`.

Use the arrow keys to select an option and press `spacebar` to toggle
the value. Press `Enter` when satisfied with the options. The port
begins to compile and install.

<div class="note">

<div class="title">

Note

</div>

After options have been set, the configuration screen is normally not
shown again. Use `make config` to display the screen and change options
before rebuilding the port with `make clean install clean`.

</div>

Many ports depend on other ports. Those other ports also have
configuration screens that are shown before compiling begins. It is a
good idea to watch the compile until it finishes and the command prompt
returns.

Installed ports are registered in the same package database that manages
packages. The `pkg info` can be used to determine which ports were
installed.

### Starting Installed Software

After packages or ports are installed, they must be configured and
started. Configuration files are usually in `/usr/local/etc` or a
subdirectory of it. Many FreeBSD packages contain a sample configuration
file as a reference. Take some time to read the software documentation
to learn which configuration options are available and which
configuration files require editing.

Most FreeBSD packages that contain a startable service include a startup
script which is automatically installed to `/usr/local/etc/rc.d/`. After
the configuration is complete, test starting the service by running the
script with the `onestart` option. For example, with openvpn installed
in the jail, these commands are run to verify that the service started:

    /usr/local/etc/rc.d/openvpn onestart
    Starting openvpn.

    /usr/local/etc/rc.d/openvpn onestatus
    openvpn is running as pid 45560.

    sockstat -4
    USER COMMAND     PID FD  PROTO   LOCAL ADDRESS   FOREIGN ADDRESS
    root openvpn     48386   4   udp4    *:54789     *:*

If it produces an error:

    /usr/local/etc/rc.d/openvpn onestart
    Starting openvpn.
    /usr/local/etc/rc.d/openvpn: WARNING: failed to start openvpn

Run `tail /var/log/messages` to see any error messages if an issue is
found. Most startup failures are related to a misconfiguration in a
configuration file.

After verifying that the service starts and is working as intended, add
a line to `/etc/rc.conf` to start the service automatically when the
jail is started. The line to start a service always ends in
*\_enable="YES"* and typically starts with the name of the software. For
example, this is the entry for the openvpn service:

    openvpn_enable="YES"

When in doubt, the startup script shows the line to put in
`/etc/rc.conf`. This is the description in
`/usr/local/etc/rc.d/openvpn`:

    # This script supports running multiple instances of openvpn.
    # To run additional instances link this script to something like
    # % ln -s openvpn openvpn_foo

    # and define additional openvpn_foo_* variables in one of
    # /etc/rc.conf, /etc/rc.conf.local or /etc/rc.conf.d /openvpn_foo

    #
    # Below NAME should be substituted with the name of this script. By default
    # it is openvpn, so read as openvpn_enable. If you linked the script to
    # openvpn_foo, then read as openvpn_foo_enable etc.
    #
    # The following variables are supported (defaults are shown).
    # You can place them in any of
    # /etc/rc.conf, /etc/rc.conf.local or /etc/rc.conf.d/NAME
    #
    # NAME_enable="NO"
    # set to YES to enable openvpn

The startup script also indicates if any additional parameters are
available:

    # NAME_if=
    # driver(s) to load, set to "tun", "tap" or "tun tap"
    #
    # it is OK to specify the if_ prefix.
    #
    # # optional:
    # NAME_flags=
    # additional command line arguments
    # NAME_configfile="/usr/local/etc/openvpn/NAME.conf"
    # --config file
    # NAME_dir="/usr/local/etc/openvpn"
    # --cd directory
