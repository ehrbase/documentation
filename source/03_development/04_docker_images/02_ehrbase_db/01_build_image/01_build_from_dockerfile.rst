Build Image From Dockerfile
===========================

.. code-block:: bash

    git clone https://github.com/ehrbase/docker.git
    cd docker/dockerfiles
    docker build -t ehrbase_db -f ehrbase-postgresql-full.dockerfile .
    docker image ls
