############
Introduction
############


The IOI API Server provides Bloomberg users with the ability to publish IOIs into Bloomberg and 
subscribe IOIs in Equities and Derivatives using Bloomberg API

The IOI API Server for subscribing requires separate authorization by the publishing broker on top of 
the Bloomberg authorization.  


.. note::

	IOI API users will need the following steps completed before using the IOI API service.

		#. Signed EMSxNET Order Originator Agreement.
		#. Install serverapi.exe and register with Bloomberg.
		#. Enable IOI API per UUID by the Global EMSX Trade Desk for Test (Beta) and Production. 
		#. To get access to IOI API in UAT and production, please click <Help><Help> on EMSX<GO>.
		#. Download Bloomberg Desktop API v3 SDK from WAPI<GO> in Bloomberg terminal.
		

To get access to IOI API in UAT and production, please click <Help><Help> on EMSX<GO>.


Sign Up - Programming Support
=============================


For general programming support, please open an account through the following URL:- 

https://service.bloomberg.com


For a new user, you will need to first start by creating the account in https://service.bloomberg.com  and select “Request a new account”.


.. image:: /image/signIn.png
	:width: 300pt


Select Enterprise Solutions:-


.. image:: /image/accountType.png
	:width: 300pt


Fill out the details on Account Registration:- 


.. image:: /image/accountReg.png
	:width: 300pt

Select B-Pipe, select the role as Technical Contact, and insert Customer #.  The Customer # can be found in the terminal by 
typing IAM<GO>. Select production information as B-Pipe and click register to finish:-


.. image:: /image/register.png
	:width: 300pt



How to install serverapi.exe
============================


Please follow the following steps to install and register the installer with Bloomberg Enterprise Solutions with the assistance from EMSX Implementation team.

The following example is based on the windows environment and thus there will be slight variations when installing from linux or solaris operating system.


* Run serverapi.exe


.. code-block:: none
	
	C:\temp>serverapiinstaller.exe


* You will see the following message


.. code-block:: none

	C:\temp>serverapiinstaller.exe
	logging to C:\temp\install.2016102610.152444.log

	Bloomberg ECD Installer for Windows (32-bit)
	Version 3.2.2.0


	Warning: This program is protected by copyright law and international treaties.

	Unauthorized reproduction or distribution of this program, or any portion of
	it, may result in severe civil and criminal penalties, and will be prosecuted
	to the maximum extent possible under law.


	logging to C:\temp\install.2016102610.152444.log

	Bloomberg ECD Installer for Windows (32-bit)
	Version 3.2.2.0


	Warning: This program is protected by copyright law and international treaties.

	Unauthorized reproduction or distribution of this program, or any portion of
	it, may result in severe civil and criminal penalties, and will be prosecuted
	to the maximum extent possible under law.


	Would you like to continue? (Y/N): 


* Type:- Y


.. code-block:: none
	
	Would you like to continue? (Y/N): y

	Checking connectivity to Bloomberg ...


	a) via Bloomberg Network to [ Hostname = 208.134.161.62 Port = 8194 ] ...
	Succeeded.

	                            [ Hostname = 208.134.161.158 Port = 8194 ] ...
	Succeeded.

	                            [ Hostname = 208.134.161.18 Port = 8194 ] ...
	Succeeded.

	                            [ Hostname = 208.134.161.179 Port = 8194 ] ...
	Succeeded.

	b) via the Internet to [ Hostname = api1.bloomberg.net Port = 8194 ] ...
	Succeeded.

	                       [ Hostname = api2.bloomberg.net Port = 8194 ] ...
	Succeeded.

	                       [ Hostname = api3.bloomberg.net Port = 8194 ] ...
	Succeeded.

	                       [ Hostname = api4.bloomberg.net Port = 8194 ] ...
	Succeeded.

	                       [ Hostname = api5.bloomberg.net Port = 8194 ] ...
	Succeeded.

	                       [ Hostname = api6.bloomberg.net Port = 8194 ] ...
	Succeeded.

	                       [ Hostname = api7.bloomberg.net Port = 8194 ] ...
	Succeeded.

	                       [ Hostname = api8.bloomberg.net Port = 8194 ] ...
	Succeeded.



	Which of the above routes will you use to connect to Bloomberg? (a/b):


* Select the appropriate network option (private vs. public/internet)


