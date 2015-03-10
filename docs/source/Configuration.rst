DRLM Setup 
==========

Server Components Configuration 
-------------------------------
This section covers configuration of: 

* GRUB
* TFTP Service
* NFS Service
* DHCP Service
* HTTP Service


Configuring loop limits
-----------------------

The default configuration allows up to eight active loop devices. If more than eight clients are needed the number of loop devices configured can be adjusted adding the parameter *max_loop=1024* in the /etc/grub.conf file as follows::

        title Red Hat Enterprise Linux (2.6.32-358.el6.x86_64) 
        root (hd0,0) 
        kernel /vmlinuz-2.6.32-358.el6.x86_64 ro root=/dev/mapper/vgroot-lvroot rd_NO_LUKS LANG=en_US.UTF-8  KEYBOARDTYPE=pc KEYTABLE=es rd_NO_MD rd_LVM_LV=vgroot/lvswap SYSFONT=latarcyrheb-sun16 crashkernel=auto rd_LVM_LV=vgroot/lvroot rd_NO_DM rhgb quiet max_loop=1024 
        initrd /initramfs-2.6.32-358.el6.x86_64.img


TFTP
----

Configuration Path: /etc/xinetd.d/tftp

Service Configuration::

	service tftp 
	{ 
       	socket_type = dgram 
      	protocol = udp 
        wait = yes 
        user = root 
        server = /usr/sbin/in.tftpd 
        server_args = -s /DRLM/STORE
        disable = no 
        per_source = 11 
        cps = 100 2 
	   	flags = IPv4 
	}

Directory structure::

	$ mkdir -p /DRLM/ARCH
	$ mkdir -p /DRLM/STORE/pxelinux.cfg

pxelinux.0::

	$ cp -p /usr/share/syslinux/pxelinux.0 /DRLM/STORE

Service Management::

	$ chkconfig xinetd on
	$ service xinetd {start|stop|status|restart|condrestart|reload}


NFS
----

Since DRLM v1.0 we don't have to configure /etc/exports file anymore, the file is automatically configured after the client is created. 

Service Management::

	$ chkconfig nfs on
	$ service nfs {start|stop|status|restart|reload|force-reload|condrestart|try-restart|condstop}
	$ chkconfig rpcbind on
	$ service rpcbind {start|stop|status|restart|reload|condrestart}


DHCP
----

Same as /etc/exports, we don't have to configure  /etc/dhcp/dhcpd.conf file, the file is automatically configured during the client creation.

Service Management::

	$ chkconfig dhcpd on
	$ service dhcpd {start|stop|restart|force-reload|condrestart|try-restart|configtest|status}


HTTP
----

We have to disable the default Virtual Host and configure the server to work with SSL.

.. describe:: CentOS

We have to edit de /etc/httpd/conf.d/ssl.conf, comment or delete the Virtual host and include the DRLM http default configuration at the end of it.

::

   Coment from here --->
   ##
   ## SSL Virtual Host Context
   ##
   
   To the end of the file and insert:

   # Include the DRLM Configuration:
   Include /usr/share/drlm/conf/HTTP/https.conf

Then we have to coment the 80 port service comenting or deleting the next lines in /etc/httpd/conf/httpd.conf file.

::

   #Listen 80
   
   #ServerAdmin root@localhost

   #DocumentRoot "/var/www/html"
   
   #<Directory />
   #    Options FollowSymLinks
   #    AllowOverride None
   #</Directory>
   
   #<Directory "/var/www/html">
   #    Options Indexes FollowSymLinks
   #    AllowOverride None
   #    Order allow,deny
   #    Allow from all
   #</Directory>
   
   #ScriptAlias /cgi-bin/ "/var/www/cgi-bin/"
   
   #<Directory "/var/www/cgi-bin">
   #    AllowOverride None
   #    Options None
   #    Order allow,deny
   #    Allow from all
   #</Directory>

To finish we have to comment the ErrorLog and CustomLog lines in /usr/share/drlm/conf/HTTP/https.conf file.

::
   
   #       ErrorLog ${APACHE_LOG_DIR}/error.log
   
   #       CustomLog ${APACHE_LOG_DIR}/ssl_access.log combined

Somethimes the SELinux can block the not default services. We will put the SELinux in permissive mode with the next command.

::
   
   setenforce 0
   

.. describe:: Debian

First of all we have to activate the SSL and rewrite modules.

::

	$ a2enmod ssl
	$ a2enmod rewrite

Then we have to include the DRLM http default configuration at the end of /etc/apache2/apache2.conf. We can do that copying the next two lines at the end of this file.

::

   # Include the DRLM Configuration:
   Include /usr/share/drlm/conf/HTTP/https.conf

We also have to remove all the links in the apache sites-enabled folder. 

::

   $ rm /etc/apache2/sites-enabled/*

The last configuration that we have to do is coment the 80 port in /etc/apache2/ports.conf file.

::
   
   # NameVirtualHost *:80
   # Listen 80