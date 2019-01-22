Bloomberg API Service for IOI API Publication
=============================================

For IOI publication, this is accomplished by referencing ``//blp/ioiapi-beta-request`` as the service name in your program. This command will allow your service  to redirect all IOI API requests to the test environment.   

Once the client has thoroughly tested the custom-built strategies, they can access the production 
environment by changing the service name from ``//blp/ioiapi-beta-request`` to ``//blp/ioiapi-request``.


Accessing the Test Environment
==============================

Bloomberg provides a test environment for clients to build and test their strategies using the IOI API.

Inside the Bloomberg Terminal type ``UAT ON <GO>``.This command allows the particular terminal window and launchpad to log into the beta environment. Please note, when a user is remote into the beta environment it only affects that particular terminal window and the other Bloomberg panels will not be affected by the ``UAT ON <GO>`` command.

To check which environment your current view is in, type ``VSAT <GO>`` inside the Bloomberg terminal.

To get back to production type ``UAT OFF <GO>``. Please note that the testing environment in Beta will not 
operate in the exact same way as the production environment. Also, please note that the beta environment is a lot slower than the 
production environment and no one should perform any volume or load testing in the beta environment.

The ``IOI<GO>`` function will show a tab for Options vs. Equities IOIs.


API Demo Tool 
=============
API Demo Tool is a handy tool while developing on any Bloomberg API services. The API Demo Tool provides real-time schema viewing tool among other handy tools that can be leveraged during the initial development.

The API Demo Tool can be downloaded from the Bloomberg terminal along with other generic Bloomberg API code samples.

    ``WAPI<GO>`` >> API Download Center >> Download 


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


Cancel IOI Message
==================


The ``cancelIoi`` request allows IOI API service to Cancel IOI message. 


Full code sample (Options IOI):-

================== ================== 
`Cancel IOI cs`_   `Cancel IOI py`_	
------------------ ------------------ 
`Cancel IOI java`_ 
================== ==================

.. _Cancel IOI cs: https://github.com/tkim/ioi_api_repository/blob/master/C%23/cs_dapi_CancelIOI.cs

.. _Cancel IOI java: https://github.com/tkim/ioi_api_repository/blob/master/Java/Java_dapi_CancelIOI.java

.. _Cancel IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_CancelIOI.py


Full code sample (Equity IOI):-

==================== ================== 
`Cancel E IOI cs`_   `Cancel E IOI py`_ 
-------------------- ------------------ 
`Cancel E IOI java`_
==================== ==================

.. _Cancel E IOI cs: https://github.com/tkim/ioi_api_repository/blob/master/C%23/cs_dapi_CancelEquityIOI.cs
.. _Cancel E IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_CancelEquityIOI.py
.. _Cancel E IOI java: https://github.com/tkim/ioi_api_repository/blob/master/Java/java_dapi_CancelEquityIOI.java

.. hint:: 

	Please right click on the top code sample link to open in a new tab.
	


.. code-block:: python
   :linenos:          
    
    def sendCancelIOI(self, session):

        service = session.getService(d_ioi)
        request = service.createRequest("cancelIoi")

        handle = request.getElement("handle")
        handle.setElement("value", "f6f07a52-a0e7-4060-b8dd-35181b036143")

        print("Sending Request: %s" % request.toString())

        self.requestID = session.sendRequest(request)
        print("CancelIOI request sent.")



Create IOI Message
==================


The ``createIoi`` request allows IOI API service to Create IOI message. 


Full code sample (Options IOI):-

================== ================ 
`Create IOI cs`_   `Create IOI py`_
------------------ ---------------- 
`Create IOI java`_ 
================== ================ 


.. _Create IOI cs: https://github.com/tkim/ioi_api_repository/blob/master/C%23/cs_dapi_CreateIOI.cs

.. _Create IOI java: https://github.com/tkim/ioi_api_repository/blob/master/Java/Java_dapi_CreateIOI.java

.. _Create IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_CreateIOI.py


Full code sample (Equity IOI):-

