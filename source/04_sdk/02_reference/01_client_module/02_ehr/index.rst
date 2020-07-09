EHR Endpoint
------------

.. warning:: WIP

The ``DefaultRestClient`` includes a ``DefaultRestEhrEndpoint`` 
with the following functionalities.

Create an EHR
^^^^^^^^^^^^^

The creation of a new EHR is as simple as calling:

.. code-block:: java

    UUID ehr = openEhrClient.ehrEndpoint().createEhr();

.. note:: The option to add a custom EHR Status object at this step is already on the road map.

Get the EHR status
^^^^^^^^^^^^^^^^^^

Retrieval of the EHR Status of a given EHR is done with a call like:

.. code-block:: java

    Optional<EhrStatus> ehrStatus = openEhrClient.ehrEndpoint().getEhrStatus(ehrId);

Update the EHR status
^^^^^^^^^^^^^^^^^^^^^

Updating works in the same way and might be used like in the following:

.. code-block:: java

    // Retrieval and modification of the Status
    ...
    // Followed by updating it
    openEhrClient.ehrEndpoint().updateEhrStatus(ehrId, ehrStatus);