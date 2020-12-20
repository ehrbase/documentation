Run DB with default parameters
----------------------------

.. code-block:: bash

    docker pull ehrbase/ehrbase-postgres:11.10
    docker run --name ehrdb -d -p 5432:5432 ehrbase/ehrbase-postgres:11.10



Customization
-------------

If you want to set specific parameters use environment variables provided with the -e option to the docker run command. This will be used to set the specific parameters for root postgres user password and ehrbase user and password. If not provided the default values will be used.

The following parameters can be set via `-e` option:

=================  =======================  ====================================
 Parameter          Usage                    Default
=================  =======================  ====================================
POSTGRES_PASSWORD  Password for postgres	postgres
EHRBASE_USER	   ehrbase db username	    ehrbase
EHRBASE_PASSWORD   ehrbase db password	    ehrbase
=================  =======================  ====================================
