Error Reporting Configuration
=============================

DRLM can be configured to report errors on scheduled backups if required.
Is possible to report by mail or integrating with your monitoring service.
At this time (DRLM 2.4) we support error reporting by mail and integration
with Nagios, Zabbix and HPOM(OVO) monitoring services, Telegram, XML and JSON.

.. note::
  All reporting configuration samples are located in: /usr/share/drlm/conf/samples

Enable DRLM reporting
---------------------

First of all, you need to enable error reporting on DRLM by setting ERR_REPORT=yes in /etc/drlm/local.conf.
Then you need to define the REPORT_TYPE according to your monitoring service.
Optionally you can define the ERR_MESSAGE to customize the error message.

::

  ~# vi /etc/drlm/local.conf
  ########
  #
  # Defines HowTo report Errors using some known and wide used methods
  #
  #    ERR_REPORT=[yes|no]
  #	default: no
  #    REPORT_TYPE=[ovo|nsca-ng|nsca|nrdp|zabbix|mail|xml|json|telegram]
  #	default: empty
  #    ERR_MESSAGE
  #	default: '$(Stamp)$PROGRAM:$WORKFLOW:$CLI_NAME:$CLI_CFG:ERROR: $@'
  #
  ########

  ERR_REPORT=yes
  REPORT_TYPE=[your_report_type]
  ERR_MESSAGE='$(Stamp)$PROGRAM:$WORKFLOW:$CLI_NAME:$CLI_CFG:ERROR: $@' 
      
Configure nrdp (Nagios based) reporting    
--------------------------------------- 

Since curl is already installed with DRLM no aditional software is needed from the DRLM side.

::

   # 
   # REPORT_TYPE=[nrdp]
   # NAGIOS VARIABLE
   #               
   #  These are default values and can be overwritten in local.conf according to your NAGIOS installation and configuration.
   #  
   #  NRDPCMD="/usr/bin/curl" # Command 
   #  NAGSVC="DRLM Service"   # Nagios Service Name
   #  NAGHOST="DRLM"                    # Nagios Host Name
   #  NRDPURL="http://Nagios/nrdp/"     # nrdp URL
   #  NRDPTOKEN="TOKEN"                 # Token
   #                                          
   NRDPCMD="/usr/bin/curl"
   NAGSVC=
   NAGHOST=
   NRDPURL=
   NRDPTOKEN=

The following options are DRLM defaults, change any of them to your installation requirements in /etc/drlm/local.conf.                                                                                                  
                                                                                                            
::                                                                                                          
                                                                                                            
  ~# vi /etc/drlm/local.conf                                                                                
  ERR_REPORT=yes                                                                                            
  REPORT_TYPE=nrdp
  NRDPCMD="/usr/bin/curl"
  NAGSVC="DRLM Service"
  NAGHOST="DRLM"
  NRDPURL="http://Nagios/nrdp/"
  NRDPTOKEN="TOKEN"


.. note::                                            
  The configuration on the server side is not in the scope of this documentation. Please check your Nagios service documentation to configure properly the NRDP service and how to report DRLM alerts.                                                                                                    
  
  For reference you can check:                                                                              
    * https://github.com/NagiosEnterprises/nrdp
    * https://support.nagios.com/kb/article.php?id=599

Configure nsca-ng (Nagios based) reporting
------------------------------------------

.. note:: We recomend use NRDP instead NCSCA-NG, but if you have nsca-ng DRLM supports it

In order to configure Nagios Error reporting on DRLM, the Nagios NSCA Client must be installed.

.. note:: We're using nsca-ng because nsca is deprecated, but if you have nsca DRLM supports it

**Debian 7/8/9/10/11**

::

  ~# apt-get install nsca-ng-client

**RHEL/Centos 6/7/8**

if nsca-ng-client is not in the repositories, it can be downloaded from:

        * https://www.nsca-ng.org/

The following options are DRLM defaults, change any of them to your installation requirements in /etc/drlm/local.conf.

