

Extending FHIR bridge
---------------------

Prepare setup
^^^^^^^^^^^^^^^^^

Create new branch
"""""""""""""""""

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
"""""""""""""""""""""""""""""""""""""""""""""""""""

* Open :code:`/fhir-bridge/src/test/java/org/ehrbase/fhirbridge/FhirBridgeApplicationTestIT.java` in the editor
* Comment out the :code:`createConditionUsingInvalidProfile` and :code:`createObservationUsingUnsupportedProfile` test

Start docker
""""""""""""

Start docker and the ehrdb if they don't already run.


Build 
""""""

Build the current fhir-bridge::

    cd fhir-bridge
    mvn clean install

IDE
"""

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
  * Source https://simplifier.net/ForschungsnetzCovid-19/~resources?fhirVersion=R4&sortBy=RankScore_desc
  * You can set a filter to the Examples
  * Caution: 
    * the profile must be a profile from simplifier-Covid 19, fitting to the profile you just downloaded
    * the profile must have a working UUID (like :code:`subject: { reference: Patient/07f602e0-579e-4fe3-95af-381728bf0d49 }`)

  
* Operational template (OPT format)

  * Target directory :code:`fhir-bridge/src/main/resources/opt`
  * Source http://88.198.146.13/ckm/projects/1246.152.26/resourcecentre (GECCO Core)
  * Example (The example files are listed in the overview)
  * Check the OPT in Pablos Tool:  toolkit.cabolabs.com 
  
* Remember to check the downloaded files for content and syntax errors.

* Here you can check your syntax

  * Check for xml: https://xmllint.com/en
  * Check for json: https://jsonlint.com/

Design the mapping
^^^^^^^^^^^^^^^^^^^^^^^^^^^
* Design the mapping by looking for 1..1 in the Fhir-profile and the fields in the OPT. 

* Example https://drive.google.com/file/d/1naGVhto6efWfF2sDoO86pYTnaUiiMq3J/view?usp=sharing

Do the mapping in code
^^^^^^^^^^^^^^^^^^^^^^

Structure Definition (Enum)
"""""""""""""""""""""""""""

* Add an entry with the FHIR URL to :code:`src/main/java/org/ehrbase/fhirbridge/fhir/Profile.java`
 
  * Source http://hl7.org/fhir/R4/observation-vitalsigns.html (search from list)
  * Example :code:`RESPIRATORY_RATE ("http://hl7.org/fhir/StructureDefinition/resprate", ResourceType.Observation)`
  
* Add an entry in :code:`src/main/java/org/ehrbase/fhirbridge/config/util/OperationalTemplateData.java`
  
  * Example :code:`HEART_RATE("", "Herzfrequenz.opt", "Herzfrequenz"),`


Use the SDK generator to create new classes from the operational template
"""""""""""""""""""""""""""

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
""""""""""""""""""""""""""""""""""""""""""

* Create a new class in :code:`/fhir-bridge/src/main/java/org/ehrbase/fhirbridge/mapping`
  * :code:`FHIRObservation[MAPPING]Openehr[MAPPING].java`
  * Example :code:`FHIRObservationRespRateOpenehrRespRate.java`
  * Copy and adapt 06_fhir_bridge/FhirObservationRespRateOpenehrResprate-java.rst
* The information which entries have to be mapped check the template in HighMed CKM and the excel file from Sarah



CreateObservation
"""""""""""""""""""""""""""

**Temporarily and initial**


* In :code:`/fhir-bridge/src/main/java/org/ehrbase/fhirbridge/rest/EhrbaseService.java` create method :code:`save[MAPPING]`
* Therefore copy and adapt :code:`saveTemp` 

**Regularly**


* In :code:`fhir-bridge/src/main/java/org/ehrbase/fhirbridge/fhir/provider/ObservationResourceProvider.java`
* Add an :code:`else` branch in the :code:`createObservation` method (
* Therefore copy and adapt the :code:`BodyTemperature` example


Integration test
^^^^^^^^^^^^^^^^

* Adapt name of the file in json-Example :code:`/fhir-bridge/src/test/resources/Observation/observation-resprate-example.json`
* Add a test in :code:`/fhir-bridge/src/test/java/org/ehrbase/fhirbridge/FhirBridgeApplicationIT.java` 

  * Therefore copy and adapt the :code:`createBodyTemp` example in the bottom of the file (for merging reasons)
  

Upload templates (POSTMAN)
^^^^^^^^^^^^^^^^^^^^^^^^^^

* Download these files: https://github.com/ehrbase/documentation/tree/master/examples

* Import the environment and the collection into POSTMAN

  * to do this, find and use the various import buttons individually
  * select a post entry under Collections-> EHRbase copy copy-> Templates (any), right-click, duplicate in the context menu
  * rename to Create [opt-name] Template to create your own OPT file
  * above change the url to :code:`localhost:8080/ehrbase/rest/openehr/v1/definition/template/adl1.4`
  * replace the existing content in the body with the content of your own / to be mapped OPT file
  * upload the template using SEND (Docker should be running;))


Test whether the template is available in ehrbase
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Is Site available? Swagger UI http://localhost:8080/ehrbase/swagger-ui.html 
* Is your template visible?

  * GET (/rest/openehr/v1/definition/template/adl1.4) -> Try it out -> Execute
  * Response Body -> an item (example breathing rate) should be in the ArrayList
  * Response code 200 -> successful
  
Reintegrate changes into develop
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

* Start a pull-request https://github.com/ehrbase/fhir-bridge/branches, assign a reviewer and coordinate a review call if needed

   
