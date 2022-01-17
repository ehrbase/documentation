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
|\|value       | String               | value       | yes      |       |
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
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|terminology": "ISO_639-1"
  }
.. code-block:: javascript 

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|code": "en",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|terminology": "ISO_639-1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_text/_language|preferred_term": "English"
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

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text|value": "term1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text|code": "at0006",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text|terminology": "local"
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text|value": "term1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text|code": "at0006",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text|terminology": "local",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text|formatting": "plain",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_language|code": "en",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_language|terminology": "ISO_639-1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_language|preferred_term": "English",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_encoding|code": "UTF-8",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_encoding|terminology": "IANA_character-sets",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_mapping:0|match": "=",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_mapping:0/target|terminology": "SNOMED-CT",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_mapping:0/target|code": "21794005",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_mapping:0/purpose|terminology": "openehr",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_mapping:0/purpose|code": "671",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_coded_text/_mapping:0/purpose|value": "research study"
  } 


+----------------+-----------------+-------------------------------------+----------+--------------------------------------------+
| Flat Path      | Flat type       | RM Path                             | Requerd  | Note                                       |
+================+=================+=====================================+==========+============================================+
| \|code         | String          | defining_code.code_string           | yes      |                                            |
+----------------+-----------------+-------------------------------------+----------+--------------------------------------------+
| \|value        | String          | value                               | (yes)    | only required for external  terminologies  |
+----------------+-----------------+-------------------------------------+----------+--------------------------------------------+
| \|terminology  | String          | defining_code.terminology_id.value  | (yes)    | only required for external  terminologies  |
+----------------+-----------------+-------------------------------------+----------+--------------------------------------------+
| \|formatting   | String          | formatting                          | no       |                                            |
+----------------+-----------------+-------------------------------------+----------+--------------------------------------------+
| /_language     | `CODE_PHRASE`_  | language                            | no       |                                            |
+----------------+-----------------+-------------------------------------+----------+--------------------------------------------+
| /_encoding     | `CODE_PHRASE`_  | encoding                            | no       |                                            |
+----------------+-----------------+-------------------------------------+----------+--------------------------------------------+
| /_mapping:i    | `TERM_MAPPING`_ | mappings                            | no       |                                            |
+----------------+-----------------+-------------------------------------+----------+--------------------------------------------+

DV_ORDINAL
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_ordinal_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal|code": "at0015",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal|value": "value1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal|ordinal": 1
  }

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal|code": "at0015",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal|value": "value1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal|ordinal": 1,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_normal_range/lower|code": "at0015",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_normal_range/lower|value": "value1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_normal_range/lower|ordinal": 1,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_normal_range/upper|code": "at0015",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_normal_range/upper|value": "value1",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_normal_range/upper|ordinal": 1,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_other_reference_ranges:0/lower|code": "at0016",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_other_reference_ranges:0/lower|value": "value2",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_other_reference_ranges:0/lower|ordinal": 2,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_other_reference_ranges:0|upper_unbounded": true,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_other_reference_ranges:0|upper_included": false,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ordinal/_other_reference_ranges:0/meaning": "high"
  }


+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+
| Flat Path                   | Flat type                        | RM Path                                | Requerd  | Note                                             |
+=============================+==================================+========================================+==========+==================================================+
| \|code                      | String                           | symbol.defining_code.code_string       | Yes      |                                                  |
+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+
| \|value                     | String                           | symbol.value                           | (Yes)    | my be left out if symbol is defined in template  |
+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+
| \|ordinal                   | Integer                          | value                                  | (Yes)    | my be left out if symbol is defined in template  |
+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+
| /_normal_range              | `DV_INTERVAL`_ <DV_ORDINAL>      | normal_range                           | no       |                                                  |
+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+
| /_other_reference_ranges:i  | `REFERENCE_RANGE`_ <DV_ORDINAL>  | _other_reference_ranges                | no       |                                                  |
+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+

DV_BOOLEAN 
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_boolean_class)

.. code-block:: javascript

 {
    "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_boolean": true
  }


+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+
| Flat Path                   | Flat type                        | RM Path                                | Requerd  | Note                                             |
+=============================+==================================+========================================+==========+==================================================+
|                             | Boolean                          | value                                  | Yes      |                                                  |
+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+

