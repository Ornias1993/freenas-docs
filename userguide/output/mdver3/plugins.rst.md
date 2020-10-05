<div class="index">

Plugin

</div>

Plugins
=======

FreeNAS<sup>®</sup> provides the ability to extend the built-in NAS
services by providing two methods for installing additional software.

`Plugins` allow the user to browse, install, and configure pre-packaged
software from the . This method is easy to use, but provides a limited
amount of available software. Each plugin is automatically installed
into its own limited [FreeBSD jail][] that cannot install additional
software.

  [FreeBSD jail]: https://en.wikipedia.org/wiki/Freebsd_jail

`Jails` provide more control over software installation, but requires
working from the command line and a good understanding of networking
basics and software installation on FreeBSD-based systems.

Look through the `Plugins` and `Jails` sections to become familiar with
the features and limitations of each. Choose the method that best meets
the needs of the application.

<div class="note">

<div class="title">

Note

</div>

`Jail Storage` must be configured before plugins are available on
FreeNAS<sup>®</sup>. This means having a suitable
`pool <Creating Pools>` created to store plugins.

</div>

Installing Plugins
------------------

A plugin is a self-contained application installer designed to integrate
into the FreeNAS<sup>®</sup> . A plugin offers several advantages:

-   the FreeNAS<sup>®</sup> provides a browser for viewing the list of
    available plugins
-   the FreeNAS<sup>®</sup> provides buttons for installing, starting,
    managing, and uninstalling plugins
-   if the plugin has configuration options, a management screen is
    added to the FreeNAS<sup>®</sup> for these options to be configured

View available plugins by clicking `Plugins`.

`Figure %s <view_list_plugins_fig>` shows some of the available plugins.

<div id="view_list_plugins_fig">

![Viewing the List of Available Plugins][]

</div>

  [Viewing the List of Available Plugins]: images/plugins-available.png

<div class="note">

<div class="title">

Note

</div>

If the list of available plugins is not displayed, open `Shell` and
verify that the FreeNAS<sup>®</sup> system can `ping` an address on the
Internet. If it cannot, add a default gateway address and DNS server
address in `Network --> Global Configuration`.

</div>

Click `Browse a Collection` to toggle the plugins list between
[iXsystems plugins][], which receive updates every few weeks, and
[Community plugins][].

  [iXsystems plugins]: https://www.freenas.org/plugins/
  [Community plugins]: https://github.com/ix-plugin-hub/iocage-plugin-index

Click `REFRESH INDEX` to refresh the current list of plugins.

Click a plugin icon to see the description, whether it is an Official or
Community plugin, the version available, and the number of installed
instances.

To install the selected plugin, click `INSTALL`.

<div id="installing_plugin_fig">

![Installing the Plex Plugin][]

</div>

  [Installing the Plex Plugin]: images/plugins-install-example.png

<div class="note">

<div class="title">

Note

</div>

A warning will display when an unofficial plugin is selected for
installation.

</div>

Enter a `Jail Name`. A unique name is required, since multiple
installations of the same plugin are supported. Names can contain
letters, numbers, periods (`.`), dashes (`-`), and underscores (`_`).

Most plugins default to `NAT`. This setting is recommended as it does
not require manual configuration of multiple available IP addresses and
prevents addressing conflicts on the network.

Some plugins default to `DHCP` as their management utility conflicts
with `NAT`. Keep these plugins set to `DHCP` unless manually configuring
an IP address is preferred.

If both `NAT` and `DHCP` are unset, an IPv4 or IPv6 address can be
manually entered. If desired, an IPv4 or IPv6 interface can be selected.
If no interface is selected the jail IP address uses the current active
interface. The IPv4 or IPv6 address must be in the range of the local
network.

Click `ADVANCED PLUGIN INSTALLATION` to show all options for the plugin
jail. The options are described in `Advanced Jail Creation`.

To start the installation, click `SAVE`.

Depending on the size of the application, the installation can take
several minutes to download and install. A confirmation message is shown
when the installation completes, along with any post-installation notes.

Installed plugins appear on the `Plugins` page as shown in
`Figure %s <view_installed_plugins_fig>`.

<div class="note">

<div class="title">

Note

</div>

Plugins are also added to `Jails` as a *pluginv2* jail. This type of
jail is editable like a standard jail, but the *UUID* cannot be altered.
See `Managing Jails` for more details about modifying jails.

</div>

<div id="view_installed_plugins_fig">

![Viewing Installed Plugins][]

</div>

  [Viewing Installed Plugins]: images/plugins-available.png

Plugins are immediately started after installation. By default, all
plugins are started when the system boots. Unsetting `Boot` means the
plugin will not start when the system boots and must be started
manually.

