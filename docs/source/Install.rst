DRLM Installation
=================

The pourpose of this manual is explain, step by step, the installation and configuration of DRLM.
At the end of this guide you should have a fully functional DRLM server.

DRLM uses DHCP, NFS, TFTP, RSYNC, NBD and HTTPS. On the following steps, is assumed you have a minimal installation, of the selected distribution, full dedicated to run DRLM server in order to avoid interference with existing services. 

Debian & Ubuntu 
---------------

Supported versions
~~~~~~~~~~~~~~~~~~

* Debian 9 (Stretch)
* Debian 10 (Buster)
* Debian 11 (Bullseye)
* Ubuntu 18.04 LTS (Bionic Beaver)
* Ubuntu 20.04 LTS (Focal Fossa)
* Ubuntu 22.04 LTS (Jammy Jellyfish)


Build DRLM package
~~~~~~~~~~~~~~~~~~

You can obtain the DRLM package building it from the source code.

**Install dependencies**

.. code-block:: console

  ~# apt update && apt -y upgrade
  ~# apt install -y build-essential curl debhelper git

**Install Golang**

.. code-block:: console

  ~# curl -OL https://go.dev/dl/go1.19.3.linux-amd64.tar.gz
  ~# rm -rf /usr/local/go && tar -C /usr/local -xzf go1.19.3.linux-amd64.tar.gz
  ~# export PATH=$PATH:/usr/local/go/bin 
  ~# go env -w GO111MODULE="auto" 

**Build package**

.. code-block:: console

  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make deb
  ~$ cd ..


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

The DEB package can be installed executing the next command

.. code-block:: console

  ~# apt install -y ./drlm_2.4.8_all.deb

Debian 10 Asciinema Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. raw:: html 

  <script id="asciicast-408482" src="https://asciinema.org/a/408482.js" async></script>


CentOS, RHEL & Rocky
--------------------

Supported versions
~~~~~~~~~~~~~~~~~~

* CentOS 8
* Rocky 8
* RHEL 8
* RHEL 9

Requirements
~~~~~~~~~~~~

It is not really a requirement, but to facilitate the installation we will disable SELinux and Firewalld. Selinux and Firewalld can be properly configured to work with DRLM Server, but will not be explained at this point.

**Disable selinux**

Edit "/etc/sysconfig/selinux" and change the variable SELINUX value from **enforcing** to **disable** in order to disable SELinux policies on system load. Use you favourite editor.

.. code-block:: console

  ~$ vi /etc/sysconfig/selinux

  # This file controls the state of SELinux on the system.
  # SELINUX= can take one of these three values:
  #     enforcing - SELinux security policy is enforced.
  #     permissive - SELinux prints warnings instead of enforcing.
  #     disabled - No SELinux policy is loaded.
  SELINUX=disabled
  # SELINUXTYPE= can take one of these two values:
  #     targeted - Targeted processes are protected,
  #     mls - Multi Level Security protection.
  SELINUXTYPE=targeted

Disable SELinux in the current instance, to avoid a reboot.

.. code-block:: console

  ~# setenforce 0

**Disable firewalld**

.. code-block:: console

  ~# systemctl stop firewalld
  ~# systemctl disable firewalld
  Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
  Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.

Build DRLM package
~~~~~~~~~~~~~~~~~~

You can obtain the DRLM package building it from the source code


**Install dependencies**

.. code-block:: console

  ~# yum -y install git rpm-build make bash-completion gcc
  

**Install Golang**

.. code-block:: console

  ~# curl -OL https://go.dev/dl/go1.19.3.linux-amd64.tar.gz
  ~# rm -rf /usr/local/go && tar -C /usr/local -xzf go1.19.3.linux-amd64.tar.gz
  ~# export PATH=$PATH:/usr/local/go/bin 
  ~# go env -w GO111MODULE="auto" 


**Build package**

.. code-block:: console

  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

The RPM package can be installed executing the next command

.. code-block:: console

	~# yum -y install ./drlm-2.4.8-1git.el*.noarch.rpm


CentOS Asciinema Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. raw:: html 

  <script id="asciicast-408477" src="https://asciinema.org/a/408477.js" async></script>


OpenSUSE & SLES
---------------

Supported versions
~~~~~~~~~~~~~~~~~~

* OpenSUSE Leap 15
* SLES 12
* SLES 15

Requirements
~~~~~~~~~~~~

It is not really a requirement, but to facilitate the installation we will disable Firewalld. Firewalld can be properly configured to work with DRLM Server, but will not be explained at this point.

**Disable firewalld**

.. code-block:: console

  ~# systemctl stop firewalld
  ~# systemctl disable firewalld
  Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
  Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.


Build DRLM package
~~~~~~~~~~~~~~~~~~

You can obtain the DRLM package building it from the source code

.. code-block:: console

  ~# zypper install git-core rpm-build go bash-completion
  ~$ go env -w GO111MODULE=auto
  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

The RPM package can be installed as follows executing the next command

.. code-block:: console

  ~# zypper in ./drlm-2.4.8-1git.noarch.rpm 
     
.. note::

  You will need to accept to install the package even though it's not signed

openSUSE Leap 15.2 Asciinema Installation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. raw:: html 

    <script id="asciicast-408492" src="https://asciinema.org/a/408492.js" async></script>


Firewalld Configuration
-----------------------

If you don't want to disable Firewalld, you will need to accept connections on the following ports:

 - `69/tcp`  (Used for TFTP)
 - `69/udp`  (Used for TFTP)
 - `443/tcp` (Used for DRLM API)
 - `873/tcp` (Used for RSYNCD)
