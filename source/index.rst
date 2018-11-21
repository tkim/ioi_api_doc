##########################
IOI API Programmers Guide
##########################

This document is for developers who will use the Bloomberg IOI API Server to publish and subscribe to 
IOI (Indication of Interest) messages. 

IOI API allows clients to use Bloomberg API 3.0 to automate the IOI publishing into Bloomberg and to subscribe IOI message from Bloomberg.

The Bloomberg API uses an event-driven model. The IOI API is an extension of Bloomberg API 3.0 and it lets users integrate streaming real-time and static data into their own custom applications. The user can choose the data they require down to the level of individual fields. The Bloomberg API 3.0 programming interface implementations are extremely lightweight. For details to the Desktop API, please refer to the Desktop API Programmers Guide from ``WAPI<GO>``.

The Bloomberg API interface is thread-safe and thread-aware, giving applications the ability to utilize multiple processors efficiently. The Bloomberg API supports run-time downloadable schema for the service it provides, and it provides methods to query these schemas at runtime. This means additional service in Bloomberg API is supported without addition to the interface.

The object model for Java, .NET and C++ are identical. The C interface provides a C-style version of the object model.


.. important::
	
	Please note that performance/load test should never be performed on any of the API environment as this is a shared environment and we monitor and increase capacity as needed.



.. toctree::
   :maxdepth: 2

   introduction
   publication/index
   subscription/index 
   faq
   glossary

