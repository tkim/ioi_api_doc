###################
IOI API Publication
###################

This document is for developers who will use the Bloomberg IOI API to publish the IOI (Indication of Interest) messages. 

The published messages can be found in ``IOI<GO>`` function in the Bloomberg terminal.

The service name for production is ``//blp/ioiapi-request`` and for test environment is ``//blp/ioiapi-beta-request``. 

Unlike the EMSX API, the IOI API service supports partial response messages which will return messages that are a subset of the information.

.. toctree::
   :maxdepth: 2

   reqResp
   


