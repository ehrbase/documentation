Directory Endpoint
------------------

.. warning:: WIP

The ``DefaultRestClient`` includes a ``DefaultRestDirectoryEndpoint``.
But unlike the other endpoints the directory handling utilizes a different paradigm.
The ``FolderDAO`` is used to keep an active record, 
i.e. a Java object constantly synced with the database entry.

Folder
^^^^^^

To start working with a folder the directory object and its root folder can be retrieved with:

.. code-block:: java

    UUID ehr = openEhrClient.ehrEndpoint().createEhr();
    FolderDAO root = openEhrClient.folder(ehr, "");

With the ``FolderDAO`` at hand the following operations are available:

* Getting and setting the name
* Listing all sub folders
* Getting a sub folder, which will be created, if it not exists already
* Adding compositions to the folder, which includes committing the composition to the backend
* Finding of compositions in the folder structure, i.e. the matching EHR context

Together an example might be:

.. code-block:: java

    UUID ehr = openEhrClient.ehrEndpoint().createEhr();

    FolderDAO root = openEhrClient.folder(ehr, "");

    FolderDAO visit = root.getSubFolder("case1/visit1");

    EhrbaseBloodPressureSimpleDeV0Composition bloodPressureSimpleDeV01 = TestData.buildEhrbaseBloodPressureSimpleDeV0();
    visit.addCompositionEntity(bloodPressureSimpleDeV01);

    EhrbaseBloodPressureSimpleDeV0Composition bloodPressureSimpleDeV02 = TestData.buildEhrbaseBloodPressureSimpleDeV0();
    visit.addCompositionEntity(bloodPressureSimpleDeV02);

    List<EhrbaseBloodPressureSimpleDeV0Composition> actual = visit.find(EhrbaseBloodPressureSimpleDeV0Composition.class);
    assertThat(actual).size().isEqualTo(2);