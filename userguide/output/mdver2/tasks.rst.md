<div class="index">

Tasks

</div>

Tasks
=====

The Tasks section of the is used to configure repetitive tasks:

-   `Cron Jobs` schedules a command or script to automatically execute
    at a specified time
-   `Init/Shutdown Scripts` configures a command or script to
    automatically execute during system startup or shutdown
-   `Rsync Tasks` schedules data synchronization to another system
-   `S.M.A.R.T. Tests` schedules disk tests
-   `Periodic Snapshot Tasks` schedules automatic creation of filesystem
    snapshots
-   `Replication Tasks` automate the replication of snapshots to a
    remote system
-   `Resilver Priority` controls the priority of resilvers
-   `Scrub Tasks` schedules scrubs as part of ongoing disk maintenance
-   `Cloud Sync Tasks` schedules data synchronization to cloud providers

Each of these tasks is described in more detail in this section.

<div class="note">

<div class="title">

Note

</div>

By default, `Scrub Tasks` are run once a month by an
automatically-created task. `S.M.A.R.T. Tests` and
`Periodic Snapshot Tasks` must be set up manually.

</div>

<div class="index">

Cron Jobs

</div>

Cron Jobs
---------

[cron(8)][] is a daemon that runs a command or script on a regular
schedule as a specified user.

  [cron(8)]: https://www.freebsd.org/cgi/man.cgi?query=cron

Go to `Tasks --> Cron Jobs` and click to create a cron job.

<div id="tasks_create_cron_job_fig">

![Cron Job Settings][]

</div>

  [Cron Job Settings]: images/tasks-cron-jobs-add.png

`Table %s <tasks_cron_job_opts_tab>` lists the configurable options for
a cron job.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="tasks_cron_job_opts_tab">

| Setting              | Value          | Description                                                                                                                                                                                                                                    |
|----------------------|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description          | string         | Enter a description of the cron job.                                                                                                                                                                                                           |
| Command              | drop-down menu | Enter the **full path** to the command or script to be run. If it is a script, testing it at the command line first is recommended.                                                                                                            |
| Run As User          | string         | Select a user account to run the command. The user must have permissions allowing them to run the command or script. Output from executing a cron task is emailed to this user if `Email` has been configured for that `user account <Users>`. |
| Schedule             | drop-down menu | Select a schedule preset or choose *Custom* to open the advanced scheduler. Note that an in-progress cron task postpones any later scheduled instance of the same task until the running task is complete.                                     |
| Hide Standard Output | checkbox       | Hide standard output (stdout) from the command. When unset, any standard output is mailed to the user account cron used to run the command.                                                                                                    |
| Hide Standard Error  | checkbox       | Hide error output (stderr) from the command. When unset, any error output is mailed to the user account cron used to run the command.                                                                                                          |
| Enable               | checkbox       | Set to allow this scheduled cron task to activate. Unsetting this option disables the cron task without deleting it.                                                                                                                           |

Cron Job Options

</div>

Cron jobs are shown in `Tasks --> Cron Jobs`. This table displays the
user, command, description, schedule, and whether the job is enabled.
This table is adjustable by setting the different column checkboxes
above it. Set `Toggle` to display all options in the table. Click for to
show the `Run Now`, `Edit`, and `Delete` options.

<div class="note">

<div class="title">

Note

</div>

`%` symbols are automatically escaped and do not need to be prefixed
with backslashes. For example, use `date '+%Y-%m-%d'` in a cron job to
generate a filename based on the date.

</div>

Init/Shutdown Scripts
---------------------

FreeNAS<sup>®</sup> provides the ability to schedule commands or scripts
to run at system startup or shutdown.

Go to `Tasks --> Init/Shutdown Scripts` and click .

<div id="tasks_init_script_fig">

![Add an Init/Shutdown Command or Script][]

</div>

  [Add an Init/Shutdown Command or Script]: images/tasks-init-shutdown-scripts-add.png

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="tasks_init_opt_tab">

<table>
<caption>Init/Shutdown Command or Script Options</caption>
<colgroup>
<col style="width: 11%" />
<col style="width: 13%" />
<col style="width: 75%" />
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
<td>Type</td>
<td>drop-down menu</td>
<td>Select <em>Command</em> for an executable or <em>Script</em> for an executable script.</td>
</tr>
<tr class="even">
<td>Command or Script</td>
<td>string</td>
<td>If <em>Command</em> is selected, enter the command with any options. When <em>Script</em> is selected, click to select the script from an existing pool.</td>
</tr>
<tr class="odd">
<td>When</td>
<td>drop-down menu</td>
<td><p>Select when the <em>Command</em> or <em>Script</em> runs:</p>
<ul>
<li><em>Pre Init</em>: early in the boot process, after mounting filesystems and starting networking</li>
<li><em>Post Init</em>: at the end of the boot process, before FreeNAS<sup>®</sup> services start</li>
<li><em>Shutdown</em>: during the system power off process.</li>
</ul></td>
</tr>
<tr class="even">
<td>Enabled</td>
<td>checkbox</td>
<td>Enable this task. Unset to disable the task without deleting it.</td>
</tr>
<tr class="odd">
<td>Timeout</td>
<td>integer</td>
<td>Automatically stop the script or command after the specified number of seconds.</td>
</tr>
</tbody>
</table>

Init/Shutdown Command or Script Options

</div>

Scheduled commands must be in the default path. The full path to the
command can also be included in the entry. The path can be tested with
`which {commandname}` in the `Shell`. When available, the path to the
command is shown:

    [root@freenas ~]# which ls
    /bin/ls

When scheduling a script, test the script first to verify it is
executable and achieves the desired results.

<div class="note">

<div class="title">

Note

</div>

Init/shutdown scripts are run with `sh`.

</div>

Init/Shutdown tasks are shown in `Tasks --> Init/Shutdown Scripts`.
Click for a task to `Edit` or `Delete` that task.

<div class="index">

Rsync Tasks

</div>

Rsync Tasks
-----------

[Rsync][] is a utility that copies specified data from one system to
another over a network. Once the initial data is copied, rsync reduces
the amount of data sent over the network by sending only the differences
between the source and destination files. Rsync is used for backups,
mirroring data on multiple systems, or for copying files between
systems.

  [Rsync]: https://www.samba.org/ftp/rsync/rsync.html

Rsync is most effective when only a relatively small amount of the data
has changed. There are also [some limitations when using rsync with
Windows files][]. For large amounts of data, data that has many changes
from the previous copy, or Windows files, `Replication Tasks` are often
the faster and better solution.

  [some limitations when using rsync with Windows files]: https://forums.freenas.org/index.php?threads/impaired-rsync-permissions-support-for-windows-datasets.43973/

Rsync is single-threaded and gains little from multiple processor cores.
To see whether rsync is currently running, use `pgrep rsync` from the
`Shell`.

Both ends of an rsync connection must be configured:

-   **the rsync server:** this system pulls (receives) the data. This
    system is referred to as *PULL* in the configuration examples.
-   **the rsync client:** this system pushes (sends) the data. This
    system is referred to as *PUSH* in the configuration examples.

FreeNAS<sup>®</sup> can be configured as either an *rsync client* or an
*rsync server*. The opposite end of the connection can be another
FreeNAS<sup>®</sup> system or any other system running rsync. In
FreeNAS<sup>®</sup> terminology, an *rsync task* defines which data is
synchronized between the two systems. To synchronize data between two
FreeNAS<sup>®</sup> systems, create the *rsync task* on the *rsync
client*.

FreeNAS<sup>®</sup> supports two modes of rsync operation:

-   **Module:** exports a directory tree, and the configured settings of
    the tree as a symbolic name over an unencrypted connection. This
    mode requires that at least one module be defined on the rsync
    server. It can be defined in the FreeNAS<sup>®</sup> under
    `Services --> Rsync Configure --> Rsync Module`. In other operating
    systems, the module is defined in [rsyncd.conf(5)][].
-   **SSH:** synchronizes over an encrypted connection. Requires the
    configuration of SSH user and host public keys.

  [rsyncd.conf(5)]: https://www.samba.org/ftp/rsync/rsyncd.conf.html

This section summarizes the options when creating an rsync task. It then
provides a configuration example between two FreeNAS<sup>®</sup> systems
for each mode of rsync operation.

<div class="note">

<div class="title">

Note

</div>

If there is a firewall between the two systems or if the other system
has a built-in firewall, make sure that TCP port 873 is allowed.

</div>

`Figure %s <tasks_add_rsync_fig>` shows the screen that appears after
navigating to `Tasks --> Rsync Tasks` and clicking .
`Table %s <tasks_rsync_opts_tab>` summarizes the configuration options
available when creating an rsync task.

<div id="tasks_add_rsync_fig">

![Adding an Rsync Task][]

</div>

  [Adding an Rsync Task]: images/tasks-rsync-tasks-add.png

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="tasks_rsync_opts_tab">