DV_URI 
--------------
(see also https://specifications.openehr.org/releases/RM/Release-1.0.4/data_types.html#_dv_uri_class)

.. code-block:: javascript

 {
     "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_uri": "https://www.google.com/"
  }


+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+
| Flat Path                   | Flat type                        | RM Path                                | Requerd  | Note                                             |
+=============================+==================================+========================================+==========+==================================================+
|                             | String                           | value                                  | Yes      |                                                  |
+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+

DV_EHR_URI 
--------------
(see also https://specifications.openehr.org/releases/RM/Release-1.0.4/data_types.html#_dv_ehr_uri_class)

.. code-block:: javascript

 {
    "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_ehr_uri": "ehr://766b3873-0762-4921-91e2-838c8546d47f"
  }


+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+
| Flat Path                   | Flat type                        | RM Path                                | Requerd  | Note                                             |
+=============================+==================================+========================================+==========+==================================================+
|                             | String                           | value                                  | Yes      |                                                  |
+-----------------------------+----------------------------------+----------------------------------------+----------+--------------------------------------------------+


DV_IDENTIFIER  
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_quantity_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_identifier|id": "A123",
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_identifier|id": "A123",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_identifier|issuer": "Issuer",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_identifier|assigner": "Assigner",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_identifier|type": "Prescription"
  } 


+-----------------------------+----------------------------------+--------------------------+----------+---------------------------------------+
| Flat Path                   | Flat type                        | RM Path                  | Requerd  | Note                                  |
+=============================+==================================+==========================+==========+=======================================+
| \|id                        | String                           | id                       | Yes      | For the input \|id might be left out. |
+-----------------------------+----------------------------------+--------------------------+----------+---------------------------------------+
| \|issuer                    | String                           | issuer                   | no       |                                       |
+-----------------------------+----------------------------------+--------------------------+----------+---------------------------------------+
| \|assigner                  | String                           | assigner                 | no       |                                       |
+-----------------------------+----------------------------------+--------------------------+----------+---------------------------------------+
| \|type                      | String                           | type                     | no       |                                       |
+-----------------------------+----------------------------------+--------------------------+----------+---------------------------------------+





DV_QUANTITY 
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_quantity_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|magnitude": 65.9,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|unit": "unit"
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|magnitude": 65.9,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|magnitude_status": "~",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|normal_status": "N",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|accuracy": 50.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|accuracy_is_percent": true,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|precision": 1,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|units_system": "units_system",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|units_display_name": "units_display_name",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/lower|magnitude": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/lower|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/upper|magnitude": 66.6,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/upper|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/lower|magnitude": 70.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/lower|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/upper|magnitude": 77.6,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/upper|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|value": "very high",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|code": "260360000",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|terminology": "SNOMED-CT"
  } 


+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| Flat Path                   | Flat type                          | RM Path                  | Requerd  | Note                       |
+=============================+====================================+==========================+==========+============================+
| \|magnitude                 | String                             | magnitude                | yes      |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| \|unit                      | Real                               | unit                     | yes      |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| \|magnitude_status          | String                             | magnitude_status         | no       | ValueSet (",>,>=,<,<=,~)   |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| \|normal_status             | String                             | normal_status            | no       | Valuset normal_status      |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| \|accuracy                  | Real                               | accuracy                 | no       |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| \|accuracy_is_percent       | Boolean                            | accuracy_is_percent      | no       |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| /_normal_range              | `DV_INTERVAL`_ <DV_QUANTITY>       | normal_range             | no       |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| /_other_reference_ranges:i  | `REFERENCE_RANGE`_ <DV_QUANTITY>   | _other_reference_ranges  | no       |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+


DV_PROPORTION 
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_proportion_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|numerator": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|type": 0
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|numerator": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion": 1.6532258064516128,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|magnitude_status": "~",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|normal_status": "N",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|accuracy": 50.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|accuracy_is_percent": true,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|precision": 1,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/lower|numerator": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/lower|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/lower|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/lower": 1.6532258064516128,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/upper|numerator": 25.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/upper|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/upper|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/upper": 2.0564516129032255,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/lower|numerator": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/lower|denominator": 18.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/lower|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/lower": 1.1141304347826089,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/upper|numerator": 25.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/upper|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/upper|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/upper": 2.0564516129032255,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/meaning": "high"
  } 


