Bloomberg API Service for IOI API Subscription
==============================================

For IOI subscription, this is accomplished by referencing ``\\blp\ioisub-beta`` as the service name in your program. This command will allow your service  to redirect all IOI API subscription to the test environment.   

Once the client has thoroughly tested the custom-built strategies, they can access the production environment by changing the service name from  ``\\blp\ioisub-beta`` to ``\\blp\ioisub``.


Accessing the Test Environment
==============================

Bloomberg provides a test environment for clients to build and test their strategies using the IOI API.

Inside the Bloomberg Terminal type DGRT Y087<GO> {Y (number zero) 87} This command allows the particular 
terminal window to log into the beta environment. Please note, when a user is remote into the beta 
environment it only affects that particular terminal window and the other screens will not be affected by 
the DGRT command.

To check which environment your current view is in, type VSAT <GO> inside the Bloomberg terminal.

To get back to production type DGRT OFF <GO>. Please note that the testing environment in Beta will not 
operate in the exact same way as the production environment. Also, please note that the beta environment is a lot slower than the production environment.


Desktop vs. Server Authentication
=================================

Desktop:

.. code-block:: python

    d_ioi = "//blp/ioiapi-beta-request"
    d_host = "localhost"
    d_port = 1234


Server:

.. code-block:: python

    d_ioi = "//blp/ioiapi-beta-request"
    d_auth = "//blp/apiauth"
    d_host = "abc.com"
    d_port = 1234
    d_user = "myAuthID"
    d_ip = "10.20.30.40"

Set authorization request:

.. code-block:: python
    
    def sendAuthRequest(self, session):

        authService = session.getService(d_auth)
        authReq = authService.createAuthorizationRequest()
        authReq.set("emrsID", d_user)
        authReq.set("ipAdress", d_ip)
        self.identity = session.createIdentity()

        print ("Sending authorization rquest: %s" % (authReq))

        session.sendAuthorizationRequest(authReq, self.identity)

        print ("Authorization request sent.")

    ...

    def processAuthorizationStatusEvent(self, event):

        print("Processing AUTHORIZATION_STATUS event")

        for msg in event:

            print("AUTHORIZATION_STATUS message: %s" % (msg))

    ...

    def processEvent(self, event, session):
        try:

        ...

        elif event.eventType() == blpapi.Event.AUTHORIZATION_STATUS:
            self.processAuthorizationStatusEvent(event)

        ...



IOI API Subscription 
====================


The IOI API Subscription allows IOI messages over subscription service.


Full code sample:-

===================== =================== 
`Subscribe IOI cs`_   `Subscribe IOI py`_	
--------------------- ------------------- 
`Subscribe IOI java`_ 
===================== =================== 


.. _Subscribe IOI cs: https://github.com/tkim/ioi_api_repository/blob/master/C%23/cs_dapi_SubscribeIOI.cs

.. _Subscribe IOI java: https://github.com/tkim/ioi_api_repository/blob/master/Java/Java_dapi_SubscribeIOI.java

.. _Subscribe IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_SubscribeIOI.py


.. hint:: 

	Please right click on the top code sample link to open in a new tab.
	


.. code-block:: python
             
    
    def createIOISubscription(self, session):

        print("Create IOI subscription")
    
        #Create the topic string for the ioi subscription.
        ioiTopic = d_ioi + "/ioi"
    
        subscriptions = blpapi.SubscriptionList()
        
        subscriptions.add(topic=ioiTopic,correlationId=ioiSubscriptionID)

        print("Sending subscription...")
        session.subscribe(subscriptions)



Description of Elements
=======================

The following elements are available for equity and options IOI subscription.