| Setting                      | Value          | Description                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Path                         | browse button  | `Browse` to the path to be copied. FreeNAS<sup>®</sup> verifies that the remote path exists. `FreeBSD path length limits <Path and Name Lengths>` apply on the FreeNAS<sup>®</sup> system. Other operating systems can have different limits which might affect how they can be used as sources or destinations. |
| User                         | drop-down menu | Select the user to run the rsync task. The user selected must have permissions to write to the specified directory on the remote host.                                                                                                                                                                           |
| Remote Host                  | string         | Enter the IP address or hostname of the remote system that will store the copy. Use the format *username@remote\_host* if the username differs on the remote host.                                                                                                                                               |
| Remote SSH Port              | integer        | Only available in *SSH* mode. Allows specifying an SSH port other than the default of *22*.                                                                                                                                                                                                                      |
| Rsync mode                   | drop-down menu | The choices are *Module* mode or *SSH* mode.                                                                                                                                                                                                                                                                     |
| Remote Module Name           | string         | At least one module must be defined in [rsyncd.conf(5)][] of the rsync server or in the `Rsync Modules` of another system.                                                                                                                                                                                       |
| Remote Path                  | string         | Only appears when using *SSH* mode. Enter the **existing** path on the remote host to sync with, for example, */mnt/pool*. Note that the path length cannot be greater than 255 characters.                                                                                                                      |
| Validate Remote Path         | checkbox       | Verifies the existence of the `Remote Path`.                                                                                                                                                                                                                                                                     |
| Direction                    | drop-down menu | Direct the flow of the data to the remote host. Choices are *Push* or *Pull*. Default is to push to a remote host.                                                                                                                                                                                               |
| Short Description            | string         | Enter a description of the rsync task.                                                                                                                                                                                                                                                                           |
| Schedule the Rsync Task      | drop-down menu | Choose how often to run the task. Choices are *Hourly*, *Daily*, *Weekly*, *Monthly*, or *Custom*. Selecting *Custom* opens the `advanced scheduler`.                                                                                                                                                            |
| Recursive                    | checkbox       | Set to include all subdirectories of the specified directory. When unset, only the specified directory is included.                                                                                                                                                                                              |
| Times                        | checkbox       | Set to preserve the modification times of files.                                                                                                                                                                                                                                                                 |
| Compress                     | checkbox       | Set to reduce the size of the data to transmit. Recommended for slow connections.                                                                                                                                                                                                                                |
| Archive                      | checkbox       | When set, rsync is run recursively, preserving symlinks, permissions, modification times, group, and special files. When run as root, owner, device files, and special files are also preserved. Equivalent to `rsync -rlptgoD`.                                                                                 |
| Delete                       | checkbox       | Set to delete files in the destination directory that do not exist in the source directory.                                                                                                                                                                                                                      |
| Quiet                        | checkbox       | Suppress rsync task status `alerts <Alert>`.                                                                                                                                                                                                                                                                     |
| Preserve permissions         | checkbox       | Set to preserve original file permissions. This is useful when the user is set to *root*.                                                                                                                                                                                                                        |
| Preserve extended attributes | checkbox       | [Extended attributes][] are preserved, but must be supported by both systems.                                                                                                                                                                                                                                    |
| Delay Updates                | checkbox       | Set to save the temporary file from each updated file to a holding directory until the end of the transfer when all transferred files are renamed into place.                                                                                                                                                    |
| Extra options                | string         | Additional [rsync(1)][] options to include. Note: The `*` character must be escaped with a backslash (`\\*.txt`) or used inside single quotes. (`'*.txt'`)                                                                                                                                                       |
| Enabled                      | checkbox       | Enable this rsync task. Unset to disable this rsync task without deleting it.                                                                                                                                                                                                                                    |

Rsync Configuration Options

</div>

  [rsyncd.conf(5)]: https://www.samba.org/ftp/rsync/rsyncd.conf.html
  [Extended attributes]: https://en.wikipedia.org/wiki/Extended_file_attributes
  [rsync(1)]: http://rsync.samba.org/ftp/rsync/rsync.html

If the rysnc server requires password authentication, enter
`--password-file={/PATHTO/FILENAME}` in the `Extra options` field,
replacing `/PATHTO/FILENAME` with the appropriate path to the file
containing the password.

Created rsync tasks are listed in `Rsync Tasks`. Click for an entry to
display buttons for `Edit`, `Delete`, or `Run Now`.

The `Status` column shows the status of the rsync task. To view the
detailed rsync logs for a task, click the `Status` entry when the task
is running or finished.

Rsync tasks also generate an `alert` on task completion. The alert shows
if the task succeeded or failed.

### Rsync Module Mode

This configuration example configures rsync module mode between the two
following FreeNAS<sup>®</sup> systems:

-   *192.168.2.2* has existing data in `/mnt/local/images`. It will be
    the rsync client, meaning that an rsync task needs to be defined. It
    will be referred to as *PUSH.*
-   *192.168.2.6* has an existing pool named `/mnt/remote`. It will be
    the rsync server, meaning that it will receive the contents of
    `/mnt/local/images`. An rsync module needs to be defined on this
    system and the rsyncd service needs to be started. It will be
    referred to as *PULL.*

On *PUSH*, an rsync task is defined in `Tasks --> Rsync Tasks`, . In
this example:

-   the `Path` points to `/usr/local/images`, the directory to be copied
-   the `Remote Host` points to *192.168.2.6*, the IP address of the
    rsync server
-   the `Rsync Mode` is *Module*
-   the `Remote Module Name` is *backups*; this will need to be defined
    on the rsync server
-   the `Direction` is *Push*
-   the rsync is scheduled to occur every 15 minutes
-   the `User` is set to *root* so it has permission to write anywhere
-   the `Preserve Permissions` option is enabled so that the original
    permissions are not overwritten by the *root* user

On *PULL*, an rsync module is defined in
`Services --> Rsync Configure --> Rsync Module`, . In this example:

-   the `Module Name` is *backups*; this needs to match the setting on
    the rsync client
-   the `Path` is `/mnt/remote`; a directory called `images` will be
    created to hold the contents of `/usr/local/images`
-   the `User` is set to *root* so it has permission to write anywhere

Descriptions of the configurable options can be found in
`Rsync Modules`.

-   `Hosts allow` is set to *192.168.2.2*, the IP address of the rsync
    client

To finish the configuration, start the rsync service on *PULL* in
`Services`. If the rsync is successful, the contents of
`/mnt/local/images/` will be mirrored to `/mnt/remote/images/`.

### Rsync over SSH Mode

SSH replication mode does not require the creation of an rsync module or
for the rsync service to be running on the rsync server. It does require
SSH to be configured before creating the rsync task:

-   a public/private key pair for the rsync user account (typically
    *root*) must be generated on *PUSH* and the public key copied to the
    same user account on *PULL*
-   to mitigate the risk of man-in-the-middle attacks, the public host
    key of *PULL* must be copied to *PUSH*
-   the SSH service must be running on *PULL*

To create the public/private key pair for the rsync user account, open
`Shell` on *PUSH* and run `ssh-keygen`. This example generates an RSA
type public/private key pair for the *root* user. When creating the key
pair, do not enter the passphrase as the key is meant to be used for an
automated task.

    ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/root/.ssh/id_rsa):
    Created directory '/root/.ssh'.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /root/.ssh/id_rsa.
    Your public key has been saved in /root/.ssh/id_rsa.pub.
    The key fingerprint is:
    f5:b0:06:d1:33:e4:95:cf:04:aa:bb:6e:a4:b7:2b:df root@freenas.local
    The key's randomart image is:
    +--[ RSA 2048]----+
    |        .o. oo   |
    |         o+o. .  |
    |       . =o +    |
    |        + +   o  |
    |       S o .     |
    |       .o        |
    |      o.         |
    |    o oo         |
    |     **oE        |
    |-----------------|
    |                 |
    |-----------------|

FreeNAS<sup>®</sup> supports RSA keys for SSH. When creating the key,
use `-t rsa` to specify this type of key. Refer to [Key-based
Authentication][] for more information.

  [Key-based Authentication]: https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/openssh.html#security-ssh-keygen

<div class="note">

<div class="title">

Note

</div>

If a different user account is used for the rsync task, use the `su -`
command after mounting the filesystem but before generating the key. For
example, if the rsync task is configured to use the *user1* user
account, use this command to become that user:

    su - user1

</div>

Next, view and copy the contents of the generated public key:

    more .ssh/id_rsa.pub
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1lBEXRgw1W8y8k+lXPlVR3xsmVSjtsoyIzV/PlQPo
    SrWotUQzqILq0SmUpViAAv4Ik3T8NtxXyohKmFNbBczU6tEsVGHo/2BLjvKiSHRPHc/1DX9hofcFti4h
    dcD7Y5mvU3MAEeDClt02/xoi5xS/RLxgP0R5dNrakw958Yn001sJS9VMf528fknUmasti00qmDDcp/kO
    xT+S6DFNDBy6IYQN4heqmhTPRXqPhXqcD1G+rWr/nZK4H8Ckzy+l9RaEXMRuTyQgqJB/rsRcmJX5fApd
    DmNfwrRSxLjDvUzfywnjFHlKk/+TQIT1gg1QQaj21PJD9pnDVF0AiJrWyWnR root@freenas.local

Go to *PULL* and paste (or append) the copied key into the
`SSH Public Key` field of `Accounts --> Users --> root -->` `--> Edit`,
or the username of the specified rsync user account. The paste for the
above example is shown in `Figure %s <tasks_pasting_sshkey_fig>`. When
pasting the key, ensure that it is pasted as one long line and, if
necessary, remove any extra spaces representing line breaks.

<div id="tasks_pasting_sshkey_fig">

![Pasting the User SSH Public Key][]

</div>

  [Pasting the User SSH Public Key]: images/accounts-users-edit-ssh-key.png

While on *PULL*, verify that the SSH service is running in `Services`
and start it if it is not.

