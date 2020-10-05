<div class="index">

Log Out, Restart, or Shut Down

</div>

Log Out, Restart, or Shut Down
==============================

The button is used to log out of the web interface or restart or shut
down the FreeNAS<sup>®</sup> system.

<div class="index">

Log Out

</div>

Log Out
-------

To log out, click , then `Log Out`. After logging out, the login screen
is displayed.

<div class="index">

Restart

</div>

Restart
-------

To restart the system, click , then `Restart`. A confirmation screen
asks for verification of the restart. `Figure %s <restart_warning_fig>`.
Click `Confirm` to confirm the action, then click `RESTART` to restart
the system.

<div id="restart_warning_fig">

![Restart Warning Message](images/power-restart.png)

</div>

An additional warning message appears when a restart is attempted when a
scrub or resilver is in progress. When that warning appears, the
recommended steps are to `CANCEL` the restart request and to
periodically run `zpool status` from `Shell` until it shows that the
scrub or verify has completed. Then the restart request can be entered
again.

To complete the restart request, click the `Confirm` checkbox and then
the `RESTART` button. Restarting the system disconnects all clients,
including the web administration interface. Wait a few minutes for the
system to boot, then use the back button in the browser to return to the
IP address of the FreeNAS<sup>®</sup> system. The login screen appears
after a successful reboot. If the login screen does not appear, using a
monitor and keyboard to physically access the FreeNAS<sup>®</sup> system
is required to determine the issue preventing the system from resuming
normal operation.

<div class="index">

Shutdown

</div>

Shut Down
---------

Click and `Shut Down` to shut down the system. The warning message shown
in `Figure %s <shutdown_warning_fig>` is displayed.

<div id="shutdown_warning_fig">

![Shut Down Warning Message](images/power-shut-down.png)

</div>

Click `Confirm` and then `SHUT DOWN` to shut down the system. Shutting
down the system disconnects all clients, including the . Physical access
to the FreeNAS<sup>®</sup> system is required to turn it back on.
