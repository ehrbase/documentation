Contribution Endpoint
---------------------

.. warning:: WIP

For managing changes to the repository requires version control mechanism and change-set mechanisms, known as a contribution.  Each contribution references a set of one or more Versions of any of the versioned items in the record that were committed. ``Client currently support create/update/delete composition change types.``  Any Composition change requires a contribution creation. To start working with a contribution we need a composition.

The ``DefaultRestClient`` includes a ``DefaultRestContributionEndpoint`` 
with the following functionalities.

Create contribution using ContributionBuilder
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Saving a contribution is done using the ``saveContribution`` method.

.. note:: Please provide data to all fields, which are required as per openEHR Reference Model. 
    Otherwise the backend server might reject the payload as invalid.

Create Contribution with addition of new Composition
""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: java

    // Composition created
    Composition composition = [..]
    
    // Audit details created 
    AuditDetails audit = createAuditDetails();
    
    // Contrubution created with Composition addition change type
     ContributionBuilder contribution = ContributionBuilder.builder(audit)
                    .addCompositionCreation(composition)
                    .build();
    
    // Contribution Saved
    VersionUid contributionEntity = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);


Create Contribution  with modification of Composition
"""""""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: java

    // Composition modification
    Composition composition = [..]
    
    // Audit details created 
    AuditDetails audit = createAuditDetails();
    
    // Contrubution created with Composition modification change type and containing VersionUid
     ContributionBuilder contribution = ContributionBuilder.builder(audit)
                    .addCompositionModification(composition)
                    .build();
    
    // Contribution Saved
    VersionUid contributionEntity = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);
    
Composition modification audit using precedent version uid.
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

.. code-block:: java

    //Precedent version uid
    String precedentVersionUid = [..]
    
    // Audit details created 
    AuditDetails audit = createAuditDetails();

    // Contrubution created with Composition modification change type and Precedent version uid
     ContributionBuilder contribution = ContributionBuilder.builder(audit)
                    .addCompositionModification(composition, precedentVersionUid)
                    .build();
    
    // Contribution Saved
    VersionUid contributionEntity = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);


Create Contribution with deletion of Composition
""""""""""""""""""""""""""""""""""""""""""""""""

.. code-block:: java

    //Any precedent composition
    Composition composition = [..]

    //Retrieve precedent composition version uid 
    String compositionPrecedingVersionUid = composition.getVersionUid().toString()
    
    // Audit details created 
    AuditDetails audit = createAuditDetails();
    
    // Contrubution created with Composition deletion change type. Contrubution can be removed by providing precedent version uid.
    ContributionBuilder contribution = ContributionBuilder.builder(audit)
                    .addCompositionDeletion(compositionPrecedingVersionUid)
                    .build();
    
    // Contribution Saved
    VersionUid contributionEntity = openEhrClient.contributionEndpoint(ehr).saveContribution(contribution);

Create contribution using Contribution Dto
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: Please provide data to all fields, which are required as per openEHR Reference Model. 
    Otherwise the backend server might reject the payload as invalid.

.. code-block:: java

    // Composition created
    Composition composition = [..]
    
    // Audit details created 
    AuditDetails contributionAudit = createAuditDetails();
    
    // OriginalVersion must contain AuditDetails and can contain compositions depends on change type
    List<OriginalVersion> originalVersions = [..]
    
    // Contribution dto is created
    ContributionCreateDto contributionDto = new ContributionCreateDto();
    contributionCreateDto.setAudit(contributionAudit);
    contributionCreateDto.setVersions(originalVersions);
    
    // Contribution Saved
    VersionUid contributionEntity = openEhrClient.contributionEndpoint(ehr).saveContribution(contributionDto);

Find contribution
^^^^^^^^^^^^^^^^^

To retrieve the latest version of a specific contribution - or to get response that allows to understand that no such contribution exists - the ``find`` method can be used.

.. code-block:: java

    UUID contributionId = $CONTRIBUTION_ID;
    Optional<Contribution> contribution = openEhrClient.contributionEndpoint(ehr).find(contributionId);
