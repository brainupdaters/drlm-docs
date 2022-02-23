Network Operations
==================

DRLM can make backups of clients in different networks. So 
for the proper functioning of DRLM is necessary to have
registered the networks in which we will register the clients. 

Since DRLM 2.4.0 networks are automatically added, but sometimes 
it will be necessary to make manual additions or modifications.

DRLM network operations allow us to add, remove, modify and
list network of database.

Add Network
-----------

This command is used to add networks to DRLM database. It is
called like this:

.. code-block:: console

  ~# drlm addnetwork [options]

The :program:`drlm addnetwork` has some options:

.. program:: `drlm addnetwork`

.. option:: -n network_name, --netname network_name

  Select Network name to add.

.. option:: -g gateway_ip, --gateway gateway_ip

  Network gateway IP address.

.. option:: -m network_mask, --mask network_mask

  Network mask

.. option:: -s server_ip, --server server_ip

  Server IP address.

.. option:: -i ip, --ipaddr ip

  Network IP address.

  Examples:

  .. code-block:: console

    ~# drlm addnetwork -s 192.168.150.24 
    ~# drlm addnetwork --server 192.168.150.24 -n backupNet
    ~# drlm addnetwork --ipaddr 192.168.150.0 -g 192.168.150.1 -m 255.255.255.0  --server 192.168.150.24 -n backupNet

Help options:

.. option:: -h, --help

  Show drlm addnetwork help.

  Examples:

  .. code-block:: console

    ~# drlm addnetwork -h
    ~# drlm addnetwork --help

Delete Network
--------------

This command is used to delete networks from DRLM database. It is
called like this:

.. code-block:: console

  ~# drlm delnetwork [options]

The :program:`drlm delnetwork` has some options:

.. program:: `drlm delnetwork`

.. option:: -n network_name, --netname network_name

  Select Network to delete by NAME.

.. option:: -I network_id, --id network_id

  Select Network to delete by ID.

  Examples:
    
  .. code-block:: console   

    ~# drlm delnetwork -n vlan12
    ~# drlm delnetwork -I 12

Help options:

.. option:: -h, --help

  Show drlm delnetwork help.

  Examples:
    
  .. code-block:: console

    ~# drlm delnetwork -h
    ~# drlm delnetwork --help

Modify Network
--------------

This command is used to modify networks from DRLM database. It is
called like this:

.. code-block:: console

  ~# drlm modnetwork [options]

The :program:`drlm modnetwork` has some required options:

.. program:: `drlm modnetwork`

.. option:: -n network_name, --netname network_name

  Select Network to change by NAME.

.. option:: -I network_id, --id network_id

  Select Network to change by ID.

Additional options:

.. option:: -g gateway_ip, --gateway gateway_ip

  Set new GATEWAY address to network. If you want to clear the gateway IP you can specify with the word 'null'

  Examples:

  .. code-block:: console

    ~# drlm modnetwork -I 12 -g 13.74.91.1
    ~# drlm modnetwork -n vlan12 -g 13.74.91.1
    ~# drlm modnetwork -n vlan12 -g null


.. option:: -m network_mask, --mask network_mask

  Assign new MASK to network.

  Examples:

  .. code-block:: console

    ~# drlm modnetwork -I 12 -m 255.255.0.0
    ~# drlm modnetwork -n vlan12 -m 255.255.0.0

.. option:: -s server_ip, --server server_ip

  Assign new SERVER to network.

  Examples:

  .. code-block:: console

    ~# drlm modnetwork -I 12 -s 13.74.91.221
    ~# drlm modnetwork -n vlan12 -s 13.74.91.221

.. option:: -e, --enable

  Enable DHCP/PXE daemon listener for this Network 

.. option:: -d, --disable

  Disable DHCP/PXE daemon listener for this Network

  Examples:

  .. code-block:: console

    ~# drlm modnetwork -I 12 -e
    ~# drlm modnetwork -n vlan12 -d

.. note::
   You can combine all necessary options in only one command for example:
   "drlm modnetwork -n vlan12 -s 13.74.91.221 -m 255.255.0.0 -g 13.74.91.1"

Help option:

.. option:: -h, --help

  Show drlm modnetwork help.

  Examples:

  .. code-block:: console

    ~# drlm modnetwork -h
    ~# drlm modnetwork --help

List Networks
-------------

This command is used to list the networks from DRLM database. It is
called like this:

.. code-block:: console

  ~# drlm listnetwork [options]

The :program:`drlm listnetwork` has some options:

.. program:: `drlm listnetwork`

.. option:: -n network_name, --netname network_name

  Select Network to list.

  Examples:
   
  .. code-block:: console

    ~# drlm listnetwork -n vlan12
    ~# drlm listnetwork --netname vlan12

.. option:: -A, --all

  List all networks. This option is set by default if any option is specified.

  Examples:

  .. code-block:: console

    ~# drlm listnetwork
    ~# drlm listnetwork -A
    ~# drlm listnetwork -all

Help options:

.. option:: -h, --help

  Show drlm listnetwork help.

  Examples:

  .. code-block:: console

    ~# drlm listnetwork -h
    ~# drlm listnetwork --help