==================== ================== 
`Create E IOI cs`_   `Create E IOI py`_ 
-------------------- ------------------ 
`Create E IOI java`_ 
==================== ==================

.. _Create E IOI cs: https://github.com/tkim/ioi_api_repository/blob/master/C%23/cs_dapi_CreateEquityIOI.cs

.. _Create E IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_CreateEquityIOI.py

.. _Create E IOI java: https://github.com/tkim/ioi_api_repository/blob/master/Java/java_dapi_CreateEquityIOI.java


.. hint:: 

	Please right click on the top code sample link to open in a new tab.

	
.. code-block:: python
   :linenos:

	 def sendCreateIOI(self, session):

        service = session.getService(d_ioi)
        request = service.createRequest("createIoi")

        ioi = request.getElement("ioi")

        # Set the good-until time of this option to 15 minutes from now
        ioi.setElement("goodUntil", datetime.datetime.utcnow() + datetime.timedelta(0,900))

        # Create the option
        option = ioi.getElement("instrument").setChoice("option")
        option.setElement("structure", "CallSpread")

        # This option has two legs. Create the first leg
        leg1 = option.getElement("legs").appendElement()
        leg1.setElement("type","Call")
        leg1.setElement("strike", 230)
        leg1.setElement("expiry", datetime.datetime(2017,12,15,12))
        leg1.setElement("style", "European")
        leg1.setElement("ratio", +1.00)
        leg1.setElement("exchange", "LN")
        leg1.getElement("underlying").setChoice("ticker")
        leg1.getElement("underlying").setElement("ticker", "VOD LN Equity")

        # Create the second leg
        leg2 = option.getElement("legs").appendElement()
        leg1.setElement("type","Call")
        leg2.setElement("strike", 240)
        leg2.setElement("expiry", datetime.datetime(2017,12,15,12))
        leg2.setElement("style", "European")
        leg2.setElement("ratio", -1.25)
        leg2.setElement("exchange", "LN")
        leg2.getElement("underlying").setChoice("ticker")
        leg2.getElement("underlying").setElement("ticker", "VOD LN Equity")

        # Create a quote consisting of a bid and an offer
        bid = ioi.getElement("bid")
        bid.getElement("price").setChoice("fixed")
        bid.getElement("price").getElement("fixed").getElement("price").setValue(83.63)
        bid.getElement("size").getElement("quantity").setValue(1000)
        bid.getElement("referencePrice").setElement("price", 202.15)
        bid.getElement("referencePrice").setElement("currency", "GBp")
        bid.setElement("notes", "bid notes")

        # Set the offer
        offer = ioi.getElement("offer")
        offer.getElement("price").setChoice("fixed")
        offer.getElement("price").getElement("fixed").getElement("price").setValue(83.64)
        offer.getElement("size").setChoice("quantity")
        offer.getElement("size").getElement("quantity").setValue(2000)
        offer.getElement("referencePrice").setElement("price", 202.15)
        offer.getElement("referencePrice").setElement("currency", "GBp")
        offer.setElement("notes", "offer notes")

        # Set targets
        includes = ioi.getElement("targets").getElement("includes")
        for acronym in ["BLPA", "BLPB"]:
            target = includes.appendElement()
            target.setChoice("acronym")
            target.setElement("acronym", acronym)
                    
        
        print("Sending Request: %s" % request.toString())

        self.requestID = session.sendRequest(request)
        print("CreateIOI request sent.") 



Update IOI Message
===================


The ``updateIoi`` request allows IOI API service to Update IOI message. 


Full code sample (Options IOI):-

================== ================ 
`Update IOI cs`_   `Update IOI py`_
------------------ ---------------- 
`Update IOI java`_ 
================== ================ 


.. _Update IOI cs: https://github.com/tkim/ioi_api_repository/blob/master/C%23/cs_dapi_UpdateIOI.cs

.. _Update IOI java: https://github.com/tkim/ioi_api_repository/blob/master/Java/Java_dapi_UpdateIOI.java

.. _Update IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_UpdateIOI.py


