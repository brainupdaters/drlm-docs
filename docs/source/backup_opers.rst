Backup Operations
=================

DRLM backup operations allow us to remotely create new backups of
clients, enable and disable restore points and make listings of
backups created among other things.

Run Backup
----------

This command is used to Run remote client backup from DRLM. It is
called like this::

   ~# drlm runbackup [options]

The :program:`drlm runbackup` has several options:

.. program:: `drlm runbackup`

.. option:: -c client_name, --client client_name

   Select Client to remotely run backup by name.

   Examples::

   ~# drlm runbackup -c clientHost1
   ~# drlm runbackup --client clientHost1

.. option:: -I client_id, --id client_id

   Select Client to remotely run backup by ID.

   Examples::

   ~# drlm runbackup -I 12
   ~# drlm runbackup --id 12

.. option:: -C config_name, --config config_name

   Since DRLM 2.4.0 it is possible to have multiple configurations for each Client. The configurations must be in **/etc/drlm/clients/client_name.cfg.d/** path and with **.cfg** extension (ex.: home_backup.cfg). 
   With -C parameter is possible to select witch client backup configuration will be used. If is not especified, default configuration **/etc/drlm/clients/client_name.cfg** will be used 

   Examples::

   ~# drlm runbackup -c clientHost1 -C home_backup
   ~# drlm runbackup --id 12 --config home_backup

.. option:: -S

   Scan for infected files after the backup is completed

   Examples::

   ~# drlm runbackup -c clientHost1 -S

.. note:: This option is available on Enterprise Edition

Help option:

.. option:: -h, --help

   Show drlm runbackup help.

   Examples::

   ~# drlm runbackup -h
   ~# drlm runbackup --help


Restore Backup
--------------

This command is used to restore backups from DRLM. It is
called like this::

   ~# drlm restore [options]

.. warning::

   This operation only works with backups of type *DATA* using *RSYNC*. 
   You may need to enable desired backup to restore, see :ref:`List Backups` :ref:`Backup Manager`.

The :program:`drlm restore` has several options:

.. program:: `drlm restore`

.. option:: -c client_name, --client client_name

   Select Client to remotely run restore by name.

   Examples::

   ~# drlm restore -c clientHost1
   ~# drlm restore --client clientHost1

.. option:: -I client_id, --id client_id

   Select Client to remotely run restore by ID.

   Examples::

   ~# drlm restore -I 12
   ~# drlm restore --id 12

.. option:: -C config_name, --config config_name

   Since DRLM 2.4.0 it is possible to have multiple configurations for each Client. The configurations must be in **/etc/drlm/clients/client_name.cfg.d/** path and with **.cfg** extension (ex.: home_backup.cfg). 
   With -C parameter is possible to select witch client backup configuration will be used. If is not especified, default configuration **/etc/drlm/clients/client_name.cfg** will be used 

   Examples::

   ~# drlm restore -c clientHost1 -C home_backup
   ~# drlm restore --id 12 --config home_backup

.. option:: -f /path/to/file.txt,/path/to/dir/, --files /path/to/file.txt,/path/to/dir/

   Select comma separated list of files/dirs to restore (no regex!).

   Examples::

   ~# drlm restore -c clientHost1 -C home_backup -f /home/user1/,/home/user3/Desktop/image.jpg
   ~# drlm restore --id 12 --config home_backup  --files /home/user2

