# debmake

Creates a basic deb package.

## requirements

minimum:

```
apt install devscripts
export DEBEMAIL="you@email"
export DEBFULLNAME="Your Name"
```

optional:

```
apt install build-essential binutils cpp cpio dpkg-dev file gcc make patch dh-make debhelper devscripts fakeroot lintian debian-policy developers-reference man-db manpages reportbug
```

## usage

```
Usage: debmake [repo] package version dist target

    Creates a basic deb package.

Options:

    repo
        GIT repo to clone from
    package
        Directory to package (package name)
    version
        Version number
    dist
        Distribution codename
    target
        Target installation directory (without leading `/`)
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