Next, copy the host key of *PULL* using Shell on *PUSH*. The command
copies the RSA host key of the *PULL* server used in our previous
example. Be sure to include the double bracket *&gt;&gt;* to prevent
overwriting any existing entries in the `known_hosts` file:

    ssh-keyscan -t rsa 192.168.2.6 >> /root/.ssh/known_hosts

<div class="note">

<div class="title">

Note

</div>

If *PUSH* is a Linux system, use this command to copy the RSA key to the
Linux system:

    cat ~/.ssh/id_rsa.pub | ssh user@192.168.2.6 'cat >> .ssh/authorized_keys'

</div>

The rsync task can now be created on *PUSH*. To configure rsync SSH mode
using the systems in our previous example, the configuration is:

-   the `Path` points to `/mnt/local/images`, the directory to be copied
-   the `Remote Host` points to *192.168.2.6*, the IP address of the
    rsync server
-   the `Rsync Mode` is *SSH*
-   the rsync is scheduled to occur every 15 minutes
-   the `User` is set to *root* so it has permission to write anywhere;
    the public key for this user must be generated on *PUSH* and copied
    to *PULL*
-   the `Preserve Permissions` option is enabled so that the original
    permissions are not overwritten by the *root* user

Save the rsync task and the rsync will automatically occur according to
the schedule. In this example, the contents of `/mnt/local/images/` will
automatically appear in `/mnt/remote/images/` after 15 minutes. If the
content does not appear, use Shell on *PULL* to read
`/var/log/messages`. If the message indicates a *n* (newline character)
in the key, remove the space in the pasted key--it will be after the
character that appears just before the *n* in the error message.

<div class="index">

S.M.A.R.T. Tests

</div>

S.M.A.R.T. Tests
----------------

[S.M.A.R.T.][] (Self-Monitoring, Analysis and Reporting Technology) is a
monitoring system for computer hard disk drives to detect and report on
various indicators of reliability. Replace the drive when a failure is
anticipated by S.M.A.R.T. Most modern ATA, IDE, and SCSI-3 hard drives
support S.M.A.R.T. -- refer to the drive documentation for confirmation.

  [S.M.A.R.T.]: https://en.wikipedia.org/wiki/S.M.A.R.T.

Click `Tasks --> S.M.A.R.T. Tests` and to add a new scheduled S.M.A.R.T.
test. `Figure %s <tasks_add_smart_test_fig>` shows the configuration
screen that appears. Tests are listed under `S.M.A.R.T. Tests`. After
creating tests, check the configuration in `Services --> S.M.A.R.T.`,
then click the power button for the S.M.A.R.T. service in `Services` to
activate the service. The S.M.A.R.T. service will not start if there are
no pools.

<div class="note">

<div class="title">

Note

</div>

To prevent problems, do not enable the S.M.A.R.T. service if the disks
are controlled by a RAID controller. It is the job of the controller to
monitor S.M.A.R.T. and mark drives as Predictive Failure when they trip.

</div>

<div id="tasks_add_smart_test_fig">

![Adding a S.M.A.R.T. Test][]

</div>

  [Adding a S.M.A.R.T. Test]: images/tasks-smart-tests-add.png

`Table %s <tasks_smart_opts_tab>` summarizes the configurable options
when creating a S.M.A.R.T. test.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="tasks_smart_opts_tab">

| Setting                      | Value          | Description                                                                                                                                                                                                                  |
|------------------------------|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| All Disks                    | checkbox       | Set to monitor all disks.                                                                                                                                                                                                    |
| Disks                        | drop-down menu | Select the disks to monitor. Available when `All Disks` is unset.                                                                                                                                                            |
| Type                         | drop-down menu | Choose the test type. See [smartctl(8)][] for descriptions of each type. Some test types will degrade performance or take disks offline. Avoid scheduling S.M.A.R.T. tests simultaneously with scrub or resilver operations. |
| Short description            | string         | Optional. Enter a description of the S.M.A.R.T. test.                                                                                                                                                                        |
| Schedule the S.M.A.R.T. Test | drop-down menu | Choose how often to run the task. Choices are *Hourly*, *Daily*, *Weekly*, *Monthly*, or *Custom*. Selecting *Custom* opens the `advanced scheduler`.                                                                        |

S.M.A.R.T. Test Options

</div>

  [smartctl(8)]: https://www.smartmontools.org/browser/trunk/smartmontools/smartctl.8.in

An example configuration is to schedule a `Short Self-Test` once a week
and a `Long Self-Test` once a month. These tests do not have a
performance impact, as the disks prioritize normal I/O over the tests.
If a disk fails a test, even if the overall status is *Passed*, consider
replacing that disk.

<div class="warning">

<div class="title">

Warning

</div>

Some S.M.A.R.T. tests cause heavy disk activity and can drastically
reduce disk performance. Do not schedule S.M.A.R.T. tests to run at the
same time as scrub or resilver operations or during other periods of
intense disk activity.

</div>

Which tests will run and when can be verified by typing
`smartd -q showtests` within `Shell`.

The results of a test can be checked from `Shell` by specifying the name
of the drive. For example, to see the results for disk *ada0*, type:

    smartctl -l selftest /dev/ada0

<div class="index">

Periodic Snapshot, Snapshot

</div>

Periodic Snapshot Tasks
-----------------------

A periodic snapshot task allows scheduling the creation of read-only
versions of pools and datasets at a given point in time. Snapshots can
be created quickly and, if little data changes, new snapshots take up
very little space. For example, a snapshot where no files have changed
takes 0 MB of storage, but as changes are made to files, the snapshot
size changes to reflect the size of the changes.

Snapshots keep a history of files, providing a way to recover an older
copy or even a deleted file. For this reason, many administrators take
snapshots often, store them for a period of time, and store them on
another system, typically using `Replication Tasks`. Such a strategy
allows the administrator to roll the system back to a specific point in
time. If there is a catastrophic loss, an off-site snapshot can be used
to restore the system up to the time of the last snapshot.

A pool must exist before a snapshot can be created. Creating a pool is
described in `Pools`.

View the list of periodic snapshot tasks by going to
`Tasks --> Periodic Snapshot Tasks`. If a periodic snapshot task
encounters an error, the status column will show *ERROR*. Click the
status to view the logs of the task.

To create a periodic snapshot task, navigate to
`Tasks --> Periodic Snapshot Tasks` and click . This opens the screen
shown in `Figure %s <zfs_periodic_snapshot_fig>`.
`Table %s <zfs_periodic_snapshot_opts_tab>` describes the fields in this
screen.

<div id="zfs_periodic_snapshot_fig">

![Creating a Periodic Snapshot][]

</div>

  [Creating a Periodic Snapshot]: images/tasks-periodic-snapshot-tasks-add.png

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="zfs_periodic_snapshot_opts_tab">

| Setting                             | Value                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------------------------------------|----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dataset                             | drop-down menu             | Select a pool, dataset, or zvol.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Recursive                           | checkbox                   | Set to take separate snapshots of the dataset and each of its child datasets. Leave unset to take a single snapshot only of the specified dataset *without* child datasets.                                                                                                                                                                                                                                                                                                                                                             |
| Exclude                             | string                     | Exclude specific child datasets from the snapshot. Use with recursive snapshots. Comma-separated list of paths to any child datasets to exclude. Example: `pool1/dataset1/child1`. A recursive snapshot of `pool1/dataset1` will include all child datasets except `child1`.                                                                                                                                                                                                                                                            |
| Snapshot Lifetime                   | integer and drop-down menu | Define a length of time to retain the snapshot on this system. After the time expires, the snapshot is removed. Snapshots which have been replicated to other systems are not affected.                                                                                                                                                                                                                                                                                                                                                 |
| Snapshot Lifetime Unit              | drop-down                  | Select a unit of time to retain the snapshot on this system.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Naming Schema                       | string                     | Snapshot name format string. The default is `auto-%Y-%m-%d_%H-%M`. Must include the strings *%Y*, *%m*, *%d*, *%H*, and *%M*, which are replaced with the four-digit year, month, day of month, hour, and minute as defined in [strftime(3)][]. For example, snapshots of *pool1* with a Naming Schema of `customsnap-%Y%m%d.%H%M` have names like `pool1@customsnap-20190315.0527`.                                                                                                                                                    |
| Schedule the Periodic Snapshot Task | drop-down menu             | When the periodic snapshot task runs. Choose one of the preset schedules or choose *Custom* to use the `advanced scheduler`.                                                                                                                                                                                                                                                                                                                                                                                                            |
| Begin                               | drop-down menu             | Hour and minute when the system can begin taking snapshots.                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| End                                 | drop-down menu             | Hour and minute the system must stop creating snapshots. Snapshots already in progress will continue until complete.                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Allow Taking Empty Snapshots        | checkbox                   | Creates dataset snapshots even when there have been no changes to the dataset from the last snapshot. Recommended for creating long-term restore points, multiple snapshot tasks pointed at the same datasets, or to be compatible with snapshot schedules or replications created in FreeNAS<sup>®</sup> 11.2 and earlier. For example, allowing empty snapshots for a monthly snapshot schedule allows that monthly snapshot to be taken, even when a daily snapshot task has already taken a snapshot of any changes to the dataset. |
| Enabled                             | checkbox                   | To activate this periodic snapshot schedule, set this option. To disable this task without deleting it, unset this option.                                                                                                                                                                                                                                                                                                                                                                                                              |

Periodic Snapshot Options

</div>

  [strftime(3)]: https://www.freebsd.org/cgi/man.cgi?query=strftime

