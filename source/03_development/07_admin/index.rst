*********
Admin API
*********

.. warning:: WIP

.. warning:: Please be aware of potential security and consistency risks in production if API security configuration is not done properly.

.. important:: The Admin REST API is not part of the official openEHR standard. This is an additional feature provided by the EHRbase team to support development and system administrators.

This section covers the Admin API for EHRbase which can be used for administrative tasks or help in development.

To generally enable the Admin API set the `ADMINAPI_ACTIVE` environment variable to true 
(see `Spring Boot Externalized Configuration`_ for more details and options on how to set such configuration attributes).

.. _Spring Boot Externalized Configuration: https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config

The Admin API interface is available at the "/admin" resource which will be appended to the base URL of the openEHR REST interface. E.g. if the base URL is "https://api.ehrbase.org/ehrbase/rest/openehr/v1" the admin API and all sub resources are available at "https://api.ehrbase.org/ehrbase/rest/openehr/v1/admin".

.. toctree::
   :name: Admin API
   :maxdepth: 2

   00_security/index
   01_ehr/index
   02_ehr_composition/index
   03_ehr_contribution/index
   04_ehr_directory/index
   05_template/index
