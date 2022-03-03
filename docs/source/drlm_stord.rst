DRLM STORD
==========

DRLM Store manager is the service that manages which backups should be enabled according to the DRLM database in case of server restart.

Management
~~~~~~~~~~

Start DRLM STORD

.. code-block:: console

  ~# drlm-stord start [client name optional]
        or
  ~# systemctl start drlm-stord.service

example:

.. code-block:: console

  root@drlmsrv:~# drlm-stord start
  2022-03-03 10:30:41 Starting DRLM Store Service: 
  2022-03-03 10:30:41 Enabling DR Backup Store /var/lib/drlm/store/drlmcli2/default (ro)
  2022-03-03 10:30:42 - Attached DR file drlmcli2.default.101.20220302141314.dr to NBD Device /dev/nbd0 (ro)
  2022-03-03 10:30:43 - Mounted /dev/nbd0p1 on /var/lib/drlm/store/drlmcli2/default (ro)
  2022-03-03 10:30:43 - Enabled RSYNC module (ro) for /var/lib/drlm/store/drlmcli2/default
  2022-03-03 10:30:43 Enabling DR Backup Store /var/lib/drlm/store/drlmcli1/default (ro)
  2022-03-03 10:30:44 - Attached DR file drlmcli1.default.100.20220302221304.dr to NBD Device /dev/nbd1 (ro)
  2022-03-03 10:30:45 - Mounted /dev/nbd1p1 on /var/lib/drlm/store/drlmcli1/default (ro)
  2022-03-03 10:30:45 - Enabled RSYNC module (ro) for /var/lib/drlm/store/drlmcli1/default

Restart DRLM STORD

.. code-block:: console

  ~# drlm-stord restart [client name optional]
        or
  ~# systemctl restart drlm-stord.service

Stop DRLM STORD

.. code-block:: console

  ~# drlm-stord stop [client name optional]
        or
  ~# systemctl stop drlm-stord.service

example:

.. code-block:: console

  root@drlmsrv:~# drlm-stord stop
  2022-03-03 10:30:05 Shutting down DRLM Store Service: 
  2022-03-03 10:30:05 Removing rsync modules
  2022-03-03 10:30:05 RSYNC:MODULES:REMOVE: .... Success!
  2022-03-03 10:30:05 Unconfiguring NFS exports
  2022-03-03 10:30:05 NFS:RELOAD:EXPORTFS: .... Success!
  2022-03-03 10:30:05 Umounting DR Images: 
  2022-03-03 10:30:06 FS:UMOUNT:MOUNT_POINT(/var/lib/drlm/store/drlmcli2/default): .... Success!
  2022-03-03 10:30:07 FS:UMOUNT:MOUNT_POINT(/var/lib/drlm/store/drlmcli1/default): .... Success!
  2022-03-03 10:30:07 Disabling NBD devices: 
  2022-03-03 10:30:08 NBD_DEVICE:DETACH:NBD_DEVICE(/dev/nbd0): .... Success!
  2022-03-03 10:30:09 NBD_DEVICE:DETACH:NBD_DEVICE(/dev/nbd1): .... Success!

Status DRLM STORD

.. code-block:: console

  ~# drlm-stord status [client name optional]  
        or
  ~# systemctl stop drlm-stord.service

example:

.. code-block:: console

  root@drlmsrv:~# drlm-stord status
  2022-03-03 10:28:19 Getting Status from DRLM Store Service: 
  2022-03-03 10:28:19 NBD Device     NET Mode   DR Store                              DR File                                     
  2022-03-03 10:28:19 (ro)/dev/nbd0  (ro)RSYNC  /var/lib/drlm/store/drlmcli2/default  /var/lib/drlm/arch/drlmcli2.default.101.20220302141314.dr
  2022-03-03 10:28:19 (ro)/dev/nbd1  (ro)RSYNC  /var/lib/drlm/store/drlmcli1/default  /var/lib/drlm/arch/drlmcli1.default.100.20220302221304.dr


Log File
~~~~~~~~

The log file for DRLM STORD can be found at /var/log/drlm/drlm-stord.log

example:

.. code-block:: console

  root@drlmsrv:~# cat /var/log/drlm/drlm-stord.log 
  2022-03-02 13:41:00 Command: drlm-stord start
  2022-03-02 13:41:00 Starting DRLM Store Service: 
  2022-03-02 17:27:19 Command: drlm-stord start
  2022-03-02 17:27:19 Starting DRLM Store Service: 
  2022-03-02 17:27:19 Enabling DR Backup Store /var/lib/drlm/store/drlmcli2/default (ro)
  2022-03-02 17:27:20 - Attached DR file drlmcli2.default.101.20220302141314.dr to NBD Device /dev/nbd0 (ro)
  2022-03-02 17:27:21 - Mounted /dev/nbd0p1 on /var/lib/drlm/store/drlmcli2/default (ro)
  2022-03-02 17:27:21 - Enabled RSYNC module (ro) for /var/lib/drlm/store/drlmcli2/default
  2022-03-02 17:27:21 Enabling DR Backup Store /var/lib/drlm/store/drlmcli1/default (ro)
  2022-03-02 17:27:22 - Attached DR file drlmcli1.default.100.20220302144304.dr to NBD Device /dev/nbd1 (ro)
  2022-03-02 17:27:23 - Mounted /dev/nbd1p1 on /var/lib/drlm/store/drlmcli1/default (ro)
  2022-03-02 17:27:23 - Enabled RSYNC module (ro) for /var/lib/drlm/store/drlmcli1/default
  2022-03-02 22:36:18 Command: drlm-stord stop
  2022-03-02 22:36:18 Shutting down DRLM Store Service: 
  2022-03-02 22:36:18 Removing rsync modules
  2022-03-02 22:36:18 RSYNC:MODULES:REMOVE: .... Success!
  2022-03-02 22:36:18 Unconfiguring NFS exports
  2022-03-02 22:36:19 NFS:RELOAD:EXPORTFS: .... Success!
  2022-03-02 22:36:19 Umounting DR Images: 
  2022-03-02 22:36:20 FS:UMOUNT:MOUNT_POINT(/var/lib/drlm/store/drlmcli2/default): .... Success!
  2022-03-02 22:36:21 FS:UMOUNT:MOUNT_POINT(/var/lib/drlm/store/drlmcli1/default): .... Success!
  ...
  ...
  ...
