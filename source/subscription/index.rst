####################
IOI API Subscription
####################


This document is for developers who will use the Bloomberg IOI API Server to subscribe to IOI (Indication of Interest) messages from various brokers. 

The subscribed message can be found in  ``IOI<GO>`` function in the Bloomberg terminal.

The service name is ``//blp/ioisub`` for production and ``//blp/ioisub-beta`` to call the test environment.

Unlike the EMSX API, the IOI API service supports partial response messages which will return messages that are a subset of the information.

`IOI API Subscription Schema`_

.. _IOI API Subscription Schema: https://github.com/tkim/ioi_api_repository/blob/master/ioisub_1.0.0.8.xml


.. toctree::
   :maxdepth: 2

   subscription
