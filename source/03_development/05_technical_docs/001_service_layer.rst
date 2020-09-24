Service Layer
=============

.. warning:: WIP

General
-------

The service layer of EHRbase is composed of ...

openEHR Platform Abstract Service Model
---------------------------------------

Based on the `openEHR Platform Abstract Service Model <https://specifications.openehr.org/releases/SM/latest/openehr_platform.html>`_
the following check list is build to give an overview and document the current
state.
Each service component has a table documenting the current state of

* implementation of the *method* itself, if applicable
* implementation and utilization of the *pre checks* of the method, if applicable
* implementation and utilization of the *post checks* of the method, if applicable

**Services**

.. contents::
    :local:

EHR
^^^

For more details see
`I_EHR_SERVICE <https://specifications.openehr.org/releases/SM/latest/openehr_platform.html#_i_ehr_service_interface>`_
in the official documentation.

.. csv-table::
   :header: "Method", "Implemented", "Pre", "Post"

        has_ehr, Yes, /, /
        has_ehr_for_subject, I, /, /
        create_ehr, C, /, No
        create_ehr_with_id, C, No, No
        create_ehr_for_subject, No, /, /
        create_ehr_for_subject_with_id, No, No, /
        get_ehr, No, No, /
        get_ehrs_for_subject, No, /, /
        i_ehr, No, /, /

Methods with **I** note are currently indirectly implemented. Their
functionality is available, but the general signature might
be different.

Methods with **C** note are currently combined in a more general `createEhr`
method.

EHR_STATUS
^^^^^^^^^^

For more details see
`I_EHR_STATUS <https://specifications.openehr.org/releases/SM/latest/openehr_platform.html#_i_ehr_status_interface>`_
the in official documentation.

.. csv-table::
   :header: "Method", "Implemented", "Pre", "Post"

        has_ehr_status_version, I, Yes, /
        get_ehr_status, Yes, Yes, /
        get_ehr_status_at_time, I, Yes, /
        set_ehr_queryable, C, No, No
        set_ehr_modifiable, C, No, No
        clear_ehr_queryable, C, No, No
        clear_ehr_modifiable, C, No, No
        update_other_details, C, /, /
        get_ehr_status_at_version, Yes, Yes, /
        get_versioned_ehr_status, No, No, No

Methods with **I** note are currently indirectly implemented. Their
functionality is available, but the general signature might
be different.

Methods with **C** note are currently combined in a more general `updateStatus`
method.

DIRECTORY
^^^^^^^^^

For more details see
`I_EHR_DIRECTORY <https://specifications.openehr.org/releases/SM/latest/openehr_platform.html#_i_ehr_directory_interface>`_
the in official documentation.

.. csv-table::
   :header: "Method", "Implemented", "Pre", "Post"

        has_directory
        has_path
        create_directory
        get_directory
        get_directory_at_time
        update_directory
        delete_directory
        has_directory_version
        get_directory_at_version
        get_versioned_directory

COMPOSITION
^^^^^^^^^^^

For more details see
`I_EHR_COMPOSITION <https://specifications.openehr.org/releases/SM/latest/openehr_platform.html#_i_ehr_composition_interface>`_
the in official documentation.

.. csv-table::
   :header: "Method", "Implemented", "Pre", "Post"

        has_composition
        get_composition_latest
        get_composition_at_time
        get_composition_at_version
        get_versioned_composition
        create_composition
        update_composition
        delete_composition

CONTRIBUTION
^^^^^^^^^^^^

For more details see
`I_EHR_CONTRIBUTION <https://specifications.openehr.org/releases/SM/latest/openehr_platform.html#_i_ehr_contribution_interface>`_
the in official documentation.

.. csv-table::
   :header: "Method", "Implemented", "Pre", "Post"

        has_contribution
        get_contribution
        commit_contribution
        list_contributions
        contribution_count