Full code sample (Equity IOI):-

==================== ================== 
`Update E IOI cs`_   `Update E IOI py`_ 
-------------------- ------------------ 
`Update E IOI java`_ 
==================== ==================

.. _Update E IOI cs: https://github.com/tkim/ioi_api_repository/blob/master/C%23/cs_dapi_UpdateEquityIOI.cs

.. _Update E IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_UpdateEquityIOI.py

.. _Update E IOI java: https://github.com/tkim/ioi_api_repository/blob/master/Java/java_dapi_UpdateEquityIOI.java


.. hint:: 

	Please right click on the top code sample link to open in a new tab.

	

.. code-block:: python
   :linenos:
	
	def sendUpdateIOI(self, session):

        service = session.getService(d_ioi)
        request = service.createRequest("updateIoi")

        handle = request.getElement("handle")
        handle.setElement("value", "5f20228a-bef6-41bb-81eb-6abe0b21a00e")

        ioi = request.getElement("ioi")

        # Set the good-until time of this option to 15 minutes from now
        ioi.setElement("goodUntil", datetime.datetime.utcnow() + datetime.timedelta(0,900))

        # Create the option
        option = ioi.getElement("instrument").setChoice("option")
        option.setElement("structure", "CallSpread")

        # This option has two legs. Create the first leg
        leg1 = option.getElement("legs").appendElement()
        leg1.setElement("type","Call")
        leg1.setElement("strike", 230)
        leg1.setElement("expiry", datetime.datetime(2017,12,15,12))
        leg1.setElement("style", "European")
        leg1.setElement("ratio", +1.00)
        leg1.setElement("exchange", "LN")
        leg1.getElement("underlying").setChoice("ticker")
        leg1.getElement("underlying").setElement("ticker", "VOD LN Equity")

        # Create the second leg
        leg2 = option.getElement("legs").appendElement()
        leg1.setElement("type","Call")
        leg2.setElement("strike", 240)
        leg2.setElement("expiry", datetime.datetime(2017,12,15,12))
        leg2.setElement("style", "European")
        leg2.setElement("ratio", -1.25)
        leg2.setElement("exchange", "LN")
        leg2.getElement("underlying").setChoice("ticker")
        leg2.getElement("underlying").setElement("ticker", "VOD LN Equity")

        # Create a quote consisting of a bid and an offer
        bid = ioi.getElement("bid")
        bid.getElement("price").setChoice("fixed")
        bid.getElement("price").getElement("fixed").getElement("price").setValue(83.63)
        bid.getElement("size").getElement("quantity").setValue(1000)
        bid.getElement("referencePrice").setElement("price", 202.15)
        bid.getElement("referencePrice").setElement("currency", "GBp")
        bid.setElement("notes", "bid notes")

        # Set the offer
        offer = ioi.getElement("offer")
        offer.getElement("price").setChoice("fixed")
        offer.getElement("price").getElement("fixed").getElement("price").setValue(83.64)
        offer.getElement("size").setChoice("quantity")
        offer.getElement("size").getElement("quantity").setValue(2000)
        offer.getElement("referencePrice").setElement("price", 202.15)
        offer.getElement("referencePrice").setElement("currency", "GBp")
        offer.setElement("notes", "offer notes")

        # Set targets
        includes = ioi.getElement("targets").getElement("includes")
        for acronym in ["BLPA", "BLPB"]:
            target = includes.appendElement()
            target.setChoice("acronym")
            target.setElement("acronym", acronym)
                    
        
        print("Sending Request: %s" % request.toString())

        self.requestID = session.sendRequest(request)
        print("UpdateIOI request sent.")



Description of Elements
=======================

The following elements are available for equity and options IOI publication.

The sell-side sending IOIs will buy from the buy-side at the bid size/price and sell to the buy-side at the offer size/price.


.. important::

    All times in IOI API are Datetime objects. Every language has slightly different syntax but unless the user specifies an offset, the datetime object defaults to UTC.


