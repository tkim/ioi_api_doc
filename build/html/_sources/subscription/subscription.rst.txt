Bloomberg API Service for IOI API Subscription
==============================================
For IOI subscription, this is accomplished by referencing ``//blp/ioisub-beta`` as the service name in your program. This command will allow your service  to redirect all IOI API subscription to the test environment.   

Once the client has thoroughly tested the custom-built strategies, they can access the production environment by changing the service name from  ``//blp/ioisub-beta`` to ``//blp/ioisub``.


Accessing the Test Environment
==============================
Bloomberg provides a test environment for clients to build and test their strategies using the IOI API.

Inside the Bloomberg Terminal type ``UAT ON <GO>``.This command allows the particular terminal window and launchpad to log into the beta environment. Please note, when a user is remote into the beta environment it only affects that particular terminal window and the other Bloomberg panels will not be affected by the ``UAT ON <GO>`` command.

To check which environment your current view is in, type ``VSAT <GO>`` inside the Bloomberg terminal.

To get back to production type ``UAT OFF <GO>``. Please note that the testing environment in Beta will not 
operate in the exact same way as the production environment. Also, please note that the beta environment is a lot slower than the 
production environment and no one should perform any volume or load testing in the beta environment.


API Demo Tool 
=============
API Demo Tool is a handy tool while developing on any Bloomberg API services. The API Demo Tool provides real-time schema viewing tool among other handy tools that can be leveraged during the initial development.

The API Demo Tool can be downloaded from the Bloomberg terminal along with other generic Bloomberg API code samples.

    ``WAPI<GO>`` >> ``API Download Center`` >> ``Download`` 

    
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
   :linenos:          
    
    def createIOISubscription(self, session):

        print("Create IOI subscription")
    
        #Create the topic string for the ioi subscription.
        ioiTopic = d_ioi + "/ioi"
    
        subscriptions = blpapi.SubscriptionList()
        
        subscriptions.add(topic=ioiTopic,correlationId=ioiSubscriptionID)

        print("Sending subscription...")
        session.subscribe(subscriptions)


Output:-

