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
| \|vlaue        | String          | value                               | (yes)    | only required for external  terminologies  |
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




DV_INTERVAL 
--------------
(see also https://specifications.openehr.org/releases/BASE/latest/foundation_types.html#_interval_class)

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/lower|magnitude": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/lower|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/upper|magnitude": 66.6,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/upper|unit": "unit"
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/lower|magnitude": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/lower|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/upper|magnitude": 66.6,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/upper|unit": "unit"
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
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|value": "very high",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|code": "260360000",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|terminology": "SNOMED-CT"
  } 

.. code-block:: javascript

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/lower|magnitude": 70.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/lower|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/upper|magnitude": 77.6,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/upper|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|value": "very high",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|code": "260360000",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_other_reference_ranges:0/meaning|terminology": "SNOMED-CT"
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



