*****
Usage
*****

.. note:: Added in EHRbase version 0.15.0

By default all status and metric endpoints are disabled in EHRbase. To opt-in endpoints you should
start the EHRbase with an environment variable per endpoint you want to enable.

**Run from command line**:

.. code-block:: bash
   
   $~/ehrbase: export MANAGEMENT_ENDPOINT_ENV_ENABLE=true
   $~/ehrbase: java -jar application/target/application-0.15.0.jar

**Run from Docker container**:

.. code-block:: bash
   
   $~/: docker run -e MANAGEMENT_ENDPOINT_ENV_ENABLE=true --name ehrbase --network ehrbase-net -p 8080:8080 -d ehrbase/ehrbase:latest

The following table lists all available endpoints that can be enabled with environment variables:

.. csv-table::
   :header: "Parameter", "Usage", "Example"

    MANAGEMENT_ENDPOINT_ENV_ENABLE,         Enable /status/env endpoint from actuator                  true / false
    MANAGEMENT_ENDPOINT_HEALTH_ENABLE,      Enable /status/health endpoint from actuator               true / false
    MANAGEMENT_ENDPOINT_INFO_ENABLE,        Enable /status/info endpoint from actuator                 true / false
    MANAGEMENT_ENDPOINT_METRICS_ENABLE,     Enable /status/metrics endpoint from actuator              true / false
    MANAGEMENT_ENDPOINT_PROMETHEUS_ENABLE,  Enable /status/prometheus endpoint from actuator           true / false

Additionally you can configure the following actuator settings if required:

.. csv-table::
   :header: "Parameter", "Usage", "Example"

   MANAGEMENT_ENDPOINT_HEALTH_PROBES_ENABLE, Enable Kubernetes probe endpoints /status/health/liveness and /status/health/readiness explicit in non Kubernetes environments, true/false


