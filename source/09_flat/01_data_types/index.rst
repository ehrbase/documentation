Data Types
=================

DV_TEXT
-------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_text_class )

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text": "DV_TEXT value"
  } 
.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text": "DV_TEXT value",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text|formatting": "plain",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|code": "en",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|terminology": "ISO_639-1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|preferred_term": "English",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_encoding|code": "UTF-8",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_encoding|terminology": "IANA_character-sets",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0|match": "=",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/target|terminology": "SNOMED-CT",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/target|code": "21794005",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/purpose|terminology": "openehr",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/purpose|code": "671",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/purpose|value": "research study"
  } 

+--------------+----------------------+-------------+----------+-------+
| Flat Path    | Flat type            | RM Path     | Requerd  | Note  |
+==============+======================+=============+==========+=======+
|              | String               | value       | yes      |       |
+--------------+----------------------+-------------+----------+-------+
| \|formatting | String               | formatting  | no       |       |
+--------------+----------------------+-------------+----------+-------+
| /_language   | `CODE_PHRASE`_       | language    | no       |       |
+--------------+----------------------+-------------+----------+-------+
| /_encoding   | `CODE_PHRASE`_       | encoding    | no       |       |
+--------------+----------------------+-------------+----------+-------+
| /_mapping:i  | `TERM_MAPPING`_      | mappings    | no       |       |
+--------------+----------------------+-------------+----------+-------+


CODE_PHRASE
-----------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_code_phrase_class )


.. code-block:: javascript
 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|code": "en",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|terminology": "ISO_639-1",
  }
.. code-block:: javascript 
 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|code": "en",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|terminology": "ISO_639-1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|preferred_term": "English",
  } 

+-------------------+------------+-----------------------+----------+-------+
| Flat Path         | Flat type  | RM Path               | Requerd  | Note  |
+===================+============+=======================+==========+=======+
| \|code            | String     | code_string           | yes      |       |
+-------------------+------------+-----------------------+----------+-------+
| \|terminology     | String     | terminology_id.value  | yes      |       |
+-------------------+------------+-----------------------+----------+-------+
| \|preferred_term  | String     | preferred_term        | no       |       |
+-------------------+------------+-----------------------+----------+-------+


TERM_MAPPING
-------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_term_mapping_class )

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0|match": "=",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/target|terminology": "SNOMED-CT",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/target|code": "21794005",
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0|match": "=",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/target|terminology": "SNOMED-CT",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/target|code": "21794005",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/purpose|terminology": "openehr",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/purpose|code": "671",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_mapping:0/purpose|value": "research study"
  } 

+------------+----------------------+----------+----------+--------+
| Flat Path  | Flat type            | RM Path  | Requerd  | Note   |
+============+======================+==========+==========+========+
| \|match    | String               | match    | yes      |        |
+------------+----------------------+----------+----------+--------+
| /target    | `CODE_PHRASE`_       | target   | yes      |        |
+------------+----------------------+----------+----------+--------+
| /purpose   | `DV_CODED_TEXT`_     | purpose  | no       |        |
+------------+----------------------+----------+----------+--------+

DV_CODED_TEXT 
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_coded_text_class)