.. code-block:: none
    
    C:\Users\_scripts>py -3 py_dapi_SubscribeIOI.py
    Bloomberg - IOI API Example - DesktopAPI - SubscribeIOI
    Connecting to localhost:8194
    Processing SESSION_STATUS event
    SessionConnectionUp = {
        server = "localhost:8194"
        encryptionStatus = "Clear"
    }

    Session connection is up
    Processing SESSION_STATUS event
    SessionStarted = {
        initialEndpoints[] = {
            initialEndpoints = {
                address = "localhost:8194"
            }
        }
    }

    Session started...
    Processing SERVICE_STATUS event
    ServiceOpened = {
        serviceName = "//blp/ioisub-beta"
    }

    IOIAPI service opened... Sending request...
    Create IOI subscription
    Sending subscription: //blp/ioisub-beta/ioi
    Processing SUBSCRIPTION_STATUS event
    SUBSCRIPTION_STATUS MESSAGE: SubscriptionStarted = {
        exceptions[] = {
        }
        streamIds[] = {
            "1"
        }
        receivedFrom = {
            address = "localhost:8194"
        }
        reason = "Subscriber made a subscription"
    }

    IOIAPI subscription started...
    SUBSCRIPTION_STATUS MESSAGE: SubscriptionStreamsActivated = {
        streams[] = {
            streams = {
                id = "1"
                endpoint = {
                    address = "localhost:8194"
                }
            }
        }
        reason = "Subscriber made a subscription"
    }

    Processing SUBSCRIPTION_DATA event
    Ioidata = {
        ioi_instrument_type = "stock"
        ioi_instrument_stock_security_ticker = "VOD LN Equity"
        ioi_goodUntil = 2020-01-23T14:45:59.000+00:00
        ioi_bid_price_type = "fixed"
        ioi_bid_price_fixed_price = 226.500000
        ioi_bid_price_fixed_currency = "GBp"
        ioi_bid_size_type = "quantity"
        ioi_bid_size_quantity = 1000
        ioi_bid_notes = "bid notes"
        ioi_bid_qualifiers_count = 0
        ioi_bid_natural = 0
        ioi_sentTime = 2020-01-23T14:30:59.267+00:00
        ioi_id = "-1034576088"
        state = "New"
        id_value = "-1034576088"
        originalId_value = "-1034576088"
        trader_uuid = 6767714
        trader_acronym = "B:BLP"
        trader_username = "TKIM"
        receivedTime = 2020-01-23T14:30:59.267+00:00
    }

    IOI MESSAGE: CorrelationID(1)
    ioi_instrument_type: stock
    ioi_instrument_option_legs_count: 0
    ioi_instrument_option_legs_0_strike: 0
    ioi_instrument_option_legs_0_expiry:
    ioi_instrument_option_legs_0_type:
    ioi_instrument_option_legs_0_ratio: 0
    ioi_instrument_option_legs_0_underlying_type:
    ioi_instrument_option_legs_0_underlying_ticker:
    ioi_instrument_option_legs_0_underlying_figi:
    ioi_instrument_option_legs_0_exchange:
    ioi_instrument_option_legs_0_style:
    ioi_instrument_option_legs_0_futureRefDate:
    ioi_instrument_option_legs_0_delta: 0
    ioi_instrument_option_legs_1_strike: 0
    ioi_instrument_option_legs_1_expiry:
    ioi_instrument_option_legs_1_type:
    ioi_instrument_option_legs_1_ratio: 0
    ioi_instrument_option_legs_1_underlying_type:
    ioi_instrument_option_legs_1_underlying_ticker:
    ioi_instrument_option_legs_1_underlying_figi:
    ioi_instrument_option_legs_1_exchange:
    ioi_instrument_option_legs_1_style:
    ioi_instrument_option_legs_1_futureRefDate:
    ioi_instrument_option_legs_1_delta: 0
    ioi_instrument_option_legs_2_strike: 0
    ioi_instrument_option_legs_2_expiry:
    ioi_instrument_option_legs_2_type:
    ioi_instrument_option_legs_2_ratio: 0
    ioi_instrument_option_legs_2_underlying_type:
    ioi_instrument_option_legs_2_underlying_ticker:
    ioi_instrument_option_legs_2_underlying_figi:
    ioi_instrument_option_legs_2_exchange:
    ioi_instrument_option_legs_2_style:
    ioi_instrument_option_legs_2_futureRefDate:
    ioi_instrument_option_legs_2_delta: 0
    ioi_instrument_option_legs_3_strike: 0
    ioi_instrument_option_legs_3_expiry:
    ioi_instrument_option_legs_3_type:
    ioi_instrument_option_legs_3_ratio: 0
    ioi_instrument_option_legs_3_underlying_type:
    ioi_instrument_option_legs_3_underlying_ticker:
    ioi_instrument_option_legs_3_underlying_figi:
    ioi_instrument_option_legs_3_exchange:
    ioi_instrument_option_legs_3_style:
    ioi_instrument_option_legs_3_futureRefDate:
    ioi_instrument_option_legs_3_delta: 0
    ioi_instrument_option_structure:
    ioi_instrument_stock_security_ticker: VOD LN Equity
    ioi_instrument_stock_security_figi:
    ioi_goodUntil: 2020-01-23T14:45:59.000+00:00
    ioi_bid_price_type: fixed
    ioi_bid_price_fixed_price: 226
    ioi_bid_price_fixed_currency: GBp
    ioi_bid_price_pegged_offsetAmount: 0
    ioi_bid_price_pegged_offsetFrom:
    ioi_bid_price_pegged_limitPrice: 0
    ioi_bid_price_reference:
    ioi_bid_price_moneyness: 0
    ioi_bid_size_type: quantity
    ioi_bid_size_quantity: 1000
    ioi_bid_size_quality:
    ioi_bid_referencePrice_price: 0
    ioi_bid_referencePrice_currency:
    ioi_bid_volatility: 0
    ioi_bid_notes: bid notes
    ioi_bid_qualifiers_count: 0
    ioi_bid_qualifiers_0:
    ioi_bid_qualifiers_1:
    ioi_bid_qualifiers_2:
    ioi_bid_qualifiers_3:
    ioi_bid_qualifiers_4:
    ioi_offer_price_type:
    ioi_offer_price_fixed_price: 0
    ioi_offer_price_fixed_currency:
    ioi_offer_price_pegged_offsetAmount: 0
    ioi_offer_price_pegged_offsetFrom:
    ioi_offer_price_pegged_limitPrice: 0
    ioi_offer_price_reference:
    ioi_offer_price_moneyness: 0
    ioi_offer_size_type:
    ioi_offer_size_quantity: 0
    ioi_offer_size_quality:
    ioi_offer_referencePrice_price: 0
    ioi_offer_referencePrice_currency:
    ioi_offer_volatility: 0
    ioi_offer_notes:
    ioi_offer_qualifiers_count: 0
    ioi_offer_qualifiers_0:
    ioi_offer_qualifiers_1:
    ioi_offer_qualifiers_2:
    ioi_offer_qualifiers_3:
    ioi_offer_qualifiers_4:
    ioi_routing_strategy_name:
    ioi_routing_strategy_brief:
    ioi_routing_strategy_detailed:
    ioi_routing_customId:
    ioi_routing_broker:
    ioi_sentTime: 2020-01-23T14:30:59.267+00:00
    change:
    Processing SUBSCRIPTION_DATA event
    Processing SUBSCRIPTION_DATA event
    Exception:  raw write() returned invalid length 72 (should have been between 0 and 36)
    Processing SUBSCRIPTION_DATA event
    Ioidata = {
        ioi_instrument_type = "stock"
        ioi_instrument_stock_security_ticker = "SINA UW Equity"
        ioi_goodUntil = 2020-01-23T14:36:15.000+00:00
        ioi_bid_price_type = "fixed"
        ioi_bid_price_fixed_price = 40.390000
        ioi_bid_price_fixed_currency = "USD"
        ioi_bid_size_type = "quantity"
        ioi_bid_size_quantity = 8279
        ioi_bid_notes = "1/22/2020 3:38:35 PM"
        ioi_bid_qualifiers_count = 0
        ioi_bid_natural = 0
        ioi_sentTime = 2020-01-23T14:31:15.183+00:00
        ioi_id = "D4291-23JAN2020-229-x0"
        state = "New"
        id_value = "1781985133"
        originalId_value = "1781985133"
        trader_uuid = 12624540
        trader_acronym = "B:BLP"
        receivedTime = 2020-01-23T14:31:15.183+00:00
    }

    IOI MESSAGE: CorrelationID(1)
    ioi_instrument_type: stock
    ioi_instrument_option_legs_count: 0
    ioi_instrument_option_legs_0_strike: 0
    ioi_instrument_option_legs_0_expiry:
    ioi_instrument_option_legs_0_type:
    ioi_instrument_option_legs_0_ratio: 0
    ioi_instrument_option_legs_0_underlying_type:
    ioi_instrument_option_legs_0_underlying_ticker:
    ioi_instrument_option_legs_0_underlying_figi:
    ioi_instrument_option_legs_0_exchange:
    ioi_instrument_option_legs_0_style:
    ioi_instrument_option_legs_0_futureRefDate:
    ioi_instrument_option_legs_0_delta: 0
    ioi_instrument_option_legs_1_strike: 0
    ioi_instrument_option_legs_1_expiry:
    ioi_instrument_option_legs_1_type:
    ioi_instrument_option_legs_1_ratio: 0
    ioi_instrument_option_legs_1_underlying_type:
    ioi_instrument_option_legs_1_underlying_ticker:
    ioi_instrument_option_legs_1_underlying_figi:
    ioi_instrument_option_legs_1_exchange:
    ioi_instrument_option_legs_1_style:
    ioi_instrument_option_legs_1_futureRefDate:
    ioi_instrument_option_legs_1_delta: 0
    ioi_instrument_option_legs_2_strike: 0
    ioi_instrument_option_legs_2_expiry:
    ioi_instrument_option_legs_2_type:
    ioi_instrument_option_legs_2_ratio: 0
    ioi_instrument_option_legs_2_underlying_type:
    ioi_instrument_option_legs_2_underlying_ticker:
    ioi_instrument_option_legs_2_underlying_figi:
    ioi_instrument_option_legs_2_exchange:
    ioi_instrument_option_legs_2_style:
    ioi_instrument_option_legs_2_futureRefDate:
    ioi_instrument_option_legs_2_delta: 0
    ioi_instrument_option_legs_3_strike: 0
    ioi_instrument_option_legs_3_expiry:
    ioi_instrument_option_legs_3_type:
    ioi_instrument_option_legs_3_ratio: 0
    ioi_instrument_option_legs_3_underlying_type:
    ioi_instrument_option_legs_3_underlying_ticker:
    ioi_instrument_option_legs_3_underlying_figi:
    ioi_instrument_option_legs_3_exchange:
    ioi_instrument_option_legs_3_style:
    ioi_instrument_option_legs_3_futureRefDate:
    ioi_instrument_option_legs_3_delta: 0
    ioi_instrument_option_structure:
    ioi_instrument_stock_security_ticker: SINA UW Equity
    ioi_instrument_stock_security_figi:
    ioi_goodUntil: 2020-01-23T14:36:15.000+00:00
    ioi_bid_price_type: fixed
    ioi_bid_price_fixed_price: 40
    ioi_bid_price_fixed_currency: USD
    ioi_bid_price_pegged_offsetAmount: 0
    ioi_bid_price_pegged_offsetFrom:
    ioi_bid_price_pegged_limitPrice: 0
    ioi_bid_price_reference:
    ioi_bid_price_moneyness: 0
    ioi_bid_size_type: quantity
    ioi_bid_size_quantity: 8279
    ioi_bid_size_quality:
    ioi_bid_referencePrice_price: 0
    ioi_bid_referencePrice_currency:
    ioi_bid_volatility: 0
    ioi_bid_notes: 1/22/2020 3:38:35 PM
    ioi_bid_qualifiers_count: 0
    ioi_bid_qualifiers_0:
    ioi_bid_qualifiers_1:
    ioi_bid_qualifiers_2:
    ioi_bid_qualifiers_3:
    ioi_bid_qualifiers_4:
    ioi_offer_price_type:
    ioi_offer_price_fixed_price: 0
    ioi_offer_price_fixed_currency:
    ioi_offer_price_pegged_offsetAmount: 0
    ioi_offer_price_pegged_offsetFrom:
    ioi_offer_price_pegged_limitPrice: 0
    ioi_offer_price_reference:
    ioi_offer_price_moneyness: 0
    ioi_offer_size_type:
    ioi_offer_size_quantity: 0
    ioi_offer_size_quality:
    ioi_offer_referencePrice_price: 0
    ioi_offer_referencePrice_currency:
    ioi_offer_volatility: 0
    ioi_offer_notes:
    ioi_offer_qualifiers_count: 0
    ioi_offer_qualifiers_0:
    ioi_offer_qualifiers_1:
    ioi_offer_qualifiers_2:
    ioi_offer_qualifiers_3:
    ioi_offer_qualifiers_4:
    ioi_routing_strategy_name:
    ioi_routing_strategy_brief:
    ioi_routing_strategy_detailed:
    ioi_routing_customId:
    ioi_routing_broker:
    ioi_sentTime: 2020-01-23T14:31:15.183+00:00
    change:
    Processing SUBSCRIPTION_DATA event
    Ioidata = {
        ioi_instrument_type = "option"
        ioi_instrument_option_legs_count = 2
        ioi_instrument_option_legs_0_strike = 230.000000
        ioi_instrument_option_legs_0_expiry = 2020-01-31T12:00:00.000+00:00
        ioi_instrument_option_legs_0_type = "Call"
        ioi_instrument_option_legs_0_ratio = 1.000000
        ioi_instrument_option_legs_0_underlying_figi = "BBG000C6K6G9"
        ioi_instrument_option_legs_0_exchange = "LN"
        ioi_instrument_option_legs_0_style = "European"
        ioi_instrument_option_legs_1_strike = 240.000000
        ioi_instrument_option_legs_1_expiry = 2020-01-31T12:00:00.000+00:00
        ioi_instrument_option_legs_1_type = "Call"
        ioi_instrument_option_legs_1_ratio = -1.250000
        ioi_instrument_option_legs_1_underlying_figi = "BBG000C6K6G9"
        ioi_instrument_option_legs_1_exchange = "LN"
        ioi_instrument_option_legs_1_style = "European"
        ioi_instrument_option_structure = "CallSpread"
        ioi_goodUntil = 2020-01-23T14:46:21.192+00:00
        ioi_bid_price_type = "fixed"
        ioi_bid_price_fixed_price = 83.630000
        ioi_bid_price_fixed_currency = ""
        ioi_bid_size_type = "quantity"
        ioi_bid_size_quantity = 1000
        ioi_bid_referencePrice_price = 202.150000
        ioi_bid_referencePrice_currency = "GBp"
        ioi_bid_notes = "bid notes"
        ioi_bid_qualifiers_count = 0
        ioi_offer_price_type = "fixed"
        ioi_offer_price_fixed_price = 83.640000
        ioi_offer_price_fixed_currency = ""
        ioi_offer_size_type = "quantity"
        ioi_offer_size_quantity = 2000
        ioi_offer_referencePrice_price = 202.150000
        ioi_offer_referencePrice_currency = "GBp"
        ioi_offer_notes = "offer notes"
        ioi_offer_qualifiers_count = 0
        state = "New"
        id_value = "0678b50e-287c-416f-813a-bf34cde3f300"
        originalId_value = "0678b50e-287c-416f-813a-bf34cde3f300"
        trader_uuid = 6767714
        trader_acronym = "S:BLP"
        receivedTime = 2020-01-23T14:31:21.577+00:00
    }

    IOI MESSAGE: CorrelationID(1)
    ioi_instrument_type: option
    ioi_instrument_option_legs_count: 2
    ioi_instrument_option_legs_0_strike: 230
    ioi_instrument_option_legs_0_expiry: 2020-01-31T12:00:00.000+00:00
    ioi_instrument_option_legs_0_type: Call
    ioi_instrument_option_legs_0_ratio: 1
    ioi_instrument_option_legs_0_underlying_type:
    ioi_instrument_option_legs_0_underlying_ticker:
    ioi_instrument_option_legs_0_underlying_figi: BBG000C6K6G9
    ioi_instrument_option_legs_0_exchange: LN
    ioi_instrument_option_legs_0_style: European
    ioi_instrument_option_legs_0_futureRefDate:
    ioi_instrument_option_legs_0_delta: 0
    ioi_instrument_option_legs_1_strike: 240
    ioi_instrument_option_legs_1_expiry: 2020-01-31T12:00:00.000+00:00
    ioi_instrument_option_legs_1_type: Call
    ioi_instrument_option_legs_1_ratio: -1
    ioi_instrument_option_legs_1_underlying_type:
    ioi_instrument_option_legs_1_underlying_ticker:
    ioi_instrument_option_legs_1_underlying_figi: BBG000C6K6G9
    ioi_instrument_option_legs_1_exchange: LN
    ioi_instrument_option_legs_1_style: European
    ioi_instrument_option_legs_1_futureRefDate:
    ioi_instrument_option_legs_1_delta: 0
    ioi_instrument_option_legs_2_strike: 0
    ioi_instrument_option_legs_2_expiry:
    ioi_instrument_option_legs_2_type:
    ioi_instrument_option_legs_2_ratio: 0
    ioi_instrument_option_legs_2_underlying_type:
    ioi_instrument_option_legs_2_underlying_ticker:
    ioi_instrument_option_legs_2_underlying_figi:
    ioi_instrument_option_legs_2_exchange:
    ioi_instrument_option_legs_2_style:
    ioi_instrument_option_legs_2_futureRefDate:
    ioi_instrument_option_legs_2_delta: 0
    ioi_instrument_option_legs_3_strike: 0
    ioi_instrument_option_legs_3_expiry:
    ioi_instrument_option_legs_3_type:
    ioi_instrument_option_legs_3_ratio: 0
    ioi_instrument_option_legs_3_underlying_type:
    ioi_instrument_option_legs_3_underlying_ticker:
    ioi_instrument_option_legs_3_underlying_figi:
    ioi_instrument_option_legs_3_exchange:
    ioi_instrument_option_legs_3_style:
    ioi_instrument_option_legs_3_futureRefDate:
    ioi_instrument_option_legs_3_delta: 0
    ioi_instrument_option_structure: CallSpread
    ioi_instrument_stock_security_ticker:
    ioi_instrument_stock_security_figi:
    ioi_goodUntil: 2020-01-23T14:46:21.192+00:00
    ioi_bid_price_type: fixed
    ioi_bid_price_fixed_price: 83
    ioi_bid_price_fixed_currency:
    ioi_bid_price_pegged_offsetAmount: 0
    ioi_bid_price_pegged_offsetFrom:
    ioi_bid_price_pegged_limitPrice: 0
    ioi_bid_price_reference:
    ioi_bid_price_moneyness: 0
    ioi_bid_size_type: quantity
    ioi_bid_size_quantity: 1000
    ioi_bid_size_quality:
    ioi_bid_referencePrice_price: 202
    ioi_bid_referencePrice_currency: GBp
    ioi_bid_volatility: 0
    ioi_bid_notes: bid notes
    ioi_bid_qualifiers_count: 0
    ioi_bid_qualifiers_0:
    ioi_bid_qualifiers_1:
    ioi_bid_qualifiers_2:
    ioi_bid_qualifiers_3:
    ioi_bid_qualifiers_4:
    ioi_offer_price_type: fixed
    ioi_offer_price_fixed_price: 83
    ioi_offer_price_fixed_currency:
    ioi_offer_price_pegged_offsetAmount: 0
    ioi_offer_price_pegged_offsetFrom:
    ioi_offer_price_pegged_limitPrice: 0
    ioi_offer_price_reference:
    ioi_offer_price_moneyness: 0
    ioi_offer_size_type: quantity
    ioi_offer_size_quantity: 2000
    ioi_offer_size_quality:
    ioi_offer_referencePrice_price: 202
    ioi_offer_referencePrice_currency: GBp
    ioi_offer_volatility: 0
    ioi_offer_notes: offer notes
    ioi_offer_qualifiers_count: 0
    ioi_offer_qualifiers_0:
    ioi_offer_qualifiers_1:
    ioi_offer_qualifiers_2:
    ioi_offer_qualifiers_3:
    ioi_offer_qualifiers_4:
    ioi_routing_strategy_name:
    ioi_routing_strategy_brief:
    ioi_routing_strategy_detailed:
    ioi_routing_customId:
    ioi_routing_broker:
    ioi_sentTime:
    change:
    Ctrl+C pressed. Stopping...
    Processing SESSION_STATUS event
    SessionConnectionDown = {
        server = "localhost:8194"
    }

    Session connection is down
    Processing SESSION_STATUS event
    SessionTerminated = {
    }

    SessionTerminated = {
    }


