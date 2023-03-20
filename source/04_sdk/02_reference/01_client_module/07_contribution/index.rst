Contribution Endpoint
---------------------

.. warning:: WIP


The ``DefaultRestClient`` includes a ``DefaultRestContributionEndpoint`` 
with the following functionalities.


Save contribution
^^^^^^^^^^^^^^^^^

Saving a contribution is done using the ``saveContribution`` method. Client accepts a ContributionCreateDto, which can be created manually or with the ``ContributionBuilder``. The creation of ``ContributionCreateDto`` explained below.

    A very simplified example of saving contribution:

.. code-block:: java

    ContributionCreateDto contribution = [..]

    // Contribution Saved
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);


Find contribution
^^^^^^^^^^^^^^^^^

To retrieve the latest version of a specific contribution - or to get response that allows to understand that no such contribution exists - the ``find`` method can be used.

.. code-block:: java

    UUID contributionId = $CONTRIBUTION_ID;
    Optional<Contribution> contribution = openEhrClient.contributionEndpoint(ehr).find(contributionId);


Create contribution using ``ContributionCreateDto``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``ContributionCreateDto`` class can be created by providing mandatory audit details and original versions. 

.. note:: Please provide data to all fields, which are required as per openEHR Reference Model. 
    Otherwise the backend server will reject the payload as invalid.

    A very simplified example of creating ``ContributionCreateDto``:

.. code-block:: java

    Composition composition = [..]
    
    // Create composition audit details 
    AuditDetails compositionAudit = [..]
    // Minimum required data for audit is setting change type see below
    compositionAudit.getChangeType().setValue(ContributionChangeType.CREATION.getName());
    compositionAudit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.CREATION.getCode()));
    
    // OriginalVersion must contain AuditDetails and can contain compositions depends on change type
    List<OriginalVersion> originalVersions = [..]
    originalVersion.setData(composition);
    originalVersion.setCommitAudit(compositionAudit);
    // Version uid must be provided in case of composition deletion and modification
    String versionUid = composition.getVersionUid().toString()
    originalVersion.setPrecedingVersionUid(new ObjectVersionId(versionUid));
    
    // Create contribution audit details 
    AuditDetails contributionAudit = [..]
    // Minimum required data for audit is setting change type see below
    contributionAudit.getChangeType().setValue(ContributionChangeType.CREATION.getName());
    contributionAudit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.CREATION.getCode()));
    
    // Create contribution dto
    ContributionCreateDto contribution = new ContributionCreateDto();
    contributionCreateDto.setAudit(contributionAudit);
    contributionCreateDto.setVersions(originalVersions);
    
    // Save contribution
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);


Create contribution using ``ContributionBuilder``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 Key objectives of the ``ContributionBuilder`` are to hide much of the complexity and make the contribution easy to create. To start working with a ``ContributionBuilder`` is needed an instance of the composition and audit details. ContributionBuilder currently supports composition and folders with ``create/update/delete`` change types. 

.. note:: Please provide data to all fields, which are required as per openEHR Reference Model. 
    Otherwise the backend server will reject the payload as invalid.
    

Create contribution with addition of new composition and save contribution 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: java

    // Composition must inlude audit details
    Composition composition = [..]
    
    // Create contribution audit details 
    AuditDetails audit = [..]
    // Minimum required data for audit is setting change type see below 
    audit.getChangeType().setValue(ContributionChangeType.CREATION.getName());
    audit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.CREATION.getCode()));
    
    // Create contribution with composition creation change type
    ContributionCreateDto contribution = ContributionBuilder.builder(audit)
                    .addCompositionCreation(composition)
                    .build();
    
    // Save contribution
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);


Create contribution with addition of new folder and save contribution 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: java

    // Folder must inlude audit details
    Folder folder = [..]
    
    // Create contribution audit details 
    AuditDetails audit = [..]
    // Minimum required data for audit is setting change type see below 
    audit.getChangeType().setValue(ContributionChangeType.CREATION.getName());
    audit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.CREATION.getCode()));
    
    // Create contribution with folder creation change type
    ContributionCreateDto contribution = ContributionBuilder.builder(audit)
                    .addFolderCreation(folder)
                    .build();
    
    // Save contribution
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);