+------------------------------+-----------------------------------------------+---------+
|Element Name                  | Description                                   | Type    |
+==============================+===============================================+=========+
|``acronym``                   | IPER code to target IOIs                      | string  |
+------------------------------+-----------------------------------------------+---------+
|``currency``                  | Currency of the IOI                           | string  |
+------------------------------+-----------------------------------------------+---------+
|``delta``                     | Options delta                                 | float64 |
+------------------------------+-----------------------------------------------+---------+
|``futureRefDate``             | future reference date                         | datetime|
+------------------------------+-----------------------------------------------+---------+
|``goodUntil``                 | good until date/time                          | datetime|
+------------------------------+-----------------------------------------------+---------+
|``handle``                    |Unique Bloomberg value to identify IOI message | string  |
+------------------------------+-----------------------------------------------+---------+
|``instrument``                | Stock or Options                              |         |
+------------------------------+-----------------------------------------------+---------+
|``limitPrice``                | Limit price                                   | float64 |
+------------------------------+-----------------------------------------------+---------+
|``natural``                   | Natural IOI indicator                         | bool    |
+------------------------------+-----------------------------------------------+---------+
|``notes``                     | Free text field                               | string  |
+------------------------------+-----------------------------------------------+---------+
|``offsetAmount``              | pegged offset amount                          | float64 |
+------------------------------+-----------------------------------------------+---------+
|``offsetFrom``                | pegged offset from Bid, Mid, Ask              | Enum    |
+------------------------------+-----------------------------------------------+---------+
|``price``                     | IOI price                                     | float64 |
+------------------------------+-----------------------------------------------+---------+
|``qualifiers``                | IOI qualifiers (e.g. H, U, V, I) [definition]_| string  |
+------------------------------+-----------------------------------------------+---------+
|``quality``                   | Small, Medium, or Large                       | enum    |
+------------------------------+-----------------------------------------------+---------+
|``quantity``                  | Actual quantity of the IOI                    | int64   |
+------------------------------+-----------------------------------------------+---------+
|``ratio``                     | Options IOI ratio                             | float64 |
+------------------------------+-----------------------------------------------+---------+
|``strike``                    | Options IOI strike                            | float64 |
+------------------------------+-----------------------------------------------+---------+
|``style``                     | Options IOI style (e.g. European or American) | enum    |
+------------------------------+-----------------------------------------------+---------+
|``structure``                 | Options IOI structure                         | enum    |
|                              |  | Custom, CallSpread, PutSpread, Straddle,   |         |
|                              |  | Strangle, SingleLegCall, SingleLegPut,     |         |
|                              |  | CalendarCallSpread, CalendarPutSpread,     |         |
|                              |  | CallSpreadReversal, PutSpreadReversal,     |         |
|                              |  | DiagonalCalendarCallSpread,                |         |
|                              |  | DiagonalCalendarPutSpread, CallButterfly,  |         |
|                              |  | PutButterfly, IronButterfly, RiskReversal, |         |
|                              |  | Box, CallLadder, PutLadder, CallCondor,    |         |
|                              |  | PutCondor, IronCondor, JellyRoll,          |         |
|                              |  | RatioCallSpread, RatioPutSpread            |         |
+------------------------------+-----------------------------------------------+---------+
|``ticker``                    | IOI ticker                                    | string  |
+------------------------------+-----------------------------------------------+---------+
|``type``                      | Options IOI type  (e.g. Call or Put)          | enum    |
+------------------------------+-----------------------------------------------+---------+
|``volatility``                | Options IOI volatility                        | float64 |
+------------------------------+-----------------------------------------------+---------+



Actionable IOI
==============

The IOIs published via IOI API Publication service from the sell-side can be actionable by the receiving buy-side firms.

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

.. [definition] H = Customer Order in Hand - Firm agency order direct from the customer, U = Customer Principal Interest - Firm principal order originating from previous facilitation, V = Swithc / Versus Trade, I = In Touch With - , X = For Crossing, W = Working, T = Over Time / Day, D = VWAP, R = Ready to Trade, S = Portfolio Shown.