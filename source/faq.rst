###
FAQ
###

General FAQ
===========

<<<<<<< HEAD
* What is a session?
	Sessions are logical data stream connections and the EMSX API supports failover betweeen physical connections. During this failover, EMSX API will handle re-subscriptions for the end application.

	If you are using multiple bloomberg API services, it is recommended to use separate sessions to avoid 
	delaying a fast stream with slow one. For most design, it's best to have separte session for 
	real-time data vs. EMSX API or reference data service. 

* Should I open and close sessions as needed?
	No, typically opening and closing a session is expensive for both the client's application and for Bloomberg back-end and thus unnecessary for most application designs while using EMSX API. 
=======
>>>>>>> origin/master
