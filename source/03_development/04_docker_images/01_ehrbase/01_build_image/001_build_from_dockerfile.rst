Build Image From Dockerfile
===========================

EHRbase's Github repository contains a `Dockerfile <https://github.com/ehrbase/ehrbase/blob/develop/Dockerfile>`_ which you can use to build your custom Docker image from. Follow steps below to build your own Docker Image (with default EHRbase settings):

.. code-block:: bash

    git clone https://github.com/ehrbase/ehrbase.git
    cd ehrbase
    docker build -t give-it-a-name .    # don't foget the `.` at the end of the command!!!
    docker image ls                     # you should be able to see the image you just created





Why To Build Own Image?
=======================

EHRbase's Dockerfile defines some environent variables with default values which will be active when you run a container from created Docker image. For example when you run the following command

.. code-block:: bash

    docker run ehrbase/ehrbase

the running Docker container will have environent variables with default values as shown in code snippet from related part of Dockerfile below:

.. code-block:: Dockerfile

    ...

    ARG DB_URL=jdbc:postgresql://ehrdb:5432/ehrbase
    ARG DB_USER="ehrbase"
    ARG DB_PASS="ehrbase"
    ARG SERVER_NODENAME=docker.ehrbase.org
    
    ENV EHRBASE_VERSION=${EHRBASE_VERSION}
    ENV DB_USER=$DB_USER
    ENV DB_PASS=$DB_PASS
    ENV DB_URL=$DB_URL
    ENV SERVER_NODENAME=$SERVER_NODENAME
    ENV SECURITY_AUTHTYPE="NONE"
    ...

The values of all `ARG(s)` can be overwritten during image build time to adjust default (run time) behaviour of your custom Docker image. Use `--build-arg ARG_name=value` to override default values when building your image. See example below:

.. code-block:: bash

    docker build --build-arg DB_URL=your-db-url \
                 --build-arg DB_USER=your-db-user \
                 --build-arg DB_PASS=your-db-pass \
                 --build-arg SERVER_NODENAME=your-system-name \
                 -t give-your-image-a-name:and-tag .

In addition to overriding default ENV values during build time it is also possible to override ENV values and even add new ENVs to a container's run time. Check next example (which assumes you pulled or created an image named `ehrbase/ehrbase`):

.. code-block:: bash

    docker run -e DB_URL=jdbc:postgresql://ehrdb:5432/ehrbase \
               -e DB_USER=foouser \
               -e DB_PASS=foopass \
               -e SERVER_NODENAME=what.ever.org \
               ehrbase/ehrbase