Error Message
=============

+----------------------------------+--------------------------------------------------------+
| Error Message                    | Description                                            |
+==================================+========================================================+
| Invalid tags in topic            | | There is an unspported tag field or tag values       |
|                                  | | detected in topic.                                   |
+----------------------------------+--------------------------------------------------------+
| Exceeded subscription limits     | Too many subscriptions from the client                 |
+----------------------------------+--------------------------------------------------------+
| Could not determine IPER         | The subscribers acronym could not be determined        |
+----------------------------------+--------------------------------------------------------+
| Could not get acronym from IPER  | The subscribers acronym code is blank                  |
+----------------------------------+--------------------------------------------------------+
| Could not get side from IPER     | The subscribers side cannot be determined              |
+----------------------------------+--------------------------------------------------------+
| UUID is not enabled in NNAB      | | The subscribers UUID is not enabled by any broker in |
|                                  | | NNAB                                                 | 
+----------------------------------+--------------------------------------------------------+
| | Failed ot get user UUID from   | | There wasn't a valid UUID passed or the UUID passed  |
| | subscription                   | | was set to zero.                                     |
+----------------------------------+--------------------------------------------------------+



Description of Elements
=======================
The following elements are available for equity and options IOI subscription.

The sell-side sending IOIs will buy from the buy-side at the bid size/price and sell to the buy-side at the offer size/price.

