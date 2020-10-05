.. index:: Settings
.. _Settings:

Settings
========

The |ui-settings| menu provides options to change the administrator
password, set preferences, and view system information.


.. _Change Password:

Change Password
---------------

To change the :literal:`root` account password, click
|ui-settings| and :guilabel:`Change Password`. The current
:literal:`root` password must be entered before a new password
can be saved.


.. _Preferences:

Preferences
-----------

The FreeNAS\ :sup:`®` User Interface can be adjusted to match the user
preferences. Go to the *Web Interface Preferences* page by
clicking the |ui-settings| menu in the upper-right and clicking
:guilabel:`Preferences`.


.. index:: Web Interface Preferences
.. _Web Interface Preferences:

Web Interface Preferences
~~~~~~~~~~~~~~~~~~~~~~~~~

This page has options to adjust global settings in the |web-ui|, manage
custom themes, and create new themes.
:numref:`Figure %s <ui_preferences_fig>` shows the different options:

.. _ui_preferences_fig:

.. figure:: images/settings-preferences.png

   Web Interface Preferences


These options are applied to the entire |web-ui|:

* :guilabel:`Choose Theme`: Change the active theme. Custom themes are
  added to this list.

* :guilabel:`Prefer buttons with icons only`: Set to preserve screen
  space and only display icons and tooltips instead of text labels.

* :guilabel:`Enable Password Toggle`: When set, an *eye* icon appears
  next to password fields. Clicking the icon reveals the password.
  Clicking it again hides the password.

* :guilabel:`Reset Table Columns to Default`: Set to reset all tables to display
  default columns.

Make any changes and click :guilabel:`UPDATE SETTINGS` to save the new
selections.


.. _Themes:

Themes
~~~~~~

The FreeNAS\ :sup:`®` |web-ui| supports dynamically changing the active theme and
creating new, fully customizable themes.


.. index:: Create New Themes
.. _Create New Themes:

Create New Themes
^^^^^^^^^^^^^^^^^

This page is used to create and preview custom FreeNAS\ :sup:`®` themes.
:numref:`Figure %s <theme_custom_fig>` shows many of the theming and
preview options:

.. _theme_custom_fig:

.. figure:: images/settings-preferences-create-custom-theme.png

   Create and Preview a Custom Theme


To create a new custom theme, click :guilabel:`CREATE NEW THEME`.
Colors from an existing theme can be used when creating a new
custom theme. Select a theme from the
:guilabel:`Load Colors from Theme` drop-down to use the colors from
that theme for the new custom theme.
:numref:`Table %s <custom_theme__general_options_table>` describes each
option:

.. tabularcolumns:: |>{\RaggedRight}p{\dimexpr 0.20\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.11\linewidth-2\tabcolsep}
                    |>{\RaggedRight}p{\dimexpr 0.68\linewidth-2\tabcolsep}|

.. _custom_theme__general_options_table:

.. table:: General Options for a New Theme
   :class: longtable

   +-------------------+--------------+------------------------------------------------------------------------------------------+
   | Setting           | Value        | Description                                                                              |
   |                   |              |                                                                                          |
   +===================+==============+==========================================================================================+
   | Custom Theme Name | string       | Enter a name to identify the new theme.                                                  |
   |                   |              |                                                                                          |
   +-------------------+--------------+------------------------------------------------------------------------------------------+
   | Menu Label        | string       | Enter a short name to use for the FreeNAS\ :sup:`®` menus.                               |
   |                   |              |                                                                                          |
   +-------------------+--------------+------------------------------------------------------------------------------------------+
   | Menu Swatch       | drop-down    | Choose a color from the theme to display next to the menu entry of the custom theme.     |
   |                   | menu         |                                                                                          |
   +-------------------+--------------+------------------------------------------------------------------------------------------+
   | Description       | string       | Enter a short description of the new theme.                                              |
   |                   |              |                                                                                          |
   +-------------------+--------------+------------------------------------------------------------------------------------------+
   | Enable Dark Logo  | checkbox     | Set this to give the FreeNAS Logo a dark fill color.                                     |
   |                   |              |                                                                                          |
   +-------------------+--------------+------------------------------------------------------------------------------------------+
   | Choose Primary    | drop-down    | Choose from either a generic color or import a specific color setting to use as the      |
   |                   | menu         | primary theme color. The primary color changes the top bar of the |web-ui|               |
   |                   |              | and the color of many of the buttons.                                                    |
   |                   |              |                                                                                          |
   +-------------------+--------------+------------------------------------------------------------------------------------------+
   | Choose Accent     | drop-down    | Choose from either a generic color or import a specific color setting to use as the      |
   |                   | menu         | accent color for the theme. This color is used for many of the buttons and smaller       |
   |                   |              | elements in the |web-ui|.                                                                |
   |                   |              |                                                                                          |
   +-------------------+--------------+------------------------------------------------------------------------------------------+


Choose the different :guilabel:`COLORS` for this new theme after setting
these general options. Click the color swatch to open a small popup with
sliders to adjust the color. Color values can also be entered as a
hexadecimal value.

Changing any color value automatically updates the
:guilabel:`Theme Preview` column. This section is completely interactive
and shows how the custom theme is applied to all the different elements
in the |web-ui|.

Click :guilabel:`SAVE CUSTOM THEME` when finished with all the
:guilabel:`GENERAL` and :guilabel:`COLORS` options. The new theme is
added to the list of available themes in
:guilabel:`Web Interface Preferences`.

Click
:menuselection:`PREVIEW --> Global Preview`
to apply the unsaved custom theme to the current session of the
FreeNAS\ :sup:`®` |web-ui|. Activating :guilabel:`Global Preview` allows going
to other pages in the |web-ui| and live testing the new custom theme.

.. note:: Setting a custom theme as a :guilabel:`Global Preview` does
   **not** save that theme! Be sure to go back to
   :menuselection:`Preferences --> Create Custom Theme`
   , complete any remaining options, and click
   :guilabel:`SAVE CUSTOM THEME` to save the current settings as a new
   theme.


.. _API:

API Documentation
-----------------

Click :guilabel:`API` to see documentation for the
`websocket protocol API <https://en.wikipedia.org/wiki/WebSocket>`__
used in FreeNAS\ :sup:`®`.


.. _About:

About
-----

Click |ui-settings| and :guilabel:`About` to view a popup window with
basic system information. This includes system :guilabel:`Version`,
:guilabel:`Hostname`, :guilabel:`Uptime`, :guilabel:`IP` address,
:guilabel:`Physical Memory`, CPU :guilabel:`Model`, and
:guilabel:`Average Load`.

