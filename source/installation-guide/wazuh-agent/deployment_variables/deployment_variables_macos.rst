.. Copyright (C) 2021 Wazuh, Inc.

.. meta::
  :description: Learn about the variables that facilitate the deployment of the Wazuh agent on macOS in this section of our documentation.
  
.. _deployment_variables_macos:

Deployment variables for macOS
==============================

For an agent to be fully deployed and connected to the Wazuh server it needs to be installed, registered and configured. To make the process simple, the installers can use variables that allow the configuration provisioning.

Below you can find a table describing the variables used by Wazuh installers, and a few examples on how to use them.


+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Option                              | Description                                                                                                                                                                         |
+=====================================+=====================================================================================================================================================================================+
|   WAZUH_MANAGER                     |  Specifies the manager IP address or hostname. In case you want to specify multiple managers, you can add them separated by commas. See :ref:`address <server_address>`.            |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_MANAGER_PORT                |  Specifies the manager’s connection port. See :ref:`port <server_port>`.                                                                                                            |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_PROTOCOL                    |  Sets the communication protocol between the manager and the agent. Accepts UDP and TCP. Default is TCP. See :ref:`protocol <server_protocol>`.                                     |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_SERVER         |  Specifies the Wazuh registration server, used for the agent registration. See :ref:`agent-auth options  <agent-auth>`.                                                             |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_PORT           |  Specifies the port used by the Wazuh registration server. See :ref:`agent-auth options  <agent-auth>`.                                                                             |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_PASSWORD       |  Sets the Wazuh registration server. See :ref:`agent-auth options  <agent-auth>`.                                                                                                   |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_KEEP_ALIVE_INTERVAL         |  Sets the time between agent checks for manager connection. See :ref:`notify-time <notify_time>`.                                                                                   |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_TIME_RECONNECT              |  Sets the time interval for the agent to reconnect with the Wazuh manager when connectivity is lost. See :ref:`time-reconnect  <time_reconnect>`.                                   |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_CA             |  Host SSL validation need of Certificate of Authority. This option specifies the CA path. See :ref:`agent-auth options  <agent-auth>`.                                              |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_CERTIFICATE    |  The SSL agent verification needs a CA signed certificate and the respective key. This option specifies the certificate path. See :ref:`agent-auth options  <agent-auth>`.          |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_REGISTRATION_KEY            |  Specifies the key path completing the required variables with WAZUH_REGISTRATION_CERTIFICATE for the SSL agent verification process. See :ref:`agent-auth options  <agent-auth>`.  |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_AGENT_NAME                  |  Designates the agent's name. By default it will be the computer name. See :ref:`agent-auth options  <agent-auth>`.                                                                 |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|   WAZUH_AGENT_GROUP                 |  Assigns the agent to one or more existing groups (separated by commas). See :ref:`agent-auth options  <agent-auth>`.                                                               |
+-------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples:

* Registration with password:

.. code-block:: console

     # launchctl setenv WAZUH_MANAGER "10.0.0.2" WAZUH_REGISTRATION_PASSWORD "TopSecret" \
          WAZUH_AGENT_NAME "macos-agent" && installer -pkg wazuh-agent-|WAZUH_LATEST|-|WAZUH_REVISION_OSX|.pkg -target /

* Registration with password and assigning a group:

.. code-block:: console

     # launchctl setenv WAZUH_MANAGER "10.0.0.2" WAZUH_REGISTRATION_SERVER "10.0.0.2" WAZUH_REGISTRATION_PASSWORD "TopSecret" \
          WAZUH_AGENT_GROUP "my-group" && installer -pkg wazuh-agent-|WAZUH_LATEST|-|WAZUH_REVISION_OSX|.pkg -target /

* Registration with relative path to CA. It will be searched at your Wazuh installation folder:

.. code-block:: console

     # launchctl setenv WAZUH_MANAGER "10.0.0.2" WAZUH_REGISTRATION_SERVER "10.0.0.2" WAZUH_AGENT_NAME "macos-agent" \
          WAZUH_REGISTRATION_CA "rootCA.pem" && installer -pkg wazuh-agent-|WAZUH_LATEST|-|WAZUH_REVISION_OSX|.pkg -target /

* Registration with protocol:

.. code-block:: console

     # launchctl setenv WAZUH_MANAGER "10.0.0.2" WAZUH_REGISTRATION_SERVER "10.0.0.2" WAZUH_AGENT_NAME "macos-agent" \
          WAZUH_PROTOCOL "udp" && installer -pkg wazuh-agent-|WAZUH_LATEST|-|WAZUH_REVISION_OSX|.pkg -target /

* Registration and adding multiple address:

.. code-block:: console

     # launchctl setenv WAZUH_MANAGER "10.0.0.2,10.0.0.3" WAZUH_REGISTRATION_SERVER "10.0.0.2" \
          WAZUH_AGENT_NAME "macos-agent" && installer -pkg wazuh-agent-|WAZUH_LATEST|-|WAZUH_REVISION_OSX|.pkg -target /

* Absolute paths to CA, certificate or key that contain spaces can be written as shown below:

.. code-block:: console

     # launchctl setenv WAZUH_MANAGER "10.0.0.2" WAZUH_REGISTRATION_SERVER "10.0.0.2" WAZUH_REGISTRATION_KEY "/var/ossec/etc/sslagent.key" \
          WAZUH_REGISTRATION_CERTIFICATE "/var/ossec/etc/sslagent.cert" && installer -pkg wazuh-agent-|WAZUH_LATEST|-|WAZUH_REVISION_OSX|.pkg -target /

.. note:: To verify agents identity with the registration server, it's necessary to use both KEY and PEM options. See the :ref:`Registration Service with host verification - Agent verification with host validation <host-verification-registration>` section.
