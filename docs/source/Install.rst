DRLM Installation
=================

The pourpose of this manual is explain, step by step, the installation and configuration of DRLM. At the end of this guide you should have a fully functional DRLM server.

Debian 9/10 & Ubuntu 18.04/20.04 LTS
-------------------------------------

.. note::

  On the following steps, is assumed you have a minimal installation of Debian 9/10 or Ubuntu 18.04/20.04 LTS.

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

	~# apt update
	~# apt upgrade
	~# apt install openssh-client openssl gawk nfs-kernel-server rpcbind isc-dhcp-server tftpd-hpa qemu-utils sqlite3 lsb-release bash-completion


Get DRLM
~~~~~~~~

You can obtain the DRLM package building it from the source code

**Build DEB package from Source**

::

	~# apt install git build-essential debhelper golang
	~$ git clone https://github.com/brainupdaters/drlm
	~$ cd drlm
	~$ make deb
	~$ cd ..


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The DEB package can be installed as follows (on Debian, Ubuntu)`

Execute the next command:

::

	~# dpkg -i drlm_2.4.0_all.deb


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* NBD Module
* TFTP Service
* DHCP Service


NBD Configuration
~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active nbd devices. If more than eight file-based guests or nbd devices are needed the number of nbd devices configured can be adjusted adding the parameter *nbds_max=128* in the **/etc/modprobe.d/nbd.conf** file as follows

::

	options nbd max_part=8 nbds_max=128

Enable NBD module with:

::

  ~# modprobe nbd

To make it available when the server is restarted add it to modules-load . Create the file named **/etc/modules-load.d/nbd.conf** with the name of module inside.

::

  # /etc/modules-load.d/nbd.conf 
  nbd


TFTP Configuration
~~~~~~~~~~~~~~~~~~

You have to update the destination folder in the /etc/default/tftpd-hpa cofiguration file as follows

::

	# /etc/default/tftpd-hpa
	TFTP_USERNAME="tftp"
	TFTP_DIRECTORY="/var/lib/drlm/store"
	TFTP_ADDRESS="0.0.0.0:69"
	TFTP_OPTIONS="--secure"

DHCP Configuration
~~~~~~~~~~~~~~~~~~

You have to update the interfaces where the DHCP server is going to listen

::

  # /etc/default/isc-dhcp-server
  INTERFACESv4="<interface-name>"


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# systemctl restart tftpd-hpa.service
  ~# systemctl status tftpd-hpa.service

  ~# systemctl restart rpcbind.service
  ~# systemctl status rpcbind.service


.. note::
 DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.


CentOS 7/8 & RHEL 7/8
---------------------

.. note::
   On the following steps, is assumed you have a minimal installation of CentOS or RHEL 7/8.

.. warning:: SELinux has been disabled

::

  ~$ cat /etc/sysconfig/selinux

  # This file controls the state of SELinux on the system.
  # SELINUX= can take one of these three values:
  #     enforcing - SELinux security policy is enforced.
  #     permissive - SELinux prints warnings instead of enforcing.
  #     disabled - No SELinux policy is loaded.
  SELINUX=disabled
  # SELINUXTYPE= can take one of these two values:
  #     targeted - Targeted processes are protected,
  #     mls - Multi Level Security protection.
  SELINUXTYPE=targeted

::

  ~# setenforce 0


.. warning:: Firewall has been disabled

::

  ~# systemctl stop firewalld
  ~# systemctl disable firewalld
      Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
      Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.

.. note::

  It is not a requirement to disable SELinux and Firewall, but to work with DRLM Server must be properly configured. We have disabled this features for easier installation.


Install requirements
~~~~~~~~~~~~~~~~~~~~

For CentOS 8 or RHEL 8:

::

	~#  yum -y install openssh-clients openssl wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp-server tftp-server xinetd nfs-utils nfs4-acl-tools qemu-img sqlite redhat-lsb-core bash-completion


For CentOS 7 or RHEL 7:

::

	~#  yum -y install openssh-clients openssl wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp tftp-server xinetd nfs-utils nfs4-acl-tools qemu-img sqlite redhat-lsb-core bash-completion


Get DRLM
~~~~~~~~

**Build RPM package from Source**

::

  ~# yum -y install epel-release
  ~# yum -y install git rpm-build golang
  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows (on Redhat, CentOS)`

Execute the next command in CentOS 8 or RHEL 8:

::

	~# rpm -ivh drlm-2.4.0-1git.el8.noarch.rpm

Or the next one in CentOS 7 or RHEL 7:

::

	~# rpm -ivh drlm-2.4.0-1git.el7.noarch.rpm


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* NBD Module
* TFTP Service


NBD Configuration 
~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active nbd devices. If more than eight file-based guests or nbd devices are needed the number of nbd devices configured can be adjusted adding the parameter *nbds_max=128* in the **/etc/modprobe.d/nbd.conf** file as follows

::

	options nbd max_part=8 nbds_max=128

Enable NBD module with:

::

  ~# modprobe nbd

To make it available when the server is restarted add it to modules-load . Create the file named **/etc/modules-load.d/nbd.conf** with the name of module inside.

::

  # /etc/modules-load.d/nbd.conf 
  nbd

TFTP Configuration
~~~~~~~~~~~~~~~~~~

You have to update the /etc/xinetd.d/tftp cofiguration file as follows:

::

  service tftp
  {
          socket_type = dgram
          protocol = udp
          wait = yes
          user = root
          server = /usr/sbin/in.tftpd
          server_args = -s /var/lib/drlm/store
          disable = no
          per_source = 11
          cps = 100 2
          flags = IPv4
  }


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# systemctl restart xinetd.service
  ~# systemctl enable xinetd.service
  ~# systemctl status xinetd.service

  ~# systemctl restart rpcbind.service
  ~# systemctl enable rpcbind.service
  ~# systemctl status rpcbind.service


