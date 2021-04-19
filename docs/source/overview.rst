DRLM Overview
==================

To use Disaster Recovery Linux Manager you always call the main script '/usr/sbin/drlm'

.. code-block:: console

   Usage: drlm [-dDsSvV] COMMAND [-- ARGS...]

   Disaster Recovery Linux Manager comes with ABSOLUTELY NO WARRANTY; for details 
   see The GNU General Public License at: http://www.gnu.org/licenses/gpl.html

   Available options:
    -d           debug mode; log debug messages
    -D           debugscript mode; log every function call
    -s           simulation mode; show what scripts drlm would include
    -S           step-by-step mode; acknowledge each script individually
    -v           verbose mode; show more output
    -V           version information

   List of commands:
    addclient       register new client to DB.
    addjob          register new job to DB.
    addnetwork      register new network to DB.
    bkpmgr          manage DRLM backup states.
    delbackup       delete backup and unregister from DB.
    delclient       delete client from DB.
    deljob          delete job from DB.
    delnetwork      delete network from DB.
    expbackup       export backup from DB.
    impbackup       import backup from DB.
    instclient      install client from DRLM
    listbackup      list client backups.
    listclient      list registered clients.
    listjob         list planned jobs.
    listnetwork     list registered networks.
    modclient       modify client properties.
    modnetwork      modify network properties.
    runbackup       run backup and register to DB.
    sched           schedule planned jobs.

   Use 'drlm COMMAND --help' for more advanced commands.