.. important::

    All times are in UTC.


+--------------------------------------------------+------------------------------------+---------+
|Element Name                                      | Description                        | Type    |
+==================================================+====================================+=========+
|``id_value``                                      |  | Unique Bloomberg value to       | string  |
|                                                  |  | identify IOI message,           |         |
|                                                  |  | also known as handle            |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_natural``                               | Indicate natural IOI               | int32   |
+--------------------------------------------------+                                    |         |
|``ioi_offer_natural``                             |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_notes``                                 | Notes section for IOI messages     | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_notes``                               |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_fixed_currency``                  |  | Three letter currency acronym   | string  |
+--------------------------------------------------+  | for the IOI                     |         |
|``ioi_offer_price_fixed_currency``                |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_fixed_price``                     | IOI fixed price                    | float64 |
+--------------------------------------------------+                                    |         | 
|``ioi_offer_price_fixed_price``                   |                                    |         |
+--------------------------------------------------+------------------------------------+---------+ 
|``ioi_bid_price_pegged_limitPrice``               | IOI pegged limit price             | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_pegged_limitPrice``             |                                    |         |       
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_pegged_offsetAmount``             | IOI pegged offset amount           | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_pegged_offsetAmount``           |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_pegged_offsetFrom``               | IOI pegged offset from             | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_pegged_offsetFrom``             |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_pegged_offsetType``               | IOI pegged offset type             | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_pegged_offsetType``             |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_reference``                       | Bid, Mid, Ask                      | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_reference``                     |                                    |         |  
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_price_type``                            | Market, limit, or unspecified      | string  | 
+--------------------------------------------------+                                    |         |
|``ioi_offer_price_type``                          |                                    |         | 
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_qualifiers_n``                          | IOI bid/offer qualifiers           | string  |
+--------------------------------------------------+ (e.g. H, U, V, I) [definitions]_   |         |
|``ioi_offer_qualifiers_n``                        |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_qualifiers_count``                      | IOI bid/offer qualifiers count     | int32   |
+--------------------------------------------------+                                    |         |
|``ioi_offer_qualifiers_count``                    |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_referencePrice_currency``               | IOI bid/offer reference currency   | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_referencePrice_currency``             |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_referencePrice_price``                  | IOI bid/offer reference price      | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_referencePrice_price``                |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_size_quality``                          | Small, Medium, or Large            | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_size_quality``                        |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_size_quantity``                         | Actual quantity of the IOI         | int64   |
+--------------------------------------------------+                                    |         |
|``ioi_offer_size_quantity``                       |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_size_type``                             | IOI bid/offer size type            | string  |
+--------------------------------------------------+                                    |         |
|``ioi_offer_size_type``                           |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_bid_volatility``                            | Options IOI bid/offer volatility   | float64 |
+--------------------------------------------------+                                    |         |
|``ioi_offer_volatility``                          |                                    |         |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_clientId``                                  | IOI Client ID                      | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_goodUntil``                                 | IOI good until time                | dateTime|
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_delta``            | Options IOI delta                  | float64 |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_exchange``         | Options IOI exchange               | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_expiry``           | Options IOI leg expiry             | dateTime|
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_futureRefDate``    | Options IOI future reference date  | dateTime|
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_listed_figi``      | Options IOI FIGI                   | string  | 
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_listed_ticker``    | Options IOI ticker                 | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_listed_type``      | Options IOI type                   | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_ratio``            | Options IOI ratio                  | float64 |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_strike``           | Options IOI strike                 | float64 |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_style``            | European, American                 | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_type``             | Options IOI leg type               | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_underlying_figi``  | Options IOI underlying figi        | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_underlying_ticker``| Options IOI underlying ticker      | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_n_underlying_type``  | Options IOI underlying type        | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_option_legs_count``              | Options IOI legs count             | string  |
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
|``ioi_instrument_stock_security_figi``            | Equity IOI security figi           | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_stock_security_ticker``          | Equity IOI security ticker         | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_stock_security_type``            | Equity IOI security type           | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_instrument_type``                           | IOI instrument type                | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_benchmark``                         |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_broker``                            |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_customId``                          |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_orderType``                         |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_strategy_brief``                    |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_strategy_detailed``                 |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_routing_strategy_name``                     |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``ioi_sentTime``                                  |IOI sent time                       | dateTime|
+--------------------------------------------------+------------------------------------+---------+
|``originalId_value``                              |                                    | string  |
+--------------------------------------------------+------------------------------------+---------+
|``state``                                         |IOI State: New, Replace and Cancel  | string  |
+--------------------------------------------------+------------------------------------+---------+
|``trader_acronym``                                |IPER code                           | string  |
+--------------------------------------------------+------------------------------------+---------+
|``trader_username``                               |Trader name                         | string  |
+--------------------------------------------------+------------------------------------+---------+
|``trader_uuid``                                   |Trader UUID                         | int64   |
+--------------------------------------------------+------------------------------------+---------+


