############
Introduction
############


The IOI API Server provides Bloomberg users with the ability to publish IOIs into Bloomberg and 
subscribe IOIs in Equities and Derivatives using Bloomberg API

The IOI API Server for subscribing requires separate authorization by the publishing broker on top of 
the Bloomberg authorization.  


.. note::

	IOI API users will need the following steps completed before using the IOI API service.

		#. Signed EMSxNET Order Originator Agreement.
		#. Install serverapi.exe and register with Bloomberg.
		#. Enable IOI API per UUID by the Global EMSX Trade Desk for Test (Beta) and Production. 
		#. To get access to IOI API in UAT and production, please click <Help><Help> on EMSX<GO>.
		#. Download Bloomberg Desktop API v3 SDK from WAPI<GO> in Bloomberg terminal.
		

To get access to IOI API in UAT and production, please click <Help><Help> on EMSX<GO>.


Sign Up - Programming Support
=============================


For general programming support, please open an account through the following URL:- 

https://service.bloomberg.com


For a new user, you will need to first start by creating the account in https://service.bloomberg.com  and select “Request a new account”.


.. image:: /image/signIn.png
	:width: 300pt


Select Enterprise Solutions:-


.. image:: /image/accountType.png
	:width: 300pt


Fill out the details on Account Registration:- 


.. image:: /image/accountReg.png
	:width: 300pt

Select B-Pipe, select the role as Technical Contact, and insert Customer #.  The Customer # can be found in the terminal by 
typing IAM<GO>. Select production information as B-Pipe and click register to finish:-


.. image:: /image/register.png
	:width: 300pt



IOI API Code Samples
=====================


.. important::

			The latest IOI API Code samples can be found `here`_.

			.. _here: https://github.com/tkim/ioi_api_repository