.. note::
	DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.


SLES 12 & OpenSUSE Leap 42
--------------------------

.. note::
      On the following steps, is assumed you have a minimal SLES 12 or OpenSUSE Leap 42

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

        ~# zypper in openssl wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind dhcp-server sqlite3 openssh qemu-tools tftp xinetd lsb-release bash-completion


Get DRLM
~~~~~~~~

You can obtain the DRLM package building it from the source code.

**Build RPM package from Source**

::

  ~# zypper install git-core rpm-build golang
  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make rpm

You can obtain the RPM DRLM package from www.drlm.org website


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows (on SLES 12 SP1)`

Execute the next command:
::

        ~# zypper in drlm-2.4.0-1git.noarch.rpm


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* NBD Module
* TFTP Service
* DHCP Service


NBD Configuration 
~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active nbd devices. If more than eight file-based guests or nbd devices are needed the number of nbd devices configured can be adjusted adding the parameter *nbds_max=128* in the **/etc/modprobe.d/nbd.conf** file as follows

::

	options nbd max_part=8 nbds_max=128

Enable NBD module with:

::

  ~# modprobe nbd

To make it available when the server is restarted add it to modules-load . Create the file named **/etc/modules-load.d/nbd.conf** with the name of module inside.

::

  # /etc/modules-load.d/nbd.conf 
  nbd


TFTP Configuration
~~~~~~~~~~~~~~~~~~
You have to update the /etc/xinetd.d/tftp cofiguration file as follows:

::

	service tftp
	{
		socket_type		= dgram
		protocol		= udp
		wait			= yes
		flags			= IPv6 IPv4
		user			= root
		server			= /usr/sbin/in.tftpd
		server_args		= -u tftp -s /var/lib/drlm/store
		per_source		= 11
		cps			= 100 2
		disable			= no
	}


DHCP Configuration
~~~~~~~~~~~~~~~~~~
Same as /etc/exports file, configuration of /etc/dhcpd.conf file is not required, the file is automatically maintained by DRLM.

but you have to change the location of /etc/dhcpd.conf

Edit /etc/drlm/local.conf

::

     DHCP_DIR="/etc"
     DHCP_FILE="$DHCP_DIR/dhcpd.conf"


DHCPD_INTERFACE by default is set as DHCPD_INTERFACE="" and dhcpd does not start, change it to "ANY"

Edit /etc/sysconfig/dhcpd

::

     DHCPD_INTERFACE="ANY"


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# systemctl restart xinetd.service
  ~# systemctl status xinetd.service

  ~# systemctl restart rpcbind.service
  ~# systemctl status rpcbind.service

  ~# systemctl enable nfs-server
  ~# systemctl start nfs-server
  ~# systemctl status nfs-server


.. note::
  DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.


SLES 15 & OpenSUSE Leap 15
--------------------------

.. note::
  On the following steps, is assumed you have a minimal SLES 15 or OpenSUSE Leap 15

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

  ~# zypper in openssl wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind dhcp-server sqlite3 openssh qemu-tools tftp lsb-release bash-completion


Get DRLM
~~~~~~~~

You can obtain the DRLM package building it from the source code.

**Build RPM package from Source**

::

  ~# zypper install git-core rpm-build go
  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make rpm

You can obtain the RPM DRLM package from www.drlm.org website


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows`

Execute the next command:
::

        ~# zypper in drlm-4.0-1git.noarch.rpm

.. note::
      You will need to accept to install the package even though it's not signed


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* NBD Module
* TFTP Service


NBD Configuration 
~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active nbd devices. If more than eight file-based guests or nbd devices are needed the number of nbd devices configured can be adjusted adding the parameter *nbds_max=128* in the **/etc/modprobe.d/nbd.conf** file as follows

::

	options nbd max_part=8 nbds_max=128

Enable NBD module with:

::

  ~# modprobe nbd

To make it available when the server is restarted add it to modules-load . Create the file named **/etc/modules-load.d/nbd.conf** with the name of module inside.

::

  # /etc/modules-load.d/nbd.conf 
  nbd


TFTP Configuration
~~~~~~~~~~~~~~~~~~
You have to update the TFTP_DIRECTORY variable in the /etc/sysconfig/tftp cofiguration file as follows:

::

  ...
  TFTP_DIRECTORY="/var/lib/drlm/store"
  ...


DHCP Configuration
~~~~~~~~~~~~~~~~~~
Same as /etc/exports file, configuration of /etc/dhcpd.conf file is not required, the file is automatically maintained by DRLM.

but you have to change the location of /etc/dhcpd.conf

Edit /etc/drlm/local.conf

::

     DHCP_DIR="/etc"
     DHCP_FILE="$DHCP_DIR/dhcpd.conf"


DHCPD_INTERFACE by default is set as DHCPD_INTERFACE="" and dhcpd does not start, change it to "ANY"

Edit /etc/sysconfig/dhcpd

::

     DHCPD_INTERFACE="ANY"


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# systemctl restart tftp.service
  ~# systemctl status tftp.service

  ~# systemctl restart rpcbind.service
  ~# systemctl status rpcbind.service

  ~# systemctl enable nfs-server
  ~# systemctl start nfs-server
  ~# systemctl status nfs-server


.. note::
    DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.


Firewalld Configuration
-----------------------

If you don't want to disable Firewalld, you will need to accept connections on the following ports:
 - `53/tcp`
 - `53/udp`
 - `69/tcp`
 - `69/udp`
 - `443/tcp`
