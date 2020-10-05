<div class="index">

Network Settings

</div>

Network
=======

The Network section of the contains these components for viewing and
configuring network settings on the FreeNAS<sup>®</sup> system:

-   `Global Configuration`: general network settings.
-   `Interfaces`: settings for each network interface and options to
    configure `Bridge <Bridges>`,
    `Link Aggregation <Link Aggregations>`, and `VLAN <VLANs>`
    interfaces.
-   `IPMI`: settings controlling connection to the appliance through the
    hardware side-band management interface if the user interface
    becomes unavailable.
-   `Static Routes`: add static routes.

Each of these is described in more detail in this section.

<div id="webui_interface_warning">

<div class="note">

<div class="title">

Note

</div>

When any network changes are made an animated icon appears in the
upper-right panel to show there are pending network changes. When the
icon is clicked it prompts to review the recent network changes.
Reviewing the network changes goes to `Network --> Interfaces` where the
changes can be permanently applied or discarded.

When `APPLY CHANGES` is clicked the network changes are temporarily
applied for 60 seconds by default. This value can be changed by entering
a positive integer in the seconds field. This feature is nice because
the network settings preview can automatically roll back any
configuration errors that are accidentally saved.

If the network settings applied work as intended, click `KEEP CHANGES`.
Otherwise, the changes can be discarded by clicking `DISCARD CHANGES`.

</div>

</div>

Global Configuration
--------------------

`Network --> Global Configuration`, shown in
`Figure %s <global_net_config_fig>`, is for general network settings
that are not unique to any particular network interface.

> Global Network Configuration

`Table %s <global_net_config_tab>` summarizes the settings on the Global
Configuration tab. `Hostname` and `Domain` fields are pre-filled as
shown in `Figure %s <global_net_config_fig>`, but can be changed to meet
requirements of the local network.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="global_net_config_tab">

| Setting                | Value      | Description                                                                                                                                                                                                                 |
|------------------------|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hostname               | string     | System host name. Upper and lower case alphanumeric, `.`, and `-` characters are allowed. The `Hostname` and `Domain` are also displayed under the iXsystems logo at the top left of the main screen.                       |
| Domain                 | string     | System domain name. The `Hostname` and `Domain` are also displayed under the iXsystems logo at the top left of the main screen.                                                                                             |
| Additional Domains     | string     | Additional space-delimited domains to search. Adding search domains can cause slow DNS lookups.                                                                                                                             |
| IPv4 Default Gateway   | IP address | Typically not set. See `this note about Gateways <Gateway Note>`. If set, used instead of the default gateway provided by DHCP.                                                                                             |
| IPv6 Default Gateway   | IP address | Typically not set. See `this note about Gateways <Gateway Note>`.                                                                                                                                                           |
| Nameserver 1           | IP address | Primary DNS server.                                                                                                                                                                                                         |
| Nameserver 2           | IP address | Secondary DNS server.                                                                                                                                                                                                       |
| Nameserver 3           | IP address | Tertiary DNS server.                                                                                                                                                                                                        |
| HTTP Proxy             | string     | Enter the proxy information for the network in the format *http://my.proxy.server:3128* or *http://user:password@my.proxy.server:3128*.                                                                                     |
| Enable netwait feature | checkbox   | If enabled, network services do not start at boot until the interface is able to ping the addresses listed in the `Netwait IP list`.                                                                                        |
| Netwait IP list        | string     | Only appears when `Enable netwait feature` is set. Enter a space-delimited list of IP addresses to ping(8). Each address is tried until one is successful or the list is exhausted. Leave empty to use the default gateway. |
| Host name database     | string     | Used to add one entry per line which will be appended to `/etc/hosts`. Use the format *IP\_address space hostname* where multiple hostnames can be used if separated by a space.                                            |

Global Configuration Settings

</div>

When using Active Directory, set the IP address of the realm DNS server
in the `Nameserver 1` field.

If the network does not have a DNS server, or NFS, SSH, or FTP users are
receiving "reverse DNS" or timeout errors, add an entry for the IP
address of the FreeNAS<sup>®</sup> system in the `Host name database`
field.