.. option:: -O, --overwrite

   Overwrite files to original path. Use it with caution!.

   .. danger::

      By default, all restores will be done in **/var/tmp/drlm/restored/** directory on clients. 
      Using this option will Overwrite data on destination client, so be careful!

   Examples::

   ~# drlm restore -c clientHost1 -C home_backup -O
   ~# drlm restore --id 12 --config home_backup  --files /home/user2  --overwrite

Help option:

.. option:: -h, --help

   Show drlm restore help.

   Examples::

   ~# drlm restore -h
   ~# drlm restore --help

.. tip::

   You can restore data backups from a client using: **rear restorefiles**.
   Specific files/dirs can be retrieved from the enabled DRLM backup: **rear restorefiles FILES_TO_RECOVER=/home/user1,/home/user3/image.jpg**.
   All restored files will be  in **/var/tmp/drlm/restored/**.

Delete Backup
-------------

This command is used to delete backups from DRLM database. It is
called like this::

   ~# drlm delbackup [options]

.. warning::

   To remove a backup, it must be disabled.

The :program:`drlm delbackup` has some required options:

.. program:: `drlm delbackup`

.. option:: -c client_name, --client client_name

   Select Client to delete the backups.

.. option:: -I backup_id, --id backup_id

   Select Backup to delete by ID.

.. option:: -A, --all

   Delete All backup.

   Examples::

   ~# drlm delbackup -I 1.2015030121245
   ~# drlm delbackup --id 1.2015030121245
   ~# drlm delbackup -c clientHost1 -A
   ~# drlm delbackup --client clientHost1 --all

Help option:

.. option:: -h, --help

   Show drlm delbackup help.

   Examples::

   ~# drlm delbackup -h
   ~# drlm delbackup --help

List Backups
------------

This command is used to list the backups that we have stored on the
server. It is called like this::

   ~# drlm listbackup [options]

The :program:`drlm listbackup` has some options:

.. program:: `drlm listbackup`

.. option:: -c client_name, --client client_name

   Select Client to list its backups.

   Examples::

   ~# drlm listbackup -c clientHost1
   ~# drlm listbackup --client clientHost1

.. option:: -A, --all

   List all backups. This option is set by default if any option is specified.

   Examples::

   ~# drlm listbackup
   ~# drlm listbackup -A
   ~# drlm listbackup --all

.. option:: -p, --pretty

   Marks those backups that might have failed with colors. By default, it colors in red the backups that are less than 200MB or that took less than 60 seconds to complete. Also, it colors in yellow the backups that are less than 800MB or that took less than 120 seconds. These values can be changed in the configuration with the following configurations:

   ::

      BACKUP_SIZE_STATUS_FAILED="200"
      BACKUP_SIZE_STATUS_WARNING="800" 
              
      BACKUP_TIME_STATUS_FAILED="60"
      BACKUP_TIME_STATUS_WARNING="120"

   .. note:: This option is enabled by default. It can be disabled by setting `DEF_PRETTY=false` in `/etc/drlm/local.conf`.

   Examples::

   ~# drlm listbackup -p
   ~# drlm listbackup -c clientHost1 --pretty
   ~# drlm listbackup --pretty

.. option:: -P, --policy

   List backups showing the policy used to keep the backup. The policy is defined in the configuration file of the client.

   Examples::

   ~# drlm listbackup -P
   ~# drlm listbackup -c clientHost1 --policy
   ~# drlm listbackup --policy

Help option:

.. option:: -h,--help

   Show this help

   Examples::

   ~# drlm listbackup -h
   ~# drlm listbackup --help

Backup Manager
--------------

This command is used to enable or disable client restore points.
Is also used to set a restore point by default. It is called like
this::

   ~# drlm bkpmgr [options]

The :program:`drlm bkpmgr` has some required options:

.. program:: `drlm bkpmgr`

.. option:: -I backup_id, --id backup_id

   Select Backup ID to modify

.. option:: -e, --enable

   Enable Backup

.. option:: -d, --disable

   Disable Backup

.. option:: -w, --write

   Enable Backup in local write mode (WARNING! Snaps in write mode are not allowed)

.. option:: -W, --full-write

   Enable Backup in local and remote write mode (WARNING! Snaps in write mode are not allowed)

.. option:: -H, --hold-on, --hold-off

   Hold backup. If a backup is holded means that will be ignored after a **drlm runbackup** when old backups are cleaning

   Examples::

   ~# drlm bkpmgr -I 1.20140519065512 -e
   ~# drlm bkpmgr -I 1.20140519065512 -d
   ~# drlm bkpmgr --id 1.20140519065512 -e

Help option:

.. option:: -h, --help

   Show drlm bkmgr help.

   Examples::

   ~# drlm bkmgr -h
   ~# drlm bkmgr --help

Export/Import Backups
---------------------

Since version 2.1.0 the possibility to import or export backups from other DRLM servers has been added. To export a backup:

Export Backups
~~~~~~~~~~~~~~

This command is used to export a backup that we have stored on the
server. It is called like this::

  ~# drlm expbackup [options]

The :program:`drlm expbackup` has the following required options:

.. program:: `drlm expbackup`

.. option:: -I backup_id, --id backup_id

   Enter the backup ID you would like to export.

.. option:: -f destination_file, --file destination_file

   Enter the output path in which you would like to export the backup,

   Examples::

   ~# drlm expbackup -I 2.20170125103105 -f /tmp/export.dr

   You could now save or copy the exported backup to another DRLM server.

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   ~# drlm expbackup -h
   ~# drlm expbackup --help

Import Backups
~~~~~~~~~~~~~~

This command is used to import a backup that we have received from other
DRLM server or to import backup between clients. It is called like this::

  ~# drlm impbackup [options]

The :program:`drlm impbackup` has the following required options:

.. option:: -c client_name, --client client_name

   You need to first register the client in the database before importing an exported DRLM backup.

.. option:: -f file, --file file

   Set the destination path of the backup to import.

   Examples::

   ~# drlm impbackup --client rear-debian -f /tmp/export.dr

.. option:: -I backup_id, --id backup_id

   Import the backup from a backup of the same server

   Examples::

   ~# drlm impbackup --client rear-debian -I 105.20190211083744

.. option:: -i , --import-config
   
   If import-config is specified impbackup will also import the backup configuration.

.. option:: -C config_name, --config config_name

   Since DRLM 2.4.0 it is possible to have multiple configurations for each Client. The configurations must be in **/etc/drlm/clients/client_name.cfg.d/** path and with **.cfg** extension (ex.: home_backup.cfg). 
   With -C parameter is possible to select witch client backup configuration will be used. If is not especified, default configuration **/etc/drlm/clients/client_name.cfg** will be used 
   
   Examples::

   ~# drlm impbackup --client rear-debian -f /tmp/only_data.dr -t 0 -C Home_Backup
   ~# drlm impbackup --client rear-debian -f /tmp/ISO_backup.dr -t 2 -C ISO_Backup_Recovery
   

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   ~# drlm expbackup -h
   ~# drlm expbackup --help

