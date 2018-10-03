DRLM Installation
=================

The pourpose of this manual is explain, step by step, the installation and configuration of DRLM. At the end of this guide you should have a fully functional DRLM server.

Debian 8/9 & Ubuntu 16.04/18.04 LTS
-----------------------------------

.. note::
   On the following steps, is assumed you have a minimal installation of Debian 8/9 or Ubuntu 16.04.

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

	$ apt update
	$ apt upgrade
	$ apt install openssh-client openssl gawk nfs-kernel-server rpcbind isc-dhcp-server tftpd-hpa apache2 qemu-utils sqlite3 lsb-release bash-completion


Get DRLM
~~~~~~~~

You can obtain the DRLM package building it from the source code

**Build DEB package from Source**

::

	$ apt install git build-essential debhelper
	$ git clone https://github.com/brainupdaters/drlm
	$ cd drlm
	$ make deb


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The DEB package can be installed as follows (on Debian, Ubuntu)`

Execute the next command:
::

	$ dpkg -i drlm_2.2.1_all.deb


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
	TFTP_DIRECTORY="/var/lib/drlm/store"
	TFTP_ADDRESS="0.0.0.0:69"
	TFTP_OPTIONS="--secure"


NFS
~~~
We don't have to configure the /etc/exports file, the file is automatically maintained by DRLM.


DHCP
~~~~
Same as /etc/exports file, configuration of /etc/dhcp/dhcpd.conf file is not required, the file is automatically maintained by DRLM.


HTTP
~~~~

::

	$ a2enmod ssl
	$ a2enmod rewrite
	$ a2enmod cgid
	$ a2enmod reqtimeout

Edit /etc/apache2/apache2.conf file

::

	# Include the DRLM Configuration:
	Include /usr/share/drlm/conf/HTTP/https.conf

::

	$ rm /etc/apache2/sites-enabled/*


Edit /etc/apache2/ports.conf file

::

	#Listen 80


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  $ systemctl restart tftpd-hpa.service
  $ systemctl status tftpd-hpa.service

  $ systemctl restart rpcbind.service
  $ systemctl status rpcbind.service

  $ systemctl restart apache2.service
  $ systemctl status apache2.service


.. note::
 DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.


Debian 7 & Ubuntu 14.04 LTS
---------------------------

.. note::
   On the following steps, is assumed you have a minimal installation of Debian 7 or Ubuntu 14.04.

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

	$ apt-get update
	$ apt-get upgrade
	$ apt-get install openssh-client openssl wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind isc-dhcp-server tftpd-hpa apache2 qemu-utils sqlite3 lsb-release bash-completion


Get DRLM
~~~~~~~~

You can obtain the DRLM package building it from the source code

**Build DEB package from Source**

::

	$ apt-get install git build-essential debhelper
	$ git clone https://github.com/brainupdaters/drlm
	$ cd drlm
	$ make deb


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The DEB package can be installed as follows (on Debian, Ubuntu)`

Execute the next command:
::

	$ dpkg -i drlm_2.2.1_all.deb


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
	TFTP_DIRECTORY="/var/lib/drlm/store"
	TFTP_ADDRESS="0.0.0.0:69"
	TFTP_OPTIONS="--secure"


NFS
~~~
We don't have to configure the /etc/exports file, the file is automatically maintained by DRLM.


DHCP
~~~~
Same as /etc/exports file, configuration of /etc/dhcp/dhcpd.conf file is not required, the file is automatically maintained by DRLM.


HTTP
~~~~

::

	$ a2enmod ssl
	$ a2enmod rewrite
	$ a2enmod cgi
	$ a2enmod reqtimeout

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


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  $ service tfrpd-hpa restart
  $ service tftpd-hpa status
  in.tftpd is running.
  $ service rpcbind restart
  $ service rpcbind status
  rpcbind is running.
  $ service apache2 restart
  $ service apache2 status
  Apache2 is running (pid 2023).


.. note::

 	 DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.


CentOS 7 & RHEL 7
-----------------

.. note::
   On the following steps, is assumed you have a minimal installation of CentOS or RHEL 7.

.. warning:: SELinux has been disabled

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

.. note::

   It is not a requirement to disable SELinux, but to work with DRLM Server must be properly configured. We have disabled this feature for easier installation.


Install requirements
~~~~~~~~~~~~~~~~~~~~

::

	 $  yum -y install openssh-clients openssl wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp tftp-server httpd xinetd nfs-utils nfs4-acl-tools mod_ssl qemu-img sqlite redhat-lsb-core bash-completion


Get DRLM
~~~~~~~~

**Build RPM package from Source**

