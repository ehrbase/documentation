**********
/admin/ehr
**********

Methods to administrate the EHR resource.

DELETE /admin/ehr/{:ehr_id}
===========================

Remove an EHR and related data physically from the database.

Request format
--------------

The request should be formatted as follows:

**Headers**

There are no headers required.

**Body**

No body required

Response format
---------------

The response will be formatted as follows:

**Headers**

There are no extra headers returned

**Status Codes**

Depending on several request conditions or errors during the request handling there will be on of the following status codes returned:

* 204 (NO CONTENT)	EHR has been deleted successfully.
* 404 (NOT FOUND)	The EHR with provided id cannot be found. This can also occur if the id is not in valid HIER_OBJECT_ID format.

**Body**

No body returned