<div id="Gateway Note">

<div class="note">

<div class="title">

Note

</div>

In many cases, a FreeNAS<sup>®</sup> configuration does not include
default gateway information as a way to make it more difficult for a
remote attacker to communicate with the server. While this is a
reasonable precaution, such a configuration does **not** restrict
inbound traffic from sources within the local network. However, omitting
a default gateway will prevent the FreeNAS<sup>®</sup> system from
communicating with DNS servers, time servers, and mail servers that are
located outside of the local network. In this case, it is recommended to
add `Static Routes` to be able to reach external DNS, NTP, and mail
servers which are configured with static IP addresses. When a gateway to
the Internet is added, make sure the FreeNAS<sup>®</sup> system is
protected by a properly configured firewall.

</div>

</div>

Interfaces
----------

`Network --> Interfaces` shows all physical Network Interface
Controllers (NICs) connected to the FreeNAS<sup>®</sup> system. These
can be edited or new *bridge*, *link aggregation*, or *Virtual LAN
(VLAN)* interfaces can be created and added to the interface list.

Be careful when configuring the network interface that controls the
FreeNAS<sup>®</sup> or
`web connectivity can be lost <webui_interface_warning>`.

To configure a new network interface, go to `Network --> Interfaces` and
click .

<div id="add_net_interface_fig">

![Adding a Network Interface][]

</div>

  [Adding a Network Interface]: images/network-interfaces-add.png

Each `Type` of configurable network interface changes the available
options. `Table %s <net_interface_config_tab>` shows which settings are
available with each interface type.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.12linewidth-2tabcolsep}
&gt;{RaggedRight}p{dimexpr 0.55linewidth-2tabcolsep}\|

</div>

<div id="net_interface_config_tab">

| Setting                     | Value                      | Type             | Description                                                                                                                                                                                                                                                                         |
|-----------------------------|----------------------------|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Type                        | drop-down menu             | All              | Choose the type of interface. *Bridge* creates a logical link between multiple networks. *Link Aggregation* combines multiple network connections into a single interface. A virtual LAN (*VLAN*) partitions and isolates a segment of the connection.                              |
| Name                        | string                     | All              | Enter a name to use for the the interface. Use the format laggX, vlanX, or bridgeX where X is a number representing a non-parent interface.                                                                                                                                         |
| Description                 | string                     | All              | Notes or explanatory text about this interface.                                                                                                                                                                                                                                     |
| DHCP                        | checkbox                   | All              | Enable [DHCP][] to auto-assign an IPv4 address to this interface. Leave unset to create a static IPv4 or IPv6 configuration. Only one interface can be configured for DHCP.                                                                                                         |
| Autoconfigure IPv6          | drop-down menu             | All              | Automatically configure the IPv6 address with [rtsol(8)][]. Only one interface can be configured this way.                                                                                                                                                                          |
| Disable Hardware Offloading | checkbox                   | All              | Turn off hardware offloading for network traffic processing. WARNING: disabling hardware offloading can reduce network performance and is only recommended when the interface is managing `jails <Jails>`, `plugins <Plugins>`, or `virtual machines (VMs) <VMs>`.                  |
| Bridge Members              | drop-down menu             | Bridge           | Network interfaces to include in the bridge.                                                                                                                                                                                                                                        |
| Lagg Protocol               | drop-down menu             | Link Aggregation | Select the `Protocol Type <Link Aggregations>`. *LACP* is the recommended protocol if the network switch is capable of active LACP. *Failover* is the default protocol choice and should only be used if the network switch does not support active LACP.                           |
| Lagg Interfaces             | drop-down menu             | Link Aggregation | Select the interfaces to use in the aggregation. **Warning:** Lagg creation fails when the selected interfaces have manually assigned IP addresses.                                                                                                                                 |
| Parent Interface            | drop-down menu             | VLAN             | Select the VLAN Parent Interface. Usually an Ethernet card connected to a switch port configured for the VLAN. A *bridge* cannot be selected as a parent interface. New `link aggregations` are not available until the system is restarted.                                        |
| Vlan Tag                    | integer                    | VLAN             | The numeric tag provided by the switched network.                                                                                                                                                                                                                                   |
| Priority Code Point         | drop-down menu             | VLAN             | Select the [Class of Service][]. The available 802.1p Class of Service ranges from *Best effort (default)* to *Network control (highest)*.                                                                                                                                          |
| MTU                         | integer                    | All              | Maximum Transmission Unit, the largest protocol data unit that can be communicated. The largest workable MTU size varies with network interfaces and equipment. *1500* and *9000* are standard Ethernet MTU sizes. Leaving blank restores the field to the default value of *1500*. |
| Options                     | string                     | All              | Additional parameters from [ifconfig(8)][]. Separate multiple parameters with a space. For example: *mtu 9000* increases the MTU for interfaces which support jumbo frames. See `this note <LAGG MTU>` about MTU and lagg interfaces.                                               |
| IP Address                  | integer and drop-down menu | All              | Static IPv4 or IPv6 address and subnet mask. Example: *10.0.0.3* and */24*. Click `ADD` to add another IP address. Clicking `DELETE` removes that `IP Address`.                                                                                                                     |