::

    $ yum install git rpm-build
    $ git clone https://github.com/brainupdaters/drlm
    $ cd drlm
    $ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows (on Redhat, CentOS)`

Execute the next command:
::

	$ rpm -ivh drlm-2.2.1-1git.el7.centos.noarch.rpm


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

	GRUB_CMDLINE_LINUX="......... max_loop=1024" ##UPDATE THIS LINE ADDING MAX_LOOP=1024 PARAMETER

	...

::

	$ grub2-mkconfig -o /boot/grub2/grub.cfg

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


NFS
~~~
We don't have to configure the /etc/exports file, the file is automatically maintained by DRLM.


DHCP
~~~~
Same as /etc/exports file, configuration of /etc/dhcp/dhcpd.conf file is not required, the file is automatically maintained by DRLM.


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

To finish we have to add APACHE_LOG_DIR variable to /etc/sysconfig/httpd

::

  echo "APACHE_LOG_DIR=logs" >> /etc/sysconfig/httpd


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  $ systemctl enable xinetd.service
  $ systemctl restart xinetd.service

  $ systemctl enable rpcbind.service
  $ systemctl restart rpcbind.service

  $ systemctl enable httpd.service
  $ systemctl restart httpd.service


.. note::
	DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.


CentOS 6 & RHEL 6
-----------------


.. note::
   On the following steps, is assumed you have a minimal installation of CentOS or RHEL 6.

.. warning:: Iptables and SELinux has been disabled

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

.. note::

   It is not a requirement to disable SELinux and Iptables, but to work with DRLM Server must be properly configured. We have disabled these features for easier installation.

Iptables

::

  $ chkconfig iptables off
  $ service iptables stop

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

	 $  yum -y install openssh-clients openssl wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp tftp-server httpd xinetd nfs-utils nfs4-acl-tools mod_ssl qemu-img sqlite redhat-lsb-core bash-completion


Get DRLM
~~~~~~~~

**Build RPM package from Source**

::

    $ yum install git rpm-build
    $ git clone https://github.com/brainupdaters/drlm
    $ cd drlm
    $ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows (on RHEL, CentOS)`

Execute the next command:
::

	$ rpm -ivh drlm-2.2.1-1git.el6.noarch.rpm


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


NFS
~~~
We don't have to configure the /etc/exports file, the file is automatically maintained by DRLM.


DHCP
~~~~
Same as /etc/exports file, configuration of /etc/dhcp/dhcpd.conf file is not required, the file is automatically maintained by DRLM.


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

To finish we have to add APACHE_LOG_DIR variable to /etc/sysconfig/httpd

::

  echo "APACHE_LOG_DIR=logs" >> /etc/sysconfig/httpd



Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  $ service xinetd restart
  $ service xinetd status
  xinetd (pid  5307) is running...
  $ service rpcbind restart
  $ service rpcbind status
  rpcbind (pid  5097) is running...
  $ service httpd restart
  $ service httpd status
  httpd (pid  5413) is running...


.. note::
	DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.

SLES 12 & OpenSUSE Leap 42
--------------------------

.. note::
      On the following steps, is assumed you have a minimal SLES 12 or OpenSUSE Leap 42

Install requirements
~~~~~~~~~~~~~~~~~~~~

::

        $ zypper in openssl wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind dhcp-server sqlite3 apache2 openssh qemu-tools tftp xinetd lsb-release bash-completion


Get DRLM
~~~~~~~~

You can obtain the DRLM package building it from the source code.

**Build RPM package from Source**

::

  $ zypper install git-core rpm-build
  $ git clone https://github.com/brainupdaters/drlm
  $ cd drlm
  $ make rpm

You can obtain the RPM DRLM package from www.drlm.org website


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

:program:`The RPM package can be installed as follows (on SLES 12 SP1)`

Execute the next command:
::

        $ zypper in drlm-2.2.1-1git.noarch.rpm


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

        GRUB_CMDLINE_LINUX="... quiet max_loop=1024" ##UPDATE THIS LINE

        ...

::

        $ grub2-mkconfig -o /boot/grub2/grub.cfg


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



NFS
~~~
We don't have to configure the /etc/exports file, the file is automatically maintained by DRLM.


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


HTTP
~~~~

::

       $ a2enmod ssl
       $ a2enmod rewrite
       $ a2enmod cgi
       $ a2enmod mod_access_compat
       $ a2enmod reqtimeout

Edit /etc/apache2/httpd.conf file

::

        # Include the DRLM Configuration:
        Include /usr/share/drlm/conf/HTTP/https.conf

To finish we have to add APACHE_LOG_DIR variable to /etc/sysconfig/apache2

::

  echo "APACHE_LOG_DIR=/var/log/apache2" >> /etc/sysconfig/apache2



Edit /etc/apache2/listen.conf file

::

       #Listen 80
       Listen 443

       #Listen 80


       <IfDefine SSL>
           <IfDefine !NOSSL>
       	       <IfModule mod_ssl.c>

       	           Listen 443

       	       </IfModule>
           </IfDefine>
       </IfDefine>


Restart & check services
~~~~~~~~~~~~~~~~~~~~~~~~

::

  $ systemctl restart xinetd.service
  $ systemctl status xinetd.service

  $ systemctl restart rpcbind.service
  $ systemctl status rpcbind.service

  $ systemctl restart apache2.service
  $ systemctl status apache2.service

  $ systemctl enable nfs-server
  $ systemctl start nfs-server
  $ systemctl status nfs-server


.. note::
    DHCP and NFS servers are not running because there is no config yet! no worries they will be reloaded automatically after first DRLM client will be added.
