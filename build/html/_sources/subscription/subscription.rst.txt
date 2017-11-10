Bloomberg API Service for IOI API Subscription
==============================================

For IOI subscription, this is accomplished by referencing  ________ as the service name in your program. This command will allow your service  to redirect all IOI API subscription to the test environment.   

Once the client has thoroughly tested the custom-built strategies, they can access the production environment by changing the service name from  ________ to  ________ .


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



