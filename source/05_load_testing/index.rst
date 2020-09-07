.. _load_testing:

============
Load Testing
============

This section describes the load testing process used to test EHRBASE.

Testehr
-------

Testehr is a Groovy script used to bomb any openEHR compliant server, following this flow:

- user provides parameters

  - ehrs: number of EHRs to be created
  - template: valid operational template
  - compositions: number of COMPOSITIONs to be committed
  - aql: valid AQL query, associated with the given template
  - scaleTemplates: optional, number of templates to use

Pseudo-code:

.. code-block:: javascript

    for (i in 1..scaleTemplates)
    {
        ehrsCreated = []

        testTemplate = copyAndChangeId(template)

        if (!server.templateExists(testTemplate))
        {
            server.templateUpload(testTemplate)
        }

        for (j in 1..ehrs)
        {
            ehrsCreated << server.createEhr(...)
        }

        commitTime = getTime()
        for (k in 1..compositions)
        {
            compo = generateComposition(testTemplate)
            ehr = ehrsCreated.pick()
            server.commitComposition(ehr, compo)
        }
        commitTime = getTime() - commitTime // elapsed time

        aqlTimes = []
        for (n in 1.. repeatAql)
        {
            aqlTime = getTime()
            server.runAql(aql)
            aqlTime = getTime() - aqlTime // elapsed time
            aqlTimes << aqlTime
        }

        [aqlTimeMax, aqlTimeMin, aqlTimeAvg] = calculateMaxMinAvg(aqlTimes)
    }


For instance, if you provider these parameters:

- ehrs = 100
- compositions = 20
- scaleTemplates = 3
- repeatAql = 5

The script will do 3 loops over 3 different templates (it's the same template but has different ID),
will create 100 EHRs, and commit 20 COMPOSITIONs, distributed between the 100 EHRs (the ehrsCreated.pick() is random).
In general, you might want to provide compositions >> ehrs (much greater then). Then an AQL query will be executed
'repeatAql' times, and the max, min and avg execution times will be calculated.


Script execution
----------------

You need Gradle (https://gradle.org/) to be installed to build, package and run. This was tested with Gradle 6.4.1.

- $ gradle clean // clean the build
- $ gradle build // compiles
- $ gradle fatJar // packages generating a .jar file with all the dependencies (standalone)
- $ gradle run --args="-ehrs 100 -template src/main/resources/opts/LabResults1.opt -compositions 20 -aql src/main/resources/queries/LabResults1.json" // run the script

Note: gradle run will build the script.

If you built the fatJar, you can run it anywhere without gradle:

- $ java -jar build/libs/load-testehr-all.jar -ehrs 10 -template src/main/resources/opts/LabResults1.opt -compositions 20 -aql src/main/resources/queries/LabResults1.json