Setting `Recursive` adds child datasets to the snapshot. Creating
separate snapshots for each child dataset is not needed.

The `Naming Schema` can be manually adjusted to include more
information. For example, after configuring a periodic snapshot task
with a lifetime of two weeks, it could be helpful to define a
`Naming Schema` that shows the lifetime: `autosnap-%Y-%m-%d.%H-%M-{2w}`.

Click `SAVE` when finished customizing the task. Defined tasks are
listed alphabetically in `Tasks --> Periodic Snapshot Tasks`.

Click for a periodic snapshot task to see options to `Edit` or `Delete`
the scheduled task.

Deleting a dataset does not delete snapshot tasks for that dataset. To
re-use the snapshot task for a different dataset, `Edit` the task and
choose the new `Dataset`. The original dataset is shown in the
drop-down, but cannot be selected.

Deleting the last periodic snapshot task used by a replication task is
not permitted while that replication task remains active. The
replication task must be disabled before the related periodic snapshot
task can be deleted.

### Snapshot Autoremoval

The periodic snapshot task autoremoval process (which removes snapshots
after their configured `Snapshot Lifetime`) is run whenever any
`Enabled` periodic snapshot task runs.

When the autoremoval process runs, all snapshots on the system are
checked for removal. First, each snapshot is matched with a periodic
snapshot task according to the following criteria:

> -   *Dataset/Recursive*: To match a task, a snapshot must be on the
>     same `Dataset` as the task, or on a child dataset if the task is
>     marked `Recursive`.
> -   *Naming Schema*: To match a task, a snapshot's name must match the
>     `Naming Schema` defined in that task.
> -   *Schedule*: To match a task, the time at which the snapshot was
>     created (according to its name and naming schema) must match the
>     schedule defined in the task
>     (`Schedule the Periodic Snapshot Task`).
> -   *Enabled*: To match a task, the periodic snapshot task must be
>     `Enabled`.

At this point, if the snapshot does not match any periodic snapshot
tasks then it is not considered for autoremoval. However, if it does
match one (or possibly more than one) periodic snapshot task, it is
deleted if its creation time (according to its name and naming schema)
is older than the longest `Snapshot Lifetime` of any of the tasks it was
matched with.

One notable detail of this process is that there is no saved memory of
which task created which snapshot, or what the parameters of the
periodic snapshot task were at the time a snapshot was created. All
checks for autoremoval are based on the current state of the system.

These details become important when existing periodic snapshot tasks are
edited, disabled, or deleted. When editing a periodic snapshot task, if
the `Naming Schema` is changed, `Recursive` is unchecked, or the task is
rescheduled (`Schedule the Periodic
Snapshot Task`), previously created snapshots may not be automatically
removed as expected since the previously created snapshots may no longer
match any periodic snapshot tasks. Similarly, if a periodic snapshot
task is deleted or marked not `Enabled`, snapshots previously created by
that task will no longer be automatically removed.

In these cases, the user must manually remove unneeded snapshots that
were previously created by the modified or deleted periodic snapshot
task.

<div class="index">

Replication

</div>

Replication
-----------

*Replication* is the process of copying
`ZFS dataset snapshots <ZFS Primer>` from one storage pool to another.
Replications can be configured to copy snapshots to another pool on the
local system or send copies to a remote system that is in a different
physical location.

Replication schedules are typically paired with
`Periodic Snapshot Tasks` to generate local copies of important data and
replicate these copies to a remote system.

Replications require a source system with dataset snapshots and a
destination that can store the copied data. Remote replications require
a saved `SSH Connection <SSH Connections>` on the source system and the
destination system must be configured to allow `SSH` connections. Local
replications do not use SSH.

Snapshots are organized and sent to the destination according to the
creation date included in the snapshot name. When replicating manually
created snapshots, make sure snapshots are named according to their
actual creation date.

First-time replication tasks can take a long time to complete as the
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

The target dataset on the destination system is created in *read-only*
mode to protect the data. To mount or browse the data on the destination
system, use a clone of the snapshot. Clones are created in *read/write*
mode, making it possible to browse or mount them. See `Snapshots` for
more details.

Replications run in parallel as long as they do not conflict with each
other. Completion time depends on the number and size of snapshots and
the bandwidth available between the source and destination computers.

Examples in this section refer to the FreeNAS<sup>®</sup> system with
the original datasets for snapshot and replication as and the
FreeNAS<sup>®</sup> system that is storing replicated snapshots as .

<div class="index">

Replication Creation Wizard

</div>

### Replication Creation Wizard

To create a new replication, go to `Tasks --> Replication Tasks` and
click .

<div id="tasks_replication_wizard_fig">

![Replication Wizard: What and Where][]

</div>

  [Replication Wizard: What and Where]: images/tasks-replication-add-wizard-step1.png

The wizard allows loading previously saved replication configurations
and simplifies many replication settings. To see all possible
`replication creation options <Advanced Replication Creation>`, click
`ADVANCED REPLICATION CREATION`.

Using the wizard to create a new replication task begins by defining
what is being replicated and where. Choosing *On a Different System* for
either the `Source Location` or `Destination Location` requires an
`SSH Connection <SSH Connections>` to the remote system. Open the
drop-down menu to choose an SSH connection or click *Create New* to add
a new connection.

Start by selecting the `Source` datasets to be replicated. To choose a
dataset, click and select the dataset from the expandable tree. The path
of the dataset can also be typed into the field. Multiple snapshot
sources can be chosen using a comma (`,`) to separate each selection.
`Recursive` replication will include all snapshots of any descendant
datasets of the chosen `Source`.

Source datasets on the local system are replicated using existing
snapshots of the chosen datasets. When no snapshots exist,
FreeNAS<sup>®</sup> automatically creates snapshots of the chosen
datasets before starting the replication. To manually define which
dataset snapshots to replicate, set `Replicate Custom Snapshots` and
define a snapshot `Naming Schema`.

Source datasets on a remote system are replicated by defining a snapshot
`Naming Schema`. The schema is a pattern of the name and [strftime(3)][]
*%Y*, *%m*, *%d*, *%H*, and *%M* strings that match names of the
snapshots to include in the replication. For example, to replicate a
snapshot named `auto-2019-12-18.05-20` from a remote source, enter
`auto-%Y-%m-%d.%H-%M` as the replication task `Naming Schema`.

  [strftime(3)]: https://www.freebsd.org/cgi/man.cgi?query=strftime

The number of snapshots that will be replicated is shown. There is also
a `Recursive` option to include child datasets with the selected
datasets.

Now choose the `Destination` to receive the replicated snapshots. To
choose a destination path, click and select the dataset from the
expandable tree or type a path to the location in the field. Only a
single `Destination` path can be defined.

Using an SSH connection for replication adds the `SSH Transfer Security`
option. This sets the data transfer security level. The connection is
authenticated with SSH. Data can be encrypted during transfer for
security or left unencrypted to maximize transfer speed. **WARNING:**
Encryption is recommended, but can be disabled for increased speed on
secure networks.

A suggested replication `Task Name` is shown. This can be changed to
give a more meaningful name to the task. When the source and destination
have been set, click `NEXT` to choose when the replication will run.

<div id="tasks_replication_wizard_screen2_fig">

![Replication Wizard: When][]

</div>

  [Replication Wizard: When]: images/tasks-replication-add-wizard-step2.png

The replication task can be configured to run on a schedule or left
unscheduled and manually activated. Choosing *Run On a Schedule* adds
the `Scheduling` drop-down to choose from preset schedules or define a
*Custom* replication schedule. Choosing *Run Once* removes all
scheduling options.

`Destination Snapshot Lifetime` determines when replicated snapshots are
deleted from the destination system:

> -   *Same as Source*: duplicate the configured *Snapshot Lifetime*
>     value from the source dataset
>     `periodic snapshot task <Periodic Snapshot Tasks>`.
> -   *Never Delete*: never delete snapshots from the destination
>     system.
> -   *Custom*: define how long a snapshot remains on the destination
>     system. Enter a number and choose a measure of time from the
>     drop-down menus.

Clicking `START REPLICATION` saves the replication configuration and
activates the schedule. When the replication configuration includes a
source dataset on the local system and has a schedule, a
`periodic snapshot task <Periodic Snapshot Tasks>` of that dataset is
also created.

Tasks set to *Run Once* will start immediately. If a one-time
replication has no valid local system source dataset snapshots,
FreeNAS<sup>®</sup> will snapshot the source datasets and immediately
replicate those snapshots to the destination dataset.

All replication tasks are displayed in `Tasks --> Replication Tasks`.
The task settings that are shown by default can be adjusted by opening
the `COLUMNS` drop-down. To see more details about the last time the
replication task ran, click the entry under the `State` column. Tasks
can also be expanded by clicking for that task. Expanded tasks show all
replication settings and have , , and buttons.

<div class="index">

Advanced Replication Creation

</div>

### Advanced Replication Creation

The advanced replication creation screen has more options for
fine-tuning a replication. It also allows creating local replications,
legacy engine replications from FreeNAS<sup>®</sup> 11.1 or earlier, or
even creating a one-time replication that is not linked to a periodic
snapshot task.

Go to `System --> Replication Tasks`, click and
`ADVANCED REPLICATION CREATION` to see these options. This screen is
also displayed after clicking and `Edit` for an existing replication.

<div id="tasks_replication_advanced_fig">

![][1]

</div>

  [1]: images/tasks-replication-add-advanced.png

