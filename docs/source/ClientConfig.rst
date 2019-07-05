DRLM Client Installation
========================


Since DRLM 2.0.0, DRLM clients (ReaR) can be installed and configured on a remote server from the DRLM server
using :program:`drlm instclient`

Let's explain a little bit the steps this feature does:

        * Create the drlm user
        * Install ReaR dependencies
        * Install ReaR package
        * Configure ReaR to be managed by DRLM
        * Configure SUDO for drlm user.
        * Start and configure required services

Supported OSs for instclient command
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Unattended Client Installation has been tested on:

       * SLES (11 & 12)
       * OpenSUSE (13 & Leap 42)
       * RHEL & CentOS (5, 6 & 7)
       * Debian (6, 7, 8 & 9)
       * Ubuntu LTS (12.04, 14.04 & 16.04)

.. note:: It should work on other RedHat, Debian or SUSE variants.


Requirements
~~~~~~~~~~~~

In order to install ReaR from DRLM server the client must have:

       * Access to EPEL Repo to install rear from repo (CentOS,RHEL)
       * instclient uses apt-get, yum and zypper, so repositories must be configured
       * SSH enabled
       * root user or user with administrator privileges to install, start services
         like rpcbind and configure ReaR, DHCP and sudo applications.


Run unattended install
~~~~~~~~~~~~~~~~~~~~~~

To perform an unattended install of a DRLM client, just is needed to run **instclient** DRLM command like one of the following examples:

.. warning::
  The client must be properly registered in DRLM with **addclient** command.

Examples::

        ~# drlm instclient -c ReaRCli1
        ~# drlm instclient -c ReaRCli1 -U http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/Debian_7.0/all/rear_1.17.2_all.deb


.. note:: See Client Operations for more information
