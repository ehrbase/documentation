.. _fhir_bridge:

===========
FHIR bridge
===========

This is a component designed as a broker between an HL7 FHIR client and an openEHR server.
It contains several ad-hoc integrations for creating, searching and getting data using the
FHIR formats in the front-end and the openEHR data structures in the backend.

You can clone the project from https://github.com/ehrbase/fhir-bridge


Design decisions
----------------

1. Each integration is designed and developed ad-hoc, there is no generic solution to map FHIR into openEHR and viceversa.
2. On the FHIR side the type of resource is needed, and a correspondent profile. For some resources there might not be a profile
   available, which is not ideal since semantics for data mapping depend on speicifc FHIR profiles.
3. On the openEHR side, the Operational Templates are needed (OPT).
4. Data mappings are mainly done between a FHIR profie and an openEHR OPT.
5. To support the 'create' FHIR operation, a mapping should be designed and implemented to receive a FHIR resource, map it's data
   to an openEHR COMPOSITION, and submit that COMPOSITION to the openEHR Server.
6. To support the 'search' FHIR operation, an openEHR query (AQL) should be designed to get the required data from the openEHR
   Server to map to FHIR resources to be retrieved. We chosen to return the COMPOSITION.uid as the FHIR resource id.
7. To support the 'read' FHIR operation, we use the COMPOSITION.uid retrieved as the FHIR resource id in the search, as the FHIR
   resource identifier to get the individial resource.


Technology stack
----------------

The FHIR bridge is mainly implemented over Spring Boot (application + configuration), using HAPI FHIR to process all FHIR related
requests, expose API endpoints (via "resource providers") and support all FHIR resource structures. The third main component is
the openEHR SDK, which contains a client library used to communicate with the openEHR server (EHRBASE).


Testing
-------

