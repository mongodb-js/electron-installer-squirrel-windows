# electron-installer-squirrel-windows [![][appveyor_img]][appveyor_url]

Generate Windows installers for [Electron][electron] apps using [Squirrel][squirrel].

## Todo

- Pull `AppModel` into it's own module

## Installation

```
# For use in npm scripts
npm i electron-installer-squirrel-windows --save-dev

# For use from cli
npm i electron-installer-squirrel-windows -g
```

## Usage

```
Usage: electron-installer-squirrel-windows <path/to/.app>

Generate Windows installers for Electron apps.

Usage:
  electron-installer-squirrel-windows ./dist/FooBar-win32-ia32
  # Creates a `.nupkg`, a `RELEASES` file, and a `.exe` installer file in `./`

Options:
  --out=<path>         The directory to place artifacts [Default: `process.cwd()`].
  --debug              Enable debug messages.
  --overwrite          Overwrite any existing `Setup.exe` [Default: `false`].
  -h --help            Show this screen.
  --version            Show version.
```

## Integration

Squirrel will spawn your app with command line flags on first run,
updates, and uninstalls. It is **very** important that your app handle
these events as _early_ as possible, and quit **immediately** after
handling them. Squirrel will give your app a short amount of time
(~15sec) to apply these operations and quit.

The [electron-squirrel-startup][electron-squirrel-startup] module will handle
the most common events for you, such as managing desktop shortcuts.  Just
add the following to the top of your `main.js` and you're good to go:

```js
if(require('electron-squirrel-startup')) return;
```

### API

```javascript
var createInstaller = require('electron-installer-squirrel-windows')
createInstaller(opts, function done (err) { })
```
#### createInstaller(opts, callback)

##### opts

**Required**

`path` - *String*
The directory generated by [electron-packager][electron-packager].

**Optional**

> **Note** All optional keys will be read from your `package.json` by default.

`name` - *String*
The application name (usually all lowercase with dashes for spaces).

`product_name` - *String*
The marketing name (usually `name` with spaces and titlecased).

`out` - *String*
The folder path to create the `.exe` installer in. [Default: `process.cwd()`]

`loading_gif` - *String*
The local path to a `.gif` file to display during install. [Default: `__dirname/resources/install-spinner.gif`]

`authors` - *String*
The authors value for the nuget package metadata.

`owners` - *String*
The owners value for the nuget package metadata. [Default: `#{authors}`]

`exe` - *String*
The name of your app's main `.exe` file. [Default: `#{product_name}Setup.exe`]

`description` - *String*
The description value for the nuget package metadata. [Default: ``]

`version` - *String*
The version value for the nuget package metadata.

`title` - *String*
The title value for the nuget package metadata. [Default: `#{product_name || name}`]

`cert_path` - *String*
The path to an Authenticode Code Signing Certificate. [Default: `null`]

`cert_password` - *String*
The password to decrypt the certificate given in `cert_path`. [Default: `null`]

`sign_with_params` - *String*
Params to pass to signtool which overrides `cert_path` and `cert_password`.  [Default: `null`]

`setup_icon` - *String*
URL to the `.ico` file to use as the icon for the generated `Setup.exe`. [Default: `http://git.io/vqdOX` (atom.ico)]

`remote_releases` - *String*
URL to your existing updates. If given, these will be downloaded to create delta updates. [Default: `null`]

`token` - *String*
Token used to authenticate access to remote release API. [Default: `null`]

`overwrite` - *Boolean*
Overwrite existing installers if they already exist. [Default: `false`]

`debug` - *Boolean*
Enable debug message output. [Default: `false`]

##### callback

`err` - *Error*
Contains errors if any.

## License

Relicensed under Apache 2.0 Copyright (c) 2015 MongoDB Inc.

Based on [atom/grunt-electron-installer][original] Copyright (c) 2015 GitHub Inc.

[appveyor_img]: https://ci.appveyor.com/api/projects/status/157smy0vsosp72bu/branch/master?svg=true
[appveyor_url]: https://ci.appveyor.com/project/mongodb-js/electron-installer-squirrel-windows/branch/master
[electron]: https://github.com/atom/electron
[squirrel]: https://github.com/Squirrel/Squirrel.Windows
[original]: https://github.com/atom/grunt-electron-installer
[electron-squirrel-startup]: https://github.com/mongodb-js/electron-squirrel-startup
