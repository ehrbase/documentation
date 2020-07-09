Composition Endpoint
--------------------

.. warning:: WIP

The ``DefaultRestClient`` includes a ``DefaultRestCompositionEndpoint`` 
with the following functionalities.

Commit composition
^^^^^^^^^^^^^^^^^^

Committing a composition is done using the ``mergeCompositionEntity`` method.
Its idea is that the client already works with an instance of the composition 
(of a generated template entity type, like described :ref:`here <sdk-reference-generator_module-usage-label>`).

For example, a new instance could be created and modified to contain two values entered via a user interface.
This object now might only contain those two values, and a representation of the structure of the composition (as generated type/class definition).

.. note:: Despite that this simplified example only mentions two values, it is still necessary to provide data to all fields,
    which are required as per openEHR Reference Model. 
    Otherwise the backend server might reject the payload as invalid.

Afterwards, calling the ``mergeCompositionEntity`` method would commit the composition to the server.
Before returning data, this method also processes the server response to enrich the given composition object.
Specifically, server-side created data values like the composition's Version Uid are written into the object.

Therefore, and after successful execution, the method returns a representation of the composition as it is now persisting on the backend server.

A very simplified example could look like:

.. code-block:: java

    // Initializing the object
    EhrbaseBloodPressureSimpleDeV0Composition composition = new EhrbaseBloodPressureSimpleDeV0Composition();
    
    // Adding values
    [...].setSystolicMagnitude(120.0);
    [...].setSystolicUnits("mm[Hg]");
    [...].setDiastolicMagnitude(120.0);
    [...].setDiastolicUnits("mm[Hg]");

    // Committing and altering the object with the response data
    client.compositionEndpoint(ehrId).mergeCompositionEntity(composition);

    // For instance, the specific Version UID can now be accessed
    composition.getVersionUid();

Find composition
^^^^^^^^^^^^^^^^

To retrieve the latest version of a specific composition - 
or to get response that allows to understand that no such composition exists -
the ``find`` method can be used.

The usage is illustrated in the following example:

.. code-block:: java

    UUID compositionId = $COMPOSITION_ID;
    Optional<EhrbaseBloodPressureSimpleDeV0Composition> compo = compositionEndpoint
        .find(compositionId, EhrbaseBloodPressureSimpleDeV0Composition.class);