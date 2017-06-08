###################
IOI API Publication
###################

This document is for developers who will use the Bloomberg IOI API to publish the IOI (Indication of Interest) messages. 

The published messages can be found in IOI<GO> function in the Bloomberg terminal.

The service name for production is ``//blp/ioiapi`` and for test environment is ``//blp/ioiapi_beta``. 

Cancel IOI Message
==================


The ``CancelIoi`` request allows IOI API service to Cancel IOI message. 


Full code sample:-

================== ================ =================
`Cancel IOI cpp`_  `Cancel IOI cs`_ `Cancel IOI vba`_	
------------------ ---------------- -----------------
`Cancel IOI java`_ `Cancel IOI py`_
================== ================ =================

.. _Cancel IOI cpp: https://github.com/tkim/ioi_api_repository/C++/CancelIOI/CancelIOI.cpp

.. _Cancel IOI cs: https://github.com/tkim/ioi_api_repository/C#/CancelIOI/CancelIOI.cs

.. _Cancel IOI java: https://github.com/tkim/ioi_api_repository/Java/CancelIOI/src/com/bloomberg/ioi/samples/CancelIOI.java

.. _Cancel IOI py: https://github.com/tkim/ioi_api_repository/Python/CancelIOI.py

.. _Cancel IOI vba: https://github.com/tkim/ioi_api_repository/VBA/CancelIOI.cls


.. hint:: 

	Please right click on the top code sample link to open in a new tab.


.. code-block:: python
             
             
	 def processServiceStatusEvent(self,event,session):
        print ("Processing SERVICE_STATUS event")
        
        for msg in event:
            
            if msg.messageType() == SERVICE_OPENED:
                print("Service opened...")

                service = session.getService(d_service)
    
                request = service.createRequest("CsancelIoi")

                request.getElement("handle").getElement("value").setValue("e58ae3d4-2d6a-426b-8844-5868c7a4e259");

                print("Request: %s" % request.toString())
                    
                self.requestID = blpapi.CorrelationId()
                
                session.sendRequest(request, correlationId=self.requestID )
                            
            elif msg.messageType() == SERVICE_OPEN_FAILURE:
                sys.stderr.write("Error: Service failed to open")     
                
    def processResponseEvent(self, event):
        print("Processing RESPONSE event")
        
        for msg in event:
            
            print("MESSAGE: %s" % msg.toString())
            print("CORRELATION ID: %d" % msg.correlationIds()[0].value())


            if msg.correlationIds()[0].value() == self.requestID.value():
                print("MESSAGE TYPE: %s" % msg.messageType())
                
                if msg.messageType() == "handle":
                    val = msg.getElementAsString("value")
                    print("Response: Value= %s" % (val))

                global bEnd
                bEnd = True




Create IOI Message
==================


The ``createIoi`` request allows IOI API service to Create IOI message. 


Full code sample:-

================== ================ =================
`Create IOI cpp`_  `Create IOI cs`_ `Create IOI vba`_	
------------------ ---------------- -----------------
`Create IOI java`_ `Create IOI py`_
================== ================ =================

.. _Create IOI cpp: https://github.com/tkim/ioi_api_repository/C%2B%2B/CreateIOI/CreateIOI.cpp

.. _Create IOI cs: https://github.com/tkim/ioi_api_repository/C%23/CreateIOI/CreateIOI.cs

.. _Create IOI java: https://github.com/tkim/ioi_api_repository/Java/CreateIOI/src/com/bloomberg/ioi/samples/CreateIOI.java

.. _Create IOI py: https://github.com/tkim/ioi_api_repository/Python/CreateIOI.py

.. _Create IOI vba: https://github.com/tkim/ioi_api_repository/VBA/CreateIOI.cls


.. hint:: 

	Please right click on the top code sample link to open in a new tab.

