****************************
/admin/{:ehr_id}/composition
****************************

Methods to administrate the Composition resource.

DELETE /admin/{:ehr_id}/composition/{:composition_id}
=====================================================

Delete the composition identified by "composition_id" physically from database. This will also remove all history information on the related composition and their entries.

The target "composition_id" must be formatted as versioned object uid (i.e. UUID). In contrast to the client API this method will physically remove the given composition and all linked metadata.

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

* 204 (NO CONTENT)	Composition has been deleted successfully.
* 404 (NOT FOUND)	The Composition with provided id cannot be found. This can also occur if the id is not in valid UID format.

**Body**

No body returned