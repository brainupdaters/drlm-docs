DRLM Client Installation
========================

**Unattended Installation**
---------------------------

Now ReaR can be installed and configured on a remote server from the DRLM server
using :program:`drlm instclient`

Let's explain a little bit the steps this feature does:

        * Create the drlm user
        * Install ReaR dependencies
        * Install ReaR package
        * Configure ReaR to be managed by DRLM
        * Configure SUDO for drlm user.
        * Start and configure required services

Supported OSs for instclient command
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unattended Client Installation has been tested on:

       * SLES (11 & 12)
       * OpenSUSE (13 & Leap 42)
       * RHEL & CentOS (5, 6 & 7)
       * Debian (6, 7 & 8)
       * Ubuntu LTS (12.04, 14.04 & 16.04)

.. note:: It should work on other RedHat, Debian or SUSE variants.


Requirements
~~~~~~~~~~~~

In order to install ReaR from DRLM server the client must have:

       * Access to EPEL Repo to install rear from repo (CentOS,RHEL)
       * instclient uses apt-get, yum and zypper, so repositories must be configured
       * SSH enabled
       * root user or user with administrator privileges to install, start services
         like rpcbind and configure ReaR, DHCP and sudo applications.


Run unattended install
~~~~~~~~~~~~~~~~~~~~~~

To perform an unattended install of a DRLM client, just is needed to run **instclient** DRLM command like one of the following examples:

.. warning::
  The client must be properly registered in DRLM with **addclient** command.

Examples::

        $ drlm instclient -c ReaRCli1

	$ drlm instclient -c ReaRCli1 -U http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb


.. note:: See Client Operations for more information



**Manual Installation**
-----------------------

Debian 7 & Ubuntu 12.04 or 14.04
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ReaR requirements for DRLM
**************************

As rear is written in bash you need bash as a bare minimum. Other requirements are:

	* syslinux (for i386 based systems)
	* ethtool
	* genisoimage
	* parted
	* gawk
	* attr
	* sudo
	* curl (rear need to get its configuration from DRLM server)
	* mingetty (rear is depending on it in recovery mode)

::

	$ apt-get install syslinux ethtool genisoimage parted gawk attr sudo curl mingetty

Download and install ReaR
**************************

.. note::
	Minimum version required of ReaR: 1.17.0

**Download ReaR**

::

    $ wget http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb

You can download other ReaR versions from `ReaR Download Page <http://relax-and-recover.org/download/>`_ or from `OpenSuse Build Service <https://build.opensuse.org/project/show/Archiving:Backup:Rear>`_ .

**Install ReaR package**

:program:`The DEB based package can be installed as follows`

Execute the next command:
::

    $ dpkg -i rear_1.17.2_all.deb

.. note::
	For more information about ReaR visit:
	http://relax-and-recover.org/documentation

Create DRLM User
****************

::

   $ useradd -d /home/drlm -c "DRLM User Agent" -m -s /bin/bash -p $(echo S3cret | openssl passwd -1 -stdin) drlm

Disable password aging for drlm user
************************************

::

   $ chage -I -1 -m 0 -M 99999 -E -1 drlm


Copy rsa key from DRLM Server to the new client
***********************************************

.. warning:: You have to execute this code from DRLM Server. The password which you will be asked for is "S3cret" and "client_ipaddr" must be changed to the client ip address.

::

   $ ssh-keygen -t rsa
   $ ssh-copy-id drlm@"client_ipaddr"

Disable password login
**********************

::

   $ passwd -l drlm

Add Sudo roles for DRLM user
****************************

Edit **/etc/sudoers.d/drlm** and add the following lines

::

   Cmnd_Alias DRLM = /usr/sbin/rear, /bin/mount, /sbin/vgs
   drlm    ALL=(root)      NOPASSWD: DRLM

Change **/etc/sudoers.d/drlm** permissions

::

   $ chmod 440 /etc/sudoers.d/drlm

Client configuration
********************

We have to specify that this ReaR client is managed from a DRLM server. We have to edit the /etc/rear/local.conf and insert the next line.

::

   DRLM_MANAGED=y

Debian 6
~~~~~~~~

ReaR requirements for DRLM
**************************

As rear is written in bash you need bash as a bare minimum. Other requirements are:

	* syslinux (for i386 based systems)
	* ethtool
	* lsb-release
	* genisoimage
	* iproute
	* iputils-ping
	* binutils
	* parted
	* openssl
	* gawk
	* attr
	* sudo
	* openssh-server (to enable comunications between DRLM and ReaR client)
	* curl (rear need to get its configuration from DRLM server)
	* mingetty (rear is depending on it in recovery mode)

.. note::

	Debian 6 is discontinued, make sure that you have the Squeeze archive repository in /etc/apt/sources.list
	
	deb http://archive.debian.org/debian/ squeeze contrib main non-free

::

	$ apt-get update
	$ apt-get install syslinux ethtool lsb-release genisoimage iproute iputils-ping binutils parted openssl gawk attr sudo openssh-server curl mingetty nfs-common