.. code-block:: python
	

	 def processServiceStatusEvent(self,event,session):
        print ("Processing SERVICE_STATUS event")
        
        for msg in event:
            
            if msg.messageType() == SERVICE_OPENED:
                print("Service opened...")

                service = session.getService(d_service)
    
                request = service.createRequest("createIoi")

                ioi = request.getElement("ioi")
        
                # Set the good-until time of this option to 15 minutes from now
                ioi.setElement("goodUntil", datetime.datetime.utcnow() + datetime.timedelta(0,900))
        
                # Create the option
                ioi.getElement("instrument").setChoice("option")
                option = ioi.getElement("instrument").getElement("option")
                option.setElement("structure", "CallSpread")
        
                # This option has two legs. Create the first leg
                leg1 = option.getElement("legs").appendElement()
                leg1.setElement("type","Call")
                leg1.setElement("strike", 230)
                leg1.setElement("expiry", datetime.datetime(2016,12,15,12))
                leg1.setElement("style", "European")
                leg1.setElement("ratio", +1.00)
                leg1.setElement("exchange", "LN")
                leg1.getElement("underlying").setChoice("ticker")
                leg1.getElement("underlying").setElement("ticker", "VOD LN Equity")
        
                # Create the second leg
                leg2 = option.getElement("legs").appendElement()
                leg1.setElement("type","Call")
                leg2.setElement("strike", 240)
                leg2.setElement("expiry", datetime.datetime(2016,12,15,12))
                leg2.setElement("style", "European")
                leg2.setElement("ratio", -1.25)
                leg2.setElement("exchange", "LN")
                leg2.getElement("underlying").setChoice("ticker")
                leg2.getElement("underlying").setElement("ticker", "VOD LN Equity")
        
                # Create a quote consisting of a bid and an offer
                bid = ioi.getElement("bid")
                bid.getElement("delta").setValue(.0041)
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
                            
                print("Request: %s" % request.toString())
                    
                self.requestID = blpapi.CorrelationId()
                
                session.sendRequest(request, correlationId=self.requestID )
                            
            elif msg.messageType() == SERVICE_OPEN_FAILURE:
                sys.stderr.write("Error: Service failed to open")     



Update IOI Message
===================


The ``updateIoi`` request allows IOI API service to Update IOI message. 


Full code sample:-

================== ================ =================
`Update IOI cpp`_  `Update IOI cs`_ `Update IOI vba`_	
------------------ ---------------- -----------------
`Update IOI java`_ `Update IOI py`_
================== ================ =================

.. _Update IOI cpp: https://github.com/tkim/ioi_api_repository/C%2B%2B/UpdateIOI/UpdateIOI.cpp

.. _Update IOI cs: https://github.com/tkim/ioi_api_repository/C%23/UpdateIOI/UpdateIOI.cs

.. _Update IOI java: https://github.com/tkim/ioi_api_repository/Java/UpdateIOI/src/com/bloomberg/ioi/samples/UpdateIOI.java

.. _Update IOI py: https://github.com/tkim/ioi_api_repository/Python/UpdateIOI.py

.. _Update IOI vba: https://github.com/tkim/ioi_api_repository/VBA/UpdateIOI.cls


.. hint:: 

	Please right click on the top code sample link to open in a new tab.


.. code-block:: python

	
	def processServiceStatusEvent(self,event,session):
        print ("Processing SERVICE_STATUS event")
        
        for msg in event:
            
            if msg.messageType() == SERVICE_OPENED:
                print("Service opened...")

                service = session.getService(d_service)
    
                request = service.createRequest("updateIoi")

                request.getElement("handle").getElement("value").setValue("46fa7703-3c16-4dbf-ae19-cec2f68db563");


                ioi = request.getElement("ioi")
        
                # Set the good-until time of this option to 15 minutes from now
                ioi.setElement("goodUntil", datetime.datetime.utcnow() + datetime.timedelta(0,900))
        
                # Update the bid
                bid = ioi.getElement("bid")
                bid.getElement("delta").setValue(.9006)
                bid.getElement("size").getElement("quantity").setValue(1000)
                bid.getElement("referencePrice").setElement("price", 202.15)
                bid.getElement("referencePrice").setElement("currency", "GBp")
                bid.setElement("notes", "bid notes updated")
        
                # Update the offer
                offer = ioi.getElement("offer")
                offer.getElement("price").setChoice("fixed")
                offer.getElement("price").getElement("fixed").getElement("price").setValue(18203.66)
                offer.getElement("size").setChoice("quantity")
                offer.getElement("size").getElement("quantity").setValue(2000)
                offer.getElement("referencePrice").setElement("price", 202.15)
                offer.getElement("referencePrice").setElement("currency", "GBp")
                offer.setElement("notes", "offer notes updated")
        
                print("Request: %s" % request.toString())
                    
                self.requestID = blpapi.CorrelationId()
                
                session.sendRequest(request, correlationId=self.requestID )
                            
            elif msg.messageType() == SERVICE_OPEN_FAILURE:
                sys.stderr.write("Error: Service failed to open")     


