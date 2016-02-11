DRLM Quick Start Guide
======================

DRLM Installation
~~~~~~~~~~~~~~~~~~~~~~~~

Follow the steps at `DRLM Installation <http://docs.drlm.org/en/latest/Install.html#drlm-installation>`_. (Select your OS)


Add Network to DRLM Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First of all we must add the network where the ReaR clients are. To do this we have to use the command "drlm addnetwork" with the parameters -i "Network IP" network", -g "Gateway IP", -s "Server IP of the network", -n "Network Name" and -m "Netmask".

::

    $ drlm -vD addnetwork -i 192.168.1.0 -g 192.168.1.1 -s 192.168.1.38 -n BuLan -m 255.255.255.0

Add Client to DRLM Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now we can add a ReaR client with the command "drlm addclient" and the parameters -n "Network Name", -i "ReaR client IP", -M "ReaR client MAC address" and -c "ReaR client hostname".

::
  
    $ drlm -vD addclient -n BuLan -i 192.168.1.45 -M 00:13:20:fe:48:16 -c minBUC

ReaR Client Installation
~~~~~~~~~~~~~~~~~~~~~~~

Follow the steps at `ReaR Client Installation <http://docs.drlm.org/en/latest/ClientConfig.html#rear-client-installation>`_. (Select your OS)

Run Client Backup
~~~~~~~~~~~~~~~~~

We are ready to take OS backups!!! At this point we have the DRLM server and ReaR client configured, you just have to run the command "drlm runbackup" with the parameter -c "ReaR client host name"

::
  
    $ drlm -vD runbackup -c ReaRCli1

Restore Client Backup
~~~~~~~~~~~~~~~~~~~~~

Follow the steps at `DRLM Client Recover <http://drlm-docs.readthedocs.org/en/latest/Restore.html>`_. 

