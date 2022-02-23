DRLM Quick Start Guide
======================

DRLM Installation
~~~~~~~~~~~~~~~~~~~~~~~~

Follow the steps at `DRLM Installation <./Install.html#drlm-installation>`_. (Select your OS)


Add Client to DRLM Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once DRLM server is istalled we can add a ReaR client with the command "drlm addclient" and the parameters -i "Client IP", -c "ReaR client name" and -I to automatically install ReaR client. The client needs to have an open SSH. By default the root user is used. You can specify another user with the ``-u <user>`` parameter. This user needs admin privileges

.. code-block:: console

    ~# drlm addclient -i 192.168.181.53 -c ReaRCli1 -I


Run Client Backup
~~~~~~~~~~~~~~~~~

We are ready to take OS backups!!! At this point we have the DRLM server and ReaR client configured, you just have to run the command "drlm runbackup" with the parameter -c "ReaR client host name"

.. code-block:: console

    ~# drlm runbackup -c ReaRCli1


Restore Client Backup
~~~~~~~~~~~~~~~~~~~~~

Follow the steps at `DRLM Client Recover <./Restore.html>`_.


Quick Start Asciinema
~~~~~~~~~~~~~~~~~~~~~

.. raw:: html

  <script id="asciicast-1xYIfv4YPwf9I9InmBDU9bPGm" src="https://asciinema.org/a/1xYIfv4YPwf9I9InmBDU9bPGm.js" async></script>