About DRLM Docs
===============


DRLM Docs contains comprehensive documentation on the DRLM (Disaster Recovery Linux Manager). This page describes documentation’s licensing, editions, and versions, and describes how to contribute to the DRLM Docs.

For more information on DRLM, see `About DRLM Project <http://drlm.org>`_. To download DRLM, see the downloads page.



License
-------

This documentation is licensed under a Creative Commons `Attribution-NonCommercial-ShareAlike 4.0 International <http://creativecommons.org/licenses/by-nc-sa/4.0/>`_ (i.e. “CC-BY-NC-SA”) license.

The DRLM Manual is copyright © 2024 Brain Updaters, S.L.



Contributing
------------

Please, we encourage you to help us to improve this documentation.

To contribute to documentation the Github interface enables users to report errata or missing sections, discuss improvements and new sections through the issue-tracker at: `DRLM Docs GitHub Issue Tracker <https://github.com/brainupdaters/drlm-docs/issues>`_.


.. note:: This documentation is under constant development. Please be patient...




Contents:
=========

.. toctree::
   :maxdepth: 2
   :caption: User Documentation

   Install
   QuickStart
   ClientConfig
   Restore
   ErrorReporting

.. toctree::
   :maxdepth: 2
   :caption: Command Reference

   overview
   network_opers
   client_opers
   backup_opers

.. toctree::
   :maxdepth: 2
   :caption: Client Configuration

   client_config_default
   client_config_incremental_data
   client_config_PXE

.. toctree::
   :maxdepth: 2
   :caption: DRLM SERVICES

   drlm_api
   drlm_proxy
   drlm_stord
   drlm_rsyncd
   drlm_tftpd

.. toctree::
   :maxdepth: 2
   :caption: Developer Documentation

   building_grub2

.. note:: This section should change continously due to changes in DRLM development, please be patient.
   Any question regarding DRLM development, please use `DRLM Dev Forum <https://groups.google.com/forum/#!forum/drlm-dev>`_. Thanks!

.. toctree::
   :maxdepth: 1
   :caption: About Documentation

   About

Indexes and tables
==================

* :ref:`genindex`
