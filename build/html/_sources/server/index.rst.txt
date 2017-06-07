####################
Server Side IOI API
####################

The Bloomberg API uses an event-driven model. The IOI API is an extension of Bloomberg API 3.0 and it 
lets users integrate streaming real-time and static data into their own custom applications. The user 
can choose the data they require down to the level of individual fields. The Bloomberg API 3.0 
programming interface implementations are extremely lightweight. For details to the core Bloomberg 
API, please refer to the All Guides from WAPI<GO>.

The Bloomberg API interface is thread-safe and thread-aware, giving applications the ability to 
utilize multiple processors efficiently. The Bloomberg API supports run-time downloadable schema for 
the service it provides, and it provides methods to query these schemas at runtime. This means 
additional service in Bloomberg API is supported without addition to the interface.

The object model for Java, .NET and C++ are identical. The C interface provides a C-style version of 
the object model.



.. toctree::
	:maxdepth: 3

	migration
   	serverApiInstallation