Backup Job Scheduler
--------------------

Since version 2.1.0 backup tasks can be scheduled. The :program:`drlm backup scheduler` allows you to **add**, **list** and **delete** scheduled jobs. You can also enable or disable the schedule function (by default it is enabled). You can set backup operations to run on a specified date and time by running:

Add Jobs
~~~~~~~~

This command is used to plan backup jobs in DRLM. It is
called like this::

    ~# drlm addjob [options]

.. program:: `drlm addjob`

Required options:

.. option:: -c client_name, --client client_name

    Client for which you want to run a scheduled backup.

.. option:: -s start_date, --start_date start_date

    Start date and time for the scheduled backup. Format: YYYY-MM-DD\ **T**\ HH:MM

Optional arguments:

.. option:: -e end_date, --end_date end_date

    End date and time for the scheduled backup. Format: YYYY-MM-DD\ **T**\ HH:MM

.. option:: -r repeat_time, --repeat repeat_time

    This argument specifies the time a backup will be performed between
    the start and the end date of a scheduled backup (if any end_date is set).
    You can specify the repeating pattern in min(s) or minute(s), hour(s),
    day(s), week(s), month(s) and year(s).

.. option:: -C config_name, --config config_name

    Since DRLM 2.4.0 it is possible to have multiple configurations for each Client. The configurations must be in **/etc/drlm/clients/client_name.cfg.d/** path and with **.cfg** extension (ex.: home_backup.cfg). 
    With -C parameter is possible to select witch Client backup configuration will be used. If is not especified, default configuration **/etc/drlm/clients/client_name.cfg** will be used 

    Examples::

    ~# drlm addjob -c rear-debian -s 2017-01-30T21:00
    ~# drlm addjob -c rear-debian -s 2017-01-30T21:00 -C home_backup
    ~# drlm addjob --client rear-centos -s 2017-02-03T08:00 -e 2017-02-05T23:00 -r 1hour
    ~# drlm addjob --client rear-centos -s 2017-02-03T08:00 -e 2017-02-05T23:00 -r 1hour --config home_backup

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   ~# drlm addjob -h
   ~# drlm addjob --help

