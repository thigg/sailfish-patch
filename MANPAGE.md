
# SAILFISH-PATCH 1 "" "sailfish-patch 2.0.0 (2019-07-05)"

## NAME
sailfish-patch - Manage your SailfishOS patches

## SYNOPSIS

`sailfish-patch` [-b] [-p] [-Po [latest]] [-Pm [latest]] [-S|-S ROUNDS]

`sailfish-patch` [-c NAME]

`sailfish-patch` [-u]

`sailfish-patch` [-i CONFIG TARBALL]

`sailfish-patch` [-C FILE]

`sailfish-patch` [-h] [-V]

## DESCRIPTION

Create a new patch skeleton with `-c NAME` and put the original files
in the directory `original`. Copy everything to the directory `patched` and
make your changes. Configure the project in `build/CONFIG`. Build with `-b`.

Note: separate settings pages are currently only supported via Patchmanager.

You can add all packages a patch is based on in the `SourcePackages` field
in the configuration file. Running `sailfish-patch -u` will update all source
files and makes it easy to adapt the patch to new upstream versions.

Note that it must be possible for `sailfish-patch` to connect to your device via
SSH if you want to use source packages from the official repos. Make sure this
is configured correctly. (This is not needed for OpenRepos sources.)

If you want to add your own settings pages or include translation files,
icons etc. in your patch, you can place them in the `extra` directory. Note
that this is not yet supported for RPMs.

Tips:

- Use `rpm -qf /path/to/file` to find out which package a file belongs to.
- Use `pkcon get-details packagename` to check which version is installed.
- Use `sailfish-patch -b -p` to test the patch directly after building (needs ssh).
- Do not push your patch to a public repository. Instead, publish the `code
  distribution package` which includes only the files that belong to you.
  Other users can rebuild the development environment by running
  `sailfish-patch -u` in the published directory. This way, you do not have to
  publish potentially copyrighted material without permission.

## OPTIONS

`-c, --create NAME`
  Create new patch skeleton in NAME

`-C, --check-config FILE`
  Validate config file FILE

`-i, --import CONFIG TARBALL`
  Setup a new working directory for the given patch

`-b, --build`
  Build RPM and tarball

`-u, --update`
  Update the working directory with the latest sources (needs a working SSH
  connection for all sources except OpenRepos)

`-f, --force`
  skip some safety checks

`-p, --publish-ssh`
  Publish and install patch on your device via SSH

`-Po, --publish-openrepos`
  Wizard for publishing in OpenRepos.
  (When given 'latest' as argument to `-Po`, the wizard will provide info for updating the entry.)

`-Pm, --publish-patchmanager`
  Wizard for publishing in PM's online catalogue. (See -Po for more details.)

`-S, --optimize-screenshots [ROUNDS]`
  Optimize PNG screenshot files of the current project. Optionally specify number
  of processing rounds for best result (default: 1).

### Debug Options
`-g, --use-git-apply`
  Use git-apply(1) instead of patch(1) for applying the patch (see `-i` and `-u`)

`-eC, --export-config`
  Export CONFIG template

`-eL, --export-license`
  Export COPYING template

`-eG, --export-gitignore`
  Export .gitignore template

`-eI, --export-icon`
  Export OpenRepos icon template

`-eJ, --export-json`
  Export patch.json template

`-eS, --export-spec`
  Export RPM spec-file template

### General Information
`-h, --help`
  Show a short help text and exit. Use `man 1 sailfish-patch` to access this complete manual.

`-V, --version`
  Show version and licensing information about `sailfish-patch`.


## CONFIGURATION FILES

Read the sample configuration file provided with this script for detailed documentation.
It can be exported using `sailfish-patch -eC`.

To enable syntax highlighting in KDE's text editor Kate put the syntax highlighting
definition file provided with this script in the following directory:

    $HOME/.local/share/org.kde.syntax-highlighting/syntax/

Some default values can be configured in the file defined by `SF_PATCH_GLOBAL_DEFAULTS`.
An example file will be created if no configuration file is present.
The value of `SF_PATCH_GLOBAL_DEFAULTS` defaults to:

    $HOME/.config/sailfish-patch.conf

## ENVIRONMENT

`SF_PATCH_PASSFILE`
  Path to a local file containing your devel-su passphrase

`SF_PATCH_SSH_TARGET`
  How to connect to your device via ssh (config name or IP)

`SF_PATCH_REMOTE_PATH`
  Path to a directory where patch RPMs will be stored on your device

`SF_PATCH_GLOBAL_DEFAULTS`
  Path to the global configuration file for some default values used in new
  patch config files

## ADDING NEW CONFIGURATION FIELDS

To add a new configuration field, you have to update the following places:

- `check_default_dummies` array with default dummy value (if necessary)
- `default_config_values` array with default configuration values
  - prefix 's@' for single-line fields and 'm@' for multi-line fields
  - use '[sm]@dummy' to use the dummy value as default value
- add the field to the list of required fields
- and to the single line or multi line check switches
- add the field to all wizards where it is needed
- is necessary: add the field to `check_allowed_in_global_config`
- add the field to the config file template in `__save_template_config`
- update the Kate syntax file

## EXIT STATUS

**0** No problems were encountered.

**>0** Any error occurred.

## INSTALLATION
Make sure all external scripts and/or libraries as listed under
**DEPENDENCIES** are properly installed. Then simply put `sailfish-patch` in any
directory listed in your `$PATH` and mark it as executable:

    chmod +x sailfish-patch

To install the manual page, you have to compile it from the Markdown source.
For this, `md2man` can be used, or any other tool at your liking. Then copy
the generated manual page to your local manual directory. Usually something
like the below should do the job:

    md2man-roff MANPAGE.md | gzip -c - > "sailfish-patch.1.gz"
    sudo cp "sailfish-patch.1.gz" "/usr/local/man/man1/sailfish-patch.1.gz"

### Notes

`md2man` is a simple tool to convert Markdown sources to proper manpages.
Install it via

    gem install md2man

or grab the source from <https://github.com/sunaku/md2man>.

## DEPENDENCIES
`sailfish-patch` is written purely in `bash` but has the following dependencies:

- patch
- git
- scp
- ssh
- sed
- xclip
- rpm
- rpmbuild
- rpm2cpio
- cpio
- tar
- hxselect
- hxnormalize (from package html-xml-utils)
- du
- numfmt
- html2text (from package python3-html2text)
- pngcrush
- pngquant
- convert (from package imagemagick)

## AUTHOR
Written by Mirian Margiani, originally based on `gen-sailfish-patch` by Cornerman.

## COPYRIGHT
Copyright (C) 2016  Cornerman, 2018-2019  Mirian Margiani

`sailfish-patch` is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version.

`sailfish-patch` is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more
details.

You should have received a copy of the GNU General Public License along with
`sailfish-patch`.  If not, see <http://www.gnu.org/licenses/>.