Create contribution with modification of composition and save contribution 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: java

    // Composition must inlude audit details
    Composition composition = [..]
    
    // Create contribution audit details 
    AuditDetails audit = [..]
    // Minimum required data for audit is setting change type see below
    audit.getChangeType().setValue(ContributionChangeType.MODIFICATION.getName());
    audit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.MODIFICATION.getCode()));
    
    // Create contribution with composition modification change type and mandatory to contain version uid
     ContributionCreateDto contribution = ContributionBuilder.builder(audit)
                    .addCompositionModification(composition)
                    .build();
    
    // Save contribution 
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);
    
   
    
Create contribution with modification of composition using version uid and save contribution 
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: java

    // Composition must inlude audit details
    Composition composition = [..]

    // Retrieve composition version uid 
    String versionUid = composition.getVersionUid().toString()

    // Create contribution audit details 
    AuditDetails audit = [..]
    // Minimum required data for audit is setting change type see below
    audit.getChangeType().setValue(ContributionChangeType.CREATION.getName());
    audit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.CREATION.getCode()));

    // Create contribution with composition modification change type and version uid
     ContributionCreateDto contribution = ContributionBuilder.builder(audit)
                    .addCompositionModification(composition, versionUid)
                    .build();
    
    // Save contribution 
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);

    
Create contribution with modification of folder using version uid and save contribution 
''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: java

    // Folder must inlude audit details
    Folder folder = [..]

    // Retrieve folder version uid 
    String versionUid = folder.getVersionUid().toString()

    // Create contribution audit details 
    AuditDetails audit = [..]
    // Minimum required data for audit is setting change type see below
    audit.getChangeType().setValue(ContributionChangeType.CREATION.getName());
    audit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.CREATION.getCode()));

    // Create contribution with folder modification change type and version uid
     ContributionCreateDto contribution = ContributionBuilder.builder(audit)
                    .addFolderModification(versionUid)
                    .build();
    
    // Save contribution 
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);
    

Create contribution with deletion of composition save contribution 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: java

    // Composition must inlude audit details
    Composition composition = [..]

    // Retrieve composition version uid 
    String versionUid = composition.getVersionUid().toString()

    // Create contribution audit details 
    AuditDetails audit = [..]
    // Minimum required data for audit is setting change type see below
    audit.getChangeType().setValue(ContributionChangeType.DELETED.getName());
    audit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.DELETED.getCode()));

    // Create contribution with composition deletion change type
    ContributionCreateDto contribution = ContributionBuilder.builder(audit)
                    .addCompositionDeletion(versionUid)
                    .build();
    
    // Contribution Saved
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);

    

Create contribution with deletion of folder save contribution 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: java

    // Folder must inlude audit details
    Folder folder = [..]

    // Retrieve folder version uid 
    String versionUid = folder.getVersionUid().toString()

    // Create contribution audit details 
    AuditDetails audit = [..]
    // Minimum required data for audit is setting change type see below
    audit.getChangeType().setValue(ContributionChangeType.DELETED.getName());
    audit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.DELETED.getCode()));

    // Create contribution with folder deletion change type
    ContributionCreateDto contribution = ContributionBuilder.builder(audit)
                    .addFolderDeletion(folder, versionUid)
                    .build();
    
    // Contribution Saved
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);
    
    

Create contribution with various compositions and save contribution 
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Contribution may include any number of compositions with different change type see below an example

.. code-block:: java

    // Compositions must inlude audit details
    Composition firstComposition = [..]   
    Composition secondComposition = [..]
    
    // Create contribution audit details 
    AuditDetails audit = [..]
    // Minimum required data for audit is setting change type see below 
    audit.getChangeType().setValue(ContributionChangeType.CREATION.getName());
    audit.getChangeType().getDefiningCode().setCodeString(String.valueOf(ContributionChangeType.CREATION.getCode()));
    
    // Create contribution with composition creation change type
    ContributionCreateDto contribution = ContributionBuilder.builder(audit)
                    .addCompositionCreation(firstComposition)
                    .addCompositionModification(secondComposition)
                    .build();
    
    // Save contribution
    VersionUid versionUid = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);
    