We preparated a set of HTTP requests to be able to tests different services of the FHIR bridge. The requests work on Insomnia REST
Client (https://insomnia.rest/).

Just install Insomnia, and import this file: https://github.com/ehrbase/fhir-bridge/blob/develop/src/test/resources/Insomnia_2020-07-27.json

Both, the openEHR server (EHRBASE) and FHIR bridge need to be running to be able to test.


'Create' operation internal flow
--------------------------------

To process a 'create resource' request, the FHIR bridge receives the FHIR resource, already parsed to an object, on a 'resource provider'
(like https://github.com/ehrbase/fhir-bridge/blob/develop/src/main/java/org/ehrbase/fhirbridge/fhir/provider/ObservationResourceProvider.java).

The method annotated with @Create will be the one that receives the resource object. For instance, in ObservationResourceProvider,
the method will be:

.. code-block:: java

    @Create
    @SuppressWarnings("unused")
    public MethodOutcome createObservation(@ResourceParam Observation observation)


If the resource complies with a profile, a check for the profile is done: is the profile supported by the FHIR bridge?

Because of that step, each time a new mapping is added, the profile should also be added to FHIR bridge.

Then the resource is mapped to the correspondent COMPOSITION, depending on the FHIR profile, we determine which COMPOSITION should
be created. Each type of COMPOSITION is determined by an OPT. This is why a profile is needed for each FHIR resource: if we don't
have a profile, a resource could really be mapped to differnt OPTs.

We created one mapping class for each FHIR profile/OPT pair, in which we implement the bidirectional mappings. For instance, for the
'body temperature' FHIR profile, we have a FhirObservationTempOpenehrBodyTemperature class that implements the FHIR -> openEHR mapping.
So we can pass the observation object and get the correspondent openEHR COMPOSITION instance. That class implements the mappings
designed at design time.

Once we have a COMPOSITION instance, the client library is used to commit the COMPOSITION to the openEHR Server. This operation
returns the UID of the COMPOSITION created in the server.

Finally, if everything worked correctly, the FHIR client should get a successful response.


'Search' operation internal flow
--------------------------------

To process a 'search resource' request, the FHIR bridge receives the search request on a 'resource provider' 
(like https://github.com/ehrbase/fhir-bridge/blob/develop/src/main/java/org/ehrbase/fhirbridge/fhir/provider/ObservationResourceProvider.java).

The method annotated with @Search is the one doing the processing. For instance in ObservationResourceProvider, the method
will be:

.. code-block:: java

    @Search
    @SuppressWarnings("unused")
    public List<Observation> getAllObservations(
        @OptionalParam(name="_profile") UriParam profile,
        @RequiredParam(name=Patient.SP_IDENTIFIER) TokenParam subjectId,
        @OptionalParam(name=Observation.SP_DATE) DateRangeParam dateRange,
        @OptionalParam(name=Observation.SP_VALUE_QUANTITY) QuantityParam qty,
        @OptionalParam(name="bodyTempOption") StringParam bodyTempOption
    )


Note: the parameters for the search are defined arbitrarily, it depends on the resource type and the requirements of the client.

For observations, we allow to search by patient identifier, date range, value of the observation (only quantities for now). Because
we have many profiles for the observation resource, we also need a profile paramter as a filter. The 'bodyTempOption' parameter is
just a test to evaluate how different implementations of the search functionality work, it will be removed in the future.

More about search parameters: https://hapifhir.io/hapi-fhir/docs/server_plain/rest_operations_search.html

When the method receives the requests, it checks the profile parameter to choose the right handler for the search. For instance,
if the profile is 'http://hl7.org/fhir/StructureDefinition/bodytemp', this method will be resolving the search:

.. code-block:: java

    List<Observation> processSearchBodyTemperature2(TokenParam subjectId, DateRangeParam dateRange, QuantityParam qty)


What the search resolution does is:

1. creates an AQL query using the filters and search parameters, this is ad-hoc per FHIR profile and openEHR OPT.
2. executes the AQL query in EHRBASE to get the matching data (should be enough the data to fill the FHIR resources to be retrieved)
3. the openEHR query results are processed, mapping the openEHR data to the FHIR resource structure
4. each FHIR resource is stored in a list to be retrieved
5. the 'resource provider' receives the list and returns it
6. HAPI FHIR does the work of serializing that list to JSON, and that is what is retrieved to the FHIR client


'Read' operation internal flow
-----------------------------

To process a 'read resource' request, the FHIR bridge receives the get request on a 'resource provider'
(like https://github.com/ehrbase/fhir-bridge/blob/develop/src/main/java/org/ehrbase/fhirbridge/fhir/provider/ConditionResourceProvider.java).

The method annotated with @Read is the one doing the processing. For instance in ConditionResourceProvider, the method
will be:

.. code-block:: java

    @Read()
    @SuppressWarnings("unused")
    public Condition getConditionById(@IdParam IdType identifier)


The logic on this one is similar to the search but simpler, since there is only one resource to be retrieved, and the
search params are just one: the resource identifier. So a similar AQL query like the one used for the serach is used to
get a COMPOSITION by identifier, we also check that complies with a specific OPT.

The query results are processed, mapping to a FHIR resource and returning that. HAPI FHIR serializes the resource to
JSON and delivers that to the FHIR client.

If the query results are empty, the FHIR bridge returns a 404 Not Found.



Adding a new FHIR profile to FHIR bridge
----------------------------------------

TBD



Extending FHIR bridge
---------------------

Each change to the FHIR bridge should have a ticket created, explaining the change. Create a new feature branch with
ticket number like: feature/123_awesome_new_feature.

    cd fhir-bridge
    git checkout develop
    git checkout -b [BRANCH-NAME]
    # At the first push:
    git push -u origin [BRANCH-NAME]
    # For later pushes:
    git push


Tickets are here: https://github.com/orgs/ehrbase/projects/28

Comment out failing tests (temporary problem only)

    * Open "/fhir-bridge/src/test/java/org/ehrbase/fhirbridge/FhirBridgeApplicationIT.java" in the editor
    * Comment out the "createConditionUsingInvalidProfile" and "createObservationUsingUnsupportedProfile" tests


Note: the tests class was modified to https://github.com/ehrbase/fhir-bridge/blob/develop/src/test/java/org/ehrbase/fhirbridge/FhirBridgeApplicationTestIT.java



Build 
-----

    cd fhir-bridge
    mvn clean install

IDE
---

Load project into development environment

  * especially for eclipse: as a Maven project


Add external files
------------------

The following files must be copied into the respective target directories.

  * FHIR data structure (XML format)
    * Target directory /fhir-bridge/src/main/resources/profiles
    * Source https://simplifier.net/ForschungsnetzCovid-19 (under Resources/Observation)
    * Example https://simplifier.net/ForschungsnetzCovid-19/RespiratoryRate/~xml (example)
  * FHIR observation sample file (JSON format)
    * Target directory / fhir-bridge / src / test / resources / Observation
    * Source http://hl7.org/fhir/R4/observation-examples.html
    * Example http://hl7.org/fhir/R4/observation-example-respiratory-rate.json.html
  * Operational template (OPT format)
    * Target directory / fhir-bridge / src / main / resources / opt
    * Source http://88.198.146.13/ckm/projects/1246.152.26/resourcecentre (GECCO Core)
    * Example (The example files are listed in the overview)


Structure Definition (Enum)
---------------------------

 * Add an entry with the FHIR URL to src / main / java / org / ehrbase / fhirbridge / fhir / Profile.java
   * Source http://hl7.org/fhir/R4/observation-vitalsigns.html (search from list)
   * Example RESPIRATORY_RATE ("http://hl7.org/fhir/StructureDefinition/resprate", ResourceType.Observation)


Use the SDK generator to create new classes from the operational template
-------------------------------------------------------------------------

(Windows example started from the path "../openEHR_SDK/generator/target")

    java
    -jar generator-0.3.7.jar
    -opt ../../../fhir-bridge/src/main/resources/opt/Atemfrequenz.opt
    -out ../../../fhir-bridge/src/main/java
    -package org.ehrbase.fhirbridge.opt

(Linux example with path information from the perspective of the home directory)

    java
    -jar ~ / Desktop / nfn / openEHR_SDK / generator / target / generator-0.3.7.jar
    -opt ~ / Desktop / nfn / 2020-08-28_fhir-bridge / src / main / resources / opt / body size.opt
    -out ~ / Desktop / nfn / 2020-08-28_fhir-bridge / src / main / java /
    -package org.ehrbase.fhirbridge.opt


Note: Ignore error message regarding missing language packages (temporary problem; TerminologyProvider).

Refresh project in the development environment

  * New classes (example breathing rate):
    * Directory / fhir-bridge / src / main / java / org / ehrbase / fhirbridge / opt / breathing frequency composition
    * Breath rateComposition.java and Breath rateCompositionContainment.java classes
    * Directory / fhir-bridge / src / main / java / org / ehrbase / fhirbridge / opt / breath frequency composition / definition
    * Respiratory RateObservation.java and Respiratory RateObservationContainment.java classes


Implement mapping (example breathing rate)
------------------------------------------

Create a new class in /fhir-bridge/src/main/java/org/ehrbase/fhirbridge/mapping

  * Example FHIRObservationRespRateOpenehrRespRate.java
    (The example is here FHIRObservationRespRateOpenehrRespRate.java.txt)


CreateObservation
-----------------

In /fhir-bridge/src/main/java/org/ehrbase/fhirbridge/fhir/provider.ObservationResourceProvider.java
add an else branch in the createObservation method (Orientation using the example of BodyTemperature;
copy, rename and adapt the content of the else branch)

To do this, create the appropriate method in /fhir-bridge/src/main/java/org/ehrbase/fhirbridge/rest/EhrbaseService.java
(Orientation on the example of saveTemp; copy, rename and adapt content)
[temporary approach; in the future there should be a generic method]


Unit test
---------

Adjust the name of the file in /fhir-bridge/src/test/resources/Observation/observation-resprate-example.json
add a test in /fhir-bridge/src/test/java/org/ehrbase/fhirbridge/FhirBridgeApplicationIT.java

  * Example createObservationRespRate (Orientation on the example createBodyTemp; copy, rename and adapt the test)


Upload templates (POSTMAN)
--------------------------

Download these files: https://github.com/ehrbase/documentation/tree/master/examples

Import the environment and the collection into POSTMAN

  * to do this, find and use the various import buttons individually
  * select a post entry under Collections-> EHRbase copy copy-> Templates (any), right-click, duplicate in the context menu
  * rename to Create [opt-name] Template to create your own OPT file
  * above change the url to localhost:8080/ehrbase/rest/openehr/v1/definition/template/adl1.4
  * replace the existing content in the body with the content of your own / to be mapped OPT file
  * upload the template using SEND (Docker should be running;))


Test whether the template is available in Ehrbase
-------------------------------------------------

Swagger UI http://localhost:8080/ehrbase/swagger-ui.html -> Templates

  * GET (/rest/openehr/v1/definition/template/adl1.4) -> Try it out -> Execute
  * Response Body -> an item (example breathing rate) should be in the ArrayList
  * Response code 200 -> successful