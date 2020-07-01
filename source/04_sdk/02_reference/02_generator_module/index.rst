Generator module
=============

.. warning:: WIP

Reference documentation of the generator module.

.. _sdk-reference-generator_module-usage-label:

Usage
-----

The generator can be used to create Java classes representing a given openEHR template.

After locally building the SDK with ``mvn clean install`` a generator .jar is available.

To generate and entity class from a template generally use:

.. code-block:: bash

    java  -jar generator-version.jar
    -h               show help
    -opt <arg>       path to opt file
    -out <arg>       path to output directory
    -package <arg>   package name

In a custom use case the generation could look like:

.. code-block:: bash

    java -jar generator/target/generator-$VERSION.jar -opt ../$PATH_TO_TEMPLATE/ehrbase_blood_pressure_simple.de.v0.opt -out ../$OUTPUT_PROJECT/src/main/java -package org.$OUTPUT_PACKAGES.opt