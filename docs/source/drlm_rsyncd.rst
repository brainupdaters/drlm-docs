DRLM RSYNCD
===========

Rsync daemon server for DRLM.


Configuration
~~~~~~~~~~~~~

.. code-block:: bash

  # /etc/drlm/rsyncd: configuration file for rsync daemon mode
  port = 873
  motd file = /etc/drlm/rsyncd/rsyncd.motd
  lock file = /var/lib/drlm/run/drlm-rsync.lock
  log file = /var/log/drlm/drlm-rsyncd.log
  pid file = /var/lib/drlm/run/drlm-rsyncd.pid
  uid = root
  gid = root
  &include /etc/drlm/rsyncd/rsyncd.d

Management
~~~~~~~~~~

Start DRLM RSYNCD

.. code-block:: console
 
  ~# systemctl start drlm-rsyncd.service

Restart DRLM RSYNCD

.. code-block:: console

  ~# systemctl restart drlm-rsyncd.service

Stop DRLM RSYNCD

.. code-block:: console

  ~# systemctl stop drlm-rsyncd.service

Log File
~~~~~~~~

The log file for DRLM PROXY can be found at /var/log/drlm/drlm-rsyncd.log

example:

.. code-block:: console

  root@drlmsrv:~# cat /var/log/drlm/drlm-rsyncd.log 
  2022/03/02 13:41:00 [24883] rsyncd version 3.2.3 starting, listening on port 873
  2022/03/02 13:43:10 [27739] connect from drlmcli1 (192.168.123.41)
  2022/03/02 13:43:10 [27739] module-list request from drlmcli1 (192.168.123.41)
  2022/03/02 13:43:45 [27740] connect from drlmcli1 (192.168.123.41)
  2022/03/02 13:43:45 [27740] rsync allowed access on module drlmcli1_default from drlmcli1 (192.168.123.41)
  2022/03/02 12:43:45 [27740] rsync to drlmcli1_default/ from drlmcli1@drlmcli1 (192.168.123.41)
  2022/03/02 12:43:45 [27740] receiving file list
  2022/03/02 12:43:45 [27740] sent 28 bytes  received 110 bytes  total size 0
  2022/03/02 13:43:47 [27742] connect from drlmcli1 (192.168.123.41)
  2022/03/02 13:43:47 [27742] rsync allowed access on module drlmcli1_default from drlmcli1 (192.168.123.41)
  2022/03/02 12:43:47 [27742] rsync to drlmcli1_default// from drlmcli1@drlmcli1 (192.168.123.41)
  2022/03/02 12:43:47 [27742] receiving file list
  2022/03/02 12:43:48 [27742] sent 105 bytes  received 128712231 bytes  total size 128680426
  2022/03/02 13:43:48 [27745] connect from drlmcli1 (192.168.123.41)
  2022/03/02 13:43:48 [27745] rsync allowed access on module drlmcli1_default from drlmcli1 (192.168.123.41)
  2022/03/02 12:43:48 [27745] rsync to drlmcli1_default//backup from drlmcli1@drlmcli1 (192.168.123.41)
  2022/03/02 12:43:48 [27745] receiving file list
  2022/03/02 12:44:11 [27745] sent 578802 bytes  received 1450267386 bytes  total size 1451527852
  2022/03/02 13:44:12 [27884] connect from drlmcli1 (192.168.123.41)
  2022/03/02 13:44:12 [27884] rsync allowed access on module drlmcli1_default from drlmcli1 (192.168.123.41)
  2022/03/02 12:44:12 [27884] rsync to drlmcli1_default//backup-20220302.1344.log.gz from drlmcli1@drlmcli1 (192.168.123.41)
  2022/03/02 12:44:12 [27884] receiving file list
  ...
  ...
  ...
