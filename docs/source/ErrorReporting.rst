Error Reporting Configuration
=============================

DRLM can be configured to report errors on scheduled backups if required.
Is possible to report by mail or integrating with your monitoring service. 
At this time (DRLM 2.0) we support error reporting by mail and integration 
with Nagios, Zabbix and HPOM(OVO) monitoring services.

.. note::
  All reporting configuration samples are located in: /usr/share/drlm/conf/samples

Enable DRLM reporting
---------------------

::

  $ vi /etc/drlm/local.conf

  ########
  #
  # Defines HowTo report Errors using some known and wide used methods
  #
  #    ERR_REPORT=[yes|no]
  #	default: no
  #    REPORT_TYPE=[ovo|nagios|zabbix|mail|...]
  #	default: empty
  #
  ########

  ERR_REPORT=yes
  REPORT_TYPE=<type>


Configure Nagios reporting
---------------------------

In order to configure Nagios Error reporting on DRLM, the Nagios NSCA Client must be installed.  

**Debian 7/8**

::

  $ apt-get install nsca-client

**RHEL/Centos 6/7**

::

  $ yum install nsca-client

.. warning::
  May be needed to add EPEL repositories if not present, because those packages are not included in distribution repositories.


The following options are DRLM defaults, change any of them to your installation requirements in /etc/drlm/local.conf.

::

  $ vi /etc/drlm/local.conf

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

  #identity = "drlm_server_hostname"

  ### server = <string>
  #   Connect and talk to the specified server address or hostname.
  #   The  default server is "localhost".

  #server = "monitoring_server"

  ### port = <string>
  #   Connect  to  the  specified  service  name or port number on the
  #   server instead of using the default port (5668).

  #port = 5667

.. note::
  The configuration on the server side is not in the scope of this documentation. Please check your Nagios service documentation
  to configure properly the NSCA service and how to report DRLM alerts.

  For reference you can check: 
      * https://assets.nagios.com/downloads/nagioscore/docs/Installing_NSCA.pdf
      * http://nagios.sourceforge.net/download/contrib/documentation/misc/NSCA_Setup.pdf
      * https://assets.nagios.com/downloads/nagiosxi/docs/Using-and-Configuring-NSCA-With-Nagios-XI.pdf


Configure Zabbix reporting
---------------------------

In order to configure Zabbix Error reporting on DRLM, the Zabbix Agent must be installed.

**Debian 7/8**

::

  $ apt-get install zabbix-agent

.. warning::
  On debian 7 (wheezy) the backports repository  must be configured in order to install zabbix-agent.

**RHEL/Centos 6/7**

::

  $ yum install zabbix-agent

.. warning::
  May be needed to add EPEL repositories if not present, because those packages are not included in distribution repositories.


The following options are DRLM defaults, change any of them to your installation requirements in /etc/drlm/local.conf.

::

  $ vi /etc/drlm/local.conf

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

**Debian 7/8**

::

  $ apt-get install heirloom-mailx


**RHEL/Centos 6/7**

::

  $ yum install mailx


The following options are DRLM defaults, change any of them to your installation requirements in /etc/drlm/local.conf.

::

  $ vi /etc/drlm/local.conf

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

  $ vi /etc/drlm/local.conf:

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

