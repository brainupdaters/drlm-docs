Client Operations
=================

DRLM client operations allow us to add, remove, modify and
list clients of database.

Add Client
----------

This command is used to add clients to DRLM database. It is
called like this:

.. code-block:: console

  ~# drlm addclient [options]

If the client you wish to add is online (network reachable), you will only need to set its IP in order to add it to the database. It will then automatically fetch all the required client parameters (hostname, network and MAC address) and save those parameters to database.
In the event that the program has not obtained the values correctly, they can be modified with the modclient command explained below.

In this case, the :program:`drlm addclient` has the following required options:

.. program:: `drlm addclient`

.. option:: -i client_ip, --ipaddr client_ip

  Examples:

  .. code-block:: console

    ~# drlm addclient -i 192.168.0.15
    ~# drlm addclient -i 192.168.0.15/24

 If the :program:`drlm addclient` does not correctly fetch the client's hostname, DRLM will asign one automatically or you can set it manually in the same command.

  Examples:

  .. code-block:: console

    ~# drlm addclient -i 192.168.0.15 -c rear-debian

.. option:: -I, --installclient

 If the client is network reachable you can also automatically install the client when is added to DRLM.
 So in only one command the client is added and installed.
 Installclient have additional options than you can add behind the -I. For more information about Installclient read the "Install Client" section.

  Examples:

  .. code-block:: console

    ~# drlm addclient -i 192.168.0.15/24 -I
    ~# drlm addclient -i 192.168.0.15/24 -c rear-debian -I
    ~# drlm addclient -i 192.168.0.15/24 -c rear-debian -I -u root -U http://url.to.rear/download

If the client is not network reachable when you want to register it in the database or you wish to manually enter all the required parameters, you can do it with the required options available:

.. program:: `drlm addclient`

.. option:: -r, --repo

  Instead of installing the recommended ReaR package, installs it from the client repositories

.. option:: -U url_rear_package, --urlrear url_rear_package

  Instead of installing the recommended ReaR package, downloads and installs it from the URL provided

.. option:: -c client_name, --client client_name

  Set the client's name.

  .. note::

    It is not mandatory, but recommended that the client_name is the same as the client hostname.

.. option:: -i ip, --ipaddr ip

  Client IP address (not in CIDR notation if you are manually adding all the required parameters).

.. option:: -M mac_address, --macaddr mac_address

  Client MAC address.

.. option:: -n network_name, --netname network_name

  Client NETWORK.

  Examples:

  .. code-block:: console

    ~# drlm addclient -c clientHost1 -M 00-40-77-DB-33-38 -i 13.74.90.10 -n vlan12
    ~# drlm addclient --client clientHost1 --macaddr 00-40-77-DB-33-38 -i 13.74.90.10 -n vlan12

  .. warning::

    If the network_name doesn't exist in DRLM database you will get an error. First
    of all register the network where the client will be registered.

Help option:

.. option:: -h, --help

  Show drlm addclient help.

  Examples:

  .. code-block:: console

    ~# drlm addclient -h
    ~# drlm addclient --help


Install Client
--------------

This command is used to install and configure DRLM and ReaR on a remote
Server. It is called like this:

.. code-block:: console


  ~# drlm instclient [options]

The :program:`drlm instclient` has some requiered options:

.. program::  `drlm instclient`

.. option:: -c client_name, --client client_name

  Select Client name to add.

.. option:: -I client_id, --id client_id

  Client Id.

Additional options:

.. option:: -u user, --user user

  User with admin privileges to install and configure software

  .. note:: if not user is specified root will be used.

.. option:: -r, --repo

  Instead of installing the recommended ReaR package, installs it from the client repositories

.. option:: -U url_rear_package, --urlrear url_rear_package

  rpm or deb package for specific distro. For example http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb

  .. note:: If not url is specified will be used the package defined in "REAR DEB PACKAGE URL" section of /usr/share/drlm/conf/default.conf

.. option:: -C, --config

  ReaR and the required packages for ReaR will not be installed, but the client will be configured. Useful when the client has no connection to the internet or repository.

  Examples:

  .. code-block:: console

    ~# drlm instclient -c ReaRCli1 -u admin -U http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb
    ~# drlm instclient -c ReaRCli2 -C
    ~# drlm instclient -c ReaRCli3

Help option:

.. option:: -h, --help

  Show drlm instclient help.

  Examples:

  .. code-block:: console

    ~# drlm instclient -h


Delete Client
-------------

This command is used to delete clients from DRLM database. It is
called like this:

.. code-block:: console

  ~# drlm delclient [options]

The :program:`drlm delclient` has some required options:

.. program:: `drlm delclient`

.. option:: -c client_name, --client client_name

  Select Client to delete by NAME.

.. option:: -I client_id, --id client_id

  Select Client to delete by ID.

  Examples:

  .. code-block:: console

    ~# drlm delclient -c clientHost1
    ~# drlm delclient -I 12

Help option:

.. option:: -h, --help

  Show drlm delclient help.

  Examples:

  .. code-block:: console

    ~# drlm delclient -h
    ~# drlm delclient --help

Modify Client
-------------

This command is used to modify clients from DRLM database. It is
called like this:

.. code-block:: console

  ~# drlm modclient [options]

The :program:`drlm modclient` has some required options:

.. program:: `drlm modclient`

.. option:: -c client_name, --client client_name

  Select Client to change by NAME

.. option:: -I client_id, --id client_id

  Select Client to change by ID

Additional options:

.. option:: -i ip, --ipaddr ip

   et new IP address to client.

  Examples:
  
  .. code-block:: console

    ~# drlm modclient -c clientHost1 -i  13.74.90.10

.. option:: -M mac_address, --macaddr mac_address

  Set new MAC address to client.

  Examples:

  .. code-block:: console

    ~# drlm modclient -c clientHost1 -M  00-40-77-DB-33-38
    ~# drlm modclient -I 12 --macaddr 00-40-77-DB-33-38

.. option:: -n network_name, --netname network_name

  Assign new NETWORK to client.

  Examples:

  .. code-block:: console

    ~# drlm modclient -c clientHost1 -n  vlan12
    ~# drlm modclient -I 12 --netname vlan12

Help option:

.. option:: -h, --help

  Show drlm modclient help.

  Examples:

  .. code-block:: console

    ~# drlm modclient -h
    ~# drlm modclient --help

List Clients
------------

This command is used to list the clients stored at the database.
It is called like this:

.. code-block:: console

  ~# drlm listclient [options]

The :program:`drlm listclient` has some options:

.. program:: `drlm listclient`

.. option:: -c client_name, --client client_name

  Select Client to list.

  Examples:

  .. code-block:: console

    ~# drlm listclient -c clientHost1
    ~# drlm listclient --client clientHost1

.. option:: -A, --all

  List all clients. This option is set by default if any option is specified.

  Examples:

  .. code-block:: console

    ~# drlm listclient
    ~# drlm listclient -A

.. option:: -U, --unsched

  List clients that have no scheduled jobs. This option needs to be run together with -A

  Examples:

  .. code-block:: console

    ~# drlm listclient -U
    ~# drlm listclient -AU
    ~# drlm listclient --all --unsched

.. option:: -p, --pretty

  Marks those clients that are online with green and those that are offline with red.

  .. note:: This option is enabled by default. It can be disabled by setting `DEF_PRETTY=false` in `/etc/drlm/local.conf`.

  Examples:

  .. code-block:: console

    ~# drlm listclient -p
    ~# drlm listclient --pretty --unsched

Help option:

.. option:: -h, --help

  Show drlm listclient help.

  Examples:

  .. code-block:: console

  ~# drlm listclient -h
