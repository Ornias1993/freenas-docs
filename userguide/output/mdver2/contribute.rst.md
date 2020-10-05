Contributing to FreeNAS<sup>®</sup>
===================================

FreeNAS<sup>®</sup> is an open source community, relying on the input
and expertise of users to grow and improve. When users take time to
assist the community, their contributions benefit everyone.

This section describes how to participate and contribute to
FreeNAS<sup>®</sup>. It is by no means an exhaustive list. If you have
an idea that will benefit the community, bring it up on one of the
resources mentioned in `Support Resources`.

This section demonstrates how to:

-   `Help with Translation <Translation>`

<div class="index">

Translation, Translate, Localize

</div>

Translation
-----------

FreeNAS<sup>®</sup> is developed and documented in English. Having
complete translations of the user interface into other languages helps
make FreeNAS<sup>®</sup> much more useful to communities around the
world.

FreeNAS<sup>®</sup> uses `.po` files stored in the [webui GitHub
repository][] to manage the translation of text shown in the
FreeNAS<sup>®</sup> graphical administrative interface. GitHub provides
an easy to use web-based editor, making it possible for individuals to
assist with translation or comment on existing translations.

  [webui GitHub repository]: https://github.com/freenas/webui/tree/master/src/assets/i18n

To view translation files, open the `/src/assets/i18n` directory of the
FreeNAS<sup>®</sup> [webui repository][], as shown in
`Figure %s <contribute_po_fig>`.

  [webui repository]: https://github.com/freenas/webui/tree/master/src/assets/i18n

<div id="contribute_po_fig">

![FreeNAS<sup>®</sup> Translation Files][]

</div>

  [FreeNAS<sup>®</sup> Translation Files]: images/external/contribute-po.png

To assist with translating FreeNAS<sup>®</sup>, first create an account
with [GitHub][] and `Fork` the [freenas/webui][] repository.

  [GitHub]: https://github.com/
  [freenas/webui]: https://github.com/freenas/webui

There are two methods for committing translations:

1.  Use the GitHub website to edit the `.po` files.

OR

1.  Make a local copy of the forked repository and use a text editor for
    translations.

### Translate with GitHub

Open a browser and go to your GitHub profile. Select the `Repositories`
tab and open your fork of the `freenas/webui` repository. Click
`src --> assets --> i18n` to open the translations directory. Click on
the desired language `.po` file to begin translating.

<div class="tip">

<div class="title">

Tip

</div>

Here is a list of [common language abbreviations][]

</div>

  [common language abbreviations]: https://www.abbreviations.com/acronyms/LANGUAGES2L

Click the `Pencil` icon in the upper right area to open the online file
editor. `Figure %s <contribute_github_editor_fig>` shows the page that
appears:

<div id="contribute_github_editor_fig">

![GitHub Online Editor][]

</div>

  [GitHub Online Editor]: images/external/contribute-github-editor.png

There are numerous `msgid ""` and `msgstr ""` entries in the file. Read
the `msgid` text and enter the translation between the `msgstr` quotes.

Scroll to the bottom of the page when finished entering translations.
Enter a descriptive title and summary of changes for the edits and click
`Commit changes`.

### Download and Translate Offline

[Install Git][]. There are numerous examples in these instructions of
using `git`, but full documentation for `git` is [available online][].

  [Install Git]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
  [available online]: https://git-scm.com/doc

These instructions show using the Command Line Interface (CLI) with
`git`, but many graphical utilities are available.

Create a suitable directory to store the local copy of the forked
repository. Download the repository with `git clone`:

`% git clone https://github.com/ghuser/webui.git`

The download can take several minutes, depending on connection speed.

Use `cd` to go to the `i18n` directory:

`% cd src/assets/i18n/`

Use a `po` editor to add translations to the desired language file. Any
capable editor will work, but [poedit][] and [gtranslator][] are two
common options.

  [poedit]: https://poedit.net/
  [gtranslator]: https://wiki.gnome.org/Apps/Gtranslator

Commit any file changes with `git commit`:

`% git commit ar.po`

Enter a descriptive message about the changes and save the commit.

When finished making commits to the branch, use `git push` to send your
changes to the online fork repository.

### Translation Pull Requests

When ready to merge translations in the original `freenas/webui`
repository, open a web browser and go to your forked repository on
GitHub. Select the `Code` tab and click `New pull request`. Set the
`base repository` drop-down to `freenas/webui` and the `head repository`
to your fork. Click `Create pull request`, write a title and summary of
your proposed changes, and click `Create pull request` again to submit
your translations to the `freenas/webui` repository.

The FreeNAS<sup>®</sup> project automatically tests pull requests for
compatibility. If there any issues with a pull request, either the
automated system will update the request or a FreeNAS<sup>®</sup> team
member will leave a message in the comment section of the request.

All assistance with translations helps to benefit the
FreeNAS<sup>®</sup> community. Thank you!
