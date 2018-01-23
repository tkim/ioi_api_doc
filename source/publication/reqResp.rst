Bloomberg API Service for IOI API Publication
=============================================

For IOI publication, this is accomplished by referencing ``//blp/ioiapi-beta-request`` as the service name in your program. This command will allow your service  to redirect all IOI API requests to the test environment.   

Once the client has thoroughly tested the custom-built strategies, they can access the production 
environment by changing the service name from ``//blp/ioiapi-beta-request`` to ``//blp/ioiapi-request``.


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

The ``IOI<GO>`` function will show a tab for Options vs. Equities IOIs.


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

================== ================== 
`Cancel E IOI py`_ 
------------------ ------------------ 
 
================== ==================

.. _Cancel E IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_CancelEquityIOI.py


.. hint:: 

	Please right click on the top code sample link to open in a new tab.
	


.. code-block:: python
             
    
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

================== ================== 
`Create E IOI py`_ 
------------------ ------------------ 
 
================== ==================


.. _Create E IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_CreateEquityIOI.py


.. hint:: 

	Please right click on the top code sample link to open in a new tab.

	
.. code-block:: python
	

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

================== ================== 
`Update E IOI py`_ 
------------------ ------------------ 
 
================== ==================


.. _Update E IOI py: https://github.com/tkim/ioi_api_repository/blob/master/Python/py_dapi_UpdateEquityIOI.py


.. hint:: 

	Please right click on the top code sample link to open in a new tab.

	

.. code-block:: python

	
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





