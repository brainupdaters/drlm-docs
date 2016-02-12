Backup Operations
=================

DRLM backup operations allow us to remotely create new backups of
clients, enable and disable restore points and make listings of 
backups created among other things.

Run Backup
----------

This command is used to Run remote client backup from DRLM. It is 
called like this::

   $ drlm runbackup [options]

The :program:`drlm runbackup` has several options:
    
.. program:: `drlm runbackup`

.. option:: -c client_name, --client client_name    

   Select Client to remotely run backup by name. 
   
   Examples::
   
   $ drlm runbackup -c clientHost1
   $ drlm runbackup --client clientHost1

.. option:: -I client_id, --id client_id
 
   Select Client to remotely run backup by ID. 

   Examples::
  
   $ drlm runbackup -I 12
   $ drlm runbackup -id 12            

.. option:: -h, --help

   Show drlm runbackup help.

   Examples::

   $drlm runbackup -h
   $drlm runbackup --help

Delete Backup
-------------

This command is used to delete backups from DRLM database. It is 
called like this::

   $ drlm delbackup [options]

The :program:`drlm delbackup` has some requiered options:
    
.. program:: `drlm delbackup`

.. option:: -c client_name, --client client_name

   Select Client to delete the backup.

.. option:: -I backup_id, --id backup_id

   Select Backup to delete by ID.

.. option:: -A, --all

   Delete All backup.

   Examples::

   $ drlm delbackup -c clientHost1 -I 2015030121245
   $ drlm delbackup --client clientHost1 --id 2015030121245
   $ drlm delbackup -c clientHost1 -A
   $ drlm delbackup --client clientHost1 --all
   
Optional options: 

.. option:: -h, --help

   Show drlm delbackup help.                              

   Examples::

   $ drlm delbackup -h
   $ drlm delbackup --help

Backup Manager
--------------

This command is used to enable or disable clients restore points. 
Is also used to put a restore point by default. It is called like
this::

   $ drlm bkpmgr [options]

The :program:`drlm bkpmgr` has some requiered options:

.. program:: `drlm bkpmgr`

.. option:: -c client_name, --client client_name

   Select Client name to modify backup

.. option:: -I backup_id, --id backup_id

   Select Backup ID to modify

.. option:: -e, --enable

   Enable Backup

.. option:: -d, --disable              

   Disable Backup

   Examples::

   $drlm bkmgr -c clientHost1 -I 20140519065512 -e
   $drlm bkmgr --client clientHost1 -I 20140519065512 -d
   $drlm bkmgr -c clientHost1 --id 20140519065512 -e

Aditional options: 

.. option:: -P

   Set backup to persistent mode. The persistent mode is used to 
   indicate what backup will be activated by default in case of 
   service restarting. A backup stops to be in persistent mode and 
   it is replaced when creating a new one backup for the same client.

   Examples::

   $drlm bkmgr -c clientHost1 - I 20140519065512 -e -P

.. option:: -h, --help

   Show drlm bkmgr help.

   Examples::

   $ drlm bkmgr -h
   $ drlm bkmgr --help

List Backups
------------
 
This command is used to list the backups that we have stored on the
server. It is called like this::

   $ drlm listbackup [options]

The :program:`drlm listbackup` has some options:

.. program:: `drlm listbackup`

.. option:: -c client_name, --client client_name

   Select Client to list its backups.

   Exampples::

   $ drlm listbackup -c clientHost1
   $ drlm listbackup --client clientHost1

.. option:: -A, --all

   List all backups

   Examples::

   $ drlm listbackup -A
   $ drlm listbackup --all

.. option:: -h,--help

   Show this help

   Examples::
  
   $ drlm listbackup -h
   $ drlm listbackup --help                            

