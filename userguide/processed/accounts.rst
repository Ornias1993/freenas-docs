.. _Accounts:

Accounts
========

:menuselection:`Accounts`
is used to manage users and groups. This section contains these entries:

* :ref:`Groups`: used to manage UNIX-style groups on the FreeNAS\ :sup:`®`
  system.

* :ref:`Users`: used to manage UNIX-style accounts on the FreeNAS\ :sup:`®`
  system.

Each entry is described in more detail in this section.


.. index:: Groups
.. _Groups:

Groups
------

The Groups interface provides management of UNIX-style groups on the
FreeNAS\ :sup:`®` system.

.. note:: It is unnecessary to recreate the network users or groups
   when a directory service is running on the same network. Instead,
   import the existing account information into FreeNAS\ :sup:`®`. Refer to
   :ref:`Directory Services` for details.

This section describes how to create a group and assign user
accounts to it. The :guilabel:`Groups` page lists all groups,
including those built in and used by the operating system.

.. _group_man_fig:

.. figure:: images/accounts-groups.png

   Group Management

The table displays group names, group IDs (GID), built-in groups, and
whether :command:`sudo` is permitted. Clicking the |ui-options| icon on
a user-created group entry displays :guilabel:`Members`,
:guilabel:`Edit`, and :guilabel:`Delete` options. Click
:guilabel:`Members` to view and modify the group membership. Built-in
groups are required by the FreeNAS\ :sup:`®` system and cannot be edited or
deleted.


.. index:: Add Group, New Group, Create Group

The |ui-add| button opens the screen shown in
:numref:`Figure %s <new_group_fig>`.
:numref:`Table %s <new_group_tab>`
summarizes the available options when creating a group.


.. _new_group_fig:

.. figure:: images/accounts-groups-add.png

   Creating a New Group


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.25\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.12\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.63\linewidth-2\tabcolsep}|

.. _new_group_tab:

.. table:: Group Creation Options
   :class: longtable

   +---------------------+-----------+--------------------------------------------------------------------------------------------------------------------------+
   | Setting             | Value     | Description                                                                                                              |
   |                     |           |                                                                                                                          |
   |                     |           |                                                                                                                          |
   +=====================+===========+==========================================================================================================================+
   | GID                 | string    | The next available group ID is suggested. By convention, UNIX groups containing user accounts have an ID greater than    |
   |                     |           | 1000 and groups required by a service have an ID equal to the default port number used by the service. Example:          |
   |                     |           | the :literal:`sshd` group has an ID of 22. This setting cannot be edited once the group is created.                      |
   +---------------------+-----------+--------------------------------------------------------------------------------------------------------------------------+
   | Name                | string    | Enter an alphanumeric name for the new group. Group names cannot begin with a hyphen (:literal:`-`) or contain           |
   |                     |           | a space, tab, or these characters: *, : + & # % ^ ( ) ! @ ~ * ? < > =* . *$* can only be used as the last character of   |
   |                     |           | the group name.                                                                                                          |
   +---------------------+-----------+--------------------------------------------------------------------------------------------------------------------------+
   | Permit Sudo         | checkbox  | Set to allow group members to use `sudo <https://www.sudo.ws/>`__. When using :command:`sudo`, a user is                 |
   |                     |           | prompted for their own password.                                                                                         |
   +---------------------+-----------+--------------------------------------------------------------------------------------------------------------------------+
   | Allow Duplicate GIDs| checkbox  | **Not recommended**. Allow more than one group to have the same group ID.                                                |
   +---------------------+-----------+--------------------------------------------------------------------------------------------------------------------------+


To change which users are members of a group, expand the group from the
list and click :guilabel:`Members`. To add users to the group, select
users in the left frame and click :guilabel:`->`. To remove users from
the group, select users in the right frame and click :guilabel:`<-`.
Click :guilabel:`SAVE` when finished changing the group members.

:numref:`Figure %s <user_group_fig>`,
shows adding a user as a member of a group.


.. _user_group_fig:

.. figure:: images/accounts-users-member-example.png

   Assigning a User to a Group


.. index:: Delete Group, Remove Group

The :guilabel:`Delete` button deletes a group. The pop-up message asks
if all users with this primary group should also be deleted, and to
confirm the action. Note built-in groups do not have a
:guilabel:`Delete` button.


.. index:: Users
.. _Users:

Users
-----

FreeNAS\ :sup:`®` supports users, groups, and permissions, allowing
flexibility in configuring which users have access to the data stored
on FreeNAS\ :sup:`®`. To assign permissions to shares,
select one of these options:

#.  Create a guest account for all users, or create a user
    account for every user in the network where the name of each
    account is the same as a login name used on a computer. For
    example, if a Windows system has a login name of *bobsmith*,
    create a user account with the name *bobsmith* on FreeNAS\ :sup:`®`.
    A common strategy is to create groups with different sets of
    permissions on shares, then assign users to those groups.

#.  If the network uses a directory service, import the existing
    account information using the instructions in
    :ref:`Directory Services`.

:menuselection:`Accounts --> Users` lists all system
accounts installed with the FreeNAS\ :sup:`®` operating system, as shown in
:numref:`Figure %s <managing_user_fig>`.


.. _managing_user_fig:

.. figure:: images/accounts-users.png

   Managing User Accounts


By default, each user entry displays the username, User ID (UID), whether the
user is built into FreeNAS\ :sup:`®`, and full name. This table
is adjustable by clicking :guilabel:`COLUMNS` and setting the desired
columns.

Clicking a column name sorts the list by that value. An arrow
indicates which column controls the view sort order. Click the arrow to
reverse the sort order.

Click |ui-options| on the user created account to display
the :guilabel:`Edit` and :guilabel:`Delete` buttons. Note built-in users
do not have a :guilabel:`Delete` button.

.. note:: Setting the email address for the built-in
   *root* user account is recommended as important system messages
   are sent to the *root* user. For security reasons, password logins
   are disabled for the *root* account and changing this setting is
   highly discouraged.


Except for the *root* user, the accounts that come with FreeNAS\ :sup:`®`
are system accounts. Each system account is used by a service and
should not be used as a login account. For this reason, the default
shell on system accounts is
`nologin(8) <https://www.freebsd.org/cgi/man.cgi?query=nologin>`__.
For security reasons and to prevent breakage of system services,
modifying the system accounts is discouraged.

.. index:: Add User, Create User, New User

The |ui-add| button opens the screen shown in
:numref:`Figure %s <add_user_fig>`.
:numref:`Table %s <user_account_conf_tab>`
summarizes the options that are available when user accounts are
created or modified.

.. warning:: When using :ref:`Active Directory`, Windows user
   passwords must be set from within Windows.


.. _add_user_fig:

.. figure:: images/accounts-users-add.png

   Adding or Editing a User Account


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.25\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.55\linewidth-2\tabcolsep}|

.. _user_account_conf_tab:

.. table:: User Account Configuration
   :class: longtable

   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Setting                    | Value           | Description                                                                                                                   |
   |                            |                 |                                                                                                                               |
   +============================+=================+===============================================================================================================================+
   | Username                   | string          | Usernames can be up to 16 characters long. When using NIS or other legacy software with limited username lengths, keep        |
   |                            |                 | usernames to eight characters or less for compatibility. Usernames cannot begin with a hyphen (:literal:`-`) or contain       |
   |                            |                 | a space, tab, or these characters: *, : + & # % ^ ( ) ! @ ~ * ? < > =* . *$* can only be used as the last character of        |
   |                            |                 | the username.                                                                                                                 |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Full Name                  | string          | This field is mandatory and may contain spaces.                                                                               |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Email                      | string          | The email address associated with the account.                                                                                |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Password                   | string          | Mandatory unless :guilabel:`Disable Password` is *Yes*. Cannot contain a :literal:`?`.                                        |
   |                            |                 | Click |ui-password-show| to view or obscure the password characters.                                                          |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Confirm Password           | string          | Required to match the value of :guilabel:`Password`.                                                                          |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | User ID                    | integer         | Grayed out if the user already exists. When creating an account, the next numeric ID is suggested. By convention, user        |
   |                            |                 | accounts have an ID greater than 1000 and system accounts have an ID equal to the default port number used by the service.    |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | New Primary Group          | checkbox        | Set by default to create a new a primary group with the same name as the user. Unset to select a different                    |
   |                            |                 | primary group name.                                                                                                           |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Primary Group              | drop-down menu  | Unset :guilabel:`New Primary Group` to access this menu. For security reasons, FreeBSD will not give a user                   |
   |                            |                 | :command:`su` permissions if *wheel* is not their primary group. To give a user :command:`su` access, add them to the         |
   |                            |                 | *wheel* group in :guilabel:`Auxiliary groups`.                                                                                |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Auxiliary groups           | drop-down menu  | Select which groups the user will be added to.                                                                                |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Home Directory             | browse button   | Choose a path to the user's home directory. If the directory exists and matches the username, it is set as the user's         |
   |                            |                 | home directory. When the path does not end with a subdirectory matching the username, a new subdirectory is created. The      |
   |                            |                 | full path to the user's home directory is shown here when editing a user.                                                     |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Home Directory Permissions | checkboxes      | Sets default Unix permissions of user's home directory. This is **read-only** for built-in users.                             |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | SSH Public Key             | string          | Paste the user's **public** SSH key to be used for key-based authentication.                                                  |
   |                            |                 | **Do not paste the private key!**                                                                                             |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Disable Password           | drop-down       | *Yes* : Disables the :guilabel:`Password` fields and removes the password from the account. The account cannot use            |
   |                            |                 | password-based logins for services. For example, disabling the password prevents using account credentials to log in to an    |
   |                            |                 | SMB share or open an SSH session on the system. The :guilabel:`Lock User` and :guilabel:`Permit Sudo` options are also        |
   |                            |                 | removed.                                                                                                                      |
   |                            |                 |                                                                                                                               |
   |                            |                 | *No* : Requires adding a :guilabel:`Password` to the account. The account can use the saved :guilabel:`Password` to           |
   |                            |                 | authenticate with password-based services.                                                                                    |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Shell                      | drop-down menu  | Select the shell to use for local and SSH logins. The *root* user shell is used for |web-ui| :ref:`Shell` sessions. See       |
   |                            |                 | :numref:`Table %s <shells_tab>` for an overview of available shells.                                                          |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Lock User                  | checkbox        | Prevent the user from logging in or using password-based services until this option is unset. Locking an account is only      |
   |                            |                 | possible when :guilabel:`Disable Password` is *No* and a :guilabel:`Password` has been created for the account.               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Permit Sudo                | checkbox        | Give this user permission to use `sudo <https://www.sudo.ws/>`__. When using sudo, a user is prompted for their account       |
   |                            |                 | :guilabel:`Password`.                                                                                                         |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Microsoft Account          | checkbox        | Set if the user is connecting from a Windows 8 or newer system or when using a Microsoft cloud service.                       |
   |                            |                 |                                                                                                                               |
   +----------------------------+-----------------+-------------------------------------------------------------------------------------------------------------------------------+


.. note:: Some fields cannot be changed for built-in users and are
   grayed out.


.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.16\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.66\linewidth-2\tabcolsep}|

.. _shells_tab:

.. table:: Available Shells
   :class: longtable

   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | Shell        | Description                                                                                                          |
   |              |                                                                                                                      |
   +==============+======================================================================================================================+
   | csh          | `C shell <https://en.wikipedia.org/wiki/C_shell>`__                                                                  |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | sh           | `Bourne shell <https://en.wikipedia.org/wiki/Bourne_shell>`__                                                        |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | tcsh         | `Enhanced C shell <https://en.wikipedia.org/wiki/Tcsh>`__                                                            |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | bash         | `Bourne Again shell <https://en.wikipedia.org/wiki/Bash_%28Unix_shell%29>`__                                         |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | ksh93        | `Korn shell <http://www.kornshell.com/>`__                                                                           |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | mksh         | `mirBSD Korn shell <https://www.mirbsd.org/mksh.htm>`__                                                              |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | rbash        | `Restricted bash <http://www.gnu.org/software/bash/manual/html_node/The-Restricted-Shell.html>`__                    |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | rzsh         | `Restricted zsh <http://www.csse.uwa.edu.au/programming/linux/zsh-doc/zsh_14.html>`__                                |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | scponly      | Select `scponly <https://github.com/scponly/scponly/wiki>`__ to restrict the user's SSH usage to only the            |
   |              | :command:`scp` and :command:`sftp` commands.                                                                         |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | zsh          | `Z shell <http://www.zsh.org/>`__                                                                                    |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | git-shell    | `restricted git shell <https://git-scm.com/docs/git-shell>`__                                                        |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+
   | nologin      | Use when creating a system account or to create a user account that can authenticate with shares but which cannot    |
   |              | login to the FreeNAS system using :command:`ssh`.                                                                    |
   |              |                                                                                                                      |
   +--------------+----------------------------------------------------------------------------------------------------------------------+


.. index:: Remove User, Delete User

Built-in user accounts needed by the system cannot be removed. A
:guilabel:`Delete` button appears for custom users that were added
by the system administrator. Clicking :guilabel:`Delete` opens a popup
window to confirm the action and offer an option to keep the
user primary group when the user is deleted.
