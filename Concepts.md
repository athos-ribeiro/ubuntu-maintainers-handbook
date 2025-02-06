# High level concepts

An Ubuntu installation is made up of packages that are copied and unpacked onto
the target machine. The Ubuntu project is the group of people who maintain
these packages, including both Canonical employees and the wider community.

Ubuntu's collection of packages is derived from the collection maintained by
the community-driven Debian project. An important part of an Ubuntu packager's
role is to collaborate with Debian by keeping the Ubuntu copies of packages
up-to-date, and by sharing improvements made in Ubuntu back up to Debian.

An overriding principle in Ubuntu is that processes should make common cases
easy to handle. In exceptional cases, a request for an exception is filed with
the appropriate review team, and if granted, will permit the deviation.


## Ubuntu release cadence

Ubuntu and Debian have distinct differences in infrastructure, procedures, and
release schedules.

Ubuntu follows a strict time-based release cycle. Every six months, a new
Ubuntu version is released and its set of packages declared "stable".
Simultaneously, a new version begins development; it is given its own codename
but also referred to as the "current development release", or "devel".

The development process follows a defined schedule, with periods of active
development followed by various freezes where the type of packaging activity
changes to focus more on stabilisation and preparations for release.

After an Ubuntu version is released, the software packages are altered only for
pressing reasons such as fixing defects or updating critically important
components. These post-release updates to the stable versions of Ubuntu are
referred to as "Stable Release Updates", or "SRUs". SRU work is a primary
activity of Ubuntu packagers, and will be discussed throughout this document.

In addition to the regular Ubuntu releases, every 2 years (i.e. every 4th
release) a release is declared to be a Long Term Support (LTS) release. The LTS
releases are special in that they have much longer than usual support periods.


## Launchpad, the project repository

One of the greatest ways Ubuntu differs from Debian is in its infrastructure,
which is built around a GitHub-like project repository named Launchpad.