Interface Configuration Options

</div>

  [DHCP]: https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol
  [rtsol(8)]: https://www.freebsd.org/cgi/man.cgi?query=rtsol
  [Class of Service]: https://en.wikipedia.org/wiki/Class_of_service
  [ifconfig(8)]: https://www.freebsd.org/cgi/man.cgi?query=ifconfig

Multiple interfaces **cannot** be members of the same subnet. See
[Multiple network interfaces on a single subnet][] for more information.
Check the subnet mask if an error is shown when setting the IP addresses
on multiple interfaces.

  [Multiple network interfaces on a single subnet]: https://forums.freenas.org/index.php?threads/multiple-network-interfaces-on-a-single-subnet.20204/

Saving a new interface adds an entry to the list in
`Network --> Interfaces`.

Expanding an entry in the list shows further details for that interface.

Editing an interface allows changing all the
`interface options <net_interface_config_tab>` except the interface
`Type` and `Name`.

<div class="index">

Network Bridge

</div>

### Network Bridges

A network bridge allows multiple network interfaces to function as a
single interface.

To create a bridge, go to `Network --> Interfaces` and click . Choose
*Bridge* as the `Type` and continue to configure the interface. See the
`Interface Configuration Options table <net_interface_config_tab>` for
descriptions of each option.

Enter `bridge{X}` for the `Name`, where *X* is a unique interface
number. Open the `Bridge Members` drop-down menu and select each
interface that will be part of the bridge. Click `SAVE` to add the new
bridge to `Network --> Interfaces` and show options to confirm or revert
the new network settings.

<div class="index">

Link Aggregation, LAGG, LACP, EtherChannel

</div>

### Link Aggregations

FreeNAS<sup>®</sup> uses the FreeBSD [lagg(4)][] interface to provide
link aggregation and link failover support. A lagg interface allows
combining multiple network interfaces into a single virtual interface.
This provides fault-tolerance and high-speed multi-link throughput. The
aggregation protocols supported by lagg both determines the ports to use
for outgoing traffic and if a specific port accepts incoming traffic.
The link state of the lagg interface is used to validate whether the
port is active.

  [lagg(4)]: https://www.freebsd.org/cgi/man.cgi?query=lagg

Aggregation works best on switches supporting LACP, which distributes
traffic bi-directionally while responding to failure of individual
links. FreeNAS<sup>®</sup> also supports active/passive failover between
pairs of links. The LACP and load-balance modes select the output
interface using a hash that includes the Ethernet source and destination
address, VLAN tag (if available), IP source and destination address, and
flow label (IPv6 only). The benefit can only be observed when multiple
clients are transferring files *from* the NAS. The flow entering *into*
the NAS depends on the Ethernet switch load-balance algorithm.

The lagg driver currently supports several aggregation protocols,
although only *Failover* is recommended on network switches that do not
support *LACP*:

**Failover:** the default protocol. Sends traffic only through the
active port. If the master port becomes unavailable, the next active
port is used. The first interface added is the master port. Any
interfaces added later are used as failover devices. By default,
received traffic is only accepted when received through the active port.
This constraint can be relaxed, which is useful for certain bridged
network setups, by going to `System --> Tunables` and clicking to add a
tunable. Set the `Variable` to *net.link.lagg.failover\_rx\_all*, the
`Value` to a non-zero integer, and the `Type` to *Sysctl*.

**LACP:** supports the IEEE 802.3ad Link Aggregation Control Protocol
(LACP) and the Marker Protocol. LACP negotiates a set of aggregable
links with the peer into one or more link aggregated groups (LAGs). Each
LAG is composed of ports of the same speed, set to full-duplex
operation. Traffic is balanced across the ports in the LAG with the
greatest total speed. In most situations there will be a single LAG
which contains all ports. In the event of changes in physical
connectivity, link aggregation quickly converges to a new configuration.
LACP must be configured on the network switch and LACP does not support
mixing interfaces of different speeds. Only interfaces that use the same
driver, like two *igb* ports, are recommended for LACP. Using LACP for
iSCSI is not recommended as iSCSI has built-in multipath features which
are more efficient.

<div class="note">

<div class="title">

Note

</div>

When using *LACP*, verify the switch is configured for active LACP.
Passive LACP is not supported.

</div>

**Load Balance:** balances outgoing traffic across the active ports
based on hashed protocol header information and accepts incoming traffic
from any active port. This is a static setup and does not negotiate
aggregation with the peer or exchange frames to monitor the link. The
hash includes the Ethernet source and destination address, VLAN tag (if
available), and IP source and destination address. Requires a switch
which supports IEEE 802.3ad static link aggregation.

**Round Robin:** distributes outgoing traffic using a round-robin
scheduler through all active ports and accepts incoming traffic from any
active port. This mode can cause unordered packet arrival at the client.
This has a side effect of limiting throughput as reordering packets can
be CPU intensive on the client. Requires a switch which supports IEEE
802.3ad static link aggregation.

**None:** this protocol disables any traffic without disabling the lagg
interface itself.

#### LACP, MPIO, NFS, and ESXi

LACP bonds Ethernet connections to improve bandwidth. For example, four
physical interfaces can be used to create one mega interface. However,
it cannot increase the bandwidth for a single conversation. It is
designed to increase bandwidth when multiple clients are simultaneously
accessing the same system. It also assumes that quality Ethernet
hardware is used and it will not make much difference when using
inferior Ethernet chipsets such as a Realtek.

LACP reads the sender and receiver IP addresses and, if they are deemed
to belong to the same TCP connection, always sends the packet over the
same interface to ensure that TCP does not need to reorder packets. This
makes LACP ideal for load balancing many simultaneous TCP connections,
but does nothing for increasing the speed over one TCP connection.

MPIO operates at the iSCSI protocol level. For example, if four IP
addresses are created and there are four simultaneous TCP connections,
MPIO will send the data over all available links. When configuring MPIO,
make sure that the IP addresses on the interfaces are configured to be
on separate subnets with non-overlapping netmasks, or configure static
routes to do point-to-point communication. Otherwise, all packets will
pass through one interface.

LACP and other forms of link aggregation generally do not work well with
virtualization solutions. In a virtualized environment, consider the use
of iSCSI MPIO through the creation of an iSCSI Portal with at least two
network cards on different networks. This allows an iSCSI initiator to
recognize multiple links to a target, using them for increased bandwidth
or redundancy. This [how-to][] contains instructions for configuring
MPIO on ESXi.

  [how-to]: https://fojta.wordpress.com/2010/04/13/iscsi-and-esxi-multipathing-and-jumbo-frames/

NFS does not understand MPIO. Therefore, one fast interface is needed,
since creating an iSCSI portal will not improve bandwidth when using
NFS. LACP does not work well to increase the bandwidth for
point-to-point NFS (one server and one client). LACP is a good solution
for link redundancy or for one server and many clients.

#### Creating a Link Aggregation

**Before** creating a link aggregation, see this
`warning <webui_interface_warning>` about changing the interface that
the uses.

To create a link aggregation, go to `Network --> Interfaces` and click .
Choose *Link Aggregation* as the `Type` and continue to fill in the
remaining configuration options. See the
`Interface Configuration Options table <net_interface_config_tab>` for
descriptions of each option.