.. code-block:: none
	
	Which of the above routes will you use to connect to Bloomberg? (a/b): b

	Bloomberg Network connectivity unavailable. Connecting via the Internet.

	Select Product Class
	1) blpddm    Software that provides development access to distribute data locally or contribute data to Bloomberg.
	2) ServerApi Provides access to Bloomberg real-time streaming and static data
	0) Quit

	Please enter selection:


* Select:- 2 for Server API


.. code-block:: none

	Please enter selection: 2

	Installation path:
	        'C:\'

	Use this path? (Y/N/Q): 


* Select:- Y


.. code-block:: none

	Use this path? (Y/N/Q): y

	Downloading latest installer ...
	done.

	logging to C:\temp\install.2016102610.152444.log

	Beginning new install ...


.. note::
	
	If the default port is already being used by a different service it may show the following message:


.. code-block:: none

	*** WARNING: Port conflict detected with other service.
	The port of the Desktop will conflict with the ServerApi should installation proceed. If you still want to install
	ServerApi, you will need to specify a different port number.

	 Do you want to continue with the installation? (Y/N) [N]:


* Select:- Y and enter the port


.. code-block:: none
	
	Do you want to continue with the installation? (Y/N) [N]:y
	Please enter ServerApi listen port: [8294]:8294


* Select the version:-

.. code-block:: none

	Versions available for ServerApi
	1) 3.46.6.0     Windows     ServerAPI 2014-07

	2) 3.48.8.1     Windows     ServerAPI 2014-09

	3) 3.48.9.1     Windows     ServerAPI 2014-11

	4) 3.50.7.1     Windows     ServerAPI 2015-01

	5) 3.56.4.1     Windows     ServerAPI 2015-04

	6) 3.60.0.1     Windows     ServerAPI 2015-07

	7) 3.64.5.1     Windows     ServerAPI 2015-10

	8) 3.70.0.1     Windows     ServerAPI 2016-01

	9) 3.72.2.1     Windows     ServerAPI 2016-04

	10) 3.82.3.1     Windows     ServerAPI 2016-10

	11) 3.46.6.0     Windows64   ServerAPI 2014-07

	12) 3.48.8.1     Windows64   ServerAPI 2014-09

	13) 3.48.9.1     Windows64   ServerAPI 2014-11

	14) 3.50.7.1     Windows64   ServerAPI 2015-01

	15) 3.56.4.1     Windows64   ServerAPI 2015-04

	16) 3.60.0.1     Windows64   ServerAPI 2015-07

	17) 3.64.5.1     Windows64   ServerAPI 2015-10

	18) 3.70.0.1     Windows64   ServerAPI 2016-01

	19) 3.72.2.1     Windows64   ServerAPI 2016-04

	20) 3.82.3.1     Windows64   ServerAPI 2016-10

	0) Quit
	Please enter version of ServerApi that you want to install:


* Select the latest:-


.. code-block:: none

	Please enter version of ServerApi that you want to install: 20
	Downloading ServerApi components ...


* Enter other information:-

		Enter the following information:

	        Country (e.g., USA): 
	        State (e.g., NY): 
	        City or Town (e.g., New York): 
	        Company Name (e.g., Bloomberg L.P.): 
	        Department Name (e.g., Equity Trading): 


* Finished:-

.. code-block:: none

		Enter the following information:

	        Country (e.g., USA): USA
	        State (e.g., NY): NY
	        City or Town (e.g., New York): New York
	        Company Name (e.g., Bloomberg L.P.): Bloomberg LP
	        Department Name (e.g., Equity Trading): EMSX

		Creating certificate ...
		done.

		Registering server ...
		done.


		Do you want to install ServerApi as a Windows Service? (Y/N): y

		Installing ServerApi as a windows Service...
		service ServerApi configured for restart on first error
		 done

		done.


		*** Please reboot your computer for changes to take effect ***


		Call Bloomberg's Global Customer Support at +1 (212) 318-2000 and ask for the
		Global Installs desk. The Bloomberg representative will ask you to read your
		registration number over the phone four characters at a time.


		Your registration key is:
		        321c-5ad5-7fa8-2954-1930-abb0-b64c-ecaf-1505-64d4


.. note::

	Once the registration process is completed. EMSX Implementation team globally will assist with configuring the Server Side EMSX API with various execution destinations per client request.


