DRLM PROXY
==========

Installing clients in offline environments can be a daunting task, as clients do not have access to download ReaR package and it should be done by hand, without the "drlm instclient" command.

.. image:: ../images/DRLM-Proxy.jpg
    :align: center

With DRLM PROXY a DRLM server, with acces to the ReaR packages repositories, can act as an intermediate, get the requestet ReaR package and serve it to the client.

.. image:: ../images/DRLM-Proxy2.jpg
    :align: center

Configuration
~~~~~~~~~~~~~

To enable DRLM PROXY you must specify where DRLM Server can get the ReaR packages from with the DRLM_PROXY_URL variable and where DRLM clients can get the packages with the URL_REAR_BASE variable.

.. code-block:: bash

  #/etc/drlm/local.conf
  ...
  ...
  ...
  ############### ----- DRLM PACKAGE PROXY
  DRLM_PROXY_URL="http://download.opensuse.org/repositories/Archiving:/Backup:"
  URL_REAR_BASE="http://drlmServerHostname"
  ########

Once DRLM PROXY is configured just restart the service

.. code-block:: bash

  ~# systemctl restart drlm-proxy.service

Management
~~~~~~~~~~

Start DRLM PROXY

.. code-block:: console
 
  ~# systemctl start drlm-proxy.service

Restart DRLM PROXY

.. code-block:: console

  ~# systemctl restart drlm-proxy.service

Stop DRLM PROXY

.. code-block:: console

  ~# systemctl stop drlm-proxy.service

Log File
~~~~~~~~

The log file for DRLM PROXY can be found at /var/log/drlm/drlm-proxy.log