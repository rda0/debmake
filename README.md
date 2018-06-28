# debmake

Creates a basic deb package.

## requirements

minimum:

```sh
apt install devscripts
export DEBEMAIL="maeder@phys.ethz.ch"
export DEBFULLNAME="Sven Mäder"
```

optional:

```
apt install build-essential binutils cpp cpio dpkg-dev file gcc make patch dh-make debhelper devscripts fakeroot lintian debian-policy developers-reference man-db manpages reportbug
```

## configuration

create a file `.debmake` in the root directory of the package with the following contents.

required:

```sh
# Target is the path where the package files will be installed (without leading `/`)
TARGET="usr/share/plymouth/themes/dphys-logo"

# The source of the software
HOMEPAGE="https://gitlab.phys.ethz.ch/core/plymouth-theme-dphys-logo.git"

# Short description (max. 80 characters)
DESCRIPTION_SHORT="Plymouth theme for the DPHYS"

# Long description will be broken to multiple lines (at 80 characters)
DESCRIPTION_LONG="Dark theme with stars background for the Department of Physics ETH Zurich"
```

optional:

```sh
# Section (default: `misc`)
SECTION="x11"

# Version (default: `1.0`)
VERSION=""

# Dependencies (add comma separated lists of packages to these variables)
DEPENDS="plymouth, plymouth-label, plymouth-theme-ubuntu-logo, plymouth-theme-ubuntu-text"
RECOMMENDS=""
SUGGESTS=""
CONFLICTS=""
BREAKS=""
PROVIDES=""
REPLACES=""
```

## usage

```
Usage: debmake [repo] package dist

    Creates a basic deb package.

Options:

    repo
        GIT repo to clone from
    package
        Directory to package (package name)
    dist
        Distribution codename
```

example:

```
debmake git@gitlab.phys.ethz.ch:core/plymouth-theme-dphys-logo.git plymouth-theme-dphys-logo 1.0 bionic usr/share/plymouth/themes/dphys-logo
```

## manual modifications

```
cd <package>/debian
```

edit the following files manually:

- add description: `control`
- modify target dir: `dirs`
- modify files to install: `install`
- modify links to create: `<package.links>`

then build the package again:

```
cd ..
debuild
```

## how to manually create a debian package

```
mv <package> <package>-1.0
tar --exclude='.git' -czf <package>_1.0.orig.tar.gz <package>-1.0
cd <package>-1.0
dh_make
rm debian/*.ex debian/*.EX debian/README.* debian/*.docs
debuild
```

then fix the errors until il builds successfully
