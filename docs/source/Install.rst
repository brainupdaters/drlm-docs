DRLM Installation
=================

The pourpose of this manual is explain, step by step, the installation and configuration of DRLM. At the end of this guide you should have a fully functional DRLM server.

Debian 7
--------

.. note::
   On the following steps, is assumed you have a minimal installation of Debian 7.

Install requirements
~~~~~~~~~~~~~~~~~~~~
 
::

	$ apt-get install openssh-client openssl netcat-traditional wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind isc-dhcp-server tftpd-hpa syslinux apache2

Get DRLM
~~~~~~~~

You can obtain the DRLM package building it from the source code or downloading from www.drlm.org website

**Build DEB package from Source**

::

	$ aptitude install git build-essential debhelper
	$ git clone https://github.com/brainupdaters/drlm
	$ cd drlm
	$ make deb


**Download DEB package From DRLM Web**

::

	$ wget http://www.drlm.org/downloads/drlm_1.1.1_all.deb


Install DRLM package 
~~~~~~~~~~~~~~~~~~~~

:program:`The DEB package can be installed as follows (on Debian, Ubuntu)`

Execute the next command:
::

	$ dpkg -i drlm_1.00_all.deb


DRLM Configuration 
~~~~~~~~~~~~~~~~~~

::

	$ vi /etc/drlm/local.conf

::

	STORDIR=/var/lib/drlm/store
	ARCHDIR=/var/lib/drlm/arch
	DHCP_SVC_NAME="isc-dhcp-server"
	

Add **drlm-stord** service to start up scripts.

::

	$ update-rc.d drlm-stord defaults


DRLM Components Configuration 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of: 

* GRUB
* TFTP Service
* NFS Service
* DHCP Service
* HTTP Service

Configuring loop limits
~~~~~~~~~~~~~~~~~~~~~~~

The default configuration allows up to eight active loop devices. If more than eight file-based guests or loop devices are needed the number of loop devices configured can be adjusted adding the parameter *max_loop=1024* in the **/etc/default/grub** file as follows::

	...

	GRUB_CMDLINE_LINUX="quiet max_loop=1024" ##UPDATE THIS LINE

	...

::

	$ grub-mkconfig -o /boot/grub/grub.cfg


TFTP
~~~~
You have to update the destination folder in the /etc/default/tftpd-hpa cofiguration file as follows

::

	# /etc/default/tftpd-hpa
	TFTP_USERNAME="tftp"
	TFTP_DIRECTORY="/var/lib/drlm/store
	TFTP_ADDRESS="0.0.0.0:69"
	TFTP_OPTIONS="--secure"

Directory structure::

	$ mkdir -p /var/lib/drlm/arch
	$ mkdir -p /var/lib/drlm/store/pxelinux.cfg


pxelinux.0::

	$ cp -p /usr/lib/syslinux/pxelinux.0 /var/lib/drlm/store/
	$ chmod 755 /var/lib/drlm/store/pxelinux.0


Service Management::

	$ update-rc.d tftpd-hpa defaults
	$ service tftpd-hpa restart

NFS
~~~
We don't have to configure the /etc/exports file, the file is automatically maintained by DRLM.

Service Management::

	$ update-rc.d nfs-kernel-server defaults
	$ update-rc.d rpcbind defaults

DHCP
~~~~
Same as /etc/exports file, configuration of /etc/dhcp/dhcpd.conf file is not required, the file is automatically maintained by DRLM.

Service Management::

	$ update-rc.d isc-dhcp-server defaults

HTTP
~~~~

::

	$ a2enmod ssl
	$ a2enmod rewrite

Edit /etc/apache2/apache2.conf file

::

	# Include the DRLM Configuration:
	Include /usr/share/drlm/conf/HTTP/https.conf

