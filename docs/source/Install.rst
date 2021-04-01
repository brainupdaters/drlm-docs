DRLM Installation
=================

The pourpose of this manual is explain, step by step, the installation and configuration of DRLM. At the end of this guide you should have a fully functional DRLM server.

Debian 9/10/11 & Ubuntu 18.04/20.04 LTS
---------------------------------------

.. note::

  On the following steps, is assumed you have a minimal installation of Debian 9/10/11 or Ubuntu 18.04/20.04 LTS.

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

	~# apt update
	~# apt upgrade
	~# apt install openssh-client openssl gawk nfs-kernel-server rpcbind isc-dhcp-server tftpd-hpa qemu-utils sqlite3 lsb-release bash-completion rsync


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


DRLM DHCP/PXE Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default DRLM does ISO and RSYNC backups. If you are interested in doing PXE rescues over the network you need to do the following additional configurations


**DHCP Configuration**

You have to update the interfaces where the DHCP server is going to listen

::

  # /etc/default/isc-dhcp-server
  INTERFACESv4="<interface-name>"


**Restart & check services**

::

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

	~#  yum -y install openssh-clients openssl wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp-server tftp-server nfs-utils nfs4-acl-tools qemu-img sqlite redhat-lsb-core bash-completion rsync


For CentOS 7 or RHEL 7:

::

	~#  yum -y install openssh-clients openssl wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp tftp-server nfs-utils nfs4-acl-tools qemu-img sqlite redhat-lsb-core bash-completion rsync


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

DRLM DHCP/PXE Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default DRLM does ISO and RSYNC backups. If you are interested in doing PXE rescues over the network you need to do the following additional configurations

**Restart & check services**

::

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

  ~# zypper in openssl wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind dhcp-server sqlite3 openssh qemu-tools tftp lsb-release bash-completion rsync


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


DRLM DHCP/PXE Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default DRLM does ISO and RSYNC backups. If you are interested in doing PXE rescues over the network you need to do the following additional configurations

* DHCP Service


**DHCP Configuration**

Same as /etc/exports file, configuration of /etc/dhcpd.conf file is not required, the file is automatically maintained by DRLM, but you have to change the location of /etc/dhcpd.conf

Edit /etc/drlm/local.conf

::

  DHCP_DIR="/etc"
  DHCP_FILE="$DHCP_DIR/dhcpd.conf"


DHCPD_INTERFACE by default is set as DHCPD_INTERFACE="" and dhcpd does not start, change it to "ANY"

Edit /etc/sysconfig/dhcpd

::

  DHCPD_INTERFACE="ANY"


**Restart & check services**

::

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

  ~# zypper in openssl wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind dhcp-server sqlite3 openssh qemu-tools tftp lsb-release bash-completion rsync


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


DRLM DHCP/PXE Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default DRLM does ISO and RSYNC backups. If you are interested in doing PXE rescues over the network you need to do the following additional configurations

**DHCP Configuration**

Same as /etc/exports file, configuration of /etc/dhcpd.conf file is not required, the file is automatically maintained by DRLM, but you have to change the location of /etc/dhcpd.conf

Edit /etc/drlm/local.conf

::

  DHCP_DIR="/etc"
  DHCP_FILE="$DHCP_DIR/dhcpd.conf"

DHCPD_INTERFACE by default is set as DHCPD_INTERFACE="" and dhcpd does not start, change it to "ANY"

Edit /etc/sysconfig/dhcpd

::

  DHCPD_INTERFACE="ANY"


**Restart & check services**

::

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
 - `873/tcp`
