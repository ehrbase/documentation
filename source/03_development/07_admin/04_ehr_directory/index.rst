**************************
/admin/{:ehr_id}/directory
**************************

Methods to administrate the Folder resource.

DELETE /admin/{:ehr_id}/directory/{:folder_id}
==============================================

Delete the Folder identified by "folder_id" physically from database. This will also remove all history information on related folders and their hierarchies.

The target "folder_id" must formatted as versioned object uid (i.e. UUID).

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

* 204 (NO CONTENT)	Folder has been deleted successfully.
* 404 (NOT FOUND)	The Folder with provided id cannot be found. This can also occur if the id is not in valid UID format.

**Body**

No body returned