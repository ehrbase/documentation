***************
/admin/template
***************

Methods to administrate the Template resource.

PUT /admin/template/:template_id
================================

Replace an existing template with the new provided data.

Request format
--------------

The request should be formatted as follows:

**Headers**

.. csv-table::
   :header: "Header-Name", "Required", "Description", "Accepted values"

        Accept, , Desired response format after a successful update operation., application/json; application/xml
        Content-Type, Yes, Format of the content body, application/json; application/xml
        Prefer, , Tell the API if you want the updated template data to be returned or not., return=representation | return=minimal

**Body**

The request body can contain the same data as for the client POST request in the desired format.

Response format
---------------

The response will be formatted as follows:

**Headers**

.. csv-table::
   :header: "Header-Name", "Description"

        Content-Type, Returned data type. Depends on data type sent with "Accept" header.

**Status Codes**

Depending on several request conditions or errors during the request handling there will be one of the following status codes returned:

.. csv-table::
    :header: "Code", "Cause/Meaning"

        200 (OK), Template has been replaced successfully and the body contains the new Template data after the update.
        204 (NO CONTENT), Template has been replaced successfully and the body contains no data since "Prefer" header was set with "return=minimal"
        400 (BAD REQUEST), The body contains invalid data to replace the existing content; e.g. missing mandatory fields or data structures that could not be serialized.
        404 (NOT FOUND), The Template with provided id cannot be found.

**Body**

Whether the clients requested "Prefer" header setting the full new Template entry after the updated has been applied will be returned or it will be empty.

DELETE /admin/template/:template_id
===================================

Delete the template identified by "template_id" physically from server. Depending on your implementation the entry has to be removed from file or database storage.

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

.. csv-table::
    :header: "Code", "Cause/Meaning"

        204 (NO CONTENT), Template has been deleted successfully.
        404 (NOT FOUND), The Template with provided id cannot be found.
        422 (UNPROCESSABLE ENTITY), The Request was correct and template can be found but it is still used by compositions.

**Body**

No body returned

DELETE /admin/template/all
==========================

Delete all templates physically from server. Depending on your implementation the entries has to be removed from file or database storage.

.. note:: The EHRbase environment variable "ADMINAPI_ALLOWDELETEALL" must be set to true. Otherwise the endpoint does not accept requests.

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

Depending on several request conditions or errors during the request handling there will be one of the following status codes returned:

.. csv-table::
    :header: "Code", "Cause/Meaning"

        200 (OK), Templates have been deleted successfully.
        422 (UNPROCESSABLE ENTITY), The Request was correct but there are templates which are still used by compositions.

**Body**

For 200 (OK): The number of deleted templates is returned in the following schema: ::

    {
    "deleted": integer
    }

For 422 (UNPROCESSABLE ENTITY): Body contains message with list of Compositions that are referencing at least one Template.
