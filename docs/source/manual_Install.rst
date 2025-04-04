DRLM Step by Step Installation
==============================

The aim of this manual is to provide a comprehensive, step-by-step guide to installing and configuring DRLM. By the end of this guide, you should have a fully operational DRLM server.

In the following steps, it is assumed that you have performed a minimal installation of the selected distribution, **dedicated exclusively** to running the DRLM server to avoid interference with existing services.

To install DRLM have to execute the following command in the terminal as root user

Debian & Ubuntu 
---------------

Build DRLM package
~~~~~~~~~~~~~~~~~~

You can obtain the DRLM package by building it from the source code.

**Install build dependencies**

.. code-block:: console

  ~# apt update
  ~# apt install -y build-essential debhelper git golang

**Build package**

.. code-block:: console

  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make deb
  ~$ cd ..


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

The DEB package can be installed by executing the following command

.. code-block:: console

  ~# apt install -y ./drlm_2.4.13*_all.deb


CentOS, RHEL & Rocky
--------------------

Requirements
~~~~~~~~~~~~

It is not strictly necessary, but to simplify the installation process, we will disable SELinux and Firewalld. SELinux and Firewalld can be configured to work seamlessly with the DRLM Server, but this configuration will not be covered in this guide.

**Disable selinux**

Edit "/etc/sysconfig/selinux" and change the variable SELINUX value from **enforcing** to **disable** in order to disable SELinux policies on system load. Use your favorite text editor.

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

You can obtain the DRLM package by building it from the source code


**Install build dependencies**

.. code-block:: console

  ~# yum -y install git rpm-build golang make bash-completion
  

**Build package**

.. code-block:: console

  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

The RPM package can be installed by executing the following command

.. code-block:: console

	~# yum -y install ./drlm-2.4.13-*.noarch.rpm

OpenSUSE & SLES
---------------

Requirements
~~~~~~~~~~~~

It is not strictly necessary, but to simplify the installation process, we will disable Firewalld. Firewalld can be properly configured to work with the DRLM Server, but this configuration will not be covered in this guide.

**Disable firewalld**

.. code-block:: console

  ~# systemctl stop firewalld
  ~# systemctl disable firewalld
  Removed symlink /etc/systemd/system/multi-user.target.wants/firewalld.service.
  Removed symlink /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.


Build DRLM package
~~~~~~~~~~~~~~~~~~

You can obtain the DRLM package by building it from the source code

.. code-block:: console

  ~# zypper install git-core rpm-build go bash-completion
  ~$ go env -w GO111MODULE=auto
  ~$ git clone https://github.com/brainupdaters/drlm
  ~$ cd drlm
  ~$ make rpm


Install DRLM package
~~~~~~~~~~~~~~~~~~~~

The RPM package can be installed by executing the following command

.. code-block:: console

  ~# zypper in ./drlm-2.4.13-*.noarch.rpm 
     
.. note::

  You will need to accept to install the package even though it's not signed
