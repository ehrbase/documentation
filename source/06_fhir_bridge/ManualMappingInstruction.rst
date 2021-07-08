Do the mapping
==============

If not further mentioned, the paths are relative to
``fhir-bridge/src/main/java/org/ehrbase/fhirbridge`` ## Prepare setup

Create new branch
~~~~~~~~~~~~~~~~~

Each change to the FHIR bridge should have a ticket created, explaining
the change. Create a new feature branch with ticket number like:
``feature/123_awesome_new_feature``, where ``123`` stands for the issue
number

::

       # optional: make a new checkout
       git clone git@github.com:ehrbase/fhir-bridge.git
       # default:
       cd fhir-bridge
       git checkout -b [BRANCH-NAME]
       # At the first push:
       git push -u origin [BRANCH-NAME]
       # For later pushes:
       git push

Start docker
~~~~~~~~~~~~

Start ``docker``, the ``erhdb`` and the ``fhirdb`` if they don’t already
run. For example (Birgit 2020-10-16)

::

   docker rm ehrdb
   docker rm ehrbase
   docker run --name ehrdb --network ehrbase-net -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 ehrbase/ehrbase-postgres:latest
   docker run --name ehrbase --network ehrbase-net -d -p 8080:8080 -e DB_URL=jdbc:postgresql://ehrdb:5432/ehrbase -e DB_USER=ehrbase -e DB_PASS=ehrbase -e AUTH_TYPE=none -e SYSTEM_ALLOW_TEMPLATE_OVERWRITE=true -e SERVER_NODENAME=local.ehrbase.org ehrbase/ehrbase:0.14.0
   docker run -p 9999:5432 --name fhir-bridge -e POSTGRES_PASSWORD=fhir_bridge -e POSTGRES_USER=fhir_bridge -d postgres

Build
~~~~~

Build the current fhir-bridge

::

       mvn clean install

IDE
~~~

Load project into development environment

-  especially for eclipse: as a Maven project

Add external files
------------------

The following files must be copied into the respective target
directories.

-  FHIR data structure (XML format)

   -  Target directory ``fhir-bridge/src/main/resources/profiles``
   -  Source https://simplifier.net/ForschungsnetzCovid-19 (under
      Resources/Observation)

-  FHIR observation sample file (JSON format)

   -  Target directory ``fhir-bridge/src/test/resources/Observation``
   -  Source
      https://simplifier.net/ForschungsnetzCovid-19/~resources?fhirVersion=R4&sortBy=RankScore_desc
   -  You can set a filter to the Examples
   -  Caution:

      -  the profile must be a profile from simplifier-Covid 19, fitting
         to the profile you just downloaded
      -  the profile must have a working UUID (like
         ``subject: { reference: Patient/07f602e0-579e-4fe3-95af-381728bf0d49 }``)

-  Operational template (OPT format)

   -  Target directory ``fhir-bridge/src/main/resources/opt``
   -  Source
      http://88.198.146.13/ckm/projects/1246.152.26/resourcecentre
      (GECCO Core)
   -  Check the OPT in Pablos Tool: toolkit.cabolabs.com

-  Remember to check the downloaded files for content and syntax errors.

-  Here you can check your syntax

   -  Check for xml: https://xmllint.com/en
   -  Check for json: https://jsonlint.com/

Design the mapping
------------------

-  Design the mapping by looking for 1..1 in the Fhir-profile and the
   fields in the OPT.
-  Example
   https://drive.google.com/file/d/1naGVhto6efWfF2sDoO86pYTnaUiiMq3J/view?usp=sharing



Structure Definition (Enum)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  copy the Fhir-Url from ``resources/profiles/[TEMPLATE].opt`` to
   ``fhir/Profile.java``

::

   <StructureDefinition xmlns="http://hl7.org/fhir">
     <url value="https://www.netzwerk-universitaetsmedizin.de/fhir/StructureDefinition/body-height" />
     <name value="BodyHeight" />

-  if your FHIR-observation-sample-file contains an extension, add its
   Profile URL, too.

-  Add an entry in ``/config/util/OperationalTemplateData.java`` with
   the name of the opt-file and the template_id-value

::

   # Koerpergroesse.opt
   [...]
       <template_id>
           <value>Körpergröße</value>
       </template_id>

::

   HEART_RATE("", "Koerpergroesse.opt", "Körpergröße"),

Use the SDK generator to create new classes from the operational template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  (Windows example started from the path
   :code:\`\ ``../openEHR_SDK/generator/target``)

::

       java
       -jar generator-0.3.7.jar
       -opt ../../../fhir-bridge/src/main/resources/opt/Atemfrequenz.opt
       -out ../../../fhir-bridge/src/main/java
       -package org.ehrbase.fhirbridge.opt

-  Linux example (only resource-opt needs to be adapted)

::

     java -jar ../openEHR_SDK/generator/target/generator-0.3.7.jar -opt src/main/resources/opt/KlinischeFrailty.opt -out src/main/java/ -package org.ehrbase.fhirbridge.opt

-  Note: Ignore error message regarding missing language packages
   (temporary problem; TerminologyProvider).

-  Refresh project in the development environment

-  New classes and structures (example breathing rate)

::

      $ opt/breathingfrequencycomposition
      ├── BreathrateComposition.java
      ├── BreathrateCompositionContainment.java
      ├── definition
          ├── RespiratoryRateObservation.java
          └── RespiratoryRateObservationContainment.java
		  
		  Structure Definition (Enum)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  copy the Fhir-Url from ``resources/profiles/[TEMPLATE].opt`` to
   ``fhir/Profile.java``

::

   <StructureDefinition xmlns="http://hl7.org/fhir">
     <url value="https://www.netzwerk-universitaetsmedizin.de/fhir/StructureDefinition/body-height" />
     <name value="BodyHeight" />

-  if your FHIR-observation-sample-file contains an extension, add its
   Profile URL, too.

-  Add an entry in ``/config/util/OperationalTemplateData.java`` with
   the name of the opt-file and the template_id-value

::

   # Koerpergroesse.opt
   [...]
       <template_id>
           <value>Körpergröße</value>
       </template_id>

::

   HEART_RATE("", "Koerpergroesse.opt", "Körpergröße"),

Use the SDK generator to create new classes from the operational template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  (Windows example started from the path
   :code:\`\ ``../openEHR_SDK/generator/target``)

::

       java
       -jar generator-0.3.7.jar
       -opt ../../../fhir-bridge/src/main/resources/opt/Atemfrequenz.opt
       -out ../../../fhir-bridge/src/main/java
       -package org.ehrbase.fhirbridge.opt

-  Linux example (only resource-opt needs to be adapted)

::

     java -jar ../openEHR_SDK/generator/target/generator-0.3.7.jar -opt src/main/resources/opt/KlinischeFrailty.opt -out src/main/java/ -package org.ehrbase.fhirbridge.opt

-  Note: Ignore error message regarding missing language packages
   (temporary problem; TerminologyProvider).

-  Refresh project in the development environment

-  New classes and structures (example breathing rate)

::

      $ opt/breathingfrequencycomposition
      ├── BreathrateComposition.java
      ├── BreathrateCompositionContainment.java
      ├── definition
          ├── RespiratoryRateObservation.java
          └── RespiratoryRateObservationContainment.java


