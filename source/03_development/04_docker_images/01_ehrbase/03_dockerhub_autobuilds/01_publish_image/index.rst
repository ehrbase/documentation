Docker Hub Autobuilds
---------------------

EHRbase Docker image is created and published on `Docker Hub Registry <https://hub.docker.com/r/ehrbaseorg/ehrbase>`_ on every push/merge to `master`, `develop` and `release-*` branch.
Created Docker images are tagged as shown in table below:

.. csv-table::
   :header: "Branch", "Docker Tag", "Example"

        master, latest, `ehrbaseorg/ehrbase:latest` 
        develop, next, `ehrbaseorg/ehrbase:next`
        release-*, semversion, `ehrbaseorg/ehrbase:0.13.0`