Enter `lagg{X}` for the `Name`, where *X* is a unique interface number.
There a several `Lagg Protocol` options, but *LACP* is preferred. Choose
*Failover* when the network switch does not support LACP. Open the
`Lagg Interfaces` drop-down menu to associate NICs with the lagg device.
Click `SAVE` to add the new aggregation to `Network --> Interfaces` and
show options to confirm or revert the new network settings.

<div class="note">

<div class="title">

Note

</div>

If interfaces are installed but do not appear in the `Lagg Interfaces`
list, check for a [FreeBSD driver][] for the interface.

</div>

  [FreeBSD driver]: https://www.freebsd.org/releases/11.2R/hardware.html#ethernet

#### Link Aggregation Options

Options are set at the lagg level from `Network --> Interfaces`. Find
the lagg interface, expand the entry with , and click . Scroll to the
`Options` field. Changes are typically made at the lagg level as each
interface member inherits settings from the lagg. Configuring at the
interface level requires repeating the configuration for each interface
within the lagg. Setting options at the individual interface level is
done by editing the parent interface in the same way as the lagg
interface.

<div id="LAGG MTU">

If the MTU settings on the lagg member interfaces are not identical, the
smallest value is used for the MTU of the entire lagg.

</div>

<div class="note">

<div class="title">

Note

</div>

A reboot is required after changing the MTU to create a jumbo frame
lagg.

</div>

Link aggregation load balancing can be tested with:

    systat -ifstat

More information about this command can be found at [systat(1)][].

  [systat(1)]: https://www.freebsd.org/cgi/man.cgi?query=systat

<div class="index">

VLAN, Trunking, 802.1Q

</div>

### VLANs

FreeNAS<sup>®</sup> uses [vlan(4)][] to demultiplex frames with IEEE
802.1q tags. This allows nodes on different VLANs to communicate through
a layer 3 switch or router. A vlan interface must be assigned a parent
interface and a numeric VLAN tag. A single parent can be assigned to
multiple vlan interfaces provided they have different tags.

  [vlan(4)]: https://www.freebsd.org/cgi/man.cgi?query=vlan

<div class="note">

<div class="title">

Note

</div>

VLAN tagging is the only 802.1q feature that is implemented.
Additionally, not all Ethernet interfaces support full VLAN processing.
See the HARDWARE section of [vlan(4)][] for details.

</div>

  [vlan(4)]: https://www.freebsd.org/cgi/man.cgi?query=vlan

To add a new VLAN interface, go to `Network --> Interfaces` and click .
Choose *VLAN* as the `Type` and continue filling in the remaining
fields. See the
`Interface Configuration Options table <net_interface_config_tab>` for
descriptions of each option.

The parent interface of a VLAN must be up, but it can either have an IP
address or be unconfigured, depending upon the requirements of the VLAN
configuration. This makes it difficult for the to do the right thing
without trampling the configuration. To remedy this, add the VLAN
interface, then select `Network --> Interfaces`, and click and `Edit`
for the parent interface. Enter `up` in the `Options` field and click
`SAVE`. This brings up the parent interface. If an IP address is
required, configure it using the rest of the options in the edit screen.

<div class="warning">

<div class="title">

Warning

</div>

Creating a VLAN causes an interruption to network connectivity. The
requires confirming the new network configuration before it is
permanently applied to the FreeNAS<sup>®</sup> system.

</div>

IPMI
----

Beginning with version 9.2.1, FreeNAS<sup>®</sup> provides a graphical
screen for configuring an IPMI interface. This screen will only appear
if the system hardware includes a Baseboard Management Controller (BMC).

IPMI provides side-band management if the graphical administrative
interface becomes unresponsive. This allows for a few vital functions,
such as checking the log, accessing the BIOS setup, and powering on the
system without requiring physical access to the system. IPMI is also
used to give another person remote access to the system to assist with a
configuration or troubleshooting issue. Before configuring IPMI, ensure
that the management interface is physically connected to the network.
The IPMI device may share the primary Ethernet interface, or it may be a
dedicated separate IPMI interface.

