DRLM dependencies
=================

Server Requeriments
-------------------

Dependencies on all distributions 

 * openssh-clients 
 * openssl 
 * nc 
 * wget 
 * gzip 
 * tar
 * gawk 
 * sed 
 * grep 
 * coreutils 
 * util-linux
 * nfs-utils 
 * rpcbind
 * dhcp 
 * tftp-server
 * syslinux
 
Install Requeriments
--------------------

.. describe:: CentOS 5, Red Hat 5

::

	$  yum -y install openssh-clients openssl nc wget gzip tar gawk sed grep coreutils util-linux portmap dhcp tftp-server syslinux 


.. describe:: CentOS 6, Red Hat 6

::

	$  yum -y install openssh-clients openssl nc wget gzip tar gawk sed grep coreutils util-linux rpcbind dhcp tftp-server syslinux 


.. describe:: Debian , Ubuntu

::

	$ apt-get install openssh-client openssl netcat-traditional wget gzip tar gawk sed grep coreutils util-linux nfs-kernel-server rpcbind isc-dhcp-server tftpd-hpa, syslinux
