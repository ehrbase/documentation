AQL Endpoint
------------

.. warning:: WIP

The ``DefaultRestClient`` includes a ``DefaultRestAqlEndpoint`` 
with the following functionalities.

Query execution
^^^^^^^^^^^^^^^

The execution is the only function of this endpoint.
It supports two types of query implementations, both based on a common definition.

Abstract query
""""""""""""""

The basic querying concept is inspired by libraries and tools like
`jOOQ <https://www.jooq.org/>`_ and therefore focuses on typesafe AQL, 
based on generated entity code and record handling.

The following examples illustrates that with a simple native AQL query. 
All types of ``Query`` and their detailed usage will be discussed below.

.. code-block:: java

    private List<UUID> queryAllEhrs() {
        Query<Record1<UUID>> query = Query.buildNativeQuery("SELECT e/ehr_id/value FROM EHR e", UUID.class);
        List<Record1<UUID>> result = client.aqlEndpoint().execute(query);

        List<UUID> ehrs = new ArrayList<>();
        result.forEach(r -> ehrs.add(r.value1()));
        return ehrs;
    }

In this example a query is created. 
Its return type is already embedding a ``Record``, which is defined depending on the amount and type of the result values.
Here, the query has one `select value`. 
The response will only have `one` column of type ``UUID``, 
so together the response is defined as ``Record1<UUID>``.

After execution the result is wrapped in a ``List``, because it can have none or many result values (rows).
Accessing the values is type safe, since the record was already defined with the specific type.

Records are supported up to 21 result types (``Record21``). 
There are no restrictions on which types can be used.

Native query
""""""""""""

A native query can be build with the input of a string representation
of a native AQL query. For instance, ``SELECT e/ehr_id/value FROM EHR e``.

Additionally, parameters can be used and added as Java variables.
They need to be formatted like ``$ehr_id`` in the native query string.
Setting its value is done using the ``ParameterValue`` class.
See the following example:

.. code-block:: java

    Query<Record2<VersionUid, TemporalAccessor>> query = Query.buildNativeQuery(
        "select  a/uid/value, a/context/start_time/value from EHR e[ehr_id/value = $ehr_id]  
            contains COMPOSITION a [openEHR-EHR-COMPOSITION.sample_encounter.v1]",
        VersionUid.class, TemporalAccessor.class);

        List<Record2<VersionUid, TemporalAccessor>> result = openEhrClient.aqlEndpoint().execute(
            query, new ParameterValue("ehr_id", ehr));


Entity query
""""""""""""

In contrast, entity queries make use of the entities generator by the Generator
(see :ref:`here <sdk-reference-generator_module-usage-label>`).
Therefore, is it not necessary to manually design and implement a custom native AQL query.

The generator creates Java POJO classes of the composition, but also containment classes that can be used in a query.
In the following example a simple exemplary blood pressure type is assumed to be generated already.

.. code-block:: java

    EhrbaseBloodPressureSimpleDeV0CompositionContainment containmentComposition = EhrbaseBloodPressureSimpleDeV0CompositionContainment.getInstance();
    BloodPressureTrainingSampleObservationContainment containmentObservation = BloodPressureTrainingSampleObservationContainment.getInstance();

    containmentComposition.setContains(containmentObservation);

    EntityQuery<Record3<TemporalAccessor, BloodPressureTrainingSampleObservation, CuffSizeDefiningcode>> entityQuery = Query.buildEntityQuery(
            containmentComposition,
            containmentComposition.START_TIME_VALUE,
            containmentObservation.BLOOD_PRESSURE_TRAINING_SAMPLE_OBSERVATION,
            containmentObservation.CUFF_SIZE_DEFININGCODE
    );
    // plus some conditions like WHERE, see below

This snippet shows two things:

First, the usage of the containment classes to define parts of the query (like select values).
The internal query handling in ``buildEntityQuery`` will create a correct native AQL query from the input.
This makes AQL querying much easier, because it removed the burden of being an expert on the query language.

Second, next to being able to retrieve simple types, this query also shows
automatic parsing and conversation to complex result types like an openEHR `Observation`.
Specifically, a ``BloodPressureTrainingSampleObservation`` type is directly available for further processing, after the query was executed.

TODO: Where and other conditions