DRLM Setup 
===============

Server Components Configuration 
-------------------------------
This section covers configuration of: 

* GRUB
* TFTP Service
* NFS Service
* DHCP Service


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