The `Transport` value changes many of the options for replication.
`Table %s <zfs_add_replication_task_opts_tab>` shows abbreviated names
of the `Transport` methods in the `Transport` column to identify fields
which appear when that method is selected.

> -   `ALL`: All `Transport` methods
> -   `SSH`: *SSH*
> -   `NCT`: *SSH+NETCAT*
> -   `LOC`: *LOCAL*
> -   `LEG`: *LEGACY*

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.13linewidth-2tabcolsep}
&gt;{RaggedRight}p{dimexpr 0.55linewidth-2tabcolsep}\|

</div>

<div id="zfs_add_replication_task_opts_tab">

<table>
<caption>Replication Task Options</caption>
<colgroup>
<col style="width: 16%" />
<col style="width: 7%" />
<col style="width: 9%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Transport</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Name</td>
<td>All</td>
<td>string</td>
<td>Descriptive name for the replication.</td>
</tr>
<tr class="even">
<td>Direction</td>
<td>SSH, NCT, LEG</td>
<td>drop-down menu</td>
<td><em>PUSH</em> sends snapshots to a destination system. <em>PULL</em> connects to a remote system and retrieves snapshots matching a <code class="interpreted-text" role="guilabel">Naming Schema</code>.</td>
</tr>
<tr class="odd">
<td>Transport</td>
<td>All</td>
<td>drop-down menu</td>
<td><p>Method of snapshot transfer:</p>
<ul>
<li><em>SSH</em> is supported by most systems. It requires a previously created <code class="interpreted-text" role="ref">SSH connection &lt;SSH Connections&gt;</code>.</li>
<li><em>SSH+NETCAT</em> uses SSH to establish a connection to the destination system, then uses <a href="https://github.com/freenas/py-libzfs">py-libzfs</a> to send an unencrypted data stream for higher transfer transfer speeds. By default, this is supported by FreeNAS<sup>®</sup> systems with 11.2 or later installed (11.3 or later is recommended). Destination systems that do not have FreeNAS<sup>®</sup> 11.2 or later installed might have to manually install <code class="interpreted-text" role="command">py-libzfs</code>.</li>
<li><em>LOCAL</em> efficiently replicates snapshots to another dataset on the same system.</li>
<li><em>LEGACY</em> uses the legacy replication engine from FreeNAS<sup>®</sup> 11.2 and earlier.</li>
</ul></td>
</tr>
<tr class="even">
<td>SSH Connection</td>
<td>SSH, NCT, LEG</td>
<td>drop-down menu</td>
<td>Choose the <code class="interpreted-text" role="ref">SSH connection &lt;SSH Connections&gt;</code>.</td>
</tr>
<tr class="odd">
<td>Netcat Active Side</td>
<td>NCT</td>
<td>drop-down menu</td>
<td>Establishing a connection requires that one of the connection systems has open TCP ports. Choose which system (<em>LOCAL</em> or <em>REMOTE</em>) will open ports. Consult your IT department to determine which systems are allowed to open ports.</td>
</tr>
<tr class="even">
<td>Netcat Active Side Listen Address</td>
<td>NCT</td>
<td>string</td>
<td>IP address on which the connection <code class="interpreted-text" role="guilabel">Active Side</code> listens. Defaults to <code>0.0.0.0</code>.</td>
</tr>
<tr class="odd">
<td>Netcat Active Side Min Port</td>
<td>NCT</td>
<td>integer</td>
<td>Lowest port number of the active side listen address that is open to connections.</td>
</tr>
<tr class="even">
<td>Netcat Active Side Max Port</td>
<td>NCT</td>
<td>integer</td>
<td>Highest port number of the active side listen address that is open to connections. The first available port between the minimum and maximum is used.</td>
</tr>
<tr class="odd">
<td>Netcat Active Side Connect Address</td>
<td>NCT</td>
<td>string</td>
<td>Hostname or IP address used to connect to the active side system. When the active side is <em>LOCAL</em>, this defaults to the <code>SSH_CLIENT</code> environment variable. When the active side is <em>REMOTE</em>, this defaults to the SSH connection hostname.</td>
</tr>
<tr class="even">
<td>Source</td>
<td>All</td>
<td>, string</td>
<td>Define the path to a system location that has snapshots to replicate. Click the to see all locations on the source system or click in the field to manually type a location (Example: <code class="interpreted-text" role="samp">pool1/dataset1</code>). Multiple source locations can be selected or manually defined with a comma (literal:<span class="title-ref">,</span>) separator.</td>
</tr>
<tr class="odd">
<td>Destination</td>
<td>All</td>
<td>, string</td>
<td><p>Define the path to a system location that will store replicated snapshots. Click the to see all locations on the destination system or click in the field to manually type a location path (Example: <code class="interpreted-text" role="samp">pool1/dataset1</code>). Selecting a location defines the full path to that location as the destination. Appending a name to the path will create new zvol at that location.</p>
<p>For example, selecting <code class="interpreted-text" role="file">pool1/dataset1</code> will store snapshots in <code class="interpreted-text" role="file">dataset1</code>, but clicking the path and typing <code>/zvol1</code> after <code>dataset1</code> will create <code class="interpreted-text" role="file">zvol1</code> for snapshot storage.</p></td>
</tr>
<tr class="even">
<td>Recursive</td>
<td>All</td>
<td>checkbox</td>
<td>Replicate all child dataset snapshots. When set, <code class="interpreted-text" role="guilabel">Exclude Child Datasets</code> becomes visible.</td>
</tr>
<tr class="odd">
<td>Exclude Child Datasets</td>
<td>SSH, NCT, LOC</td>
<td>string</td>
<td>Exclude specific child dataset snapshots from the replication. Use with <code class="interpreted-text" role="guilabel">Recursive</code> replications. List child dataset names to exclude. Separate multiple entries with a comma (<code>,</code>). Example: <code class="interpreted-text" role="samp">pool1/dataset1/child1</code>. A recursive replication of <code class="interpreted-text" role="file">pool1/dataset1</code> snapshots includes all child dataset snapshots except <code class="interpreted-text" role="file">child1</code>.</td>
</tr>
<tr class="even">
<td>Properties</td>
<td>SSH, NCT, LOC</td>
<td>checkbox</td>
<td>Include dataset properties with the replicated snapshots.</td>
</tr>
<tr class="odd">
<td>Periodic Snapshot Tasks</td>
<td>SSH, NCT, LOC</td>
<td>drop-down menu</td>
<td>Snapshot schedule for this replication task. Choose from configured <code class="interpreted-text" role="ref">Periodic Snapshot Tasks</code>. This replication task must have the same <code class="interpreted-text" role="guilabel">Recursive</code> and <code class="interpreted-text" role="guilabel">Exclude Child Datasets</code> values as the chosen periodic snapshot task. Selecting a periodic snapshot schedule removes the <code class="interpreted-text" role="guilabel">Schedule</code> field.</td>
</tr>
<tr class="even">
<td>Naming Schema</td>
<td>SSH, NCT, LOC</td>
<td>string</td>
<td>Visible with <em>PULL</em> replications. Pattern of naming custom snapshots to be replicated. Enter the name and <a href="https://www.freebsd.org/cgi/man.cgi?query=strftime">strftime(3)</a> <em>%Y</em>, <em>%m</em>, <em>%d</em>, <em>%H</em>, and <em>%M</em> strings that match the snapshots to include in the replication.</td>
</tr>
<tr class="odd">
<td>Also Include Naming Schema</td>
<td>SSH, NCT, LOC</td>
<td>string</td>
<td><p>Visible with <em>PUSH</em> replications. Pattern of naming custom snapshots to include in the replication with the periodic snapshot schedule. Enter the <a href="https://www.freebsd.org/cgi/man.cgi?query=strftime">strftime(3)</a> strings that match the snapshots to include in the replication.</p>
<p>When a periodic snapshot is not linked to the replication, enter the naming schema for manually created snapshots. Has the same <em>%Y</em>, <em>%m</em>, <em>%d</em>, <em>%H</em>, and <em>%M</em> string requirements as the <code class="interpreted-text" role="guilabel">Naming Schema</code> in a <code class="interpreted-text" role="ref">periodic snapshot task &lt;zfs_periodic_snapshot_opts_tab&gt;</code>.</p></td>
</tr>
<tr class="even">
<td>Run Automatically</td>
<td>SSH, NCT, LOC</td>
<td>checkbox</td>
<td>Set to either start this replication task immediately after the linked periodic snapshot task completes or continue to create a separate <code class="interpreted-text" role="guilabel">Schedule</code> for this replication.</td>
</tr>
<tr class="odd">
<td>Schedule</td>
<td>SSH, NCT, LOC</td>
<td>checkbox and drop-down menu</td>
<td>Start time for the replication task. Select a preset schedule or choose <em>Custom</em> to use the advanced scheduler. Adds the <code class="interpreted-text" role="guilabel">Begin</code> and <code class="interpreted-text" role="guilabel">End</code> fields.</td>
</tr>
<tr class="even">
<td>Begin</td>
<td>SSH, NCT, LOC</td>
<td>drop-down menu</td>
<td>Start time for the replication task.</td>
</tr>
<tr class="odd">
<td>End</td>
<td>SSH, NCT, LOC</td>
<td>drop-down menu</td>
<td>End time for the replication task. A replication that is already in progress can continue to run past this time.</td>
</tr>
<tr class="even">
<td>Replicate Specific Snapshots</td>
<td>SSH, NCT, LOC</td>
<td>checkbox and drop-down menu</td>
<td>Only replicate snapshots that match a defined creation time. To specify which snapshots will be replicated, set this checkbox and define the snapshot creation times that will be replicated. For example, setting this time frame to <em>Hourly</em> will only replicate snapshots that were created at the beginning of each hour.</td>
</tr>
<tr class="odd">
<td>Begin</td>
<td>SSH, NCT, LOC</td>
<td>drop-down menu</td>
<td>Daily time range for the specific periodic snapshots to replicate, in 15 minute increments. Periodic snapshots created before the <em>Begin</em> time will not be included in the replication.</td>
</tr>
<tr class="even">
<td>End</td>
<td>SSH, NCT, LOC</td>
<td>drop-down menu</td>
<td>Daily time range for the specific periodic snapshots to replicate, in 15 minute increments. Snapshots created after the <em>End</em> time will not be included in the replication.</td>
</tr>
<tr class="odd">
<td>Only Replicate Snapshots Matching Schedule</td>
<td>SSH, NCT, LOC</td>
<td>checkbox</td>
<td>Set to use the <code class="interpreted-text" role="guilabel">Schedule</code> in place of the <code class="interpreted-text" role="guilabel">Replicate Specific Snapshots</code> time frame. The <code class="interpreted-text" role="guilabel">Schedule</code> values are read over the <code class="interpreted-text" role="guilabel">Replicate Specific Snapshots</code> time frame.</td>
</tr>
<tr class="even">
<td>Replicate from scratch if incremental is not possible</td>
<td>SSH, NCT, LOC</td>
<td>checkbox</td>
<td>If the destination system has snapshots but they do not have any data in common with the source snapshots, destroy all destination snapshots and do a full replication. <strong>Warning:</strong> enabling this option can cause data loss or excessive data transfer if the replication is misconfigured.</td>
</tr>
<tr class="odd">
<td>Hold Pending Snapshots</td>
<td>SSH, NCT, LOC</td>
<td>checkbox</td>
<td>Prevent source system snapshots that have failed replication from being automatically removed by the <code class="interpreted-text" role="guilabel">Snapshot Retention Policy</code>.</td>
</tr>
<tr class="even">
<td>Snapshot Retention Policy</td>
<td>SSH, NCT, LOC</td>
<td>drop-down menu</td>
<td><p>When replicated snapshots are deleted from the destination system:</p>
<ul>
<li><em>Same as Source</em>: use <code class="interpreted-text" role="guilabel">Snapshot Lifetime</code> value from the source <code class="interpreted-text" role="ref">periodic snapshot task &lt;Periodic Snapshot Tasks&gt;</code>.</li>
<li><em>Custom</em>: define a <code class="interpreted-text" role="guilabel">Snapshot Lifetime</code> for the destination system.</li>
<li><em>None</em>: never delete snapshots from the destination system.</li>
</ul></td>
</tr>
<tr class="odd">
<td>Snapshot Lifetime</td>
<td>All</td>
<td>integer and drop-down menu</td>
<td>Added with a <em>Custom</em> retention policy. How long a snapshot remains on the destination system. Enter a number and choose a measure of time from the drop-down.</td>
</tr>
<tr class="even">
<td>Stream Compression</td>
<td>SSH</td>
<td>drop-down menu</td>
<td>Select a compression algorithm to reduce the size of the data being replicated. Only appears when <em>SSH</em> is chosen for <code class="interpreted-text" role="guilabel">Transport</code>.</td>
</tr>
<tr class="odd">
<td>Limit (Examples: 500 KiB, 500M, 2 TB)</td>
<td>SSH</td>
<td>integer</td>
<td>Limit replication speed to this number of bytes per second. Zero means no limit.</td>
</tr>
<tr class="even">
<td>Send Deduplicated Stream</td>
<td>SSH, NCT, LOC</td>
<td>checkbox</td>
<td>Deduplicate the stream to avoid sending redundant data blocks. The destination system must also support deduplicated streams. See <a href="https://www.freebsd.org/cgi/man.cgi?query=zfs">zfs(8)</a>.</td>
</tr>
<tr class="odd">
<td>Allow Blocks Larger than 128KB</td>
<td>SSH, NCT, LOC</td>
<td>checkbox</td>
<td>Allow sending large data blocks. The destination system must also support large blocks. See <a href="https://www.freebsd.org/cgi/man.cgi?query=zfs">zfs(8)</a>.</td>
</tr>
<tr class="even">
<td>Allow Compressed WRITE Records</td>
<td>SSH, NCT, LOC</td>
<td>checkbox</td>
<td>Use compressed WRITE records to make the stream more efficient. The destination system must also support compressed WRITE records. See <a href="https://www.freebsd.org/cgi/man.cgi?query=zfs">zfs(8)</a>.</td>
</tr>
<tr class="odd">
<td>Number of retries for failed replications</td>
<td>SSH, NCT, LOC</td>
<td>integer</td>
<td>Number of times the replication is attempted before stopping and marking the task as failed.</td>
</tr>
<tr class="even">
<td>Logging Level</td>
<td>All</td>
<td>drop-down menu</td>
<td>Message verbosity level in the replication task log.</td>
</tr>
<tr class="odd">
<td>Enabled</td>
<td>All</td>
<td>checkbox</td>
<td>Activates the replication schedule.</td>
</tr>
</tbody>
</table>