Launchpad and GitHub operate on similar principles. Each has users/groups and
projects. But while GitHub puts users at the top level (e.g.
https://github.com/canonical), Launchpad does so with projects (e.g.,
https://launchpad.net/ubuntu). Users and groups in Launchpad begin with a tilde
instead (e.g., https://launchpad.net/~ubuntu-server).


### Quick access

You can quickly access Launchpad pages using the shortcut URL
[pad.lv](https://pad.lv), or using `!unpkg some-package` in DuckDuckGo.

Alternatively, if you're using Firefox you can add a shortcut bookmark. Create
a 'Shortcuts' folder under 'Other Bookmarks' in your browser. Inside the
'Shortcuts' folder add a 'New Bookmark', with these fields:

```text
Name:     Launchpad Bugs
Location: https://bugs.launchpad.net/ubuntu/+bug/%s
Tags:     
Keyword:  lpb
```

Add another one for Debian bugs:

```text
Name:     Debian Bugs
Location: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=%s
Tags:     
Keyword:  debb
```

If you expect to work frequently with other upstream bug trackers, add
shortcuts for them too.


## Suite (package) model

There are binary and source packages, each existing in parallel namespaces.

In practice, binary and source packages are stored together in one shared
directory. Their namespaces are parallel because they have different filename
endings (`.deb` for binary packages, `.dsc` for source packages).

By convention, we give binary and source packages the same base name. Note,
however, that a single source package can generate multiple binary packages, so
the name of a binary package may not match exactly with the source package.

For example, in http://archive.ubuntu.com/ubuntu/pool/main/b/bash:

```text
bash_4.2-2ubuntu2.6.diff.gz         2014-10-09 12:18    90K
bash_4.2-2ubuntu2.6.dsc             2014-10-09 12:18    2.2K
bash_4.2-2ubuntu2.6_amd64.deb       2014-10-09 12:18    626K
bash_4.2-2ubuntu2.6_i386.deb        2014-10-09 12:18    601K
bash_4.2-2ubuntu2.diff.gz           2012-04-03 16:05    79K
bash_4.2-2ubuntu2.dsc               2012-04-03 16:05    1.5K
bash_4.2-2ubuntu2_amd64.deb         2012-04-03 16:05    626K
bash_4.2-2ubuntu2_i386.deb          2012-04-03 16:07    601K
bash_4.2.orig.tar.gz                2011-02-24 01:04    4.1M
bash_4.3-6ubuntu1.debian.tar.gz     2014-04-09 00:58    76K
bash_4.3-6ubuntu1.dsc               2014-04-09 00:58    1.6K
bash_4.3-6ubuntu1_amd64.deb         2014-04-09 01:18    561K
bash_4.3-6ubuntu1_i386.deb          2014-04-09 01:18    535K
bash_4.3-7ubuntu1.7.debian.tar.gz   2017-05-17 17:03    98K
bash_4.3-7ubuntu1.7.dsc             2017-05-17 17:03    2.2K
bash_4.3-7ubuntu1.7_amd64.deb       2017-05-17 17:03    561K
bash_4.3-7ubuntu1.7_i386.deb        2017-05-17 17:03    535K
```

All packages for all Ubuntu releases are stored together in one shared
directory, and pointed to by the various releases. This means that you cannot
have two of the same package with the same version, even across releases, but
you can have multiple releases point to the same package version.

Once a package is uploaded to Launchpad, it cannot be replaced, even if the
package Launchpad uploads to the APT repository gets deleted (for whatever
reason). If a package does get deleted from the archive, it will show as
"Deleted" in Launchpad.


### Suites

Each release of Ubuntu is made up of suites, containing references to packages.
We use release codenames in suites rather than version numbers (Bionic vs.
18.04) because the suite will be created before release. This decides the
version number since we use the release month of the year for the second part
of the number.

For example, in http://archive.ubuntu.com/ubuntu/dists:

```text
bionic-backports/   2018-12-12 05:30    -
bionic-proposed/    2018-12-12 06:50    -
bionic-security/    2018-12-12 06:50    -
bionic-updates/     2018-12-12 07:06    -
bionic/             2018-04-26 23:38    - 
```

Within each suite is a `Release` file, and then various repositories, each with
different levels of support.

For example, in http://archive.ubuntu.com/ubuntu/dists/bionic:

```text
Contents-amd64.gz   2018-04-26 05:59    38M
Contents-i386.gz    2018-04-26 07:25    37M
InRelease           2018-04-26 23:38    236K
Release             2018-04-26 23:38    236K
Release.gpg         2018-04-26 23:38    819
by-hash/            2017-10-25 09:04    -
main/               2018-04-24 01:33    -
multiverse/         2017-10-25 13:33    -
restricted/         2017-10-24 22:44    -
universe/           2017-10-25 13:33    -
```


### Repositories

#### Main

The main packages in this distribution, supported by the Ubuntu team.

#### Restricted

Packages supported by the Ubuntu team, which are not available under a
completely free license.

#### Universe

Software from this repository is mostly unsupported by the Ubuntu Server team.
Software in Universe may be reviewed or get updates from the Server team if the
Universe package is directly related to a Server Package in Main.

#### Multiverse

Software from this repository is entirely unsupported by the Ubuntu team, and
may not be provided under a free license. Software in multiverse will not
receive any review or updates from the Ubuntu security team.


### APT

The Advanced Packaging Tool, APT, has a command-line tool (`apt`), which uses
`sources.list` and `sources.list.d` to tell it which suites to use. `apt` has
two possible formats, `one-line` and `deb822`. `deb822` has been the default
since [Noble Numbat 24.04](https://discourse.ubuntu.com/t/ubuntu-24-04-lts-noble-numbat-release-notes/39890#deb822-sources-management). 

`one-line` example `/etc/apt/sources.list`:

```text
deb http://archive.ubuntu.com/ubuntu/ focal main restricted universe
deb-src http://archive.ubuntu.com/ubuntu/ focal main restricted universe

## Major bug fix updates produced after the final release of the distribution.
deb http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe
# deb-src http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe

## Important security fixes.
deb http://security.ubuntu.com/ubuntu/ focal-security main restricted universe
# deb-src http://security.ubuntu.com/ubuntu/ focal-security main restricted universe
```

`deb822` example:

```text
Types: deb
URIs: http://archive.ubuntu.com/ubuntu/
Suites: noble noble-updates noble-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb-src
URIs: http://archive.ubuntu.com/ubuntu/
Suites: noble noble-updates noble-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```
Note that in deb822 format, `deb` and `deb-src` can be combined in the `Types` key.
They are shown separated here for easier commenting out.

When `apt` encounters multiple versions of a package, it uses an internal
scoring system to decide which version should be installed.

Notice that some lines start with `deb`, and others with `deb-src`. The 'deb'
lines provide binary packages, while the 'deb-src' ones provide source
packages. You'll usually notice the 'deb-src' lines are commented out with `#`
to disable them; this makes updates run a bit faster for the vast majority of
people who don't need access to the sources. You're one of the select few who
do need access, so uncomment the 'deb-src' lines appropriate to what you'll be
working on.

Depending on your geographical area, you may find it beneficial to pull from
one of Canonical's mirrors. To do this, replace the hostname portion of each
line above with the corresponding mirror hostname. For example, in Germany you
might use:

```text
deb http://de.archive.ubuntu.com/ubuntu/ focal main restricted universe
deb-src http://de.archive.ubuntu.com/ubuntu/ focal main restricted universe
# etc.
```


### Partial suites

Some suites are known as 'partial suites'. They contain only a subset of the
total packages required to install Ubuntu, but contain packages that supersede
those in a different suite if overlaid on top of it. Some examples of partial
suites are `backports`, `proposed`, `security`, and `updates`.

#### updates

Once an Ubuntu version is released, the `series` pocket stops receiving new
package versions, but updates to the initial set are still uploaded with
diverse fixes. Versions with regular bug fixes, or potential new features,
are present in `series-updates`.

#### security

The `series-security` suite carries security fixes to packages in `series`.
The updates there are maintained by the security team.

#### proposed

When any package is uploaded to Ubuntu, it will land in `series-proposed`. This
suite is used for testing the package to make sure it builds and that tests
pass before the uploaded version can be copied into the release itself. During
the development of a new release, packages that are considered in a good state
migrate from `series-proposed` to `series`. After the release, updates to the
packages migrate from `series-proposed` to `series-updates` (as part of the
SRU process).

#### backports

Software in `series-backports` may not have been tested as extensively as that
contained in the above examples, although it includes newer versions of some
applications that may provide useful features. Those versions are usually
present in subsequent Ubuntu releases, and are copied back to previous
releases. Software in backports will not receive any review or updates from the
Ubuntu security team.


## Source (launchpad) model

Launchpad only understands source package files. Uploading packages to
Launchpad effectively 'creates' them -- there's no other way to make Launchpad
aware of a source package. Remember that everything is stored in the same
namespace, so you can't have multiple copies of the same version of a package.

It's easiest to think of a package name as a directory, with each version of
the package stored within that directory. For example,
`https://launchpad.net/ubuntu/+source/hello` would conceptually be:

```text
hello
├── hello_2.7.orig.tar.gz
├── hello_2.7-2.debian.tar.gz
├── hello_2.7-2.dsc
├── hello_2.8.orig.tar.gz
├── hello_2.8-4.debian.tar.gz
├── hello_2.8-4.dsc
├── hello_2.10.orig.tar.gz
├── hello_2.10-1.debian.tar.xz
├── hello_2.10-1.dsc
├── hello_2.10-1build1.debian.tar.xz
├── hello_2.10-1build1.dsc
├── hello_2.10-1ubuntu1.debian.tar.xz
└── hello_2.10-1ubuntu1.dsc
```

The `.dsc` file tells you which files are part of this source package.

For example, `hello_2.10-1ubuntu1.dsc`:

```text
Checksums-Sha1:
 f7bebf6f9c62a2295e889f66e05ce9bfaed9ace3 725946 hello_2.10.orig.tar.gz
 b5e2e2280486138576a4c978fb2d36028985bb33 6356 hello_2.10-1ubuntu1.debian.tar.xz
```

The `.dsc` file also tells you what dependencies the package has:

```text
Build-Depends: debhelper (>= 9.20120311)
```

And which binary packages to produce:

```text
Package-List:
 hello deb devel optional arch=any
```

In the case of `hello_2.10-1ubuntu1.dsc`, the following files are produced for
the archive:

```text
hello-dbgsym_2.10-1ubuntu1_amd64.ddeb (33.9 KiB)
hello-dbgsym_2.10-1ubuntu1_arm64.ddeb (33.4 KiB)
hello-dbgsym_2.10-1ubuntu1_armhf.ddeb (33.4 KiB)
hello-dbgsym_2.10-1ubuntu1_i386.ddeb (30.9 KiB)
hello-dbgsym_2.10-1ubuntu1_ppc64el.ddeb (44.4 KiB)
hello-dbgsym_2.10-1ubuntu1_s390x.ddeb (33.4 KiB)
hello_2.10-1ubuntu1.debian.tar.xz (6.2 KiB)
hello_2.10-1ubuntu1.dsc (1.7 KiB)
hello_2.10-1ubuntu1_amd64.deb (27.4 KiB)
hello_2.10-1ubuntu1_arm64.deb (26.7 KiB)
hello_2.10-1ubuntu1_armhf.deb (25.7 KiB)
hello_2.10-1ubuntu1_i386.deb (27.9 KiB)
hello_2.10-1ubuntu1_ppc64el.deb (30.5 KiB)
hello_2.10-1ubuntu1_s390x.deb (26.9 KiB)
hello_2.10.orig.tar.gz (708.9 KiB)
```


### Series and pockets, vs. suites

Whereas APT has the concept of 'suites', Launchpad has the concept of 'series
and pockets':

| Series | Pocket   | Corresponding suite |
| ------ | -------- | ------------------- |
| Warty  | Release  | warty               |
| Warty  | Updates  | warty-updates       |
| Bionic | Release  | bionic              |
| Bionic | Security | bionic-security     |

Launchpad automatically maps from series and pocket to suite when generating
binary packages.


### Source packages

Let's next examine the files involved in a specific release. From the above,
let's look at the `2.7-2` release:

```text
├── hello_2.7.orig.tar.gz
├── hello_2.7-2.debian.tar.gz
├── hello_2.7-2.dsc
```

The first file, `hello_2.7.orig.tar.gz`, or the 'orig tarball' as it's termed,
corresponds to the upstream project's official source code for their `2.7`
release. Often, this will be a copy of the upstream project's released archive
tarball, renamed to suit Debian's file naming policy. Other times there are
more structured processes provided by Debian for managing the orig tarball,
such as 'pristine-tar'.

The next file, `hello_2.7-2.debian.tar.gz` is sometimes referred to as the
'Debian delta'. It contains all of the packaging changes needed to transform
the orig tarball into the appropriate `.deb` file(s). The `-2` in its filename
indicates it is the second update to the Debian delta for `2.7`. This 'dash
number' is updated when new packaging changes are uploaded.

Before we can work on these files, we need to unpack them into a working tree,
with the Debian changes applied. A variety of tools exist to do this,
including `dget <url>.dsc`, `apt-get source <pkg>`, etc. A low level way to do
this is with the `dpkg-source` command:

```bash
$ dpkg-source -x hello_2.10-2ubuntu2.dsc

dpkg-source: info: extracting hello in hello-2.10
dpkg-source: info: unpacking hello_2.10.orig.tar.gz
dpkg-source: info: unpacking hello_2.10-2ubuntu2.debian.tar.xz
```

Looking into the `hello-2.10/` directory, you'll notice there has been a
`debian/` directory added. Debian's policy is to contain all of its additions
to the orig tarball in a `debian/` directory within the unpacked tree. Let's
look at the contents of the `debian/` directory:

```bash
$ find hello-2.10/debian/

hello-2.10/debian/
hello-2.10/debian/control
hello-2.10/debian/copyright
hello-2.10/debian/changelog
hello-2.10/debian/source
hello-2.10/debian/source/format
hello-2.10/debian/tests
hello-2.10/debian/tests/control
hello-2.10/debian/rules-old
hello-2.10/debian/watch
hello-2.10/debian/rules
```

- The control file contains metadata about the source and binary packages.
- The rules file contains build directives.
- The changelog file lists the sequence of changes made to the package, with
  the most recent at the top. Here's one recent change from `hello`'s
  changelog:

```text
hello (2.10-2ubuntu1) eoan; urgency=low

  * Merge from Debian unstable.  Remaining changes:
    - Run the upstream tests as an autopkg test as well.
  * Dropped changes, included in Debian:
    - Bump the standards version.

 -- Steve Langasek <steve.langasek@ubuntu.com>  Wed, 22 May 2019 16:36:23 -0700
```

We'll be creating our own changelog entry later and will discuss the various
elements at that point. For now, note how the first line contains the package
name (`hello`), version number (`2.10-2ubuntu1`), and the Ubuntu release
codename (`eoan`).

Note that some packages also have a `patches/` directory, with `.diff` or
`.patch` files that will be applied to the packaging before building.


### d/control file and how it helps to submit changes

Let's focus on the `control` file inside the `debian/` directory. The
`debian/control` file contains a number of fields.

- Each line begins with a tag such as `Package:` or `Version:`.
- Each field gives us a new bit of knowledge about the package, such as package
  name, package type, version or description.

You will find each field described in more detail by visiting the
[Debian maintainers guide](https://www.debian.org/doc/manuals/maint-guide/dreq.en.html#control).

Example of a control file:

```text
Source: ipmitool
Section: utils
Priority: optional
Maintainer: Jörg Frings-Fürst <debian@jff.email>
Build-Depends:
 debhelper-compat (= 13),
 init-system-helpers (>= 1.58),
 libncurses-dev,
 libfreeipmi-dev [!hurd-i386],
 libreadline-dev,
 libssl-dev
Standards-Version: 4.6.1.0
Rules-Requires-Root: no
Vcs-Git: git://jff.email/opt/git/ipmitool.git
Vcs-Browser: https://jff.email/cgit/ipmitool.git
Homepage: https://github.com/ipmitool/ipmitool
```

Here are just a few examples of fields that help us understand where to look
and who to contact about this package:

1. **Maintainer**

   This field typically gives us information on who created the package. If we
   have suggestions or want to propose something, we should talk to the
   maintainer whose email is given in that field.

   Here we see that the maintainer is a person (not a group).

2. **Homepage**

   Here we can find information about the upstream project's home page URL,
   which may lead us to more information and subject matter experts.

3. **VCS**

   Here you can find the code and maybe propose a pull request for changes. In
   the example, we can easily spot the entry, which tells us that the package
   is developed in git, but not hosted on
   [salsa](https://salsa.debian.org/public).


#### General process to decide where to submit changes:

1. Decide where it needs to go based on what kind of change you have.
1. To get it to Debian:
   1. Each package is different. Some prefer mails, some bugs, some PRs -- in
      the worst case you have to find out by trying and asking around.
      * Entries in `d/control` can help you to identify git, mails or
        project to check what they prefer.
    2. If PRs are allowed, creating them from `git-ubuntu`` is easy:
       * Find and fork the repository referenced from `d/control`.
       * Create a branch and then cherry-pick the commits there.
       * Ensure only what is needed for Debian is in the commit content,
         commit message and changelog.
       * Push to your fork of the repository on
         [salsa](https://salsa.debian.org/public), and open a pull request describing what problem you solve.
   1. If we should submit a bug:
       * Check if a bug like that already exists:
         * https://tracker.debian.org/pkg/ipmitool
         * https://bugs.debian.org/cgi-bin/pkgreport.cgi?repeatmerged=no&src=ipmitool
       * If not, consider creating a new one
         [following these steps](https://www.debian.org/Bugs/Reporting).
       * If you have a solution ready to propose, attach the debdiff to the
         bug report you created.
   2. If we need to send a mail:
      * Use the maintainer field in `d/control` as shown above to know which
        person or group to contact.
      * If you have a solution ready to propose, attach the debdiff to the
        mail.
2. To get it upstream:
    1. Look at the homepage field in `d/control` to check how the upstream
       contribution rules are defined.


#### Let us play through an example:

We want to propose autopkgtests that are not present in Debian, but they
are present in Ubuntu in a package. Should we propose this to upstream,
Debian git, Debian bugs or the maintainer?

1. Since this example is about an autopkgtests, upstream will not care.
2. In this case the git repository does not allow PRs, so we need to fall back
   to bugs or mail.
3. Then we created a bug to refer to.
4. Then we submitted the debdiff to that bug.


## The build process

The build process starts when a source package is uploaded to a series/pocket
and accepted in Launchpad.

Upon acceptance, it will appear in the `Latest upload` section of the package
source page (for example, https://launchpad.net/ubuntu/+source/hello).

Launchpad will also schedule builders to build the required binary packages.
You can see the build queue at https://launchpad.net/builders.

If you click on the latest version in the `+source/some-package` page, you'll
see (under `Builds`) the latest status of the builds for each architecture.

Once built, the build artifacts are queued for publication, and eventually get
pushed to the APT master mirror. If the build fails, the build info page will
show the build log.


## Change process

All changes to Ubuntu packages follow a similar process:

1. Open a bug report explaining why a change is needed. You use the same
   process even for merges (which technically aren't bugs).
2. Clone the source repository.
3. Make a branch for yourself.
4. Make your changes and push them to your launchpad repository.
5. Open a merge proposal and get reviews.
6. Upload, or get a sponsor for your changes.
7. Track migration of your package through the build system.
8. Verify that the package works as intended, and hasn't introduced
   regressions.


## Source code repositories

All changes to packages are done through their source code repositories. This
used to be done through bazaar, but is now done through git. The preferred
method is to use `git-ubuntu`, which you can install using `snap``:

```bash
sudo snap install --edge --classic git-ubuntu
```

(Note:  `--edge` requests installation of the current development version of
git-ubuntu, which is the best tested.)


### Cloning a repository

```bash
$ git ubuntu clone hello
```

This will attempt to clone the `hello` Ubuntu source code repository into a
subdirectory called `hello`. There will be many branches and tags set up, for
example:

```bash
$ git ubuntu clone hello
From https://git.launchpad.net/~usd-import-team/ubuntu/+source/hello
 * [new branch]      applied/debian/buster          -> pkg/applied/debian/buster
 * [new branch]      applied/debian/jessie          -> pkg/applied/debian/jessie
 ...
 * [new branch]      applied/ubuntu/artful          -> pkg/applied/ubuntu/artful
 * [new branch]      applied/ubuntu/artful-devel    -> pkg/applied/ubuntu/artful-devel
 * [new branch]      applied/ubuntu/bionic          -> pkg/applied/ubuntu/bionic
 * [new branch]      applied/ubuntu/bionic-devel    -> pkg/applied/ubuntu/bionic-devel
 ...
 * [new branch]      debian/buster                  -> pkg/debian/buster
 * [new branch]      debian/jessie                  -> pkg/debian/jessie
 ...
 * [new branch]      importer/debian/dsc            -> pkg/importer/debian/dsc
 * [new branch]      importer/debian/pristine-tar   -> pkg/importer/debian/pristine-tar
 * [new branch]      importer/ubuntu/dsc            -> pkg/importer/ubuntu/dsc
 * [new branch]      importer/ubuntu/pristine-tar   -> pkg/importer/ubuntu/pristine-tar
 ...
 * [new branch]      ubuntu/artful                  -> pkg/ubuntu/artful
 * [new branch]      ubuntu/artful-devel            -> pkg/ubuntu/artful-devel
 * [new branch]      ubuntu/bionic                  -> pkg/ubuntu/bionic
 * [new branch]      ubuntu/bionic-devel            -> pkg/ubuntu/bionic-devel
 ...
 * [new branch]      ubuntu/devel                   -> pkg/ubuntu/devel
 ...
 * [new tag]         applied/2.1.1-4                -> pkg/applied/2.1.1-4
 * [new tag]         applied/2.1.1-5                -> pkg/applied/2.1.1-5
 * [new tag]         applied/2.10-1                 -> pkg/applied/2.10-1
 * [new tag]         applied/2.10-1build1           -> pkg/applied/2.10-1build1
 * [new tag]         applied/2.10-1ubuntu1          -> pkg/applied/2.10-1ubuntu1
 ...
 * [new tag]         import/2.1.1-4                 -> pkg/import/2.1.1-4
 * [new tag]         import/2.1.1-5                 -> pkg/import/2.1.1-5
 * [new tag]         import/2.10-1                  -> pkg/import/2.10-1
 * [new tag]         import/2.10-1build1            -> pkg/import/2.10-1build1
 * [new tag]         import/2.10-1ubuntu1           -> pkg/import/2.10-1ubuntu1
 ...
 * [new tag]         upstream/debian/2.10.gz        -> pkg/upstream/debian/2.10.gz
 * [new tag]         upstream/debian/2.2.gz         -> pkg/upstream/debian/2.2.gz
 * [new tag]         upstream/debian/2.4.gz         -> pkg/upstream/debian/2.4.gz
```

The branches you'll be interested in are `ubuntu/somerelease-devel`.
`ubuntu/somerelease` is the package set as it existed on release day. The
`-devel` branch contains that release, plus all changes to date. The special
`ubuntu/devel` branch points to the (unreleased) leading edge of development
for the package in Ubuntu.

You'll be using the `-devel` branches for your changes.


### Creating a branch for your work

Before making changes, create a branch for yourself. It's recommended to give
the branch a meaningful name that will remind you of its purpose after not
looking at it for a few months. You'll also potentially have matching PPAs and
LXC containers, so having a consistent naming scheme helps.

For example:

```bash
hello-fix-lp1234567-segfault-bionic
```

Where:
* `hello` is the package you are changing.
* `fix-lp1234567-segfault` is the job being done, including a bug #, merge #,
  or Debian version.
* `bionic` is the Ubuntu release this change is for.

Be aware that git, LXD, and PPA each have restrictions on what kinds of
punctuation they accept, and LXD has a length limit of <63 chars max. Dashes
are safe in all three, but other punctuation is disallowed by one or another.

Create the branch like so:

```bash
$ git branch hello-fix-lp1234567-segfault-bionic pkg/ubuntu/bionic-devel
```

### Pushing your changes

Once your changes are ready to push, do so:

```bash
$ git push mylaunchpadusername hello-fix-lp1234567-segfault-bionic
```

Now you'll be able to see it on launchpad. Go to your code section:
https://code.launchpad.net/~your-launchpad-username/+git

You'll see a list of repositories:

```text
lp:~yourlaunchpadname/ubuntu/+source/hello      2018-12-20
lp:~yourlaunchpadname/ubuntu/+source/logwatch   2018-12-18
lp:~yourlaunchpadname/ubuntu/+source/tomcat8    2018-12-10
lp:~yourlaunchpadname/ubuntu/+source/php7.3     2018-12-05
```

Click on a repository, and you'll see a list of branches at the bottom:
```text
Name                                  Last Modified   Last Commit
hello-fix-lp1234567-segfault-bionic   2018-12-10      changelog 
```

Click on the branch to discover its merge status. It will either provide a
link to `Propose for merging`, or if it's already been proposed, it will show
the merge status, like so:

```text
Approved for merging into ubuntu/+source/hello:ubuntu/bionic-devel

   John Smith: Approve on 2018-12-20
   Canonical Server Team: Pending requested 2018-12-21
   Diff: 171 lines (+149/-0)
   3 files modified
```

Clicking the merge link (`Approved` in this case) brings you to the actual
merge proposal.


## See also

* [Debian policy](https://www.debian.org/doc/debian-policy/)

* [Ubuntu policy](https://ubuntu.com/legal/terms-and-policies)

* `apt install ubuntu-policy`