In addition to the `Jail` name, the `Columns` menu can be used to
display more information about installed Plugins.

More information such as *RELEASE* and *VERSION* is shown by clicking .
Options to `RESTART`, `STOP`, `UPDATE`, `MANAGE`, and `UNINSTALL` the
plugin are also displayed. If an installed plugin has notes, the notes
can be viewed by clicking `POST INSTALL NOTES`.

Plugins with additional documentation also have a `DOCUMENTATION` button
which opens the README in the plugin repository.

The plugin must be started before the installed application is
available. Click and `START`. The plugin `Status` changes to `up` when
it starts successfully.

Stop and immediately start an `up` plugin by clicking and `RESTART`.

Click and `MANAGE` to open a management or configuration screen for the
application. Plugins with a management interface show the IP address and
port to that page in the *Admin Portal* column.

<div class="note">

<div class="title">

Note

</div>

Not all plugins have a functional management option. See
`Managing Jails` for more instructions about interacting with a plugin
jail with the shell.

</div>

Some plugins have options that need to be set before their service will
successfully start. Check the website of the application to see what
documentation is available. If there are any difficulties using a
plugin, refer to the official documentation for that application.

If the application requires access to the data stored on the
FreeNAS<sup>®</sup> system, click the entry for the associated jail in
the `Jails` page and add storage as described in `Additional Storage`.

Click and `Shell` for the plugin jail in the `Jails` page. This will
give access to the shell of the jail containing the application to
complete or test the configuration.

If a plugin jail fails to start, open the plugin jail shell from the
`Jail` page and type `tail /var/log/messages` to see if any errors were
logged.

Updating Plugins
----------------

When a newer version of a plugin or release becomes available in the
official repository, click and `UPDATE`. Updating a plugin updates the
operating system and version of the plugin.

<div id="updating_installed_plugin_fig">

![Updating a Plugin][]

</div>

  [Updating a Plugin]: images/plugins-update.png

Updating a plugin also restarts that plugin. To update or upgrade the
plugin jail operating system, see `Jail Updates and Upgrades`.

Uninstalling Plugins
--------------------

Installing a plugin creates an associated jail. Uninstalling a plugin
deletes the jail because it is no longer required. This means all
**datasets or snapshots that are associated with the plugin are also
deleted.** Make sure to back up any important data from the plugin
**before** uninstalling it.

`Figure %s <deleting_installed_plugin_fig>` shows an example of
uninstalling a plugin by expanding the plugin's entry and clicking
`UNINSTALL`. A two-step dialog opens to confirm the action. **This is
the only warning.** Enter the plugin name, set the `Confirm` checkbox,
and click `DELETE` to remove the plugin and the associated jail,
dataset, and snapshots.

<div id="deleting_installed_plugin_fig">

![Uninstalling a Plugin and its Associated Jail and Dataset][]

</div>

  [Uninstalling a Plugin and its Associated Jail and Dataset]: images/plugins-delete-example.png

Create a Plugin
---------------

If an application is not available as a plugin, it is possible to create
a new plugin for FreeNAS<sup>®</sup> in a few steps. This requires an
existing [GitHub][] account.

  [GitHub]: https://github.com

**Create a new artifact repository on** [GitHub][].

  [GitHub]: https://github.com

Refer to `table %s <plugin-artifact-files>` for the files to add to the
artifact repository.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.67linewidth-2tabcolsep}\|

</div>

<div id="plugin-artifact-files">

<table>
<caption>FreeNAS<sup>®</sup> Plugin Artifact Files</caption>
<colgroup>
<col style="width: 26%" />
<col style="width: 73%" />
</colgroup>
<thead>
<tr class="header">
<th>Directory/File</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code class="interpreted-text" role="file">post_install.sh</code></td>
<td>This script is run <em>inside</em> the jail after it is created and any packages installed. Enable services in <code class="interpreted-text" role="file">/etc/rc.conf</code> that need to start with the jail and apply any configuration customizations with this this script.</td>
</tr>
<tr class="even">
<td><code class="interpreted-text" role="file">ui.json</code></td>
<td><p>JSON file that accepts the key or value options. For example:</p>
<p><code class="interpreted-text" role="samp">adminportal: "http://%%IP%%/"</code></p>
<p>designates the web-interface of the plugin.</p></td>
</tr>
<tr class="odd">
<td><code class="interpreted-text" role="file">overlay/</code></td>
<td>Directory of files overlaid on the jail after install. For example, <code class="interpreted-text" role="file">usr/local/bin/myfile</code> is placed in the <code class="interpreted-text" role="file">/usr/local/bin/myfile</code> location of the jail. Can be used to supply custom files and configuration data, scripts, and any other type of customized files to the plugin jail.</td>
</tr>
<tr class="even">
<td><code class="interpreted-text" role="file">settings.json</code></td>
<td><p>JSON file that manages the settings interface of the plugin. Required fields include:</p>
<ul>
<li><code class="interpreted-text" role="samp">"servicerestart" : "service foo restart"</code></li>
</ul>
<p>Command to run when restarting the plugin service after changing settings.</p>
<ul>
<li><code class="interpreted-text" role="samp">"serviceget" : "/usr/local/bin/myget"</code></li>
</ul>
<p>Command used to get values for plugin configuration. Provided by the plugin creator. The command accepts two arguments for key or value pair.</p>
<ul>
<li><code class="interpreted-text" role="samp">"options" : { }</code></li>
</ul>
<p>This subsection contains arrays of elements, starting with the "key" name and required arguments for that particular type of setting.</p>
<p>See <code class="interpreted-text" role="ref">options subsection example &lt;plugin-json-options&gt;</code> below.</p></td>
</tr>
</tbody>
</table>

