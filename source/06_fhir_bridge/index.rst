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

Create new branch
^^^^^^^^^^^^^^^^^

Each change to the FHIR bridge should have a ticket created, explaining the change. Create a new feature branch with
ticket number like: :code:`feature/123_awesome_new_feature`, where :code:`123` stands for the issue number::
    cd fhir-bridge
    git checkout develop
    git pull
    git checkout -b [BRANCH-NAME]
    # At the first push:
    git push -u origin [BRANCH-NAME]
    # For later pushes:
    git push


Comment out failing tests (temporary problem only)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Open :code:`/fhir-bridge/src/test/java/org/ehrbase/fhirbridge/FhirBridgeApplicationTestIT.java` in the editor
* Comment out the :code:`createConditionUsingInvalidProfile` and :code:`createObservationUsingUnsupportedProfile` test

Start docker
^^^^^^^^^^^^

Start docker and the ehrdb if it not already runs


Build 
^^^^^

Build the current fhir-bridge::

    cd fhir-bridge
    mvn clean install

IDE
^^^

Load project into development environment

  * especially for eclipse: as a Maven project


Add external files
^^^^^^^^^^^^^^^^^^

The following files must be copied into the respective target directories.

* FHIR data structure (XML format)

  * Target directory :code:`fhir-bridge/src/main/resources/profiles`
  * Source https://simplifier.net/ForschungsnetzCovid-19 (under Resources/Observation)
  * Example https://simplifier.net/ForschungsnetzCovid-19/RespiratoryRate/~xml (example)
  
* FHIR observation sample file (JSON format)

  * Target directory :code:`fhir-bridge/src/test/resources/Observation`
  * Source http://hl7.org/fhir/R4/observation-examples.html
  * Example http://hl7.org/fhir/R4/observation-example-respiratory-rate.json.html
  
* Operational template (OPT format)

  * Target directory :code:`fhir-bridge/src/main/resources/opt`
  * Source http://88.198.146.13/ckm/projects/1246.152.26/resourcecentre (GECCO Core)
  * Example (The example files are listed in the overview)
  
* Remember to check the downloaded files for content and syntax errors.

* Here you can check your syntax

  * Check for xml: https://xmllint.com/en
  * Check for json: https://jsonlint.com/


Structure Definition (Enum)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Add an entry with the FHIR URL to :code:`src/main/java/org/ehrbase/fhirbridge/fhir/Profile.java`
 
  * Source http://hl7.org/fhir/R4/observation-vitalsigns.html (search from list)
  * Example :code:`RESPIRATORY_RATE ("http://hl7.org/fhir/StructureDefinition/resprate", ResourceType.Observation)`
  
* Add an entry in :code:`src/main/java/org/ehrbase/fhirbridge/config/util/OperationalTemplateData.java`
  
  * Example :code:`HEART_RATE("", "Herzfrequenz.opt", "Herzfrequenz"),`


Use the SDK generator to create new classes from the operational template
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* (Windows example started from the path :code:``../openEHR_SDK/generator/target`)::

    java
    -jar generator-0.3.7.jar
    -opt ../../../fhir-bridge/src/main/resources/opt/Atemfrequenz.opt
    -out ../../../fhir-bridge/src/main/java
    -package org.ehrbase.fhirbridge.opt

* Linux example with path information from the perspective of the home directory)::

    java
    -jar ~ / Desktop / nfn / openEHR_SDK / generator / target / generator-0.3.7.jar
    -opt ~ / Desktop / nfn / 2020-08-28_fhir-bridge / src / main / resources / opt / body size.opt
    -out ~ / Desktop / nfn / 2020-08-28_fhir-bridge / src / main / java /
    -package org.ehrbase.fhirbridge.opt


* Note: Ignore error message regarding missing language packages (temporary problem; TerminologyProvider).

* Refresh project in the development environment

* New classes and structures (example breathing rate)

.. code-block:: none

   $ fhir-bridge/src/main/java/org/ehrbase/fhirbridge/opt/breathingfrequencycomposition
   ├── BreathrateComposition.java
   ├── BreathrateCompositionContainment.java
   ├── definition
       ├── RespiratoryRateObservation.java
       └── RespiratoryRateObservationContainment.java
       

Implement mapping (example breathing rate)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Create a new class in :code:`/fhir-bridge/src/main/java/org/ehrbase/fhirbridge/mapping`
  * :code:`FHIRObservation[MAPPING]Openehr[MAPPING].java`
  * Example :code:`FHIRObservationRespRateOpenehrRespRate.java`
* The information which entries have to be mapped check the template in HighMed CKM and the excel file from Sarah



CreateObservation
^^^^^^^^^^^^^^^^^

Temporarily and initial
"""""""""""""""""""""""

* In :code:`/fhir-bridge/src/main/java/org/ehrbase/fhirbridge/rest/EhrbaseService.java` create method :code:`save[MAPPING]`
* Therefore copy and adapt :code:`saveTemp` 

Regularly
"""""""""

* In :code:`fhir-bridge/src/main/java/org/ehrbase/fhirbridge/fhir/provider/ObservationResourceProvider.java`
* Add an :code:`else` branch in the :code:`createObservation` method (
* Therefore copy and adapt the :code:`BodyTemperature` example


Unit test
---------

* Adapt name of the file in json-Example :code:`/fhir-bridge/src/test/resources/Observation/observation-resprate-example.json`
* Add a test in :code:`/fhir-bridge/src/test/java/org/ehrbase/fhirbridge/FhirBridgeApplicationIT.java`

  * Therefore copy and adapt the :code:`createBodyTemp` example
  
* Adapt the patient-id in the json-example file::

    "subject": {
    "reference": "Patient/07f602e0-579e-4fe3-95af-381728bf0d49"
    },

Upload templates (POSTMAN)
--------------------------

* Download these files: https://github.com/ehrbase/documentation/tree/master/examples

* Import the environment and the collection into POSTMAN

  * to do this, find and use the various import buttons individually
  * select a post entry under Collections-> EHRbase copy copy-> Templates (any), right-click, duplicate in the context menu
  * rename to Create [opt-name] Template to create your own OPT file
  * above change the url to :code:`localhost:8080/ehrbase/rest/openehr/v1/definition/template/adl1.4`
  * replace the existing content in the body with the content of your own / to be mapped OPT file
  * upload the template using SEND (Docker should be running;))


Test whether the template is available in ehrbase
-------------------------------------------------

* Is Site available? Swagger UI http://localhost:8080/ehrbase/swagger-ui.html 
* Is your template visible?

  * GET (/rest/openehr/v1/definition/template/adl1.4) -> Try it out -> Execute
  * Response Body -> an item (example breathing rate) should be in the ArrayList
  * Response code 200 -> successful
  
Reintegrate changes into develop
---------------------------------------

* Check that all your tests run without any failure and you do not have uncommited changes
* Reintegrate the develop branch before sending your pull request::

    git checkout develop
    git pull
    git checkout feature/...
    git merge develop

* Do your tests still run without failure? If necessary, then ::

    git add .
    git commit -m "YOUR MESSAGE"
    git push

* Start a pull-request https://github.com/ehrbase/fhir-bridge/branches  and assign a reviewer

   
