# Usage

```bash
dash-qt [command-line options]     
```

Dash Core QT GUI, use same command line options as dashd with additional options for UI as described below.

## Wallet Options:

```text
  -windowtitle=<name>
       Sets a window title which is appended to "Dash Core - "
```

## UI Options:

```text
  -choosedatadir
       Choose data directory on startup (default: 0)

  -custom-css-dir
       Set a directory which contains custom css files. Those will be used as
       stylesheets for the UI.

  -font-family
       Set the font family. Possible values: SystemDefault, Montserrat.
       (default: SystemDefault)

  -font-scale
       Set a scale factor which gets applied to the base font size. Possible
       range -100 (smallest fonts) to 100 (largest fonts). (default: 0)

  -font-weight-bold
       Set the font weight for bold texts. Possible range 0 to 8 (default: 4)

  -font-weight-normal
       Set the font weight for normal texts. Possible range 0 to 8 (default: 2)

  -lang=<lang>
       Set language, for example "de_DE" (default: system locale)

  -min
       Start minimized

  -rootcertificates=<file>
       Set SSL root certificates for payment request (default: -system-)

  -splash
       Show splash screen on startup (default: 1)

  -resetguisettings
       Reset all settings changed in the GUI
```