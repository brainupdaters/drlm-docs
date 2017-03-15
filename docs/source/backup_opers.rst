Backup Operations
=================

DRLM backup operations allow us to remotely create new backups of
clients, enable and disable restore points and make listings of
backups created among other things.

Run Backup
----------

This command is used to Run remote client backup from DRLM. It is
called like this::

   $ drlm runbackup [options]

The :program:`drlm runbackup` has several options:

.. program:: `drlm runbackup`

.. option:: -c client_name, --client client_name

   Select Client to remotely run backup by name.

   Examples::

   $ drlm runbackup -c clientHost1
   $ drlm runbackup --client clientHost1

.. option:: -I client_id, --id client_id

   Select Client to remotely run backup by ID.

   Examples::

   $ drlm runbackup -I 12
   $ drlm runbackup --id 12

Help option:

.. option:: -h, --help

   Show drlm runbackup help.

   Examples::

   $drlm runbackup -h
   $drlm runbackup --help

Delete Backup
-------------

This command is used to delete backups from DRLM database. It is
called like this::

   $ drlm delbackup [options]

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

   $ drlm delbackup -I 1.2015030121245
   $ drlm delbackup --id 1.2015030121245
   $ drlm delbackup -c clientHost1 -A
   $ drlm delbackup --client clientHost1 --all

Help option:

.. option:: -h, --help

   Show drlm delbackup help.

   Examples::

   $ drlm delbackup -h
   $ drlm delbackup --help

List Backups
------------

This command is used to list the backups that we have stored on the
server. It is called like this::

   $ drlm listbackup [options]

The :program:`drlm listbackup` has some options:

.. program:: `drlm listbackup`

.. option:: -c client_name, --client client_name

   Select Client to list its backups.

   Examples::

   $ drlm listbackup -c clientHost1
   $ drlm listbackup --client clientHost1

.. option:: -A, --all

   List all backups. This option is set by default if any option is specified.

   Examples::

   $ drlm listbackup
   $ drlm listbackup -A
   $ drlm listbackup --all
   
Help option:

.. option:: -h,--help

   Show this help

   Examples::

   $ drlm listbackup -h
   $ drlm listbackup --help

Backup Manager
--------------

This command is used to enable or disable client restore points.
Is also used to set a restore point by default. It is called like
this::

   $ drlm bkpmgr [options]

The :program:`drlm bkpmgr` has some required options:

.. program:: `drlm bkpmgr`

.. option:: -I backup_id, --id backup_id

   Select Backup ID to modify

.. option:: -e, --enable

   Enable Backup

.. option:: -d, --disable

   Disable Backup

   Examples::

   $drlm bkpmgr -I 1.20140519065512 -e
   $drlm bkpmgr -I 1.20140519065512 -d
   $drlm bkpmgr --id 1.20140519065512 -e

Help option:

.. option:: -h, --help

   Show drlm bkmgr help.

   Examples::

   $ drlm bkmgr -h
   $ drlm bkmgr --help

Export/Import Backups
---------------------

Since version 2.1.0 the possibility to import or export backups from other DRLM servers has been added. To export a backup::

Export Backups
~~~~~~~~~~~~~~

This command is used to export a backup that we have stored on the
server. It is called like this::

  $ drlm expbackup [options]

The :program:`drlm expbackup` has the following required options:

.. program:: `drlm expbackup`

.. option:: -I backup_id, --id backup_id

   Enter the backup ID you would like to export.

.. option:: -f destination_file, --file destination_file

   Enter the output path in which you would like to export the backup,

   Examples::

   $ drlm expbackup -I 2.20170125103105 -f /tmp/export.dr

   You could now save or copy the exported backup to another DRLM server.

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   $ drlm expbackup -h
   $ drlm expbackup --help

Import Backups
~~~~~~~~~~~~~~

This command is used to import a backup that we have received from other
DRLM server. It is called like this::

  $ drlm impbackup [options]

The :program:`drlm impbackup` has the following required options:

.. option:: -c client_name, --client client_name

   You need to first register the client in the database before importing an exported DRLM backup.

.. option:: -f file, --file file

   Set the destination path of the backup to import.

   Examples::

   $ drlm impbackup --client rear-debian -f /tmp/export.dr

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   $ drlm expbackup -h
   $ drlm expbackup --help

Backup Job Scheduler
--------------------

Since version 2.1.0 backup tasks can be scheduled. The :program:`drlm backup scheduler` allows you to **add**, **list** and **delete** scheduled jobs. You can also enable or disable the schedule function (by default it is enabled). You can set backup operations to run on a specified date and time by running::

Add Jobs
~~~~~~~~

This command is used to plan backup jobs in DRLM. It is
called like this::

    $ drlm addjob [options]

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

    Examples::

    $ drlm addjob -c rear-debian -s 2017-01-30T21:00
    $ drlm addjob --client rear-centos -s 2017-02-03T08:00 -e 2017-02-05T23:00 -r 1hour

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   $ drlm addjob -h
   $ drlm addjob --help

List Jobs
~~~~~~~~~

This command is used to list backup jobs planned in DRLM.
It is called like this::

   $ drlm listjob [options]

.. program:: `drlm listjob` arguments:

.. option:: -J job_id, --job_id job_id

   To list a job by its ID.

.. option:: -c client_name, --client client-name

   To list all the jobs scheduled for a specific client.

.. option:: -A, --all

   To list all the active scheduled jobs.

   Examples::

   $ drlm listjob -A
   $ drlm listjob -c rear-suse
   $ drlm listjob --job_id 3

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   $ drlm listjob -h
   $ drlm listjob --help

Delete Jobs
~~~~~~~~~~~

This command is used to delete planned backup jobs in DRLM.
It is called like this::

   $ drlm deljob [options]

.. program:: `drlm deljob` required options:

.. option:: -c client_name, --client client_name

   To delete all scheduled jobs for a specific client.

.. option:: -J job_id, --job_id job_id

   To delete a specific scheduled backup job.

   Examples::

   $ drlm deljob -J 5
   $ drlm deljob -c rear-centos

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   $ drlm deljob -h
   $ drlm deljob --help

Scheduler Management
~~~~~~~~~~~~~~~~~~~~

With this command you can **enable or disable** the job scheduler facility
or force to **run** jobs planned at "now" by running::

   drlm sched [options]

.. program:: `drlm sched` available options:

.. option:: -e, --enable

   Enables job scheduler utility.

.. option:: -d, --disable

   Disables job scheduler utility.

.. option:: -r, --run

   Runs all planned jobs (starting from the nearest date).

   Examples::

    $ drlm sched -e
    $ drlm sched -r

Help option:

.. option:: -h, --help

   Shows help menu.

   Examples::

   $ drlm sched -h
   $ drlm sched --help
