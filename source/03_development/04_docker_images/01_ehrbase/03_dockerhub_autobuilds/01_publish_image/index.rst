Docker Hub Autobuilds
---------------------

EHRbase Docker image is created and published on `Docker Hub Registry <https://hub.docker.com/r/ehrbase/ehrbase>`_ on every push/merge to `master`, `develop` and `release-*` branch.
Created Docker images are tagged as shown in table below:

.. csv-table::
   :header: "Branch", "Docker Tag", "Example"

        master, latest, `ehrbase/ehrbase:latest` 
        develop, next, `ehrbase/ehrbase:next`
        release-*, semversion, `ehrbase/ehrbase:0.13.0`
