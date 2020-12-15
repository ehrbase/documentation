******************
Status and Metrics
******************

.. note:: Added in EHRbase version 0.15.0

.. warning:: The status and metrics endpoints can contain critical and sensitive information on the
   running ehrbase instance that could serve possible attackers to identify vulnerabilities. Please
   ensure to use a solid security config on the EHRbase server or disable the metrics and status
   endpoints in production environments with sensitive data.

This section covers the status and metric endpoints provided by Spring-Boot-Actuator to provide
additional information on the running EHRbase server including metrics on the usage of the API
since last boot.

The Status and Metric endpoints provide additional information which is useful in environments that
use a bunch of microservices that rely on each other as in cloud orchestration like Kubernets.
The status endpoint can support the management of these complex systems by providing *liveness* and
*readiness* endpoints which allow orchestration tools to check whether the EHBase service is
running and if it capable of serving incoming requests.

Additional metrics allow to identify bottlenecks in connections between services as between EHRbase
service and a connected database server if the connection metric shows very long durations for
communications or if the connection pool limit is exhausted.
Another possible use case could be detection of attacks against the EHRbase service due to a 
high occurrence of authorization client errors. 
As you see there are many things you can do with these endpoints.

The Status and Metrics API interface is available at the "/status" resource which will be appended 
to the base URL of the ehrbase instance. E.g. if EHRbase is running at "https://api.ehrbase.org" the
status API and all sub endpoints are available at "https://api.ehrbase.org/ehrbase/status".

.. toctree::
   :name: Status and Metrics
   :maxdepth: 2

   security/index
   usage/index
   env_endpoint/index
   health_endpoint/index
   info_endpoint/index 
   metrics_endpoint/index
   prometheus_endpoint/index