::

  ~# vi /etc/drlm/local.conf

  #
  # REPORT_TYPE=nagios
  # NAGIOS VARIABLES
  #
  # These are default values and can be overwritten in local.conf according to your NAGIOS installation and configuration.
  #

  NAGCMD="/usr/sbin/send_nsca"
  NAGSVC="DRLM"
  NAGHOST="$HOSTNAME"
  NAGCONF"/etc/drlm/alerts/nagios.cfg"

**nagios_sample.cfg**

Copy the sample DRLM configuration for Nagios to previously defined $NAGCONF and adjust it to your environment needs.

::

  #### DRLM (Disaster Recovery Linux Manager) Nagios error reporting sample configuration file.
  #### Default: /etc/drlm/alerts/nagios.cfg

  ### identity = <string>
  #   Send  the  specified  client identity to the server.
  #   By default, localhost will be used.

  identity = "< client identity >"

  ### server = <string>
  #   Connect and talk to the specified server address or hostname.
  #   The  default server is "localhost".

  server = "< nagios based server >"

  ### port = <string>
  #   Connect  to  the  specified  service  name or port number on the
  #   server instead of using the default port (5668).

  port = < nagios based listening port  >
  password = "change-me"

.. note::
  The configuration on the server side is not in the scope of this documentation. Please check your Nagios service documentation
  to configure properly the NSCA service and how to report DRLM alerts.

  For reference you can check:
      * https://www.nsca-ng.org/documentation/nsca-ng.pdf
      * https://www.nsca-ng.org/documentation/nsca-ng.cfg.pdf
      * https://www.nsca-ng.org/documentation/send_nsca.pdf
      * https://www.nsca-ng.org/documentation/send_nsca.cfg.pdf




Configure Zabbix reporting
---------------------------

In order to configure Zabbix Error reporting on DRLM, the Zabbix Agent must be installed.

**Debian 7/8/9/10/11**

::

  ~# apt-get install zabbix-agent

.. warning::
  On debian 7 (wheezy) the backports repository  must be configured in order to install zabbix-agent.

**RHEL/Centos 6/7/8**

::

  ~# yum install zabbix-agent

.. warning::
  May be needed to add EPEL repositories if not present, because those packages are not included in distribution repositories.


The following options are DRLM defaults, change any of them to your installation requirements in /etc/drlm/local.conf.

::

  ~# vi /etc/drlm/local.conf

  #
  # REPORT_TYPE=zabbix
  # ZABBIX VARIABLES
  #
  # These are default values and can be overwritten in local.conf according to your ZABBIX installation and configuration.
  #

  ZABBCMD="/usr/bin/zabbix_sender"
  ZABBKEY="DRLM"
  ZABBCONF="/etc/drlm/alerts/zabbix.cfg"

**zabbix_sample.cfg**

Copy the sample DRLM configuration for Zabbix to previously defined $ZABBCONF and adjust it to your environment needs.

::

  #### DRLM (Disaster Recovery Linux Manager) Zabbix error reporting sample configuration file.
  #### Default: /etc/drlm/alerts/zabbix.cfg

  ### Option: ServerActive
  #	List of comma delimited IP:port (or hostname:port) pairs of Zabbix servers for active checks.
  #	If port is not specified, default port is used.

  #ServerActive=monitoring_server:port,monitoring_proxy:port

  ### Option: Hostname
  #	Unique, case sensitive hostname.
  #	Required for active checks and must match hostname as configured on the server.

  #Hostname=drlm_server_hostname

.. note::
  The configuration on the server side is not in the scope of this documentation. Please check your Zabbix service documentation
  to configure properly the Trapper item and how to report DRLM alerts.

  For reference you can check:
      * https://www.zabbix.com/documentation/3.2/manual/config/items/itemtypes/trapper
      * https://www.zabbix.com/documentation/3.2/manpages/zabbix_sender


Configure Mail reporting
---------------------------

