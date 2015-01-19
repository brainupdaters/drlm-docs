Client Configuration 
====================

Dependencies
-----------------
As rear is written in bash you need bash as a bare minimum. Other requirements are: 


	* mkisofs (or genisoimage) 
	* mingetty (rear is depending on it in recovery mode) 
	* syslinux (for i386 based systems) 
	* nfs-utils (when using NFS to store the archives) 
	* cifs-utils (when using SMB to store the archives) 
	* portmap (RHEL 5)
	* rpcbind  (RHEL 6)

Dependencies Installation
-------------------------

.. describe:: CentOS 5, Red Hat 5

::

	$  yum -y install mkisofs mingetty syslinux nfs-utils cifs-utils portmap


.. describe:: CentOS 6, Red Hat 6

::

	$  yum -y install mkisofs mingetty syslinux nfs-utils cifs-utils rpcbind


.. describe:: Debian , Ubuntu

::

	$ apt-get install mkisofs mingetty syslinux nfs-utils cifs-utils rpcbind


Download ReaR 
---------------------

:program:`Download rear package from` 
http://relax-and-recover.org/download/


Install ReaR package 
--------------------

:program:`The RPM based package can be installed as follows`

Execute the next command:
::

	$ rpm -ivh rear-[VERSION].[DIST].noarch.rpm



:program:`The DEB based package can be installed as follows`

Execute the next command:
::

	$ dpkg -i rear[VERSION].[DIST]all.deb

.. note::

	ReaR supported versions > 1.15.9


Remove ReaR package
-------------------

:program:`The RPM based package can be removed as follows:`

Execute the next command:
::

	$ rpm -e rear

:program:`The DEB based package can be removed as follows:`

Execute the next command:
::

	$ apt-get remove rear

.. note::

	For more information about installing ReaR visit:
	http://relax-and-recover.org/documentation


