<div class="index">

Settings

</div>

Settings
========

The menu provides options to change the administrator password, set
preferences, and view system information.

Change Password
---------------

To change the `root` account password, click and `Change Password`. The
current `root` password must be entered before a new password can be
saved.

Preferences
-----------

The FreeNAS<sup>®</sup> User Interface can be adjusted to match the user
preferences. Go to the *Web Interface Preferences* page by clicking the
menu in the upper-right and clicking `Preferences`.

<div class="index">

Web Interface Preferences

</div>

### Web Interface Preferences

This page has options to adjust global settings in the , manage custom
themes, and create new themes. `Figure %s <ui_preferences_fig>` shows
the different options:

<div id="ui_preferences_fig">

![Web Interface Preferences][]

</div>

  [Web Interface Preferences]: images/settings-preferences.png

These options are applied to the entire :

-   `Choose Theme`: Change the active theme. Custom themes are added to
    this list.
-   `Prefer buttons with icons only`: Set to preserve screen space and
    only display icons and tooltips instead of text labels.
-   `Enable Password Toggle`: When set, an *eye* icon appears next to
    password fields. Clicking the icon reveals the password. Clicking it
    again hides the password.
-   `Reset Table Columns to Default`: Set to reset all tables to display
    default columns.

Make any changes and click `UPDATE SETTINGS` to save the new selections.

### Themes

The FreeNAS<sup>®</sup> supports dynamically changing the active theme
and creating new, fully customizable themes.

<div class="index">

Create New Themes

</div>

#### Create New Themes

This page is used to create and preview custom FreeNAS<sup>®</sup>
themes. `Figure %s <theme_custom_fig>` shows many of the theming and
preview options:

<div id="theme_custom_fig">

![Create and Preview a Custom Theme][]

</div>

  [Create and Preview a Custom Theme]: images/settings-preferences-create-custom-theme.png

To create a new custom theme, click `CREATE NEW THEME`. Colors from an
existing theme can be used when creating a new custom theme. Select a
theme from the `Load Colors from Theme` drop-down to use the colors from
that theme for the new custom theme.
`Table %s <custom_theme__general_options_table>` describes each option:

<div class="tabularcolumns">

&gt;{RaggedRight}p{dimexpr 0.11linewidth-2tabcolsep}

</div>

<div id="custom_theme__general_options_table">

| Setting           | Value          | Description                                                                                                                                                                                 |
|-------------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Custom Theme Name | string         | Enter a name to identify the new theme.                                                                                                                                                     |
| Menu Label        | string         | Enter a short name to use for the FreeNAS<sup>®</sup> menus.                                                                                                                                |
| Menu Swatch       | drop-down menu | Choose a color from the theme to display next to the menu entry of the custom theme.                                                                                                        |
| Description       | string         | Enter a short description of the new theme.                                                                                                                                                 |
| Enable Dark Logo  | checkbox       | Set this to give the FreeNAS Logo a dark fill color.                                                                                                                                        |
| Choose Primary    | drop-down menu | Choose from either a generic color or import a specific color setting to use as the primary theme color. The primary color changes the top bar of the and the color of many of the buttons. |
| Choose Accent     | drop-down menu | Choose from either a generic color or import a specific color setting to use as the accent color for the theme. This color is used for many of the buttons and smaller elements in the .    |

General Options for a New Theme

</div>

Choose the different `COLORS` for this new theme after setting these
general options. Click the color swatch to open a small popup with
sliders to adjust the color. Color values can also be entered as a
hexadecimal value.

Changing any color value automatically updates the `Theme Preview`
column. This section is completely interactive and shows how the custom
theme is applied to all the different elements in the .

Click `SAVE CUSTOM THEME` when finished with all the `GENERAL` and
`COLORS` options. The new theme is added to the list of available themes
in `Web Interface Preferences`.

Click `PREVIEW --> Global Preview` to apply the unsaved custom theme to
the current session of the FreeNAS<sup>®</sup> . Activating
`Global Preview` allows going to other pages in the and live testing the
new custom theme.

<div class="note">

<div class="title">

Note

</div>

Setting a custom theme as a `Global Preview` does **not** save that
theme! Be sure to go back to `Preferences --> Create Custom Theme` ,
complete any remaining options, and click `SAVE CUSTOM THEME` to save
the current settings as a new theme.

</div>

API Documentation
-----------------

Click `API` to see documentation for the [websocket protocol API][] used
in FreeNAS<sup>®</sup>.

  [websocket protocol API]: https://en.wikipedia.org/wiki/WebSocket

About
-----

Click and `About` to view a popup window with basic system information.
This includes system `Version`, `Hostname`, `Uptime`, `IP` address,
`Physical Memory`, CPU `Model`, and `Average Load`.
