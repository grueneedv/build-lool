# build-lool

Script collection to build libreoffice online or collabora online.

Supported Linux Distributions:
* Ubuntu 16.04.x, xenial
* Ubuntu 18.04.x, bionic
* Debian 8.x, jessie
* Debian 9.x, stretch

Updates the system, installs and sets up the required packages. Finally builds
libreoffice and libreoffice online.

## Requirements

A minimal or standard installation of a supported distribution as VM or on real
Hardware is required. For flawless building, the following system resources
are recommended:

* Disk space: >= 32 GByte
* Memory: >= 4 GByte
* Number of CPU cores: >= 4

Additionally you need an user account with sudo capability. Building as user
root does not work.

## Configuration

Before you start please customize the configuration.
Copy config/lool-config.sh.example to config/lool-config.sh.
Adjust config/lool-config.sh to your needs.

Especially take care for the settings of the two variables LOOL_MAX_CON and
LOOL_MAX_DOC. These two variables define the limits for the allowed number of
simultaneous connections to your online office and the allowed number of
simultaneous open documents. It's not possible to set these variables to unlimited.
You are restricted by the available system resources anyway.

You may define different versions for poco, libreoffice core and
libreoffice online. The following software package combinations are
tested and known to work:

Package            | version
-------------------|---------------------
poco               | 1.8.1
libreoffice core   | libreoffice-6.0.1.1
libreoffice online | libreoffice-6.0.0.3

Package            | version
-------------------|---------------------
poco               | 1.8.1
libreoffice core   | cp-5.3-43
libreoffice online | 3.1-final

Package            | version
-------------------|---------------------
poco               | 1.9.0
libreoffice core   | cp-5.3-47
libreoffice online | cd-3.2.2-2

Package            | version
-------------------|---------------------
poco               | 1.9.0
libreoffice core   | cp-5.3-58
libreoffice online | cd-3.4.0-final

Package            | version
-------------------|---------------------
poco               | 1.9.0
libreoffice core   | cp-6.0-25
libreoffice online | cp-4.0.1-1

It's recommended to leave LOOL_PREFIX as "/opt/lool" and POCO_PREFIX empty.

## Building

As mentioned above, you need a minimal or standard installation of a supported
distribution. Transfer the build-lool tar archive your build system. Unpack the archive
to a suitable location, ~/tools/ for example. Adjust the configuration to your needs
as described above. Start the shell script build-lool.sh as ordinary user, building
as user root does not work. The script acts as follows:

* Appends a line to .bashrc to startup the script at login.
* Enables the corresponding source package repositories for all enabled package repositories.
* Updates the system software packages.
* Installs all required software packages.
* Reboots the system.

After login again:

* Removes the script start command from .bashrc.
* Gets the source packages.
* Builds poco.
* Builds libreoffice core.
* Builds libreoffice online.
* Builds installable package.

After the script has finished you find the built packages in the subdirectory "packages".
There are packages for poco, libreoffice core and loolwsd. Depending on the configuration
you will find poco-1.9.0.tar.xz, core-cp-5.3-58.tar.xz, loolwsd-3.4.0-final.tar.xz and a
combined package lool-poco-1.9.0-core-cp-5.3-58-online-3.4.0-final.tar.xz to be installed
to "/opt/lool".
Since version 2.0.2 a list of required packages for the production system is generated. It's
stored in the package directory for example as
lool-poco-1.9.0-core-cp-5.3-58-online-3.4.0-final-required-packages.txt. The list is used by
the script install-lool.sh from the install-cloud-server script colletcion to automatically
install missing packages.

If you change source versions after a first build, only the necessary components
are rebuild.

The build will take some time depending on your system resources. I did some tests
with a local VM and an 1&amp;1 Cloud Server XXL. My local VM uses 4 cores of an
Intel(R) Xeon(R) E5-1660 v2 CPU, 4 GByte Memory and 32 GByte disk space. The 1&amp;1
Cloud Server XXL uses 4 CPU cores, 8 GByte memory and 160 GByte disk space.

Both, the 1&amp;1 Cloud Server and my VM, take less than 3 hours to install all necessary
software packages and to build poco, libreoffice core and libreoffice online. So, the
actual cost for using the 1&amp;1 Cloud Server XXL is about 0.20 €. The accounting for the
1&amp;1 Cloud Server is carried out based on used minutes. Using an 1&amp;1 Cloud Server
for building libreoffice online may be advantageous especially if you have a very limited
upload bandwidth.


## Installation

You may install the packages manually, or use the script collection install-cloud-server,
see https://github.com/RainerEmrich/install-cloud-server.
