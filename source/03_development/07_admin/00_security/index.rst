**********
Security
**********

This security documentation describes how to configure the target system for using and securing the admin API resources.

General
=======

The Admin API resources allow several operations on data that circumvent the versioning of changes in the openEHR system. 
For instance, after using a DELETE operation via the standard client API the entries will still be available inside the history tables and all changes to that resources can be seen in the audit of the data entry. 
The admin API allows the physical deletion of the entry and all of the history entries as well, thus the changes cannot be traced and the data is completely lost.

In health IT systems all operations on data must be stored in a manipulation safe way and all changes must be traceable.
Our recommendation is therefore to not use the admin API in production. 

During development of EHRbase and connected systems it is often necessary to replace or remove data from the system completely. 
This could also be done via common database tools, but this solution is often very cumbersome and not cleaning up all data related to the resource.

Role based access control
=========================

As a minimum security measurement the EHRbase Security configuration should use a role based access control. Independent from the selected security mechanism (Basic auth, OpenID-Connect, etc.) each resource request should be checked for the users role and then be permitted or rejected. 

We are currently using this two roles in the system:

.. csv-table::
   :header: "Role", "Permissions"

        user, Can access all client resources specified by openEHR REST API specification and do not have access to all resources in the /admin and subsequent endpoints
        admin, Can access all resources; i.e. all client resources and the /admin endpoints

Security related response codes
===============================

Execution of requests to each endpoint can have two security related error codes that have to be handled by client applications:

.. csv-table::
   :header: "Code", "Reason", "Possible Solution"

        401, Authorization information is incorrect / expired, Reauthenticate the user via a new login or by refreshing the authorization information (id_token etc.)
        403, Authorization information is valid but permission to requested resource is denied, The users role is not allowed to access this resource. Thus this is okay or the users role must be configured in the related security system (OpenID-Connect server or config files etc.)

Depending on the security configuration the 401 error could be handled automatically, e.g. if using OAuth2 the client could get a new id_token if the used one has expired and then will re-do the request with new information. Also it could be possible to show the user a login screen to reauthenticate against the security system.

All resources in the admin API can return this response code and will not be mentioned in the documentation pages itself.