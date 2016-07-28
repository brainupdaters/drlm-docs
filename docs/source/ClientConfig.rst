DRLM Client Installation 
========================


Now ReaR can be installed and configured on a remote server from the DRLM server
using the new feature :program:`drlm instclient`

Let's explain a little bit the steps the new feature does:

        * Create the drlm user
        * Install rear dependencies
        * Install rear package
        * Start services and configure those on the startup
        * Configure ReaR to be managed by DRLM
        * Configure sudo for drlm user.

Supported OS's on the new feature instclient
--------------------------------------------

Install Client feature has been tested on:

       * Suse 12 SP1
       * RedHat 6,7
       * CentOS 6,7
       * Debian 7,8

.. note:: It should work on Redhat & CentOS 5 and also on Debian 6.


Requirements
------------

In order to install ReaR from DRLM server the client must have:

       * Access to EPEL Repo to install rear from repo (CentOS,RedHat)
       * instclient uses apt-get, yum and zypper, so repositories must be configured
       * SSH enabled
       * root user or user with administrator privileges to install ,start services
         like rpcbind and configure aplications ReaR,DHCP,sudo.

.. note:: See Install Client Operations for more information


::

        $ drlm instclient -c ReaRCli1


Manual Installation
-------------------


Debian 7
--------

ReaR requirements for DRLM
~~~~~~~~~~~~~~~~~~~~~~~~~~

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
~~~~~~~~~~~~~~~~~~~~~~~~~

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
~~~~~~~~~~~~~~~~

::

   $ useradd -d /home/drlm -c "DRLM User Agent" -m -s /bin/bash -p $(echo S3cret | openssl passwd -1 -stdin) drlm

Disable password aging for drlm user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

   $ chage -I -1 -m 0 -M 99999 -E -1 drlm


Copy rsa key from DRLM Server to the new client
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning:: You have to execute this code from DRLM Server. The password which you will be asked for is "S3cret" and "client_ipaddr" must be changed to the client ip address.

::

   $ ssh-keygen -t rsa
   $ ssh-copy-id drlm@"client_ipaddr"

Disable password login
~~~~~~~~~~~~~~~~~~~~~~

::

   $ passwd -l drlm

Add Sudo roles for DRLM user
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Edit **/etc/sudoers.d/drlm** and add the following lines

::

   Cmnd_Alias DRLM = /usr/sbin/rear* 
   drlm    ALL=(root)      NOPASSWD: DRLM
   
Change **/etc/sudoers.d/drlm** permissions

::

   $ chmod 440 /etc/sudoers.d/drlm

Client configuration
~~~~~~~~~~~~~~~~~~~~

We have to specify that this ReaR client is managed from a DRLM server. We have to edit the /etc/rear/local.conf and insert the next line.
 
::
 
   DRLM_MANAGED=y
   
Add client config file at DRLM server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning:: You have to do this at DRLM Server.

We have to add a new file called as "client host name".cfg at /etc/drlm/clients/
For example: If our client host name is ReaRCli1 we have to create /etc/drlm/clients/ReaRCli1.cfg and add the follwing lines.
Where CLI_NAME="Client Host Name" and SRV_NET_IP="DRLM Server IP".

::

	CLI_NAME=ReaRCli1
	SRV_NET_IP=192.168.1.38

	OUTPUT=PXE
	OUTPUT_PREFIX=$OUTPUT
	OUTPUT_PREFIX_PXE=$CLI_NAME/$OUTPUT
	OUTPUT_URL=nfs://${SRV_NET_IP}/var/lib/drlm/store/${CLI_NAME}

	BACKUP=NETFS
	NETFS_PREFIX=BKP
	BACKUP_URL=nfs://${SRV_NET_IP}/var/lib/drlm/store/${CLI_NAME}

	SSH_ROOT_PASSWORD=drlm

.. warning:: This file must be readable by Apache

::
  
        $ chmod 644 /etc/drlm/clients/ReaRCli1.cfg

CentOS 6, Red Hat 6
-------------------

ReaR requirements for DRLM
~~~~~~~~~~~~~~~~~~~~~~~~~~

As rear is written in bash you need bash as a bare minimum. Other requirements are: 
 
	* mkisofs
	* mingetty (rear is depending on it in recovery mode)	
	* syslinux (for i386 based systems) 
	* nfs-utils
	* cifs-utils
	* rpcbind
	* wget
	* sudo 
	* curl (rear need to get its configuration from DRLM server) 
	
::

	$ yum -y install mkisofs mingetty syslinux nfs-utils cifs-utils rpcbind wget curl sudo

Download and install ReaR 
~~~~~~~~~~~~~~~~~~~~~~~~~
	
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
~~~~~~~~~~~~~~~~

::

   $ useradd -d /home/drlm -c "DRLM User Agent" -m -s /bin/bash -p $(echo S3cret | openssl passwd -1 -stdin) drlm

Disable password aging for drlm user
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

   $ chage -I -1 -m 0 -M 99999 -E -1 drlm

Copy rsa key from DRLM Server to the new client
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning:: You have to execute this code from DRLM Server. The password which you will be asked for is "S3cret" and "client_ipaddr" must be changed to the client ip address.

::

   $ ssh-keygen -t rsa
   $ ssh-copy-id drlm@"client_ipaddr"

Disable password login
~~~~~~~~~~~~~~~~~~~~~~

::

   $ passwd -l drlm

Add Sudo roles to DRLM user
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Edit **/etc/sudoers.d/drlm** and add the following lines

::

   Cmnd_Alias DRLM = /usr/sbin/rear* 
   drlm    ALL=(root)      NOPASSWD: DRLM
   
Change **/etc/sudoers.d/drlm** permissions

::

   $ chmod 440 /etc/sudoers.d/drlm

Client configuration
~~~~~~~~~~~~~~~~~~~~

We have to specify that this ReaR client is managed from a DRLM server. We have to edit the /etc/rear/local.conf and insert the next line.
 
::
 
   DRLM_MANAGED=y

Services
~~~~~~~~

**rpcbind**

::

        $ service rpcbind start
        $ chkconfig rpcbind on

**nfs**

::

        $ service nfs start
        $ chkconfig nfs on

Add client config file at DRLM SERVER
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. warning:: You have to do this at DRLM Server.

We have to add a new file called as "client host name".cfg at /etc/drlm/clients/
For example: If our client host name is ReaRCli1 we have to create /etc/drlm/clients/ReaRCli1.cfg and add the follwing lines.
Where CLI_NAME="Client Host Name" and SRV_NET_IP="DRLM Server IP".

::

	CLI_NAME=ReaRCli1
	SRV_NET_IP=192.168.1.38

	OUTPUT=PXE
	OUTPUT_PREFIX=$OUTPUT
	OUTPUT_PREFIX_PXE=$CLI_NAME/$OUTPUT
	OUTPUT_URL=nfs://${SRV_NET_IP}/var/lib/drlm/store/${CLI_NAME}

	BACKUP=NETFS
	NETFS_PREFIX=BKP
	BACKUP_URL=nfs://${SRV_NET_IP}/var/lib/drlm/store/${CLI_NAME}

	SSH_ROOT_PASSWORD=drlm

.. warning:: This file must be readable by Apache

::
  
        $ chmod 644 /etc/drlm/clients/ReaRCli1.cfg