+--------------------------------------------------+------------------------------------+---------+
|Element Name                                      | Description                        | Type    |
+==================================================+====================================+=========+
|``change``                                        | IOI State: New, Replace and Cancel | string  |
+--------------------------------------------------+------------------------------------+---------+
|``id_value``                                      |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_natural``                               |                                    | int32   |
+--------------------------------------------------+                                    |         |
|``ioi_offer_natural``                             |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_notes``                                 |                                    | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_notes``                               |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_fixed_currency``                  |                                    | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_fixed_currency``                |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_fixed_price``                     |                                    | float64 |
+--------------------------------------------------+                                    |         | 
|``ioi_offer_price_fixed_price``                   |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_moneyness``                       |                                    | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_moneyness``                     |                                    |         |
+--------------------------------------------------+------------------------------------+---------+ 
|``ioi_bid_price_pegged_limitPrice``               |                                    | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_pegged_limitPrice``             |                                    |         |       
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_pegged_offsetAmount``             |                                    | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_pegged_offsetAmount``           |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_pegged_offsetFrom``               |                                    | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_pegged_offsetFrom``             |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_pegged_offsetType``               |                                    | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_pegged_offsetType``             |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_reference``                       | Bid, Mid, Ask                      | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_reference``                     |                                    |         |  
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_type``                            |                                    | string  | 
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_type``                          |                                    |         | 
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_qualifiers_n``                          |                                    | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_qualifiers_n``                        |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_qualifiers_count``                      |                                    | int32   |
+--------------------------------------------------+                                    |         |
|``ioi_offer_qualifiers_count``                    |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_referencePrice_currency``               |                                    | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_referencePrice_currency``             |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_referencePrice_price``                  |                                    | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_referencePrice_price``                |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_size_quality``                          |                                    | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_size_quality``                        |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_size_quantity``                         |                                    | int64   |
+--------------------------------------------------+                                    |         |
|``ioi_offer_size_quantity``                       |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_size_type``                             |                                    | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_size_type``                           |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_volatility``                            |                                    | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_volatility``                          |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_clientId``                                  |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_goodUntil``                                 |                                    | dateTime|
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_delta``            |                                    | float64 |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_exchange``         |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_expiry``           |                                    | dateTime|
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_futureRefDate``    |                                    | dateTime|
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_listed_figi``      |                                    | string  | 
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_listed_ticker``    |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_listed_type``      |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_ratio``            |                                    | float64 |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_strike``           |                                    | float64 |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_style``            | European, American                 | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_type``             |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_underlying_figi``  |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_underlying_ticker``|                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_underlying_type``  |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_count``              |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_structure``               | Custom, CallSpread, PutSpread,     | string  |
|                                                  +------------------------------------+         |
|                                                  | Straddle, Strangle, SingleLegCall, |         |
|                                                  +------------------------------------+         |
|                                                  | SingleLegPut, CalendarCallSpread,  |         |
|                                                  +------------------------------------+         |
|                                                  | CalendarPutSpread,                 |         |
|                                                  +------------------------------------+         |
|                                                  | CallSpreadReversal,                |         |
|                                                  +------------------------------------+         |
|                                                  | PutSpreadReversal,                 |         | 
|                                                  +------------------------------------+         |
|                                                  | DiagonalCalendarCallSpread,        |         |
|                                                  +------------------------------------+         |
|                                                  | DiagonalCalendarPutSpread,         |         |
|                                                  +------------------------------------+         |
|                                                  | CallButterfly, PutButterfly,       |         |
|                                                  +------------------------------------+         |
|                                                  | IronButterfly, RiskReversal, Box,  |         |
|                                                  +------------------------------------+         |
|                                                  | CallLadder, PutLadder, CallCondor, |         |
|                                                  +------------------------------------+         |
|                                                  | PutCondor, IronCondor, JellyRoll,  |         |
|                                                  +------------------------------------+         |
|                                                  | RatioCallSpread, RatioPutSpread    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_stock_security_figi``            |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_stock_security_ticker``          |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_stock_security_type``            |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_type``                           |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_benchmark``                         |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_broker``                            |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_customId``                          |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_orderType``                         |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_strategy_brief``                    |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_strategy_detailed``                 |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_strategy_name``                     |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_sentTime``                                  |                                    | DateTime|
+--------------------------------------------------+------------------------------------+---------+
|``originalId_value``                              |                                    | String  |
+--------------------------------------------------+------------------------------------+---------+