List Jobs
~~~~~~~~~

This command is used to list backup jobs planned in DRLM.
It is called like this::

   ~# drlm listjob [options]

.. program:: `drlm listjob` arguments:

.. option:: -I job_id, --job_id job_id

   To list a job by its ID.

.. option:: -c client_name, --client client-name

   To list all the jobs scheduled for a specific client.

   Examples::

   ~# drlm listjob
   ~# drlm listjob -c rear-suse
   ~# drlm listjob --job_id 3

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   ~# drlm listjob -h
   ~# drlm listjob --help

Delete Jobs
~~~~~~~~~~~

This command is used to delete planned backup jobs in DRLM.
It is called like this::

   ~# drlm deljob [options]

.. program:: `drlm deljob` required options:

.. option:: -c client_name, --client client_name

   To delete all scheduled jobs for a specific client.

.. option:: -I job_id, --job_id job_id

   To delete a specific scheduled backup job.

   Examples::

   ~# drlm deljob -I 5
   ~# drlm deljob -c rear-centos

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   ~# drlm deljob -h
   ~# drlm deljob --help

Scheduler Management
~~~~~~~~~~~~~~~~~~~~

With this command you can **enable or disable** the job scheduler facility, 
**enable or disable** an individual job or force to **run** jobs planned 
at "now" by running::

   drlm sched [options]

.. program:: `drlm sched` available options:

.. option:: -e, --enable

   Enables job scheduler utility or and individual job if Job ID is specified.

.. option:: -d, --disable

   Disables job scheduler utility or and individual job if Job ID is specified.

.. option:: -r, --run

   Runs all planned jobs (starting from the nearest date).

   Examples::

    ~# drlm sched -e
    ~# drlm sched -e -I 25
    ~# drlm sched -r

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   ~# drlm sched -h
   ~# drlm sched --help


Scan Backup
-----------

This command is used to scan for infected files on an existing backup.
It is called like
this::

   ~# drlm scan [options]

The :program:`drlm scan` has some required options:

.. program:: `drlm scan`

.. option:: -I backup_id, --id backup_id

   Select Backup ID to scan

.. option:: -c, --client

   Scan enabled backup

   Examples::

   ~# drlm scan -I 1.20140519065512 
   ~# drlm scan -c rear-centos

Help option:

.. option:: -h, --help

   Show drlm scan help.

   Examples::

   ~# drlm scan -h
   ~# drlm scan --help

.. note:: This feature is available only for DRLM Enterprise.

Archive Backup
--------------

This command is used to archive existing backups to the Cloud.
It is called like
this::

   ~# drlm archive [options]

The :program:`drlm archive` has some required options:

.. program:: `drlm archive`

.. option:: -I backup_id, --id backup_id

   Select Backup ID to archive

.. option:: -U, --upload

   Upload backup to the Cloud

   Examples::

   ~# drlm archive -I 100.20240519065512 -U

.. option:: -D, --download

   Download backup from the Cloud

   Examples::

   ~# drlm archive -I 100.20240519065512 -D

.. option:: -R, --remove

   Remove backup from the Cloud

   Examples::

   ~# drlm archive -I 100.20240519065512 -R

.. option:: -L, --list

   Lists backups from the Cloud

   Examples::

   ~# drlm archive -L

   ~# drlm archive -c client01 -L

Help option:

.. option:: -h, --help

   Show drlm archive help.

   Examples::

   ~# drlm archive -h
   ~# drlm archive --help

.. note:: This feature is available only for DRLM Enterprise.
