Docker environment examples
===========================

Here you can find some example settings for common use cases for the usage of EHRbase Docker
containers. You can also use the environent variables with the normal .jar execution by setting
the variables according to your operating system.

Use BASIC auth
--------------

Run the docker image with this setting:

.. code-block:: bash

    docker run --network ehrbase-net --name ehrbase -e SECURITY_AUTHTYPE=BASIC \
    -e SECURITY_AUTHUSER=myuser -e SECURITY_AUTHPASSWORD=ThePasswordForUser \
    -e SECURITY_AUTHADMINUSER=myadmin -e SECURITY_AUTHADMINPASSWORD=SecretAdminPassword \
    -d -p 8080:8080 ehrbase/ehrbase:latest

This will set the used authentication method to BASIC auth and all requests against the EHRbase
must be provided with the Authorization header set to `Basic %username%:%password%` whereas the
username and password must be encoded with base64. 

.. note::
  
  Ensure you use an encrypted connection over https otherwise the username and password can be
  descripted easily

Use OAuth2
----------

Run the docker image with this setting.

.. code-block:: bash

  docker run --network ehrbase-net --name ehrbase -e SECURITY_AUTHTYPE=OAUTH \
  -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUERURI=https://keycloak.example.com/auth/realms/ehrbase \
  -d -p 8080:8080 ehrbase/ehrbase:latest

You have to prepare the authentication server including a valid client at the target server to
get this setup run.

Use OAuth2 and Attribute-based Access Control
---------------------------------------------

Run the docker image with this setting.

.. code-block:: bash

  docker run --network ehrbase-net --name ehrbase 
  -e SECURITY_AUTHTYPE=OAUTH \
  -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUERURI=https://keycloak.example.com/auth/realms/ehrbase \
  -e ABAC_ENABLED=true
  -e ABAC_SERVER=http://localhost:3001/rest/v1/policy/execute/name/
  -d -p 8080:8080 ehrbase/ehrbase:latest

Additionally, add the configuration of the endpoints and policies either here with additional -e parameters
or more user-friendly in a separate docker-compose.yml file.