::

	$ rm /etc/apache2/sites-enabled/*
	

Edit /etc/apache2/ports.conf file

::
	
	#NameVirtualHost *:80
	#Listen 80

::

	$ update-rc.d apache2 defaults

::

	service apache2 restart



Restart & check all is up & running
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

	$ service tftpd-hpa status
	in.tftpd is running.
	$ service rpcbind status
	rpcbind is running.
	$ service apache2 status
	Apache2 is running (pid 2023).
	$ service nfs-kernel-server status
	nfsd not running
	$ service isc-dhcp-server status
	Status of ISC DHCP server: dhcpd is not running.


.. note::
	If DHCP or NFS not running is because there is no config yet! no worries they will be reloaded after first DRLM client will be added. 


CentOS 6, Red Hat 6
-------------------

.. note::
   On the following steps, is assumed you have a minimal installation of CentOS 6.

.. warning:: iptables and selinux has been disabled 

::

  $ cat /etc/sysconfig/selinux

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
  
  $ setenforce 0


IPTABLES

::

  $ chkconfig iptables off
  $ service iptables stop

Install requirements
~~~~~~~~~~~~~~~~~~~~
 
::

	 $  yum -y install openssh-clients openssl nc wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp tftp-server syslinux httpd xinetd nfs-utils nfs4-acl-tools mod_ssl

Get DRLM
~~~~~~~~

**Build RPM package from Source**

::

	$ yum install git rpm-build
	$ git clone https://github.com/brainupdaters/drlm
	$ cd drlm
	$ make rpm


**Download RPM package From DRLM Web**

::
	
	$ wget http://www.drlm.org/downloads/drlm-1.1.1-1git.el6.noarch.rpm


Install DRLM package 
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows (on Redhat, CentOS)`

Execute the next command:
::

	$ rpm -ivh drlm-1.1.1-1git.el6.noarch.rpm


DRLM Configuration 
~~~~~~~~~~~~~~~~~~

::

	$ vi /etc/drlm/local.conf

::

	STORDIR=/var/lib/drlm/store
	ARCHDIR=/var/lib/drlm/arch
	

DRLM Components Configuration 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section covers configuration of: 

* GRUB
* TFTP Service
* NFS Service
* DHCP Service
* HTTP Service

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

Directory structure::

	$ mkdir -p /var/lib/drlm/arch
	$ mkdir -p /var/lib/drlm/store/pxelinux.cfg


pxelinux.0::

	$ cp -p /usr/lib/syslinux/pxelinux.0 /var/lib/drlm/store/
	$ chmod 755 /var/lib/drlm/store/pxelinux.0


Service Management::

	$ chkconfig xinetd on
	$ service xinetd start

NFS
~~~
We don't have to configure the /etc/exports file, the file is automatically maintained by DRLM. 

Service Management::

        $ chkconfig nfs on
        $ service nfs start
        $ chkconfig rpcbind on
        $ service rpcbind start

DHCP
~~~~
Same as /etc/exports file, configuration of /etc/dhcp/dhcpd.conf file is not required, the file is automatically maintained by DRLM.

Service Management::

        $ chkconfig dhcpd on
        $ service dhcpd start

HTTP
~~~~

Disable the default Virtual Host and configure the server to work with SSL.

We have to edit de /etc/httpd/conf.d/ssl.conf, comment or delete the Virtual host and include the DRLM http default configuration at the end of it.

::

   Coment from here --->
   ##
   ## SSL Virtual Host Context
   ##      


        At the end of the file and insert:

::
  
        # Include the DRLM Configuration:
        Include /usr/share/drlm/conf/HTTP/https.conf

Then we have to coment the 80 port service commenting or deleting the next lines in /etc/httpd/conf/httpd.conf file.

::

   #Listen 80
   
   #ServerAdmin root@localhost

   #DocumentRoot "/var/www/html"
   
   #<Directory />
   #    Options FollowSymLinks
   #    AllowOverride None
   #</Directory>
   
   #<Directory "/var/www/html">
   #    Options Indexes FollowSymLinks
   #    AllowOverride None
   #    Order allow,deny
   #    Allow from all
   #</Directory>
   
   #ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"
   
   #<Directory "/var/www/cgi-bin">
   #    AllowOverride None
   #    Options None
   #    Order allow,deny
   #    Allow from all
   #</Directory>

To finish we have to comment the ErrorLog and CustomLog lines in /usr/share/drlm/conf/HTTP/https.conf file.

::
   
   #       ErrorLog ${APACHE_LOG_DIR}/error.log
   
   #       CustomLog ${APACHE_LOG_DIR}/ssl_access.log combined

Service Management::

        $ chkconfig httpd on
        $ service httpd start



Restart & check all is up & running
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

	$ service xinetd status
	xinetd (pid  5307) is running...
	$ service rpcbind status
	rpcbind (pid  5097) is running...
	$ service httpd status
	httpd (pid  5413) is running...
	$ service nfs status
	rpc.svcgssd is stopped
	rpc.mountd (pid 5216) is running...
	nfsd (pid 5232 5231 5230 5229 5228 5227 5226 5225) is running...
	$ service dhcpd status
	dhcpd is stopped


.. note::
	If DHCP or NFS not running is because there is no config yet! no worries they will be reloaded after first DRLM client will be added.

