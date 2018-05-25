About DRLM Docs
===============


DRLM Docs contains comprehensive documentation on the DRLM (Disaster Recovery Linux Manager). This page describes documentation’s licensing, editions, and versions, and describes how to contribute to the DRLM Docs.

For more information on DRLM, see `About DRLM Project <http://drlm.org/about/>`_. To download DRLM, see the downloads page.



License
-------

This documentation is licensed under a Creative Commons `Attribution-NonCommercial-ShareAlike 4.0 International <http://creativecommons.org/licenses/by-nc-sa/4.0/>`_ (i.e. "CC-BY-NC-SA") license.

The DRLM Manual is copyright © 2017 Brain Updaters, S.L.L.



Contributing
------------

Please, we encourage you to help us to improve this documentation.

To contribute to documentation the Github interface enables users to report errata or missing sections, discuss improvements and new sections through the issue-tracker at: `DRLM Docs GitHub Issue Tracker <https://github.com/brainupdaters/drlm-docs/issues>`_.


Product Features
----------------

The following features are supported on the most recent releases of
DRLM. Anything labeled as (NEW!) was added as the most recent
release. New functionality for previous releases can be seen in the next
chapter that details each release.

  * Hot maintenance capability. A client backup can be made online
    while the system is running.

  * Command line interface. DRLM doesnot require a graphical
    interface to run. (console is enough).

  * Multiarch netboot client support (x86_64-efi, i386-efi, i386-pc)

  * Automatic client intallation from DRLM server

  * Parallel backups

  * Error reporting support to:

      - HP OpenView

      - Nagios (NSCA & NSCA-ng)

      - Zabbix

      - Mail

  * Centralized backup scheduling with a job scheduler

  * Export and Import backup between DRLM servers or DRLM clients

  * Real time clients log in DRLM server

DRLM Version 2.2.1 (June 2018) - Release Notes
----------------------------------------------
  * Updated ssh_install_rear_xxx funcitons to solve issue #62.
  

DRLM Version 2.2.0 (September 2017) - Release Notes
---------------------------------------------------
  * "Make deb" improved deleting residual files.

  * NEW Real time clients log in DRLM server.

  * NEW bash_completion feature added to facilitate the use.

  * It is possible to perform a "rear recover" without the parameters DRLM_SERVER, REST_OPTS and ID.

  * listbackup, listclient and listnetwork with "-A" parameter by default.

  * SSH_OPTS variable created in default.conf for remove hardcoded ssh options.

  * Debian 9 compatibility added.

  * Improved client configuration template.

  * Improved treatment of deleted client backups


DRLM Version 2.1.3 (May 2017) - Release Notes
---------------------------------------------
  * Update Debian 6 installclient dependencies. (issue #57)

  * Now "apt-get update" is done before "apt-get install" in instclient debian workflow.

  * Set global UMASK value for all DRLM creating files durting execution.


DRLM Version 2.1.2 (March 2017) - Release Notes
-----------------------------------------------

  * SUDO_CMDS_DRLM added in default.conf allowing to easy add new sudo commands.

  * Automatic creation of /etc/sudoers.d if not exists on systems RedHat/CenOS 5.

  * Fixed some errors for dependencies on default.conf.

  * DRLM_USER variable deleted on addclient and help.

  * Added sudo for command stat to allow check size on File Systems without perms.

  * Sudo configuration files are dynamically created according to the OS type.

  * Solved problem for start services with non root user.


DRLM Version 2.1.1 (February 2017) - Release Notes
--------------------------------------------------

  * Solved some of bugs. (issue #49, #50)

  * No Client ID required for delete backups. (issue #40)

  * No Client ID required for manage backups. (issue #46)

  * bkpmgr: Persistent mode deleted.

  * Solved PXE files: forced console=ttyS0 in kernel options. (issue #52)

  * Solved hardcoded PXE filenames (initrd.xz (lzma) now supported). (issue #52)

  * While recommended, It ain't mandatory to use hostname as client_name. (issue #52)

  * Solved drlm user hardcoded in installclient. (issue #51)

  * NAGSRV and NAGPORT added in default.conf.


DRLM Version 2.1.0 (February 2017) - Release Notes
--------------------------------------------------

  * DRLM reporting with nsca-ng, nsca. (issue #47)

  * DRLM Server for SLES. (issue #45)

  * Support for drlm unattended installation (instclient) on Ubuntu (issue #43)

  * NEW Import & Export DR images between DRLM servers. (issue #39)

  * Pass DRLM global options to ReaR. (issue #37)

  * New DRLM backup job scheduler (issue #35)

  * Addclient install mode (automatize install client after the client creation) (issue #32)

  * Solved lots of bugs


DRLM Version 2.0.0 (July 2016) -  Release Notes
-----------------------------------------------

  * Multiarch netboot with GRUB2 - x86_64-efi i386-efi i386-pc - (issue #2)

  * New installclient workflow (issue #5)

  * Added support for systemd distros - RHEL7 CentOS7 Debian8 - (issue #14)

  * Use bash socket implementation instead of netcat (issue #15)

  * runbackup workflow enhacement with sparse raw images with qemu-img
    reducing backup time and improving management (issue #16)

  * Added support for parallel backups on DRLM (issue #22)

  * Added support for new DB backend sqlite3 (issue #23)

  * Added support for Nagios error reporting (issue #28)

  * Added support for Zabbix error reporting (issue #29)

  * Added support for Mail error reporting (issue #30)

  * Added timeout var for Sqlite in sqlite3-driver.sh for avoiding database locks.

  * Added source of local.conf and site.conf files in drlm-stord

  * Solved lots of bugs

  * DRLM documentation updated to reflect version 2.0 changes


.. note:: This documentation is under constant development. Please be patient...
