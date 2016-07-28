Install Client Operations
=========================


This command is used to install and configure DRLM and ReaR on a remote
Server. It is called like this::

   $ drlm instclient [options]

The :program:`drlm instclient` has some requiered options:

.. program::  `drlm instclient`  

.. option:: -c client_name, --client client_name

   Select Client name to add.

.. option:: -I client_id, --id client_id

   Client Id.

.. note:: Since Debian don't have the ReaR package on his repositories 
   the following option is a requeriment also :program:`-U|--url_rear <URL_REAR>`

Optional options:

.. option:: -u user, --user user

   User with admin privileges to install and configure software

.. option:: -d drlm_user, --drlm_user drlm_user

   Force drlm_user name , default is drlm  

.. note:: if not user is specified root will be used.

.. option:: -U url_rear, --url_rear url_rear

   rpm or deb package for especific distro for example http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb

.. option:: -h, --help

   Show drlm instclient help.

   Examples::
  
   $ drlm instclient -h

   $ drlm instclient -c ReaRCli1 -u admin -U http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb

   $ drlm instclient -c ReaRCli2