<div class="warning">

<div class="title">

Warning

</div>

It is recommended to first ensure that the IPMI has been patched against
the Remote Management Vulnerability before enabling IPMI. This
[article][] provides more information about the vulnerability and how to
fix it.

</div>

  [article]: https://www.ixsystems.com/blog/how-to-fix-the-ipmi-remote-management-vulnerability/

<div class="note">

<div class="title">

Note

</div>

Some IPMI implementations require updates to work with newer versions of
Java. See [PSA: Java 8 Update 131 breaks ASRock's IPMI Virtual
console][] for more information.

</div>

  [PSA: Java 8 Update 131 breaks ASRock's IPMI Virtual console]: https://forums.freenas.org/index.php?threads/psa-java-8-update-131-breaks-asrocks-ipmi-virtual-console.53911/

IPMI is configured from `Network --> IPMI`. The IPMI configuration
screen, shown in `Figure %s <ipmi_config_fig>`, provides a shortcut to
the most basic IPMI configuration. Those already familiar with IPMI
management tools can use them instead. `Table %s <ipmi_options_tab>`
summarizes the options available when configuring IPMI with the
FreeNAS<sup>®</sup> .

> IPMI Configuration

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="ipmi_options_tab">

| Setting              | Value          | Description                                                                                                                                                                                  |
|----------------------|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Channel              | drop-down menu | Select the [communications channel][] to use. Available channel numbers vary by hardware.                                                                                                    |
| Password             | string         | Enter the password used to connect to the IPMI interface from a web browser. The maximum length accepted in the UI is 20 characters, but different hardware might require shorter passwords. |
| DHCP                 | checkbox       | If left unset, `IPv4 Address`, `IPv4 Netmask`, and `Ipv4 Default Gateway` must be set.                                                                                                       |
| IPv4 Address         | string         | IP address used to connect to the IPMI .                                                                                                                                                     |
| IPv4 Netmask         | drop-down menu | Subnet mask associated with the IP address.                                                                                                                                                  |
| IPv4 Default Gateway | string         | Default gateway associated with the IP address.                                                                                                                                              |
| VLAN ID              | string         | Enter the VLAN identifier if the IPMI out-of-band management interface is not on the same VLAN as management networking.                                                                     |
| IDENTIFY LIGHT       | button         | Show a dialog to activate an IPMI identify light on the compatible connected hardware.                                                                                                       |

IPMI Options

</div>

  [communications channel]: https://www.thomas-krenn.com/en/wiki/IPMI_Basics#Channel_Model

After configuration, the IPMI interface is accessed using a web browser
and the IP address specified in the configuration. The management
interface prompts for a username and the configured password. Refer to
the IPMI device documentation to determine the default administrative
username.

After logging in to the management interface, the default administrative
username can be changed, and additional users created. The appearance of
the IPMI utility and the functions that are available vary depending on
the hardware.

Network Summary
---------------

`Network --> Network Summary` shows a quick summary of the addressing
information of every configured interface. For each interface name, the
configured IPv4 and IPv6 addresses, default routes, and DNS namerservers
are displayed.

<div class="index">

Route, Static Route

</div>

Static Routes
-------------

No static routes are defined on a default FreeNAS<sup>®</sup> system. If
a static route is required to reach portions of the network, add the
route by going to `Network --> Static Routes`, and clicking . This is
shown in `Figure %s <add_static_route_fig>`.

<div id="add_static_route_fig">

![Adding a Static Route][]

</div>

  [Adding a Static Route]: images/network-static-routes-add.png

The available options are summarized in
`Table %s <static_route_opts_tab>`.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="static_route_opts_tab">

| Setting     | Value   | Description                                            |
|-------------|---------|--------------------------------------------------------|
| Destination | integer | Use the format *A.B.C.D/E* where *E* is the CIDR mask. |
| Gateway     | integer | Enter the IP address of the gateway.                   |
| Description | string  | Optional. Add any notes about the route.               |

Static Route Options

</div>

Added static routes are shown in `Network --> Static Routes`. Click on a
route entry to access the `Edit` and `Delete` buttons.