FreeNAS<sup>®</sup> Plugin Artifact Files

</div>

This example `settings.json` file is used for the `Quasselcore` plugin.
It is also available online in the [iocage-plugin-quassel artifact
repository][].

  [iocage-plugin-quassel artifact repository]: https://github.com/freenas/iocage-plugin-quassel/blob/master/settings.json

<div id="plugin-json-options">

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

</div>

**Create and submit a new JSON file for the plugin:**

Clone the [iocage-plugin-index][] GitHub repository.

  [iocage-plugin-index]: https://github.com/ix-plugin-hub/iocage-plugin-index

<div class="tip">

<div class="title">

Tip

</div>

Full tutorials and documentation for GitHub and `git` commands are
available on [GitHub Guides][].

</div>

  [GitHub Guides]: https://guides.github.com/

On the local copy of `iocage-plugin-index`, create a new JSON file for
the FreeNAS<sup>®</sup> plugin. The JSON file describes the plugin, the
packages it requires for operation, and other installation details. This
file is named `{pluginname}.json`. For example, the [Madsonic][] plugin
is named `madsonic.json`.

  [Madsonic]: https://github.com/ix-plugin-hub/iocage-plugin-index/blob/master/madsonic.json

The fields of the file are described in
`table %s <plugins-plugin-jsonfile-contents>`.

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.67linewidth-2tabcolsep}\|

</div>

<div id="plugins-plugin-jsonfile-contents">

<table>
<caption>Plugin JSON File Contents</caption>
<colgroup>
<col style="width: 27%" />
<col style="width: 72%" />
</colgroup>
<thead>
<tr class="header">
<th>Data Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>"name":</code></td>
<td>Name of the plugin.</td>
</tr>
<tr class="even">
<td><code>"plugin_schema":</code></td>
<td>Optional. Enter <em>2</em> if simplified post-install information has been supplied in <code class="interpreted-text" role="file">post_install.sh</code>. After specifying <em>2</em>, echo the information to be presented to the user in <code class="interpreted-text" role="file">/root/PLUGIN_INFO</code> inside the <code class="interpreted-text" role="file">post_install.sh</code> file. See the <code class="interpreted-text" role="ref">rslsync.json &lt;rslsync-plugin-schema&gt;</code> and <code class="interpreted-text" role="ref">rslsync post_install.sh &lt;rslsync-post-install&gt;</code> examples.</td>
</tr>
<tr class="odd">
<td><code>"release":</code></td>
<td>FreeBSD RELEASE to use for the plugin jail.</td>
</tr>
<tr class="even">
<td><code>"artifact":</code></td>
<td>URL of the plugin artifact repository.</td>
</tr>
<tr class="odd">
<td><code>"pkgs":</code></td>
<td>The FreeBSD packages required by the plugin.</td>
</tr>
<tr class="even">
<td><code>"packagesite":</code></td>
<td>Content Delivery Network (CDN) used by the plugin jail. Default for the TrueOS CDN is <code>http://pkg.cdn.trueos.org/iocage</code>.</td>
</tr>
<tr class="odd">
<td><code>"fingerprints":</code></td>
<td><p><code>"function":</code></p>
<p>Default is <code>sha256</code>.</p>
<p><code>"fingerprint":</code></p>
<p>The pkg fingerprint for the artifact repository. Default is <code>226efd3a126fb86e71d60a37353d17f57af816d1c7ecad0623c21f0bf73eb0c7</code></p></td>
</tr>
<tr class="even">
<td><code>"official":</code></td>
<td>Define whether this is an official iXsystems-supported plugin. Enter <code>true</code> or <code>false</code>.</td>
</tr>
</tbody>
</table>

Plugin JSON File Contents

</div>

<div id="rslsync-plugin-schema">

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

</div>

