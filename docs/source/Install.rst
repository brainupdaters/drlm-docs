DRLM Installation
=================

The pourpose of this manual is explain, step by step, the installation and configuration of DRLM. At the end of this guide you should have a fully functional DRLM server.

Debian 8/9 & Ubuntu 16.04/18.04 LTS
-----------------------------------

.. note::
   On the following steps, is assumed you have a minimal installation of Debian 8/9 or Ubuntu 16.04/18.04 LTS.

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

	~# dpkg -i drlm_2.3.1_all.deb


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* GRUB
* TFTP Service


Configuring loop limits
~~~~~~~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active loop devices. If more than eight file-based guests or loop devices are needed the number of loop devices configured can be adjusted adding the parameter *max_loop=1024* in the **/etc/default/grub** file as follows::

	...

	GRUB_CMDLINE_LINUX="quiet max_loop=1024" ##UPDATE THIS LINE

	...

::

	~# grub-mkconfig -o /boot/grub/grub.cfg


TFTP
~~~~
You have to update the destination folder in the /etc/default/tftpd-hpa cofiguration file as follows

::

	# /etc/default/tftpd-hpa
	TFTP_USERNAME="tftp"
	TFTP_DIRECTORY="/var/lib/drlm/store"
	TFTP_ADDRESS="0.0.0.0:69"
	TFTP_OPTIONS="--secure"

DHCP
~~~~
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


Debian 7 & Ubuntu 14.04 LTS
---------------------------

.. note::
   On the following steps, is assumed you have a minimal installation of Debian 7 or Ubuntu 14.04.

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

	~# apt-get update
	~# apt-get upgrade
	~# apt-get install openssh-client openssl wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind isc-dhcp-server tftpd-hpa qemu-utils sqlite3 lsb-release bash-completion


Get DRLM
~~~~~~~~

You can obtain the DRLM package building it from the source code

**Build DEB package from Source**

::

	~# apt-get install git build-essential debhelper golang
	~$ git clone https://github.com/brainupdaters/drlm
	~$ cd drlm
	~$ make deb
	~$ cd ..


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The DEB package can be installed as follows (on Debian, Ubuntu)`

Execute the next command:
::

	~# dpkg -i drlm_2.3.1_all.deb


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* GRUB
* TFTP Service

Configuring loop limits
~~~~~~~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active loop devices. If more than eight file-based guests or loop devices are needed the number of loop devices configured can be adjusted adding the parameter *max_loop=1024* in the **/etc/default/grub** file as follows::

	...

	GRUB_CMDLINE_LINUX="max_loop=1024" ##UPDATE THIS LINE

	...

::

	~# grub-mkconfig -o /boot/grub/grub.cfg


TFTP
~~~~
You have to update the destination folder in the /etc/default/tftpd-hpa cofiguration file as follows

::

	# /etc/default/tftpd-hpa
	TFTP_USERNAME="tftp"
	TFTP_DIRECTORY="/var/lib/drlm/store"
	TFTP_ADDRESS="0.0.0.0:69"
	TFTP_OPTIONS="--secure"


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  ~# service tfrpd-hpa restart
  ~# service tftpd-hpa status
  in.tftpd is running.
  ~# service rpcbind restart
  ~# service rpcbind status
  rpcbind is running.


.. note::

 	 DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.


CentOS 7 & RHEL 7
-----------------

.. note::
   On the following steps, is assumed you have a minimal installation of CentOS or RHEL 7.

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

::

	 ~#  yum -y install openssh-clients openssl wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp tftp-server xinetd nfs-utils nfs4-acl-tools qemu-img sqlite redhat-lsb-core bash-completion


Get DRLM
~~~~~~~~

**Build RPM package from Source**

::

    ~# yum install epel-release
    ~# yum install git rpm-build golang
    ~$ git clone https://github.com/brainupdaters/drlm
    ~$ cd drlm
    ~$ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows (on Redhat, CentOS)`

Execute the next command:
::

	~# rpm -ivh drlm-2.3.1-1git.el7.noarch.rpm


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* GRUB
* TFTP Service


