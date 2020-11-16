=====
Flows
=====


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