+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+
| Flat Path                   | Flat type                           | RM Path                  | Requerd  | Note                      |
+=============================+=====================================+==========================+==========+===========================+
| \|numerator                 | Real                                | numerator                | yes      |                           |
+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+
| \|denominator               | Real                                | denominator              | yes      |                           |
+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+
| \|type                      | Integer                             | type                     | yes      | ValueSet proportion_kind  |
+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+
|                             | Real                                | magnitude                | no       | calculated on output      |
+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+
| \|magnitude_status          | String                              | magnitude_status         | no       | ValueSet (",>,>=,<,<=,~)  |
+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+
| \|normal_status             | String                              | normal_status            | no       | Valuset normal_status     |
+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+
| /_normal_range              | `DV_INTERVAL`_ <DV_PROPORTION>      | normal_range             | no       |                           |
+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+
| /_other_reference_ranges:i  | `REFERENCE_RANGE`_ <DV_PROPORTION>  | _other_reference_ranges  | no       |                           |
+-----------------------------+-------------------------------------+--------------------------+----------+---------------------------+

DV_COUNT  
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_count_class)

.. code-block:: javascript

 {
   "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count": 7
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count": 7,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count|magnitude_status": "~",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count|normal_status": "N",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count|accuracy": 50.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count|accuracy_is_percent": true,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count/_normal_range/lower": 1,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count/_normal_range/upper": 8,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count/_other_reference_ranges:0/lower": 8,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count/_other_reference_ranges:0/upper": 10,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_count/_other_reference_ranges:0/meaning": "high"
  } 


+-----------------------------+-------------------------------------+--------------------------+----------+----------------------------+
| Flat Path                   | Flat type                           | RM Path                  | Requerd  | Note                       |
+=============================+=====================================+==========================+==========+============================+
|                             | Integer                             | magnitude                | Yes      |                            |
+-----------------------------+-------------------------------------+--------------------------+----------+----------------------------+
| \|magnitude_status          | String                              | magnitude_status         | no       | ValueSet (",>,>=,<,<=,~)   |
+-----------------------------+-------------------------------------+--------------------------+----------+----------------------------+
| \|normal_status             | String                              | normal_status            | no       | Valuset normal_status      |
+-----------------------------+-------------------------------------+--------------------------+----------+----------------------------+
| /_normal_range              | `DV_INTERVAL`_ <DV_COUNT>           | normal_range             | no       |                            |
+-----------------------------+-------------------------------------+--------------------------+----------+----------------------------+
| /_other_reference_ranges:i  | `REFERENCE_RANGE`_ <DV_COUNT>       | _other_reference_ranges  | no       |                            |
+-----------------------------+-------------------------------------+--------------------------+----------+----------------------------+

DV_DATE    
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_date_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date": "2022-01-12"
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date": "2022-01-12",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date|magnitude_status": "~",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date|normal_status": "N",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date/_accuracy": "P2D",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date/_normal_range/lower": "2022-01-12",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date/_normal_range/upper": "2022-02-12",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date/_other_reference_ranges:0/lower": "2022-02-12",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date/_other_reference_ranges:0/upper": "2022-03-12",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date/_other_reference_ranges:0/meaning": "high"
  } 


+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| Flat Path                   | Flat type                     | RM Path                  | Requerd  | Note                       |
+=============================+===============================+==========================+==========+============================+
|                             | String                        | value                    | Yes      | ISO8601 date               |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| /_accuracy                  | `DV_DURATION`_                | accuracy                 | no       |                            |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| \|magnitude_status          | String                        | magnitude_status         | no       | ValueSet (",>,>=,<,<=,~)   |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| \|normal_status             | String                        | normal_status            | no       | Valuset normal_status      |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| /_normal_range              | `DV_INTERVAL`_ <DV_DATE>      | normal_range             | no       |                            |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| /_other_reference_ranges:i  | `REFERENCE_RANGE`_ <DV_DATE>  | _other_reference_ranges  | no       |                            |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+

DV_DATE_TIME    
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_date_class)

.. code-block:: javascript

 {
    "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time": "2022-01-12T13:22:34.000868+01:00"
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time": "2022-01-12T13:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time|magnitude_status": "~",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time|normal_status": "N",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time/_accuracy": "P2DT9H52M",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time/_normal_range/lower": "2022-01-12T13:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time/_normal_range/upper": "2022-02-12T13:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time/_other_reference_ranges:0/lower": "2022-02-12T13:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time/_other_reference_ranges:0/upper": "2022-03-12T13:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date_time/_other_reference_ranges:0/meaning": "high",
  } 


