Client Operations
=================

DRLM client operations allow us to add, remove, modify and 
list clients of database.

Add Client
----------

This command is used to add clients to DRLM database. It is 
called like this::

   $ drlm addclient [options]

If the client you wish to add is online (network reachable), you will only need to set its IP in CIDR notation in order to add it to the database. It will then automatically fetch and prompt all the required client parameters (hostname, network and MAC address), leaving to you the option to keep and save those parameters or to enter them manually in case you refuse. 

In this case, the :program:`drlm addclient` has the following required options:

 .. program:: `drlm addclient`

 .. option:: -i ip_in_CIDR_format, --ipaddr ip_in_CIDR_format

 Examples::

   $ drlm addclient -I 192.168.0.15/24

If the :program:`drlm addclient` does not correctly fetch the client's hostname, you can set it manually in the same command:

   $ $ drlm addclient -I 192.168.0.15/24 -c rear-debian

If the client is not network reachable when you want to register it in the database or you wish to manually enter all the required parameters, you can do it with the required options available:  
    
.. program:: `drlm addclient`

.. option:: -c client_name, --client client_name

   Select Client name to add. Remember that it has to **match the client's hostname**.

.. option:: -i ip, --ipaddr ip

   Client IP address.

.. option:: -M mac_address, --macaddr mac_address

   Client MAC address.

.. option:: -n network_name, --netname network_name

   Client NETWORK.                               

   Examples:: 

   $ drlm addclient -c clientHost1 -M 00-40-77-DB-33-38 -i 13.74.90.10 -n vlan12
   $ drlm addclient --client clientHost1 --macaddr 00-40-77-DB-33-38 -i 13.74.90.10 -n vlan12

   .. warning::

      If the network_name doesn't exist in DRLM database you will get an error. First
      of all register the network where the client will be registered.

   .. warning::

      We have to manually add to the client configuration file in the DRLM server called /etc/drlm/clients/client_name.cfg with the next content:

      OUTPUT=PXE
      OUTPUT_PREFIX=PXE
      BACKUP=NETFS
      NETFS_PREFIX=BKP
      BACKUP_URL=nfs://SERVER_IP/DRLM/STORE/client_name
      OUTPUT_URL=nfs://SERVER_IP/DRLM/STORE/client_name
      OUTPUT_PREFIX_PXE=client_name/$OUTPUT_PREFIX

      You have to replace the SERVER_IP for the IP of the DRLM server and the client_name for the client host name.

Optional options: 

.. option:: -h, --help

   Show drlm addclient help.

   Examples::

   $ drlm addclient -h
   $ drlm addclient --help


Install Client
--------------

This command is used to install and configure DRLM and ReaR on a remote
Server. It is called like this::

   $ drlm instclient [options]

The :program:`drlm instclient` has some requiered options:

.. program::  `drlm instclient`  

.. option:: -c client_name, --client client_name

   Select Client name to add.

.. option:: -I client_id, --id client_id

   Client Id.

.. note:: Since Debian doesn't have the ReaR package on its repositories, 
      the following option is a requirement also :program:`-U|--url_rear <URL_REAR>`

Optional options:

.. option:: -u user, --user user

   User with admin privileges to install and configure software

.. option:: -d drlm_user, --drlm_user drlm_user

   Force drlm_user name , default is drlm  

.. note:: if not user is specified root will be used.

.. option:: -U url_rear, --url_rear url_rear

   rpm or deb package for specific distro. For example http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb

.. option:: -h, --help

   Show drlm instclient help.

   Examples::
  
   $ drlm instclient -h
   $ drlm instclient -c ReaRCli1 -u admin -U http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb
   $ drlm instclient -c ReaRCli2


Delete Client
-------------

This command is used to delete clients from DRLM database. It is 
called like this::

   $ drlm delclient [options]

The :program:`drlm delclient` has some required options:
    
.. program:: `drlm delclient`

.. option:: -c client_name, --client client_name

   Select Client to delete by NAME.

.. option:: -I client_id, --id client_id

   Select Client to delete by ID.

   Examples::

   $ drlm delclient -c clientHost1
   $ drlm delclient --client clientHost1
   $ drlm delclient -I 12
   $ drlm delclient --id 12
   

Optional options: 

.. option:: -h, --help

   Show drlm delclient help.                              

   Examples::

   $ drlm delclient -h
   $ drlm delclient --help

Modify Client
-------------

This command is used to modify clients from DRLM database. It is 
called like this::

   $ drlm modclient [options]

The :program:`drlm modclient` has some required options:
    
.. program:: `drlm modclient`

.. option:: -c client_name, --client client_name

   Select Client to change by NAME

.. option:: -I client_id, --id client_id

   Select Client to change by ID


Optional options:
 
.. option:: -i ip, --ipaddr ip

   Set new IP address to client.

   Examples::

   $ drlm modclient -c clientHost1 -i  13.74.90.10

.. option:: -M mac_address, --macaddr mac_address

   Set new MAC address to client.

   Examples::

   $ drlm modclient -c clientHost1 -M  00-40-77-DB-33-38
   $ drlm modclient --client clientHost1 --macaddr  00-40-77-DB-33-38
   $ drlm modclient -I 12 --macaddr 00-40-77-DB-33-38
   $ drlm modclient --id 12 -M 00-40-77-DB-33-38

.. option:: -n network_name, --netname network_name

   Assign new NETWORK to client.

   Examples::

   $ drlm modclient -c clientHost1 -n  vlan12
   $ drlm modclient --client clientHost1 --netname  vlan12
   $ drlm modclient -I 12 --netname vlan12
   $ drlm modclient --id 12 -n vlan12

.. option:: -h, --help

   Show drlm modclient help.

   Examples::

   $ drlm modclient -h
   $ drlm modclient --help

List Clients
------------

This command is used to list the clients stored at the database. 
It is called like this::

   $ drlm listclient [options]

The :program:`drlm listclient` has some options:

.. program:: `drlm listclient`

.. option:: -c client_name, --client client_name

   Select Client to list.

   Examples::

   $ drlm listclient -c clientHost1
   $ drlm listclient --client clientHost1

.. option:: -A, --all

   List all clients.

   Examples::

   $ drlm listclient -A
   $ drlm listclient --all

.. option:: -h, --help

   Show drlm listclient help.

   Examples::

   $ drlm listclient -h
   $ drlm listclient --help