Download and install ReaR
**************************

.. note::
	Minimum version required of ReaR: 1.17.0 (Recommended ReaR 1.18)

**Download ReaR**


amd64 architecture:
::
	$ wget http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/amd64/rear_1.18_amd64.deb

i386 architecture:
::
	$ wget http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/i386/rear_1.18_i386.deb


You can download other ReaR versions from `ReaR Download Page <http://relax-and-recover.org/download/>`_ or from `OpenSuse Build Service <https://build.opensuse.org/project/show/Archiving:Backup:Rear>`_ .

**Install ReaR package**

:program:`The DEB based package can be installed as follows`

Execute the next command:
::

	$ dpkg -i rear_1.18_amd64.deb

.. note::
	Use "dpkg -i rear_1.18_i386.deb" to install i386 architecture.
	For more information about ReaR visit: http://relax-and-recover.org/documentation

Create DRLM User
****************

::

	$ useradd -d /home/drlm -c "DRLM User Agent" -m -s /bin/bash -p $(echo S3cret | openssl passwd -1 -stdin) drlm

Disable password aging for drlm user
************************************

::

	$ chage -I -1 -m 0 -M 99999 -E -1 drlm


Copy rsa key from DRLM Server to the new client
***********************************************

.. warning:: You have to execute this code from DRLM Server. The password which you will be asked for is "S3cret" and "client_ipaddr" must be changed to the client ip address.

::

	$ ssh-keygen -t rsa
	$ ssh-copy-id drlm@"client_ipaddr"

Disable password login
**********************

::

	$ passwd -l drlm

Add Sudo roles for DRLM user
****************************

Edit **/etc/sudoers.d/drlm** and add the following lines

::

	Cmnd_Alias DRLM = /usr/sbin/rear, /bin/mount, /sbin/vgs
	drlm    ALL=(root)      NOPASSWD: DRLM

Change **/etc/sudoers.d/drlm** permissions

::

	$ chmod 440 /etc/sudoers.d/drlm

Client configuration
********************

We have to specify that this ReaR client is managed from a DRLM server. We have to edit the /etc/rear/local.conf and insert the next line.

::

	DRLM_MANAGED=y

CentOS & RHEL 6
~~~~~~~~~~~~~~~

ReaR requirements for DRLM
**************************

As rear is written in bash you need bash as a bare minimum. Other requirements are:

	* mkisofs
	* mingetty (rear depends on it in recovery mode)
	* syslinux (for i386 based systems)
	* nfs-utils
	* cifs-utils
	* rpcbind
	* wget
	* sudo
	* curl (rear needs it to get the configuration from DRLM server)

::

	$ yum -y install mkisofs mingetty syslinux nfs-utils cifs-utils rpcbind wget curl sudo

Download and install ReaR
*************************

.. note::
	Minimum version required of ReaR: 1.17.0

**Download ReaR**

::

   $ DISTRO="CentOS_CentOS-6" or DISTRO="RedHat_RHEL-6"

   $ wget http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/$DISTRO/$(uname -m)/rear-1.17.2-1.el6.$(uname -m).rpm

You can download other ReaR versions from `ReaR Download Page <http://relax-and-recover.org/download/>`_ or from `OpenSuse Build Service <https://build.opensuse.org/project/show/Archiving:Backup:Rear>`_ .

**Install ReaR package**

:program:`The RPM based package can be installed as follows`

Execute the next command:
::

    $ yum install rear-1.17.2-1.el6.x86_64.rpm

.. note::
	For more information about ReaR visit:
	http://relax-and-recover.org/documentation

Create DRLM User
****************

::

   $ useradd -d /home/drlm -c "DRLM User Agent" -m -s /bin/bash -p $(echo S3cret | openssl passwd -1 -stdin) drlm

Disable password aging for drlm user
************************************

::

   $ chage -I -1 -m 0 -M 99999 -E -1 drlm

Copy rsa key from DRLM Server to the new client
***********************************************

.. warning:: You have to execute this code from DRLM Server. The password which you will be asked for is "S3cret" and "client_ipaddr" must be changed to the client ip address.

::

   $ ssh-keygen -t rsa
   $ ssh-copy-id drlm@"client_ipaddr"

Disable password login
**********************

::

   $ passwd -l drlm

Add Sudo roles to DRLM user
***************************

Edit **/etc/sudoers.d/drlm** and add the following lines

::

   Cmnd_Alias DRLM = /usr/sbin/rear, /bin/mount, /sbin/vgs
   drlm    ALL=(root)      NOPASSWD: DRLM

Change **/etc/sudoers.d/drlm** permissions

::

   $ chmod 440 /etc/sudoers.d/drlm

Client configuration
********************

We have to specify that this ReaR client is managed from a DRLM server. We have to edit the /etc/rear/local.conf and insert the next line.

::

   DRLM_MANAGED=y

Services
********

**rpcbind**

::

        $ service rpcbind start
        $ chkconfig rpcbind on

**nfs**

::

        $ service nfs start
        $ chkconfig nfs on