Actionable IOI
==============
The IOIs published via IOI API Publication service can be actionable by the receiving buy-side firms.

The sell-side using IOI API Publication service can specify the targeting EMSX broker code along with ``customId`` element.
The ``customId`` will allow the order receiving sell-side to tie the order back to the original IOI generated from the sell-side.


+------------------------------+-----------------------------------------------+---------+
|Element Name                  | Description                                   | Type    |
+==============================+===============================================+=========+
|``broker``                    |  | The broker code used in EMSX to submit the | string  |
|                              |  | order. This is viewable as                 |         |
|                              |  | ``ioi_routing_broker`` element in the      |         | 
|                              |  | IOI API Subscription service.              |         |
+------------------------------+-----------------------------------------------+---------+
|``customId``                  |  | Optional, can be created by the sell-side  | string  |
|                              |  | to correlate back to an order. This is     |         |
|                              |  | viewable as ``ioi_routing_id`` element in  |         |
|                              |  | the IOI API Subscription service.          |         |
+------------------------------+-----------------------------------------------+---------+
|``strategy``                  |  | Optinal, if specified and the strategy     | string  |
|                              |  | exists in ``EQMB<GO>``, this element will  |         |
|                              |  | be accepted.                               |         |
+------------------------------+-----------------------------------------------+---------+

.. [definitions] H = Customer Order in Hand - Firm agency order direct from the customer, U = Customer Principal Interest - Firm principal order originating from previous facilitation, V = Swithc / Versus Trade, I = In Touch With - , X = For Crossing, W = Working, T = Over Time / Day, D = VWAP, R = Ready to Trade, S = Portfolio Shown.