+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| Flat Path                   | Flat type                          | RM Path                  | Requerd  | Note                       |
+=============================+====================================+==========================+==========+============================+
|                             | String                             | value                    | Yes      | ISO8601 date               |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| /_accuracy                  | `DV_DURATION`_                     | accuracy                 | no       |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| \|magnitude_status          | String                             | magnitude_status         | no       | ValueSet (",>,>=,<,<=,~)   |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| \|normal_status             | String                             | normal_status            | no       | Valuset normal_status      |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| /_normal_range              | `DV_INTERVAL`_ <DV_DATE_TIME>      | normal_range             | no       |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+
| /_other_reference_ranges:i  | `REFERENCE_RANGE`_ <DV_DATE_TIME>  | _other_reference_ranges  | no       |                            |
+-----------------------------+------------------------------------+--------------------------+----------+----------------------------+



DV_TIME    
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_date_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_date": "2022-01-12"
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time": "13:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time|magnitude_status": "~",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time|normal_status": "N",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time/_accuracy": "PT9H52M",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time/_normal_range/lower": "13:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time/_normal_range/upper": "14:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time/_other_reference_ranges:0/lower": "14:10:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time/_other_reference_ranges:0/upper": "15:22:34.000868+01:00",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_time/_other_reference_ranges:0/meaning": "high"
  } 


+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| Flat Path                   | Flat type                     | RM Path                  | Requerd  | Note                       |
+=============================+===============================+==========================+==========+============================+
|                             | String                        | value                    | Yes      | ISO8601 date               |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| /_accuracy                  | `DV_DURATION`_                | accuracy                 | no       |                            |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| \|magnitude_status          | String                        | magnitude_status         | no       | ValueSet (",>,>=,<,<=,~)   |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| \|normal_status             | String                        | normal_status            | no       | Valuset normal_status      |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| /_normal_range              | `DV_INTERVAL`_ <DV_TIME>      | normal_range             | no       |                            |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+
| /_other_reference_ranges:i  | `REFERENCE_RANGE`_ <DV_TIME>  | _other_reference_ranges  | no       |                            |
+-----------------------------+-------------------------------+--------------------------+----------+----------------------------+


DV_DURATION   
--------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_dv_duration_class)

.. code-block:: javascript

 {
   "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration": "P2DT11H33M"
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration": "P2DT11H33M",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration|magnitude_status": "~",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration|normal_status": "N",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration|accuracy": 50.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration|accuracy_is_percent": true,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration/_normal_range/lower": "P2DT11H33M",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration/_normal_range/upper": "P2DT12H33M",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration/_other_reference_ranges:0/lower": "P2DT11H33M",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration/_other_reference_ranges:0/upper": "P2DT15H33M",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_duration/_other_reference_ranges:0/meaning": "high"
  } 