Replication Task Options

</div>

### Replication Tasks

Saved replications are shown on the `Replication Tasks` page.

<div id="zfs_repl_task_list_fig">

<figure>
<img src="images/tasks-replication-tasks.png" style="width:90.0%" alt="Replication Task List" /><figcaption aria-hidden="true">Replication Task List</figcaption>
</figure>

</div>

The replication name and configuration details are shown in the list. To
adjust the default table view, open the `COLUMNS` menu and select the
replication details to show in the normal table view.

The `State` column shows the status of the replication task. To view the
detailed replication logs for a task, click the `State` entry when the
task is running or finished.

Expanding an entry shows additional buttons for starting or editing a
replication task.

### Limiting Replication Times

The `Schedule`, `Begin`, and `End` times in a replication task make it
possible to restrict when replication is allowed. These times can be set
to only allow replication after business hours, or at other times when
disk or network activity will not slow down other operations like
snapshots or `Scrub Tasks`. The default settings allow replication to
occur at any time.

These times control when replication task are allowed to start, but will
not stop a replication task that is already running. Once a replication
task has begun, it will run until finished.

### Troubleshooting Replication

Replication depends on SSH, disks, network, compression, and encryption
to work. A failure or misconfiguration of any of these can prevent
successful replication.

Replication logs are saved in `var/log/zettarepl.log`. Logs of
individual replication tasks can be viewed by clicking the replication
`State`.

#### SSH

`SSH` must be able to connect from the source system to the destination
system with an encryption key. This is tested from `Shell` by making an
`SSH` connection from the source system to the destination system. For
example, this is a connection from *Alpha* to *Beta* at *10.0.0.118*.
Start the `Shell` on the source machine (*Alpha*), then enter this
command:

    ssh -vv 10.0.0.118

On the first connection, the system might say

    No matching host key fingerprint found in DNS.
    Are you sure you want to continue connecting (yes/no)?

Verify that this is the correct destination computer from the preceding
information on the screen and type `yes`. At this point, an `SSH` shell
connection is open to the destination system, *Beta*.

If a password is requested, SSH authentication is not working. An SSH
key value must be present in the destination system
`/root/.ssh/authorized_keys` file. `/var/log/auth.log` file can show
diagnostic errors for login problems on the destination computer also.

#### Compression

Matching compression and decompression programs must be available on
both the source and destination computers. This is not a problem when
both computers are running FreeNAS<sup>®</sup>, but other operating
systems might not have *lz4*, *pigz*, or *plzip* compression programs
installed by default. An easy way to diagnose the problem is to set
`Replication Stream Compression` to *Off*. If the replication runs,
select the preferred compression method and check `/var/log/debug.log`
on the FreeNAS<sup>®</sup> system for errors.

#### Manual Testing

On *Alpha*, the source computer, the `/var/log/messages` file can also
show helpful messages to locate the problem.

On the source computer, *Alpha*, open a `Shell` and manually send a
single snapshot to the destination computer, *Beta*. The snapshot used
in this example is named `auto-20161206.1110-2w`. As before, it is
located in the *alphapool/alphadata* dataset. A `@` symbol separates the
name of the dataset from the name of the snapshot in the command.

    zfs send alphapool/alphadata@auto-20161206.1110-2w | ssh 10.0.0.118 zfs recv betapool

If a snapshot of that name already exists on the destination computer,
the system will refuse to overwrite it with the new snapshot. The
existing snapshot on the destination computer can be deleted by opening
a `Shell` on *Beta* and running this command:

    zfs destroy -R betapool/alphadata@auto-20161206.1110-2w

Then send the snapshot manually again. Snapshots on the destination
system, *Beta*, are listed from the `Shell` with `zfs list -t snapshot`
or from `Storage --> Snapshots`.

Error messages here can indicate any remaining problems.

<div class="index">

Resilver Priority

</div>

Resilver Priority
-----------------

Resilvering, or the process of copying data to a replacement disk, is
best completed as quickly as possible. Increasing the priority of
resilvers can help them to complete more quickly. The
`Resilver Priority` menu makes it possible to increase the priority of
resilvering at times where the additional I/O or CPU usage will not
affect normal usage. Select `Tasks --> Resilver Priority` to display the
screen shown in `Figure %s <storage_resilver_pri_fig>`.
`Table %s <storage_resilver_pri_opts_tab>` describes the fields on this
screen.

