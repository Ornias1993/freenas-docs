<div class="index">

VMware Recommendations

</div>

VMware Recommendations
======================

This section offers FreeNAS<sup>®</sup> configuration recommendations
and troubleshooting tips when using FreeNAS<sup>®</sup> with a
[VMware][] hypervisor.

  [VMware]: https://www.vmware.com/

FreeNAS<sup>®</sup> as a VMware Guest
-------------------------------------

This section has recommendations for configuring FreeNAS<sup>®</sup>
when it is installed as a Virtual Machine (VM) in VMware.

To create a new FreeNAS<sup>®</sup> Virtual Machine in VMware, see the
`VMware ESXi` section of this guide.

Configure and use the [vmx(4)][] drivers for the FreeNAS<sup>®</sup>
system.

  [vmx(4)]: https://www.freebsd.org/cgi/man.cgi?query=vmx

Network connection errors for plugins or jails inside the
FreeNAS<sup>®</sup> VM can be caused by a misconfigured [virtual
switch][] or [VMware port group][]. Make sure MAC spoofing and
promiscuous mode are enabled on the switch first, and then the port
group the VM is using.

  [virtual switch]: https://pubs.vmware.com/vsphere-51/index.jsp?topic=%2Fcom.vmware.wssdk.pg.doc%2FPG_Networking.11.4.html
  [VMware port group]: https://pubs.vmware.com/vsphere-4-esx-vcenter/index.jsp?topic=/com.vmware.vsphere.server_configclassic.doc_40/esx_server_config/networking/c_port_groups.html

Hosting VMware Storage with FreeNAS<sup>®</sup>
-----------------------------------------------

This section has recommendations for configuring FreeNAS<sup>®</sup>
when the system is being used as a VMware datastore.

Make sure guest VMs have the latest version of `vmware-tools` installed.
VMware provides instructions to [install VMware Tools][] on different
guest operating systems.

  [install VMware Tools]: https://www.vmware.com/support/ws5/doc/new_guest_tools_ws.html

Increase the VM disk timeouts to better survive long disk operations.
Set the timeout to a minimum of *300 seconds*. See the guest operating
system documentation for setting disk timeouts. VMware provides
instructions for setting disk timeouts on some specific guest operating
systems:

-   Windows guest operating system:
    <https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.storage.doc/GUID-EA1E1AAD-7130-457F-8894-70A63BD0623A.html>
-   Linux guests running kernel version *2.6*:
    <https://kb.vmware.com/s/article/1009465>

When FreeNAS<sup>®</sup> is used as a VMware datastore,
`coordinated ZFS and VMware snapshots <VMware-Snapshots>` can be used.

<div class="index">

VAAI for iSCSI

</div>

VAAI for iSCSI
--------------

VMware's vStorage APIs for Array Integration, or *VAAI*, allows storage
tasks such as large data moves to be offloaded from the virtualization
hardware to the storage array. These operations are performed locally on
the NAS without transferring bulk data over the network.

VAAI for iSCSI supports these operations:

-   *Atomic Test and Set* (*ATS*) allows multiple initiators to
    synchronize LUN access in a fine-grained manner rather than locking
    the whole LUN and preventing other hosts from accessing the same LUN
    simultaneously.
-   *Clone Blocks* (*XCOPY*) copies disk blocks on the NAS. Copies occur
    locally rather than over the network. This operation is similar to
    [Microsoft ODX][].
-   *LUN Reporting* allows a hypervisor to query the NAS to determine
    whether a LUN is using thin provisioning.
-   *Stun* pauses virtual machines when a pool runs out of space. The
    space issue can then be fixed and the virtual machines can continue
    rather than reporting write errors.
-   *Threshold Warning* the system reports a warning when a configurable
    capacity is reached. In FreeNAS<sup>®</sup>, this threshold is
    configured at the pool level when using zvols (see
    `Table %s <iscsi_targ_global_config_tab>`) or at the extent level
    (see `Table %s <iscsi_extent_conf_tab>`) for both file and device
    based extents. Typically, the warning is set at the pool level,
    unless file extents are used, in which case it must be set at the
    extent level.
-   *Unmap* informs FreeNAS<sup>®</sup> that the space occupied by
    deleted files should be freed. Without unmap, the NAS is unaware of
    freed space created when the initiator deletes files. For this
    feature to work, the initiator must support the unmap command.
-   *Zero Blocks* or *Write Same* zeros out disk regions. When
    allocating virtual machines with thick provisioning, the zero write
    is done locally, rather than over the network. This makes virtual
    machine creation and any other zeroing of disk regions much quicker.

  [Microsoft ODX]: https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831628(v=ws.11)