+-----------------------------+-----------------------------------+--------------------------+----------+----------------------------+
| Flat Path                   | Flat type                         | RM Path                  | Requerd  | Note                       |
+=============================+===================================+==========================+==========+============================+
|                             | String                            | value                    | Yes      | ISO8601 duration           |
+-----------------------------+-----------------------------------+--------------------------+----------+----------------------------+
| \|accuracy                  | Real                              | accuracy                 | no       |                            |
+-----------------------------+-----------------------------------+--------------------------+----------+----------------------------+
| accuracy_is_percent         | Boolean                           | accuracy_is_percent      | no       |                            |
+-----------------------------+-----------------------------------+--------------------------+----------+----------------------------+
| \|magnitude_status          | String                            | magnitude_status         | no       | ValueSet (",>,>=,<,<=,~)   |
+-----------------------------+-----------------------------------+--------------------------+----------+----------------------------+
| \|normal_status             | String                            | normal_status            | no       | Valuset normal_status      |
+-----------------------------+-----------------------------------+--------------------------+----------+----------------------------+
| /_normal_range              | `DV_INTERVAL`_ <DV_DURATION>      | normal_range             | no       |                            |
+-----------------------------+-----------------------------------+--------------------------+----------+----------------------------+
| /_other_reference_ranges:i  | `REFERENCE_RANGE`_ <DV_DURATION>  | _other_reference_ranges  | no       |                            |
+-----------------------------+-----------------------------------+--------------------------+----------+----------------------------+



DV_INTERVAL 
--------------
(see also https://specifications.openehr.org/releases/BASE/latest/foundation_types.html#_interval_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_interval/any_event:0/interval_dv_quantity/lower|magnitude": 72.83,
  "conformance-ehrbase.de.v0/conformance_section/conformance_interval/any_event:0/interval_dv_quantity/lower|unit": "Unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_interval/any_event:1/interval_dv_quantity/upper|magnitude": 150.83,
  "conformance-ehrbase.de.v0/conformance_section/conformance_interval/any_event:1/interval_dv_quantity/upper|unit": "Unit",
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|numerator": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion": 1.6532258064516128,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|magnitude_status": "~",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|normal_status": "N",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|accuracy": 50.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|accuracy_is_percent": true,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion|precision": 1,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/lower|numerator": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/lower|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/lower|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/lower": 1.6532258064516128,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/upper|numerator": 25.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/upper|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/upper|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_normal_range/upper": 2.0564516129032255,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/lower|numerator": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/lower|denominator": 18.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/lower|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/lower": 1.1141304347826089,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/upper|numerator": 25.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/upper|denominator": 12.4,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/upper|type": 0,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/upper": 2.0564516129032255,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_proportion/_other_reference_ranges:0/meaning": "high"
  } 


+--------------------+------------+------------------+----------+--------------------+
| Flat Path          | Flat type  | RM Path          | Requerd  | Note               |
+====================+============+==================+==========+====================+
| /lower             | T          | /lower           | no       |                    |
+--------------------+------------+------------------+----------+--------------------+
| /upper             | T          | /upper           | no       |                    |
+--------------------+------------+------------------+----------+--------------------+
| \|lower_unbounded  | Boolean    | lower_unbounded  | no       | defaults to false  |
+--------------------+------------+------------------+----------+--------------------+
| \|upper_unbounded  | Boolean    | upper_unbounded  | no       | defaults to false  |
+--------------------+------------+------------------+----------+--------------------+
| \|lower_included   | Boolean    | lower_included   | no       | defaults to true   |
+--------------------+------------+------------------+----------+--------------------+
| \|upper_included   | Boolean    | upper_included   | no       | defaults to true   |
+--------------------+------------+------------------+----------+--------------------+



REFERENCE_RANGE  
-----------------
(see also https://specifications.openehr.org/releases/RM/latest/data_types.html#_reference_range_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/lower|magnitude": 70.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/lower|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/upper|magnitude": 77.6,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/upper|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|value": "high",
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/lower|magnitude": 70.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/lower|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0|upper_unbounded": true,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0|upper_included": false,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|value": "very high",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|code": "260360000",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|terminology": "SNOMED-CT",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:1|lower_unbounded": true,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:1|lower_included": false,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:1/upper|magnitude": 77.6,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:1/upper|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:1/meaning|value": "very high",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:1/meaning|code": "260360000",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:1/meaning|terminology": "SNOMED-CT",
  } 


+--------------------+--------------+------------------------+----------+--------------------+
| Flat Path          | Flat type    | RM Path                | Requerd  | Note               |
+====================+==============+========================+==========+====================+
| /lower             | T            | range.lower            | no       |                    |
+--------------------+--------------+------------------------+----------+--------------------+
| /upper             | T            | range.upper            | no       |                    |
+--------------------+--------------+------------------------+----------+--------------------+
| \|lower_unbounded  | Boolean      | range.lower_unbounded  | no       | defaults to false  |
+--------------------+--------------+------------------------+----------+--------------------+
| \|upper_unbounded  | Boolean      | range.upper_unbounded  | no       | defaults to false  |
+--------------------+--------------+------------------------+----------+--------------------+
| \|lower_included   | Boolean      | range.lower_included   | no       |  defaults to true  |
+--------------------+--------------+------------------------+----------+--------------------+
| \|upper_included   | Boolean      | range.upper_included   | no       |  defaults to true  |
+--------------------+--------------+------------------------+----------+--------------------+
| \meaning           | `DV_TEXT`_   | meaning                | yes      |                    |
+--------------------+--------------+------------------------+----------+--------------------+