In order to configure Zabbix Error reporting on DRLM, the Heirloom Mailx must be installed.

**Debian 7/8/9/10/11**

::

  ~# apt-get install heirloom-mailx


**RHEL/Centos 6/7/8**

::

  ~# yum install mailx


The following options are DRLM defaults, change any of them to your installation requirements in /etc/drlm/local.conf.

::

  ~# vi /etc/drlm/local.conf

  #
  # REPORT_TYPE=mail
  # MAIL VARIABLES
  #
  # These are default values and can be overwritten in local.conf according to your MAIL installation and configuration.
  #

  MAILCMD="/bin/mailx"
  MAILSUBJECT="DRLM ERROR ALERT ($HOSTNAME)"
  MAILCONF="/etc/drlm/alerts/mail.cfg"
  MAIL_TO="root@localhost"
  MAIL_CC=""
  MAIL_BCC=""

**mail_sample.cfg**

Copy the sample DRLM configuration for Mailx to previously defined $MAILCONF and adjust it to your environment needs.

::

  #### DRLM (Disaster Recovery Linux Manager) Mail error reporting sample configuration file.
  #### Default: /etc/drlm/alerts/mail.cfg

  ### Configure MAIL_FROM [ address(friendly_name) ].

  #set from="john@doe.org(John Doe)"

  ### Set SMTP server configuration [ ipaddr_or_dnsname:port ].

  #set smtp=smtp_server:25

  ### Set SMTP server Auth Options [ Username (mail address) and Password ] if required.

  #set smtp-auth=login
  #set smtp-auth-user=john@doe.org
  #set smtp-auth-password=SoMePaSsWoRd

  ###############################################
  #### Example using external Gmail smtp servers:

  #set from="john@doe.org(John Doe)"
  #set smtp-use-starttls
  #set ssl-verify=ignore
  #set smtp-auth=login
  #set smtps=smtp://smtp.gmail.com:587
  #set smtp-auth-user=some_user@gmail.com
  #set smtp-auth-password=pAsSwOrD
  #set nss-config-dir=/etc/ssl/certs

.. note::
  The configuration on the Mail server is not in the scope of this documentation. Please check your Mail service configuration
  to configure properly mailx to report DRLM alerts.


Configure HPOM (former OVO) reporting
-------------------------------------

In order to configure HPOM(OVO) Error reporting on DRLM, the HPOM(OVO) agent must be installed. This may vary depending on your version,
please check your product documentation in order to install it properly.
DRLM uses **opcmsg** binary to report errors to HPOM server.

The following options are DRLM defaults, change any of them acording to your installation requirements in /etc/drlm/local.conf.

::

  ~# vi /etc/drlm/local.conf:

  #
  # REPORT_TYPE=ovo
  # HP OVO VARIABLES
  #
  # These are default values and can be overwritten in local.conf according to your HP OVO installation and configuration.
  #

  OVOCMD="/opt/OV/bin/OpC/opcmsg"
  OVOAPP="DRLM"
  OVOSEV="Major"
  OVOOBJ="OS"
  OVOMSGGRP="LINUX"

.. note::
  The configuration on the server side is not in the scope of this documentation. Please check HPOM (OVO) documentation
  to configure properly the server side and define how to report DRLM alerts.

XML/JSON reporting
------------------

In order to configure XML or JSON Error reporting on DRLM you need to define the REPORT_TYPE as xml or json.
Then you need to define the DRLM_SEND_ERROR_URL to the desired URL to send the XML or JSON to.