<div id="storage_resilver_pri_fig">

![Resilver Priority][]

</div>

  [Resilver Priority]: images/tasks-resilver-priority.png

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.2linewidth-2tabcolsep}

</div>

<div id="storage_resilver_pri_opts_tab">

| Setting          | Value      | Description                                                                                                              |
|------------------|------------|--------------------------------------------------------------------------------------------------------------------------|
| Enabled          | checkbox   | Set to run resilver tasks between the configured times.                                                                  |
| Begin Time       | drop-down  | Choose the hour and minute when resilver tasks can be started.                                                           |
| End Time         | drop-down  | Choose the hour and minute when new resilver tasks can no longer be started. This does not affect active resilver tasks. |
| Days of the Week | checkboxes | Select the days to run resilver tasks.                                                                                   |

Resilver Priority Options

</div>

<div class="index">

Scrub

</div>

Scrub Tasks
-----------

A scrub is the process of ZFS scanning through the data on a pool.
Scrubs help to identify data integrity problems, detect silent data
corruptions caused by transient hardware issues, and provide early
alerts of impending disk failures. FreeNAS<sup>®</sup> makes it easy to
schedule periodic automatic scrubs.

It is recommneded that each pool is scrubbed at least once a month. Bit
errors in critical data can be detected by ZFS, but only when that data
is read. Scheduled scrubs can find bit errors in rarely-read data. The
amount of time needed for a scrub is proportional to the quantity of
data on the pool. Typical scrubs take several hours or longer.

The scrub process is I/O intensive and can negatively impact
performance. Schedule scrubs for evenings or weekends to minimize impact
to users. Make certain that scrubs and other disk-intensive activity
like `S.M.A.R.T. Tests` are scheduled to run on different days to avoid
disk contention and extreme performance impacts.

Scrubs only check used disk space. To check unused disk space, schedule
`S.M.A.R.T. Tests` of `Type` *Long Self-Test* to run once or twice a
month.

Scrubs are scheduled and managed with `Tasks --> Scrub Tasks`.

When a pool is created, a scrub is automatically scheduled. An entry
with the same pool name is added to `Tasks --> Scrub Tasks`. A summary
of this entry can be viewed with `Tasks --> Scrub Tasks`.
`Figure %s <zfs_view_volume_scrub_fig>` displays the default settings
for the pool named `pool1`. In this example, and `Edit` for a pool is
clicked to display the `Edit` screen. `Table %s <zfs_scrub_opts_tab>`
summarizes the options in this screen.

<div id="zfs_view_volume_scrub_fig">

![Viewing Pool Default Scrub Settings][]

</div>

  [Viewing Pool Default Scrub Settings]: images/tasks-scrub-tasks-actions-edit.png

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.16linewidth-2tabcolsep}

</div>

<div id="zfs_scrub_opts_tab">

| Setting                 | Value          | Description                                                                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pool                    | drop-down menu | Choose a pool to scrub.                                                                                                                                                                                                                                                                                                                                                                                        |
| Threshold days          | string         | Days before a completed scrub is allowed to run again. This controls the task schedule. For example, scheduling a scrub to run daily and setting `Threshold days` to *7* means the scrub attempts to run daily. When the scrub is successful, it continues to check daily but does not run again until seven days have elapsed. Using a multiple of seven ensures the scrub always occurs on the same weekday. |
| Description             | string         | Describe the scrub task.                                                                                                                                                                                                                                                                                                                                                                                       |
| Schedule the Scrub Task | drop-down menu | Choose how often to run the scrub task. Choices are *Hourly*, *Daily*, *Weekly*, *Monthly*, or *Custom*. Selecting *Custom* opens the `advanced scheduler`.                                                                                                                                                                                                                                                    |
| Enabled                 | checkbox       | Unset to disable the scheduled scrub without deleting it.                                                                                                                                                                                                                                                                                                                                                      |

ZFS Scrub Options

</div>

Review the default selections and, if necessary, modify them to meet the
needs of the environment. Scrub tasks cannot run for locked or unmounted
pools.

Scheduled scrubs can be deleted with the `Delete` button, but this is
not recommended. **Scrubs can provide an early indication of disk issues
before a disk failure.** If a scrub is too intensive for the hardware,
consider temporarily deselecting the `Enabled` button for the scrub
until the hardware can be upgraded.

<div class="index">

Cloud Sync

</div>

Cloud Sync Tasks
----------------

Files or directories can be synchronized to remote cloud storage
providers with the `Cloud Sync Tasks` feature.

<div class="warning">

<div class="title">

Warning

</div>

This Cloud Sync task might go to a third party commercial vendor not
directly affiliated with iXsystems. Please investigate and fully
understand that vendor's pricing policies and services before creating
any Cloud Sync task. iXsystems is not responsible for any charges
incurred from the use of third party vendors with the Cloud Sync
feature.

</div>

`Cloud Credentials` must be defined before a cloud sync is created. One
set of credentials can be used for more than one cloud sync. For
example, a single set of credentials for Amazon S3 can be used for
separate cloud syncs that push different sets of files or directories.

A cloud storage area must also exist. With Amazon S3, these are called
*buckets*. The bucket must be created before a sync task can be created.

After the cloud credentials have been configured,
`Tasks --> Cloud Sync Tasks` is used to define the schedule for running
a cloud sync task. The time selected is when the Cloud Sync task is
allowed to begin. An in-progress cloud sync must complete before another
cloud sync can start. The cloud sync runs until finished, even after the
selected ending time. To stop the cloud sync task before it is finished,
click `--> Stop`.

An example is shown in `Figure %s <tasks_cloudsync_status_fig>`.

<div id="tasks_cloudsync_status_fig">

![Cloud Sync Status][]

</div>

  [Cloud Sync Status]: images/tasks-cloud-sync-tasks.png

The cloud sync `Status` indicates the state of most recent cloud sync.
Clicking the `Status` entry shows the task logs and includes an option
to download them.

Click to display the `Add Cloud Sync` menu shown in
`Figure %s <tasks_cloudsync_add_fig>`.

<div id="tasks_cloudsync_add_fig">

![Adding a Cloud Sync][]

</div>

  [Adding a Cloud Sync]: images/tasks-cloud-sync-tasks-add.png

`Table %s <tasks_cloudsync_opts_tab>` shows the configuration options
for Cloud Syncs.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.20linewidth-2tabcolsep}

</div>

<div id="tasks_cloudsync_opts_tab">

