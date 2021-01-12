Run EHRbase in Docker
=====================

.. note:: 

    Remember: EHRbase requires a properly configured and running PostgreSQL DB to work.
    Make sure to set this up first before you try run EHRbase.

To run EHRbase in a Docker Container first pull the official Docker image from Docker Hub:

.. code-block:: bash

    docker pull ehrbase/ehrbase


**OR**
 
build your own image form Dockerfile:


.. code-block:: bash

    git clone https://github.com/ehrbase/ehrbase.git
    cd ehrbase
    docker build -t myehrbase/ehrbase .
    docker image ls


**THEN** use the `docker run` command adjusting parameters to your needs to change Container's default behaviour.

.. note:: 

    Remember: Container's default behaviour is set during Docker image build time.


.. code-block:: bash

    docker run -e DB_URL=jdbc:postgresql://ehrdb:5432/ehrbase \
               -e DB_USER=foouser \
               -e DB_PASS=foopass \
               -e SYSTEM_NAME=what.ever.org \
               -p 8080:8080 \
               ehrbase/ehrbase


.. csv-table::
   :header: "Parameter", "Usage", "Example"

    DB_URL,                                 Database URL. Must point to the running database server.,    jdbc:postgresql://ehrdb:5432/ehrbase
    DB_USER,                                Database user configured for the ehr schema.,                ehrbase
    DB_PASS,                                DB user password,                                            ehrbase
    SYSTEM_NAME,                            Name of local system,                                        local.ehrbase.org
    SECURITY_AUTHTYPE,                      HTTP security method,                                        BASIC / OAUTH
    SECURITY_AUTHUSER,                      BASIC Auth username,                                         myuser
    SECURITY_AUTHPASSWORD,                  BASIC Auth password,                                         myPassword432
    SECURITY_AUTHADMINUSER,                 BASIC auth admin user,	                                      myadmin
    SECURITY_AUTHADMINPASSWORD,             BASIC auth admin password,                                   mySuperAwesomePassword123
    ADMINAPI_ACTIVE,                        Should admin endpoints be enabled,                           true / false
    ADMINAPI_ALLOWDELETEALL,                Allow admin to delete all resources - i.e. all EHRs,         true / false
    MANAGEMENT_ENDPOINT_ENV_ENABLED,        Enable /status/env endpoint from actuator                    true / false
    MANAGEMENT_ENDPOINT_HEALTH_ENABLED,     Enable /status/health endpoint from actuator               true / false
    MANAGEMENT_ENDPOINT_INFO_ENABLED,       Enable /status/info endpoint from actuator                   true / false
    MANAGEMENT_ENDPOINT_METRICS_ENABLED,    Enable /status/metrics endpoint from actuator             true / false
    MANAGEMENT_ENDPOINT_PROMETHEUS_ENABLED, Enable /status/prometheus endpoint from actuator       true / false


.. note::

    Do NOT set `SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUERURI` in combination with `SECURITY_AUTHTYPE=BASIC`!
    This will crash EHRbase at start up.


.. csv-table::
   :header: "Parameter", "Usage"

    SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUERURI, OAuth2 server isuer uri
    example:,                                            https://keycloak.example.com/auth/realms/ehrbase




Run EHRbase + DB with Docker-Compose
====================================

.. note::

    Prerequisite: docker-compose is installed on your machine

With `Docker-Compose <https://github.com/docker/compose>`_ you can start EHRbase and the required DB from a configuration file written in YAML format.

There is an example `docker-compose.yml <https://github.com/ehrbase/ehrbase/blob/develop/application/docker-compose.yml>`_ configuration file in our Git repository. Using it allows you to set up and start EHRbase along with the required database with a few simple steps:


.. code-block:: bash

    # download the docker-compose.yml file to your local
    wget https://github.com/ehrbase/ehrbase/raw/develop/application/docker-compose.yml
    wget https://github.com/ehrbase/ehrbase/raw/develop/application/.env.ehrbase
    docker-compose up

    # OR: start both containers detached, without blocking the terminal
    docker-compose up -d


.. note::

    It is not necessary to have the whole Git repository on your machine, just copy the docker-compose.yml file to a local working directory and run `docker-compose up`.


.. note::

    DB data is saved in ./.pgdata for easier access.

You can configure all environment variables via the file `.env.ehrbase` which is located at the same
folder as the `docker-compose.yml` file. This is also required for setting boolean values due to
Docker compose files do not allow setting boolean values directly inside docker-compose.yml.

