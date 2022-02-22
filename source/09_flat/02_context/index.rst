Context Information
===================

.. warning:: WIP

To simplify the input, the flat formate offers the option to set context values  which set values in the rm-tree.

.. code-block:: javascript

 {
  "ctx/language": "de",
  "ctx/territory": "US",
  "ctx/time": "2021-04-01T12:40:31.418954+02:00",
  "ctx/composer_name": "Silvia Blake",
  "ctx/composer_id": "123",
  "ctx/id_namespace": "HOSPITAL-NS",
  "ctx/id_scheme": "HOSPITAL-NS",
  "ctx/work_flow_id|id": "567",
  "ctx/work_flow_id|id_scheme": "HOSPITAL-NS",
  "ctx/work_flow_id|namespace": "HOSPITAL-NS",
  "ctx/work_flow_id|type": "ORGANISATION",
  "ctx/participation_name:0": "Dr. Marcus Johnson",
  "ctx/participation_function:0": "requester",
  "ctx/participation_mode:0": "face-to-face communication",
  "ctx/participation_id:0": "199",
  "ctx/participation_identifiers:0": "issuer1::assigner1::id1::PERSON;issuer2::assigner2::id2::PERSON",
  "ctx/participation_name:1": "Lara Markham",
  "ctx/participation_function:1": "performer",
  "ctx/participation_id:1": "198",
  "ctx/participation_identifiers:1|issuer:0": "issuer3",
  "ctx/participation_identifiers:1|assigner:0": "assigner3",
  "ctx/participation_identifiers:1|id:0": "id3",
  "ctx/participation_identifiers:1|type:0": "PERSON",
  "ctx/participation_identifiers:1|issuer:1": "issuer4",
  "ctx/participation_identifiers:1|assigner:1": "assigner4",
  "ctx/participation_identifiers:1|id:1": "id4",
  "ctx/participation_identifiers:1|type:1": "PERSON",
  "ctx/health_care_facility|name": "Hospital",
  "ctx/health_care_facility|id": "9091",
  }


composer: composer_name; composer_id; composer_self
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/composer_name": "Silvia Blake",
  "ctx/composer_id": "123",
  "ctx/id_namespace": "HOSPITAL-NS",
  "ctx/id_scheme": "HOSPITAL-NS",
 }

.. code-block:: javascript

 {
  "ctx/composer_self": true,
  "ctx/composer_id": "123",
  "ctx/id_namespace": "HOSPITAL-NS",
  "ctx/id_scheme": "HOSPITAL-NS",
 }



* composer_name :  sets  composer to party identified and sets the name
* composer_self:   sets  composer to party_self
* composer_id: sets composer.external_ref.id.value and sets it to party identified unless composer_self is set to true

id_namespace, id_scheme
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/composer_id": "123",
  "ctx/id_namespace": "HOSPITAL-NS",
  "ctx/id_scheme": "HOSPITAL-NS",
 }

* id_namespace : default namespace for external references: OBJECT_REF.namespace
* id_scheme : default scheme for external references: OBJECT_REF.id.scheme

language, territory
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/language": "de",
  "ctx/territory": "US",
 }

* language : set the default language for ENTRY.language && COMPOSITION.language
* territory: set the default territory for COMPOSITION.territory

work_flow_id
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/work_flow_id|id": "567",
  "ctx/work_flow_id|id_scheme": "HOSPITAL-NS",
  "ctx/work_flow_id|namespace": "HOSPITAL-NS",
  "ctx/work_flow_id|type": "ORGANISATION",
 }

* set the default for ENTRY.workflowId
* work_flow_id|id_scheme can be left out  if ctx/id_scheme is set
* work_flow_id|namespace can be left out if ctx/namespace is set

participation
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/participation_name:0": "Dr. Marcus Johnson",
  "ctx/participation_function:0": "requester",
  "ctx/participation_mode:0": "face-to-face communication",
  "ctx/participation_id:0": "199",
  "ctx/participation_identifiers:0": "issuer1::assigner1::id1::PERSON;issuer2::assigner2::id2::PERSON",

  "ctx/participation_name:1": "Lara Markham",
  "ctx/participation_function:1": "performer",
  "ctx/participation_id:1": "198",
  "ctx/participation_identifiers:1|issuer:0": "issuer3",
  "ctx/participation_identifiers:1|assigner:0": "assigner3",
  "ctx/participation_identifiers:1|id:0": "id3",
  "ctx/participation_identifiers:1|type:0": "PERSON",
  "ctx/participation_identifiers:1|issuer:1": "issuer4",
  "ctx/participation_identifiers:1|assigner:1": "assigner4",
  "ctx/participation_identifiers:1|id:1": "id4",
  "ctx/participation_identifiers:1|type:1": "PERSON",
 }

* sets the default for EVENT_CONTEXT.participations && ENTRY.otherParticipations
* participation_identifiers can be set in a compact or non compat form.

health_care_facility
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/health_care_facility|name": "Hospital",
  "ctx/health_care_facility|id": "9091",
  "ctx/id_namespace": "HOSPITAL-NS",
  "ctx/id_scheme": "HOSPITAL-NS",
 }

set the default for COMPOSITION.context.healthCareFacility

time
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/time": "2021-04-01T12:40:31.418954+02:00",
 }

* set the default time for ACTION.time, COMPOSITION.context.startTime, OBSERVATION.history.orgin, EVENT.time
* ctx/time will be set to now() if not set explicitly

end_time
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/end_time": "2021-05-01T12:40:31.418954+02:00",
 }

* set the default time COMPOSITION.context.endTime

history_origin
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/history_origin": "2021-05-01T12:40:31.418954+02:00",
 }

* set the default time for OBSERVATION.history.orgin


action_time
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/action_time": "2021-05-01T12:40:31.418954+02:00",
 }

* set the default time for ACTION.time

activity_timing
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/activity_timing": "R4/2022-01-31T10:00:00+01:00/P3M",
 }

* set the default for ACTIVITY.timing

provider
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/provider_name": "Silvia Blake",
  "ctx/provider_id": "123",
  "ctx/id_namespace": "HOSPITAL-NS",
  "ctx/id_scheme": "HOSPITAL-NS",
 }

* set the default PARTY_IDENTIFIED for ENTRY.provider

action_ism_transition_current_state
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/action_ism_transition_current_state": "completed",
 }

 .. code-block:: javascript

 {
  "ctx/action_ism_transition_current_state": "532",
 }


* set the default  for ACTION.ismTransition.currentState
* either value or code is acepted

instruction_narrative
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/instruction_narrative": "Human readable instruction narrative",
 }

* set the default for INSTRUCTION.narrative

location
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/location": "Lab B2",
 }

* set the default for COMPOSITION.context.location

setting
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/setting": "other care",
 }

 .. code-block:: javascript

 {
  "ctx/setting": "238",
 }


* set the default for COMPOSITION.context.setting
* either value or code is acepted
* ctx/setting will be set to "other care" if not set explicitly  

link
--------------------------------------------------------

.. code-block:: javascript

 {
  "ctx/link:0|type": "problem",
  "ctx/link:0|meaning": "problem related note",
  "ctx/link:0|target": "ehr://ehr.network/347a5490-55ee-4da9-b91a-9bba710f730e",
 }

* set the default for LOCATABLE.links