Configuring loop limits
~~~~~~~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active loop devices. If more than eight file-based guests or loop devices are needed the number of loop devices configured can be adjusted adding the parameter *max_loop=1024* in the **/etc/default/grub** file as follows::

	...

	GRUB_CMDLINE_LINUX="......... max_loop=1024" ##UPDATE THIS LINE ADDING MAX_LOOP=1024 PARAMETER

	...

::

	~# grub2-mkconfig -o /boot/grub2/grub.cfg

TFTP
~~~~
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


CentOS 6 & RHEL 6
-----------------


.. note::
   On the following steps, is assumed you have a minimal installation of CentOS or RHEL 6.

.. warning:: Iptables and SELinux has been disabled

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

  ~# chkconfig iptables off
  ~# service iptables stop

.. note::

   It is not a requirement to disable SELinux and Iptables, but to work with DRLM Server must be properly configured. We have disabled these features for easier installation.


Install requirements
~~~~~~~~~~~~~~~~~~~~

::

	 ~#  yum -y install openssh-clients openssl wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp tftp-server xinetd nfs-utils nfs4-acl-tools qemu-img sqlite redhat-lsb-core bash-completion


Get DRLM
~~~~~~~~

**Build RPM package from Source**

::

    ~# yum install epel-release
    ~# yum install git rpm-build golang
    ~$ git clone https://github.com/brainupdaters/drlm
    ~$ cd drlm
    ~$ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows (on RHEL, CentOS)`

Execute the next command:
::

	~# rpm -ivh drlm-2.3.1-1git.el7.noarch.rpm


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* GRUB
* TFTP Service


Configuring loop limits
~~~~~~~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active loop devices. If more than eight clients are needed, the number of loop devices configured can be adjusted adding the parameter *max_loop=1024* in the **/etc/grub.conf** file as follows:

::

  title Red Hat Enterprise Linux (2.6.32-358.el6.x86_64)
  root (hd0,0)
  kernel /vmlinuz-2.6.32-358.el6.x86_64 ro root=/dev/mapper/vgroot-lvroot rd_NO_LUKS LANG=en_US.UTF-8  KEYBOARDTYPE=pc KEYTABLE=es rd_NO_MD rd_LVM_LV=vgroot/lvswap SYSFONT=latarcyrheb-sun16 crashkernel=auto rd_LVM_LV=vgroot/lvroot rd_NO_DM rhgb quiet max_loop=1024
  initrd /initramfs-2.6.32-358.el6.x86_64.img


TFTP
~~~~
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

  ~# service xinetd restart
  ~# service xinetd status
  xinetd (pid  5307) is running...
  ~# service rpcbind restart
  ~# service rpcbind status
  rpcbind (pid  5097) is running...


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

        ~# zypper in drlm-2.3.1-1git.noarch.rpm


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* GRUB
* TFTP Service


Configuring loop limits
~~~~~~~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active loop devices. If more than eight file-based guests or loop devices are needed the number of loop devices configured can be adjusted adding the parameter *max_loop=1024* in the **/etc/default/grub** file as follows::

        ...

        GRUB_CMDLINE_LINUX="... quiet max_loop=1024" ##UPDATE THIS LINE

        ...

::

        ~# grub2-mkconfig -o /boot/grub2/grub.cfg


TFTP
~~~~
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


DHCP
~~~~
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

        ~# zypper in openssl wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind dhcp-server sqlite3 openssh qemu-tools tftp xinetd lsb-release bash-completion


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

        ~# zypper in drlm-2.3.1-1git.noarch.rpm

.. note::
      You will need to accept to install the package even though it's not signed


DRLM Components Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of:

* GRUB
* TFTP Service


Configuring loop limits
~~~~~~~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active loop devices. If more than eight file-based guests or loop devices are needed the number of loop devices configured can be adjusted adding the parameter *max_loop=1024* in the **/etc/default/grub** file as follows::

        ...

        GRUB_CMDLINE_LINUX="... quiet max_loop=1024" ##UPDATE THIS LINE

        ...

::

        ~# grub2-mkconfig -o /boot/grub2/grub.cfg


TFTP
~~~~
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


DHCP
~~~~
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


Firewalld Configuration
-----------------------

If you don't want to disable Firewalld, you will need to accept connections on the following ports:
 - `53/tcp`
 - `53/udp`
 - `69/tcp`
 - `69/udp`
 - `443/tcp`
