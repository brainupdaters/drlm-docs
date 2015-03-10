Client Customization 
====================

In this section are shown the customizations to meet the client requirements
about the user creation, Sudo configuration and location of PXE configuration
files on the ReaR client.

.. todo:: 

   Write this section.

.. warning::

   Ensure the next services are Running

:program:`Red Hat 5`
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


Client configuration
--------------------

We have to specify that this ReaR client is managed from a DRLM server. We have to edit the /etc/rear/site.conf and insert the next line.
 
::
 
   DRLM_MANAGED=y
   
