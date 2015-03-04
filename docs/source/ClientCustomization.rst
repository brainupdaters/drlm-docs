Client Customization 
====================

In this section are shown the customizations to meet the client requirements
about the user creation, Sudo configuration and location of PXE configuration
files on the ReaR client.

.. todo:: 

   Write this section.

.. warning::

   Ensure the next services are Running

*Red Hat 5*
::

   $ service portmap start
   $ chkconfig portmap on

:program:`Red Hat 6`
::

   $ service rpcbind start
   $ chkconfig rpcbind on

:program:`Debian / Ubuntu`
::

   $ service rpcbind start   
   $ update-rc.d rpcbind defaults
   


Create User
-----------
::

   $ useradd -d /home/drlm -c "DRLM User Agent" -m -s /bin/bash -p $(echo S3cret | openssl passwd -1 -stdin) drlm

Disable password aging for drlm user
------------------------------------
::

   $ chage -I -1 -m 0 -M 99999 -E -1 drlm


Copy rsa key from DRLM Server to the new client
-----------------------------------------------
::

   $ ssh-keygen -t rsa
   $ ssh-copy-id drlm@”clientname”


   
SUDO DRLM user configuration
----------------------------

Create `/etc/sudoers.d` directory if not exists
::

   $ mkdir /etc/sudoers.d
   $ chmod 750 /etc/sudoers.d
   $ echo "#includedir /etc/sudoers.d" >> /etc/sudoers

Add roles to user drlm
::

   $ vi /etc/sudoers.d/drlm

::

   Cmnd_Alias DRLM = /usr/sbin/rear* 
   drlm    ALL=(root)      NOPASSWD: DRLM
   
::

   $ chmod 440 /etc/sudoers.d/drlm


Disable password login
::

   $ passwd -l drlm


BOOT client config
------------------

*81_create_pxelinux_cfg.sh* must be changed to show the path to the DRLM Server and ensure the system is booting from the correct initrd file.
 
::
 
   $ vi /usr/share/rear/output/PXE/default/81_create_pxelinux_cfg.sh
   
::
 
    <<<< replace <<<< 
    kernel $PXE_KERNEL 
    append initrd=$PXE_INITRD root=/dev/ram0 vga=normal rw $KERNEL_CMDLINE 
    >>>>>  with  >>>> 
    kernel *$DRLM_NAME/$OUTPUT_PREFIX/$PXE_KERNEL* 
    append initrd=*$DRLM_NAME/$OUTPUT_PREFIX*/$PXE_INITRD root=/dev/ram0 vga=normal rw $KERNEL_CMDLINE 
 