::

  # REPORT_TYPE=[xml|json]
  #
  # XML VARIABLES
  # =============
  #
  # These are default values and can be overwritten in local.conf according to your XML installation and configuration.
  #  DRLM_SEND_ERROR_BIN="/usr/sbin/drlm-send-error"   #Default drlm-send-error command path
  #  DRLM_SEND_ERROR_URL="http://servertostorexml:9090/"	 #Desired URL to send the XML to
  #  DRLM_SEND_ERROR_MSG=									 
  #	If DRLM_SEND_ERROR_MSG is set to "" will be send a default error like the next one:
  #
  #			<drlm>
  #			   <version>2.4.13-git</version>
  #			   <type>ERROR</type>
  #			   <server>drlmserver</server>
  #			   <client>drlmclient</client>
  #			   <configuration>default</configuration>
  #			   <os>Debian 11.6</os>
  #			   <rear>2.6/2020-06-17</rear>
  #			   <workflow>runbackup</workflow>
  #			   <message>2023-02-09 09:11:21 drlm:runbackup:drlmclient:ERROR: Client drlmclient SSH Server on 22 port is not available</message>
  #			</drlm>
  #
  #   But DRLM_SEND_ERROR_MSG can be customized specifying an XML string containing DRLM runtime environment variables. 
  #	For example: DRLM_SEND_ERROR_MSG='<drlm><server>$HOSTNAME</server><client>$CLI_NAME</client><nbd>$NBD_DEVICE</nbd><message>$ERRMSG</message></drlm>'
  #	In the header of the runbackup scripts (/usr/share/drlm/backup/run/default/*.sh) you can find all the variables available at any time
  #   
  # JSON VARIABLES
  # ==============
  #
  # These are default values and can be overwritten in local.conf according to your JSON installation and configuration.
  #  DRLM_SEND_ERROR_BIN="/usr/sbin/drlm-send-error"       #Default drlm-send-error command path
  #  DRLM_SEND_ERROR_URL="http://servertostorejson:9090/"	 #Desired URL to send the JSON to
  #  DRLM_SEND_ERROR_MSG=									 
  #	If DRLM_SEND_ERROR_MSG is set to "" will be send a default error like the next one:
  #
  #   {
  #	  "program":"drlm", 
  #	  "version":"2.4.13",
  #	  "type":"ERROR",
  #	  "server":"drlmserver",
  #	  "client":"drlmclient",
  #	  "configuration":"default",
  #	  "os":"Debian 11.6",
  #	  "rear":"2.6/2020-06-17",
  #	  "workflow":"runbackup",
  #	  "message":"2023-02-09 11:40:58 drlm:runbackup:drlmclient:ERROR: Client drlmclient SSH Server on 22 port is not available"
  #	}
  #
  #   But DRLM_SEND_ERROR_MSG can be customized specifying an JSON string containing DRLM runtime environment variables. 
  #	For example: DRLM_SEND_ERROR_MSG=''{\"name\":\"$HOSTNAME\", \"backup_type\":\"$DRLM_BKP_TYPE\", \"ERROR\":\"$ERRMSG\"}''
  #	In the header of the runbackup scripts (/usr/share/drlm/backup/run/default/*.sh) you can find all the variables available at any time
  #   

  ERR_REPORT=yes
  REPORT_TYPE=xml
  ERR_MESSAGE='$(Stamp)$(hostname):$WORKFLOW:$CLI_NAME:$CLI_CFG:$DRLM_BKP_TYPE:ERROR: $@'

  DRLM_SEND_ERROR_BIN="/usr/sbin/drlm-send-error"
  DRLM_SEND_ERROR_URL="https://server_to_store_xml:9090/endpoint"
  DRLM_SEND_ERROR_MSG=""

Telegram reporting
------------------

In order to configure Telegram Error reporting on DRLM you need to create a Telegram Bot and get the token and chatid.
DRLM uses **curl** binary to report errors to Telegram server.

::

  ERR_REPORT=yes
  REPORT_TYPE=telegram
  ERR_MESSAGE='$(Stamp)$(hostname):$WORKFLOW:$CLI_NAME:$CLI_CFG:$DRLM_BKP_TYPE:ERROR: $@'

  TELEGRAM_CMD="/usr/bin/curl"
  TELEGRAM_TOKEN="6664444777:ABFWZmTx_BEgXIxGzd9gjLYOhf69Xq0QWNR"
  TELEGRAM_CHATID="-1430996632434" 
