DRLM Components Installation 
============================

Server Components Install
-------------------------

This section covers the installation of distribution based packages (CentOS, Debian) for the 3 main services needed for DRLM.

* TFTP Service
* NFS Service
* DHCP Service
* HTTP Service


TFTP
----

This server component is used to serve client specific recovery images and PXELINUX image  when the server receive a PXE request.

.. describe:: CentOS

::

	$ yum -y install tftp-server

.. describe:: Debian

::

	$ apt-get install tftpd-hpa

NFS
---

This server component is used to upload PXE booting files and OS backups from clients to the server, when the clients run mkrescue/mkbackup work-flow, and for remote storage of OS backups that uses the recover work-flow.

.. describe:: CentOS

::

	$ yum install nfs-utils nfs4-acl-tools rpcbind

.. describe:: Debian

::

	$ apt-get install nfs-common nfs-kernel-server rpcbind


DHCP
----

This server component is used to assign fixed IP addresses, only to defined clients, and in conjunction with tftp service acts as a PXE Booting service. We used fixed IP associated to the MAC addresses of target clients because we need to be sure there aren't any conflicts with other DHCP services inside the client network.

.. describe:: CentOS

::

	$ yum -y install dhcp

.. describe:: Debian

::

	$ apt-get install isc-dhcp-server


HTTP
----

This server component is used to send through the network the clients configurations.

.. describe:: CentOS

::

	$ yum -y install httpd mod_ssl openssl

.. describe:: Debian

::

	$ apt-get install apache2
