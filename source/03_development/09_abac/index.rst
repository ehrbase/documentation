******************************
Attribute-based Access Control
******************************

This section covers EHRbase's Attribute-based Access Control (ABAC) system and how to configure it.

Concept
=======

EHRbase offers a predefined set of rules, logics and configurations to enable and use ABAC checks for several operations.

ABAC checks are available for many endpoints. Endpoints are clustered and processed by resource/path. So all .../composition/... endpoints are defined the same and run through the same logic.

There are five of those resource groups:

- EHR
- EHR Status
- Composition
- Contribution
- Query

.. note::
    Directory endpoints are only handling references. Definitions aren't patient (or most likely practitioner) facing. So both aren't available for ABAC checks.

Each can be configured to create an ABAC request with

- the given policy name and
- a set of parameters (rendered as simple JSON request body)

The set of parameters can composed via configuration from the predefined set of attributes:

- Organization (taken from the JWT claim)
- Patient (taken from the JWT claim, with current scope's EHR subject as internal comparison)
- Template ID (taken from the scopes composition, where applicable)


Configuration
=============

Enable with ``ABAC_DISABLED=false`` or matching application.yml attribute.

.. note::
    The ABAC handling relies on OAuth2 tokens. Therefore, ABAC can practically only used together with OAuth enabled.

The ABAC server can be set with ``ABAC_SERVER``. The URL is expected to end with "/" as the ABAC requests will directly add the policy name like: ".../policy/execute/name/" will be modified to ".../policy/execute/name/has_consent_template" in a specific instance.

Real example to enable ABAC with OAuth: ``SECURITY_AUTHTYPE=OAUTH;ABAC_DISABLED=false;ABAC_SERVER=http://localhost:3001/rest/v1/policy/execute/name/;SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUERURI=https://keycloak.PROJECT.com/auth/realms/ctr``

Both the organization and patient claim of the expected JWT can be configured via ``ABAC_ORGANIZATIONCLAIM`` and ``ABAC_PATIENTCLAIM`` respectively. Their default is ``organization_id`` and ``patient_id``.

For the configuration of the endpoints please refer to the application.yml. Here is one groups' section as example, where the composition endpoints are configured to have the policy ``has_consent_template`` with the listed attributes in the request body:

.. code-block:: yaml

    policy:
        composition:
        name: 'has_consent_template'
        parameters: 'organization, patient, template'

.. warning::
    If parameters like ``organization`` and ``patient`` are configured to be used by the logic, but the context's JWT doesn't carry the matching claim the ABAC check will fail.

Detailed endpoint overview
==========================

EHR
---

Enabled ABAC-parameters: organization, patient

Disabled: template - not valid in scope of EHR

Implemented and tested:

- Get(s)

Not included:

- Post, Put: Completely new EHR has no subject context and EHR has no template. No ABAC checks.
  
EHR Status
----------

Enabled ABAC-parameters: organization, patient

Disabled: template - not valid in scope of EHR_STATUS

Implemented and tested:

- Get(s)
- Put
- Get(s) - Versioned sub-type - EHR_STATUS version


Composition
-----------

Enabled ABAC-parameters: organization, patient, template

Implemented and tested:

- Get(s)
- Post, Put
- Delete
- Get(s) - Versioned sub-type - Composition version


Contribution
------------

Enabled ABAC-parameters: organization, patient, template

Implemented and tested:

- Post

Not included:

- Get: Only returns references, so no ABAC checks necessary.

Query
-----

Enabled ABAC-parameters: organization, patient, template

Note: Currently this handling relies on the AuditResultMap of the QueryService to provide the ABAC logic with patient and template context data. This entails, that patient and template ID(s - multiple per query possible) are only given, when the query directly sets them in the SELECT.

Implemented and tested:

- All four query endpoints