<div id="rslsync-post-install">

    #!/bin/sh -x

    # Enable the service
    sysrc -f /etc/rc.conf rslsync_enable="YES"
    # Start the service
    service rslsync start 2>/dev/null

    echo "rslsync now installed" > /root/PLUGIN_INFO
    echo "foo" >> /root/PLUGIN_INFO

</div>

Here is `quasselcore.json` reproduced as an example:

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
used for the `"pkgs":` value. Find the package name and directory by
searching [FreshPorts][] and checking the "To install the port:" line.
For example, the *Quasselcore* plugin uses the directory and package
name `/irc/quassel-core`.

  [FreshPorts]: https://www.freshports.org/

Now edit `iocage-plugin-index/INDEX`. Add an entry for the new plugin
that includes these fields:

-   `"MANIFEST":` Add the name of the newly created `plugin.json` file
    here.
-   `"name":` Use the same name used within the `.json` file.
-   `"icon":` Most plugins will have a specific icon. Search the web and
    save the icon to the `iocage-plugin-index/icons/` directory as a
    `.png`. The naming convention is `pluginname.png`. For example, the
    `Madsonic` plugin has the icon file `madsonic.png`.
-   `"description":` Describe the plugin in a single sentence.
-   `"official":` Specify if the plugin is supported by iXsystems. Enter
    `false`.

See the [INDEX][] for examples of `INDEX` entries.

  [INDEX]: https://github.com/ix-plugin-hub/iocage-plugin-index/blob/master/INDEX

**Submit the plugin**

Open a pull request for the [iocage-plugin-index repo][]. Make sure the
pull request contains:

  [iocage-plugin-index repo]: https://github.com/ix-plugin-hub/iocage-plugin-index

-   the new `plugin.json` file.
-   the plugin icon `.png` added to the `iocage-plugin-index/icons/`
    directory.
-   an update to the `INDEX` file with an entry for the new plugin.
-   a link to the artifact repository populated with all required plugin
    files.

### Test a Plugin

<div class="warning">

<div class="title">

Warning

</div>

Installing experimental plugins is not recommended for general use of
FreeNAS<sup>®</sup>. This feature is meant to help plugin creators test
their work before it becomes generally available on FreeNAS<sup>®</sup>.

</div>

Plugin pull requests are merged into the `master` branch of the
[iocage-plugin-index][] repository. These plugins are not available in
the until they are tested and added to a *RELEASE* branch of the
repository. It is possible to test an in-development plugin by using
this `iocage` command:
`iocage fetch -P --name {PLUGIN} {IPADDRESS_PROPS} --branch 'master'`

  [iocage-plugin-index]: https://github.com/ix-plugin-hub/iocage-plugin-index

This will install the plugin, configure it with any chosen properties,
and specifically use the `master` branch of the repository to download
the plugin.

Here is an example of downloading and configuring an experimental plugin
with the FreeNAS<sup>®</sup> `Shell`:

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

This plugin appears in the `Jails` and `Plugins` screens as `mineos` and
can be tested with the FreeNAS<sup>®</sup> system.

<div class="index">

Asigra Plugin

</div>

Asigra Plugin
-------------

The Asigra plugin connects FreeNAS<sup>®</sup> to a third party service
and is subject to licensing. Please read the [Asigra Software License
Agreement][] before using this plugin.

  [Asigra Software License Agreement]: https://www.asigra.com/legal/software-license-agreement

To begin using Asigra services after installing the plugin, open the
plugin options and click `Register`. A new browser tab opens to
[register a user with Asigra][].

  [register a user with Asigra]: https://licenseportal.asigra.com/licenseportal/user-registration.do

The FreeNAS<sup>®</sup> system must have a public static IP address for
Asigra services to function.

Refer to the Asigra documentation for details about using the Asigra
platform:

-   [DS-Operator Management Guide][]: Using the `DS-Operator` interface
    to manage the plugin `DS-System` service. Click `Management` in the
    plugin options to open the `DS-Operator` interface.
-   [DS-Client Installation Guide][]: How to install the `DS-Client`
    system. `DS-Client` aggregates backup content from endpoints and
    transmits it to the `DS-System service`.
-   [DS-Client Management Guide][]: Managing the `DS-Client` system
    after it has been successfully installed at one or more locations.

  [DS-Operator Management Guide]: https://s3.amazonaws.com/asigra-documentation/Help/v14.1/DS-System%20Help/index.html
  [DS-Client Installation Guide]: https://s3.amazonaws.com/asigra-documentation/Guides/Cloud%20Backup/v14.1/Client_Software_Installation_Guide.pdf
  [DS-Client Management Guide]: https://s3.amazonaws.com/asigra-documentation/Help/v14.1/DS-Client%20Help/index.html