Creating User Identities
========================


In the server environment, the user identities must be created and cached prior to the making requests.  Therefore, the process would look as follows:-


.. image:: /image/userIdentity2.png


.. note::

	Note: The IOI API Server code samples demonstrate how to create identity object.


The first new step is to open the authentication service. This is done in the same way as for any other service in the Bloomberg API. For example:-


.. code-block:: none

		d_authsvc = "//blp/apiauth";
		session.openServiceAsync(d_authsvc);


Once the service is opened, we need to create and send an authorization request. To create an identity for a specific user, you will need the AuthID for the user. This is the name the user is known by in the EMRS system for your server. The values for these names will have been agreed with you as part of the implementation of the server, or subsequently when adding a new user. Also, an IP address is required. The only requirement for this IP address is that it is unique amongst all the identities generated for a session. You can create and send the request as follows:-


.. code-block:: none
	
		private Identity userIdentity;

		*
		*
		*

		Service authService = session.getService(d_authsvc);
		Request authReq = authService.createAuthorizationRequest();
				
		authReq.set("authId", authID);
		authReq.set("ipAddress", appIP);
				
		userIdentity = session.createIdentity();
				
		authRequestID = new CorrelationID();
				
		try
		{
			session.sendAuthorizationRequest(authReq, userIdentity, authRequestID);
		}
		catch (Exception e)
		{
			System.out.println("Unable to send authorization request: " + e.getMessage());
		}


In the above code, you can see that an empty identity object is created using ``session.createIdentity()``. This is the object that will be populated once successful authentication has been achieved, and it is the object that will need to be cached.

We will receive a Response event for the Authentication service. In the example below, we use a ``CorrelationID`` to identify messages from the Authentication service, and check for success or failure:-


.. code-block:: none

		if(msg.correlationID()==authRequestID) {
		
		if(msg.messageType().equals(AUTHORIZATION_SUCCESS)) {
			System.out.println("Authorised...Opening EMSX service...");
			System.out.println("Seat Type: " + userIdentity.seatType().toString());
			session.openServiceAsync(d_service);
		} else if(msg.messageType().equals(AUTHORIZATION_FAILURE)) {
			System.out.println("Authorisation failed...");
			System.out.println(msg.toString());
			wait(1000);
			// Automatically retry...
			sendAuthRequest(session);
		} else { 
			System.out.println("Unexpected authorisation message...");
			System.out.println(msg.toString());
		}
	}


When we receive the successful authorization, we can continue with opening the usual EMSX service. If multiple authorization requests have been sent, for a number of different UUIDs, it is necessary to wait for all the responses before being able to use all the identity objects.

In the above code, you will see that we examine the ‘seatType’ of the identity. The seat type in this case will be either BPS or non-BPS.


Using User Identities
=====================


In the server environment, there is no Bloomberg terminal, and therefore no implied user can be identified.  Moreover, the server is capable of connecting to any number of EMSX user blotters, simultaneously. Therefore, the application making the call must indicate which user is the intended target. This is done through the creation and use of Identity object.

An Identity object represents a specific Bloomberg UUID.  Once created, an Identity object can be cached for 24hrs, and used with every ``sendRequest()`` and ``subscribe()`` call. 

Identity objects are live, that is they remain connected to Bloomberg in real-time and are capable of receiving events. We recommend that an identity is recreated every 24hrs, to ensure that it picks up the latest changes to any user settings, including access to EMSX. 

Any number of user Identity object can be created by a server-side application. If the application uses the identities of real traders within a firm, then each trader would have an identity created to represent them in the server application. The server application would, perhaps, receive an instruction from the upstream client-side application to create an order in a trader’s blotter. The server application would select the appropriate user identity from the cache and add it to the request.

Migrating the existing desktop application call to a server application simply involves changing all ``sendRequest()`` and ``subscribe()`` calls to include the appropriate identity, as follows:-


.. code-block:: none

		DAPI:
			session.sendRequest(request, requestID);
			session.subscribe(subscriptions);

		Server:
			session.sendRequest(request, Identity, requestID);
			session.subscribe(subscriptions, Identity);


IOI API Code Samples
=====================


.. important::

			The latest IOI API Code samples can be found `here`_.

			.. _here: https://github.com/tkim/ioi_api_repository_new




