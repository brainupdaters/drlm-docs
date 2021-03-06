DRLM Client Recover
===================

In this section we show how to recover a system which has been backed up.

In this example your client and server has the following configuration. You have to adapt it to your case.

::

	DRLM Server Host Name: DRLMsrv 
	DRLM Server IP: 192.168.2.120

	ReaR Client Host Name: fosdemcli4 
	ReaR Client IP: 192.168.2.102


Step by Step Client Recover
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Reboot the Client and select boot from network. Automaticaly will boot from PXE.


1. The DRLM server gives us through PXE/TFTP the client boot system. We just have to select first menu option to enter in the recovery system.


.. image:: ../images/RecoverImage1_v2.png
      :width: 640px
      :height: 480px


2. Once we have the system ready Login as "root". No password required.


.. image:: ../images/RecoverImage2.jpg
      :width: 640px
      :height: 480px


3a. Now we can recover the system with the command "rear recover".

.. image:: ../images/RecoverImage3a.png
      :width: 640px
      :height: 480px

3b. If we want to recover an imported DR image from another DRLM server the
SSL certificates for this server won't be present in the image and DRLM related
configuration in DR image won't be correct for the new DRLM server.

Your can overwrite them with the following variables in the command line: 
SERVER="DRLM Server IP" REST_OPTS=-k ID="ReaR Client Name".
In the following example: "rear recover SERVER=192.168.2.120 REST_OPTS=-k ID=fosdemcli4"

.. image:: ../images/RecoverImage3.jpg
      :width: 640px
      :height: 480px


4. The system is recovering.


.. image:: ../images/RecoverImage4.jpg
      :width: 640px
      :height: 480px


5. System recovered! So we only have to restart the client.


.. image:: ../images/RecoverImage5.jpg
      :width: 640px
      :height: 480px
