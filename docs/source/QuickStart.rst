DRLM Quick Start Guide
======================

DRLM Server Installation
~~~~~~~~~~~~~~~~~~~~~~~~

Follow the steps at `DRLM Istallation <http://docs.drlm.org/en/latest/Install.html#drlm-installation>`_. (Select your appropriate SO)


Add a Network to DRLM Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First of all we will need to add the network where the ReaR clients are. To do this we have to use the command "drlm addnetwork" with the parameters -i "Network IP" network)", -g "Gateway IP", -s "Server IP on the network", -n "Network name" and -m "Network mask".

::

    $ drlm -vD addnetwork -i 192.168.1.0 -g 192.168.1.1 -s 192.168.1.38 -n BuLan -m 255.255.255.0

Add a Client to DRLM Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now we can add a ReaR client with the command "drlm addclient" and the parameters -n "Network name ID", -i "ReaR client IP", -M "ReRr client MAC address" and -c "ReaR client host name".

::
  
    $ drlm -vD addclient -n BuLan -i 192.168.1.45 -M 00:13:20:fe:48:16 -c minBUC

ReaR Client Istallation
~~~~~~~~~~~~~~~~~~~~~~~

Follow the steps at `ReaR Client Istallation <http://docs.drlm.org/en/latest/ClientConfig.html#rear-client-installation>`_. (Select your appropriate SO)

Run Client Backup
~~~~~~~~~~~~~~~~~

We are ready to backup!!! At this point we have the DRLM server and ReaR client configured, you just have to run the command "drlm runbackup" with the parameter -c "ReaR client host name"

::
  
    $ drlm -vD runbackup -c ReaRCli1

Restore Client Backup
~~~~~~~~~~~~~~~~~~~~~

Reboot the Client and select boot from network. Automaticaly will boot from PXE.

::
  
    $ rear recover SERVER=192.168.1.38 REST_OPTS=-k ID=ReaRCli1