<table>
<caption>Cloud Sync Options</caption>
<colgroup>
<col style="width: 14%" />
<col style="width: 11%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Value Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Description</td>
<td>string</td>
<td>A description of the Cloud Sync Task.</td>
</tr>
<tr class="even">
<td>Direction</td>
<td>drop-down menu</td>
<td><em>PUSH</em> sends data to cloud storage. <em>PULL</em> receives data from cloud storage. Changing the direction resets the <code class="interpreted-text" role="guilabel">Transfer Mode</code> to <em>COPY</em>.</td>
</tr>
<tr class="odd">
<td>Credential</td>
<td>drop-down menu</td>
<td>Select the cloud storage provider credentials from the list of available <code class="interpreted-text" role="ref">Cloud Credentials</code>. The credential is tested and an error is displayed if a connection cannot be made. Click <code class="interpreted-text" role="guilabel">Fix Credential</code> to go to the configuration page for that <code class="interpreted-text" role="ref">Cloud Credential &lt;Cloud Credentials&gt;</code>. <code class="interpreted-text" role="guilabel">SAVE</code> is disabled until a valid credential is selected.</td>
</tr>
<tr class="even">
<td>Bucket/Container</td>
<td>drop-down menu</td>
<td><p><code class="interpreted-text" role="guilabel">Bucket</code>: Only appears when an S3 credential is the <em>Provider</em>. Select the predefined S3 bucket to use.</p>
<p><code class="interpreted-text" role="guilabel">Container</code>: The pre-configured container name. Only appears when a <code>AZUREBLOB</code> or <code>hubiC</code> credential is selected as the <code class="interpreted-text" role="guilabel">Credential</code>.</p></td>
</tr>
<tr class="odd">
<td>Folder</td>
<td>browse button</td>
<td>The name of the predefined folder within the selected bucket or container. Type the name or click to list the remote filesystem and choose the folder.</td>
</tr>
<tr class="even">
<td>Server Side Encryption</td>
<td>drop-down menu</td>
<td>Active encryption on the cloud provider account. Choose <em>None</em> or <em>AES-256</em>. Only visible when the cloud provider supports encryption.</td>
</tr>
<tr class="odd">
<td>Storage Class</td>
<td>drop-down menu</td>
<td>Classification for each S3 object. Choose a class based on the specific use case or performance requirements. See <a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html">Amazon S3 Storage Classes</a> for more information on which storage class to choose. <code class="interpreted-text" role="guilabel">Storage Class</code> only appears when an S3 credential is the <em>Provider</em>.</td>
</tr>
<tr class="even">
<td>Upload Chunk Size (MiB)</td>
<td>integer</td>
<td>Files are split into chunks of this size before upload. The number of chunks that can be simultaneously transferred is set by the <code class="interpreted-text" role="guilabel">Transfers</code> number. The single largest file being transferred must fit into no more than 10,000 chunks.</td>
</tr>
<tr class="odd">
<td>Use --fast-list</td>
<td>checkbox</td>
<td><a href="https://rclone.org/docs/\#fast-list">Use fewer transactions in exchange for more RAM</a>. Modifying this setting can speed up <em>or</em> slow down the transfer. Only appears with a compatible <code class="interpreted-text" role="guilabel">Credential</code>.</td>
</tr>
<tr class="even">
<td>Directory/Files</td>
<td>browse button</td>
<td>Select directories or files to be sent to the cloud for <em>Push</em> syncs, or the destination to be written for <em>Pull</em> syncs. Be cautious about the destination of <em>Pull</em> jobs to avoid overwriting existing files.</td>
</tr>
<tr class="odd">
<td>Transfer Mode</td>
<td>drop-down menu</td>
<td><p><em>SYNC</em>: Files on the destination are <strong>changed</strong> to match those on the source. If a file does not exist on the source, it is also <strong>deleted</strong> from the destination. There are <code class="interpreted-text" role="ref">exceptions &lt;sync task notes&gt;</code> to this behavior.</p>
<p><em>COPY</em>: Files from the source are <strong>copied</strong> to the destination. If files with the same names are present on the destination, they are <strong>overwritten</strong>.</p>
<p><em>MOVE</em>: After files are <strong>copied</strong> from the source to the destination, they are <strong>deleted</strong> from the source. Files with the same names on the destination are <strong>overwritten</strong>.</p></td>
</tr>
<tr class="even">
<td>Take Snapshot</td>
<td>checkbox</td>
<td>Take a snapshot of the dataset before a <em>PUSH</em>. This cannot be enabled when the chosen dataset to <em>PUSH</em> has nested datasets.</td>
</tr>
<tr class="odd">
<td>Pre-script</td>
<td>string</td>
<td>A script to execute before the Cloud Sync Task is run.</td>
</tr>
<tr class="even">
<td>Post-script</td>
<td>string</td>
<td>A script to execute after the Cloud Sync Task is run.</td>
</tr>
<tr class="odd">
<td>Remote Encryption</td>
<td>checkbox</td>
<td><p>Use <a href="https://rclone.org/crypt/">rclone crypt</a> to manage data encryption during <em>PUSH</em> or <em>PULL</em> transfers:</p>
<p><em>PUSH:</em> Encrypt files before transfer and store the encrypted files on the remote system. Files are encrypted using the <code class="interpreted-text" role="guilabel">Encryption Password</code> and <code class="interpreted-text" role="guilabel">Encryption Salt</code> values.</p>
<p><em>PULL:</em> Decrypt files that are being stored on the remote system before the transfer. Transferring the encrypted files requires entering the same <code class="interpreted-text" role="guilabel">Encryption Password</code> and <code class="interpreted-text" role="guilabel">Encryption Salt</code> that was used to encrypt the files.</p>
<p>Adds the <code class="interpreted-text" role="guilabel">Filename Encryption</code>, <code class="interpreted-text" role="guilabel">Encryption Password</code>, and <code class="interpreted-text" role="guilabel">Encryption Salt</code> options. Additional details about the encryption algorithm and key derivation are available in the <a href="https://rclone.org/crypt/#file-formats">rclone crypt File formats documentation</a>.</p></td>
</tr>
<tr class="even">
<td>Filename Encryption</td>
<td>checkbox</td>
<td><p>Encrypt (<em>PUSH</em>) or decrypt (<em>PULL</em>) file names with the rclone <a href="https://rclone.org/crypt/#file-name-encryption-modes">"Standard" file name encryption mode</a>. The original directory structure is preserved. A filename with the same name always has the same encrypted filename.</p>
<p><em>PULL</em> tasks that have <code class="interpreted-text" role="guilabel">Filename Encryption</code> enabled and an incorrect <code class="interpreted-text" role="guilabel">Encryption Password</code> or <code class="interpreted-text" role="guilabel">Encryption Salt</code> will not transfer any files but still report that the task was successful. To verify that files were transferred successfully, click the finished <code class="interpreted-text" role="ref">task status &lt;tasks_cloudsync_status_fig&gt;</code> to see a list of transferred files.</p></td>
</tr>
<tr class="odd">
<td>Encryption Password</td>
<td>string</td>
<td>Password to encrypt and decrypt remote data. <strong>Warning</strong>: Always securely back up this password! Losing the encryption password will result in data loss.</td>
</tr>
<tr class="even">
<td>Encryption Salt</td>
<td>string</td>
<td>Enter a long string of random characters for use as <a href="https://searchsecurity.techtarget.com/definition/salt">salt</a> for the encryption password. <strong>Warning</strong>: Always securely back up the encryption salt value! Losing the salt value will result in data loss.</td>
</tr>
<tr class="odd">
<td>Schedule the Cloud Sync Task</td>
<td>drop-down menu</td>
<td>Choose how often or at what time to start a sync. Choices are <em>Hourly</em>, <em>Daily</em>, <em>Weekly</em>, <em>Monthly</em>, or <em>Custom</em>. Selecting <em>Custom</em> opens the <code class="interpreted-text" role="ref">advanced scheduler</code>.</td>
</tr>
<tr class="even">
<td>Transfers</td>
<td>integer</td>
<td>Number of simultaneous file transfers. Enter a number based on the available bandwidth and destination system performance. See <a href="https://rclone.org/docs/#transfers-n">rclone --transfers</a>.</td>
</tr>
<tr class="odd">
<td>Follow Symlinks</td>
<td>checkbox</td>
<td>Include symbolic link targets in the transfer.</td>
</tr>
<tr class="even">
<td>Enabled</td>
<td>checkbox</td>
<td>Enable this Cloud Sync Task. Unset to disable this Cloud Sync Task without deleting it.</td>
</tr>
<tr class="odd">
<td>Bandwidth Limit</td>
<td>string</td>
<td>A single bandwidth limit or bandwidth limit schedule in rclone format. Example: <em>08:00,512 12:00,10MB</em> <em>13:00,512 18:00,30MB 23:00,off</em>. Units can be specified with the beginning letter: b, k (default), M, or G. See <a href="https://rclone.org/docs/#bwlimit-bandwidth-spec">rclone --bwlimit.</a></td>
</tr>
<tr class="even">
<td>Exclude</td>
<td>string</td>
<td>List of files and directories to exclude from sync, one per line. See <a href="https://rclone.org/filtering/">https://rclone.org/filtering/</a>.</td>
</tr>
</tbody>
</table>

Cloud Sync Options

</div>

<div id="sync task notes">

There are specific circumstances where a *SYNC* task does not delete
files from the destination:

</div>

-   If [rclone sync][] encounters any errors, files are not deleted in
    the destination. This includes a common error when the Dropbox
    [copyright detector][] flags a file as copyrighted.
-   Syncing to a `B2 bucket <cloud_cred_tab>` does not delete files from
    the bucket, even when those files have been deleted locally.
    Instead, files are tagged with a version number or moved to a hidden
    state. To automatically delete old or unwanted files from the
    bucket, adjust the [Backblaze B2 Lifecycle Rules][]
-   Files stored in Amazon S3 Glacier or S3 Glacier Deep Archive cannot
    be deleted by [rclone sync][2]. These files must first be restored
    by another means, like the [Amazon S3 console][].

  [rclone sync]: https://rclone.org/commands/rclone_sync/
  [copyright detector]: https://techcrunch.com/2014/03/30/how-dropbox-knows-when-youre-sharing-copyrighted-stuff-without-actually-looking-at-your-stuff/
  [Backblaze B2 Lifecycle Rules]: https://www.backblaze.com/blog/backblaze-b2-lifecycle-rules/
  [2]: https://rclone.org/s3/#glacier-and-glacier-deep-archive/
  [Amazon S3 console]: https://docs.aws.amazon.com/AmazonS3/latest/user-guide/restore-archived-objects.html

To modify an existing cloud sync, click to access the `Run Now`, `Edit`,
and `Delete` options.

### Cloud Sync Example

This example shows a *Push* cloud sync that copies files from a
FreeNAS<sup>®</sup> pool to a cloud service provider.

The cloud service provider was configured with a location to store data
received from the FreeNAS<sup>®</sup> system.

In the FreeNAS<sup>®</sup> , go to `System --> Cloud Credentials` and
click to configure the cloud service provider credentials:

<div id="tasks_cloudsync_example_cred_fig">

![Example: Adding Cloud Credentials][]

</div>

  [Example: Adding Cloud Credentials]: images/system-cloud-credentials-add-example.png

Go to `Tasks --> Cloud Sync` and click to create a cloud sync job. The
`Description` is filled with a simple note describing the job. Data is
being sent to cloud storage, so this is a *Push*. The provider comes
from the cloud credentials defined in the previous step, and the
destination folder was configured in the cloud provider account.

The `Directory/Files` is set to the file or directory to copy to the
cloud provider.

The `Transfer Mode` is set to *COPY* so that only the files stored by
the cloud provider are modified.

The remaining requirement is to schedule the task. The default is to
send the data to cloud storage daily, but the schedule can be
`customized <Advanced Scheduler>` to fine-tune when the task runs.

The `Enabled` field is enabled by default, so this cloud sync will run
at the next scheduled time.

An example of a completed cloud sync task is shown in
`Figure %s <tasks_cloudsync_example_fig>`:

<div id="tasks_cloudsync_example_fig">

![Example: Successful Cloud Sync][]

</div>

  [Example: Successful Cloud Sync]: images/tasks-cloud-sync-tasks-example.png
