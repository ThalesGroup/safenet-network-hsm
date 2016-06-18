


# safenet-network-hsm
- - -

This repository contains the Pivotal Cloud Foundry Tile that enables the binding of a SafeNet Luna HSMs to applications in PCF on-prem environments.
[![Join the chat at https://gitter.im/gemalto-oss/safenet-network-hsm](https://badges.gitter.im/gemalto-oss/safenet-network-hsm.svg)](https://gitter.im/gemalto-oss/safenet-network-hsm?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Join the chat at https://gemalto-oss.slack.com](http://slackin.apps.cf-hsm.io/badge.svg)](https://slackin.apps.cf-hsm.io)

## Baseline and Pre-requisites

*   An account at http://cf-hsm.io.  If you don't have one, you can sign up for free at: http://cf-hsm.io/.
*   You are using cf CLI v6.16 or later
*   Pivotal Ops Manager v1.5
*   You are authenticated with Cloud Foundry as an admin user or as a space developer.
*   You are using a private deployment of Pivotal Cloud Foundry. Note: This service broker does not work on Pivotal Web Services
*   You are testing or developing your applications.

## Getting Your API Key, Secret, and Endpoint

*   Access to the cf-hsm.io service is controlled by an API key, secret, and endpoint that is unique to each account. To get your unique credentials, click on **""LOG IN"**" in the top right of the homepage, or browse to **"LOG IN"**.
*   On successful sign in/up, you will be returned to the home page, and the **"LOG IN"** link will be replaced by **"MY ACCOUNT"**.
*   Click on **"MY ACCOUNT"**, and your **"API key"**, **"API secret"**, and **"endpoint"** will be displayed.  You will need this information when configuring the service broker (below)

## Installation of the Tile

*    Download the Gemalto SafeNet Network HSM Service Broker Tile
*    Login to PCF Ops Manager
*    On the left side of the page click on the button **"Import a Product"**
*    Select the downloaded **.pivotal** tile file (e.g. gemalto-safenet-hsm-external-0.1.0.pivotal)
*    Allow the file upload to be completed.  **Note:** Although there are no indications during the upload, a pop-up message appears upon completion/failure.

Once the tile is uploaded to PCF environment:
*    Hover the mouse on **"gemalto-safenet-hsm-external-x.x.x.pivotal"** on the left sidebar titled **"Available Products"**.
*    Select **"Add>>"** button and then click **"Apply changes"** on the upper right
*    Click on **"SafeNet Network HSM"** that newly appeared on the page.
*    Under **"Settings"** tab select **"HSM Service Settings"**
*    Enter the **"API key"**, **"API secret"**, and **"endpoint"** recorded from your account (above)
*    Click the **"Save"** button
*    Go back to **"Installation Dashboard"** (link on top left of the page)
*    Click the **"Apply Changes"** button on top right of the page.
*    Wait for the changes to be applied.  The time will vary depending on the products installed, machine configuration, etc.
*    Once changes are applied, the tile will appear in the **"Marketplace"**


## Use the SafeNet Network HSM Service Broker using PCF Console

*    Login to your PCF console
*    Select the **"Org"**, or alternatively create new **"Org"** and **"Space"** as needed
*    Click on **"Marketplace"**
*    Select **"SafeNet Network HSM"** Tile and click on **"View Plan Options"**
*    Select the desired plan from the left (e.g. small_paritition) and click on **"Select this plan"**
*    Enter the **"Instance Name"**
*    Select the desired **"space"**
*    Select the **"application"** to which to bind the service
*    Click the **"Add"** button


## Use the SafeNet Network HSM Service Broker using the CLI
**Note:** Because of the sensitive data used in the command, you may choose to turn off ~/.bash_history before executing this command. In Linux this is done with the command:

	set +o history
After completing the command, ~/.bash_history is re-enabled with the command:
	
	set -o history
	
*   Login to your PCF Instance:

	    cf api <cfApiEndpoint> [--skip-ssl-validation]
	    cf login -u <user> -p <password>
	
     **Note:** you will be prompted to select the desired org and space if you have multiple org/spaces.
*   Navigate to your application
*   The service broker is register using:

		$ cf create-service-broker <myBrokerName> <apiKey> <apiSecret> <apiEndPoint>
	

	For example:
	
		$ cf create-service-broker hsm-service-broker kkkkkkkkkkkkkkkkkkkkkkkkkkkkkkkk ssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssss https://servicebroker.apps.cf-hsm.io/xxxxxxxx-xxxx-xxxx-xxxxxxxxxxxxxxxx
	

*   To enable access to the newly registered service broker, use:
		$ cf enable-service-access <myBrokerName>
	
	For example:
	
		$ cf enable-service-access hsm-service-broker
		
*   To create a service that can be used by applications, use:

		$ cf create-service <myBrokerName> <brokerPlan> <serviceInstance>
	
	For example:
	
		$ cf create-service hsm-service-broker small_partition hsm
	
*   To bind your application to the service broker, use:

		$ cf bind-service <appName> <serviceInstance>
	For example:
	
		$ cf bind-service myApp hsm
*    To restage the application, use:

	    $ cf restage <appName>
	For example:
		
		$cf restage myApp

## Additional Resources
*  [Official Documentation](http://cf-hsm.io/docs/register-cf-hsm-io-service-broker/)
