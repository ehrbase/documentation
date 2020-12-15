**************
Security
**************

.. note:: Added in EHRbase version 0.15.0

Since the Status and Metric endpoints provide such valuable information they are secured by 
EHRbase integrated security solutions. If you are using at least the *Basic Auth* method to secure
incoming requests the endpoints are only available for users with the *Admin*-Role.

If you want to access specific metric and health endpoint from a statistics and management service
ensure not sending the passwords in clear text. For Basic Auth encode them into a Base64 string
as described here: `EHRbase Security docs <https://github.com/ehrbase/ehrbase/tree/develop/doc/security#basic-auth>`_.

.. warning:: All auth methods can be attacked easily if you do not use HTTPS encrypted communication
   outside trusted networks as internal VPN or secured cloud system networks. Ensure to secure your
   environment properly.

For OAuth2 methods please configure the auth server appropriate and provide the *Admin*-Role to the
user logging in. Also check your monitoring and management service manuals how to authorize
against the OAuth2 service and how to obtain new access tokens on token expiration (refresh).


