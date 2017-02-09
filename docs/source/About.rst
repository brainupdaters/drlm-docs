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
release. 

  * Hot maintenance capability. A client backup can be made online
    while the system is running.

  * Command line interface. DRLM does not require a graphical
    interface to run. (console is enough).

  * Multiarch netboot client support (x86_64-efi, i386-efi, i386-pc)

  * Centralized backup scheduling

  * Installclient workflow (NEW!)

  * Parallel backups (NEW!)

  * Sqlite3 database backend (NEW!)

  * Error reporting support to:

      - HP OpenView

      - Nagios (NEW!)

      - Zabbix (NEW!)

      - Mail (NEW!)

DRLM Version 2.1.0 (January 2017) -  Release Notes
--------------------------------------------------

  * DRLM reporting with nsca-ng, nsca. (issue #47)

  * DRLM Server for SLES. (issue #45)
  
  * Support for drlm unattended installation (instclient) on Ubuntu (issue #43)
  
  * NEW Import & Export DR images between DRLM servers. (issue #39)
  
  * Pass DRLM global options to ReaR. (issue #37)
  
  * New DRLM backup job scheduler. (issue #35)
  
  * Addclient install mode (automatize install client after the client creation) (issue #32)


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
