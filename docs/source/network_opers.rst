Network Operations
==================

DRLM can make backups of clients in different networks. So
the first step we have to do for the proper functioning of
DRLM is register the networks in which later we will register
the clients.

DRLM network operations allow us to add, remove, modify and
list network of database.

Add Network
-----------

This command is used to add networks to DRLM database. It is
called like this::

   $ drlm addnetwork [options]

The :program:`drlm addnetwork` has some requiered options:

.. program:: `drlm addnetwork`

.. option:: -n network_name, --netname network_name

   Select Network name to add.

.. option:: -i ip, --ipaddr ip

   Network IP address.

.. option:: -g gateway_ip, --gateway gateway_ip

   Network gateway address.

.. option:: -m network_mask, --mask network_mask

   Network mask

.. option:: -s server_ip, --server server_ip

   Network server address.

   Examples::

   $ drlm addnetwork -i 13.74.90.0 -g 13.74.90.1 -m 255.255.255.0  -s 13.74.90.222 -n vlan12
   $ drlm addnetwork -i 13.74.90.0 --gateway 13.74.90.1 --mask 255.255.255.0  --server 13.74.90.222 -n vlan12
   $ drlm addnetwork --ipaddr 13.74.90.0 -g 13.74.90.1 -m 255.255.255.0  --server 13.74.90.222 -n vlan12

Help options:

.. option:: -h, --help

   Show drlm addnetwork help.

   Examples::

   $ drlm addnetwork -h
   $ drlm addnetwork --help

Delete Network
--------------

This command is used to delete networks from DRLM database. It is
called like this::

   $ drlm delnetwork [options]

The :program:`drlm delnetwork` has some options:

.. program:: `drlm delnetwork`

.. option:: -n network_name, --netname network_name

   Select Network to delete by NAME.

   Examples::

   $ drlm delnetwork -n vlan12
   $ drlm delnetwork -name vlan12

.. option:: -I network_id, --id network_id

   Select Network to delete by ID.

   Examples::

   $ drlm delnetwork -I 12
   $ drlm delnetwork --id 12

Help options:

.. option:: -h, --help

   Show drlm delnetwork help.

   Examples::

   $ drlm delnetwork -h
   $ drlm delnetwork --help

Modify Network
--------------

This command is used to modify networks from DRLM database. It is
called like this::

   $ drlm modnetwork [options]

The :program:`drlm modnetwork` has some required options:

.. program:: `drlm modnetwork`

.. option:: -n network_name, --netname network_name

   Select Network to change by NAME.

.. option:: -I network_id, --id network_id

   Select Network to change by ID.

Additional options:

.. option:: -g gateway_ip, --gateway gateway_ip

   Set new GATEWAY address to network.

   Examples::

   $ drlm modnetwork -I 12 -g 13.74.91.1 -m 255.255.255.0
   $ drlm modnetwork --id 12 --gateway 13.74.91.1 -m 255.255.255.0
   $ drlm modnetwork -n vlan12 -g 13.74.91.1 -m 255.255.255.0
   $ drlm modnetwork --netname vlan12 --gateway 13.74.91.1 -m 255.255.255.0

.. option:: -m network_mask, --mask network_mask

   Assign new MASK to network.

   Examples::

   $ drlm modnetwork -I 12 -m 255.255.0.0
   $ drlm modnetwork --id 12 -m 255.255.0.0
   $ drlm modnetwork -n vlan12 -m 255.255.0.0
   $ drlm modnetwork --netname vlan12 --mask 255.255.0.0

.. option:: -s server_ip, --server server_ip

   Assign new SERVER to network.

   Examples::

   $ drlm modnetwork -I 12 -s 13.74.91.221 -m 255.255.255.0
   $ drlm modnetwork --id 12 --server 13.74.91.221 -m 255.255.255.0
   $ drlm modnetwork -n vlan12 -s 13.74.91.221 -m 255.255.255.0
   $ drlm modnetwork --netname vlan12 --server 13.74.91.221 -m 255.255.255.0

Help option:

.. option:: -h, --help

   Show drlm modnetwork help.

   Examples::

   $ drlm modnetwork -h
   $ drlm modnetwork --help

List Networks
-------------

This command is used to list the networks from DRLM database. It is
called like this::

   $ drlm listnetwork [options]

The :program:`drlm listnetwork` has some options:

.. program:: `drlm listnetwork`

.. option:: -n network_name, --netname network_name

   Select Network to list.

   Exmples::

   $ drlm listnetwork -n vlan12
   $ drlm listnetwork --netname vlan12

.. option:: -A, --all

   List all networks. This option is set by default if any option is specified.

   Examples::

   $ drlm listnetwork
   $ drlm listnetwork -A
   $ drlm listnetwork -all

Help options:

.. option:: -h, --help

   Show drlm listnetwork help.

   Examples::

   $ drlm listnetwork -h
   $ drlm listnetwork --help
