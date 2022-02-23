General Description
===================

.. warning:: WIP

The Flat format offers a simplified representation of a openEHR Composition. Data is represented as a map of human readable parts to values.

.. code-block:: json

 {
  "ctx/language": "de",
  "ctx/territory": "US",
  "ctx/time": "2021-04-01T12:40:31.418954+02:00",
  "ctx/composer_name": "Silvia Blake",
  "conformance-ehrbase.de.v0/context/start_time": "2021-12-21T14:19:31.649613+01:00",
  "conformance-ehrbase.de.v0/context/setting|code": "238",
  "conformance-ehrbase.de.v0/context/setting|value": "other care",
  "conformance-ehrbase.de.v0/context/setting|terminology": "openehr",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|magnitude": 65.9,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|unit": "unit"
  }


Web-template 
--------------
The Flat format is based on a simplified representation of the template the so called Web-template.

To get one from a template call the get Web-template Endpoint.

.. code-block:: javascript

 {
 GET http://localhost:8080/ehrbase/rest/ecis/v1/template/{templateId}
  } 

.. code-block:: json
  :linenos: 
  :emphasize-lines: 10,20,30,40,46,50,60

   {
    "templateId": "conformance-ehrbase.de.v0",
    "semVer": "1.0.0",
    "version": "2.3",
    "defaultLanguage": "en",
    "languages": [
      "en"
    ],
    "tree": {
      "id": "conformance-ehrbase.de.v0",
      "name": "conformance-ehrbase.de.v0",
      "localizedName": "conformance-ehrbase.de.v0",
      "rmType": "COMPOSITION",
      "nodeId": "openEHR-EHR-COMPOSITION.conformance_composition_.v0",
      "min": 1,
      "max": 1,
      "aqlPath": "",
      "children": [
        {
          "id": "conformance_section",
          "name": "conformance section",
          "localizedName": "conformance section",
          "rmType": "SECTION",
          "nodeId": "openEHR-EHR-SECTION.conformance_section.v0",
          "min": 0,
          "max": 1,
          "aqlPath": "/content[openEHR-EHR-SECTION.conformance_section.v0]",
          "children": [
            {
              "id": "conformance_observation",
              "name": "Conformance Observation",
              "localizedName": "Conformance Observation",
              "rmType": "OBSERVATION",
              "nodeId": "openEHR-EHR-OBSERVATION.conformance_observation.v0",
              "min": 0,
              "max": 1,
              "aqlPath": "/content[openEHR-EHR-SECTION.conformance_section.v0]/items[openEHR-EHR-OBSERVATION.conformance_observation.v0]",
              "children": [
                {
                  "id": "any_event",
                  "name": "Any event",
                  "localizedName": "Any event",
                  "rmType": "EVENT",
                  "nodeId": "at0002",
                  "min": 0,
                  "max": -1,
                  "aqlPath": "/content[openEHR-EHR-SECTION.conformance_section.v0]/items[openEHR-EHR-OBSERVATION.conformance_observation.v0]/data[at0001]/events[at0002]",
                  "children": [
                    {
                      "id": "dv_quantity",
                      "name": "DV_QUANTITY",
                      "localizedName": "DV_QUANTITY",
                      "rmType": "DV_QUANTITY",
                      "nodeId": "at0008",
                      "min": 0,
                      "max": 1,
                      "aqlPath": "/content[openEHR-EHR-SECTION.conformance_section.v0]/items[openEHR-EHR-OBSERVATION.conformance_observation.v0]/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value",
                      "inputs": [
                        {
                          "suffix": "magnitude",
                          "type": "DECIMAL"
                        },
                        {
                          "suffix": "unit",
                          "type": "CODED_TEXT"
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        }
      ]
    }
   }


Flat Path
--------------

To build a Flat Path

* add the id from the `Web-template`_ together
* if a element is multi valued add a index
* Once at A Data Value uses "|" to select the Attribute

.. code-block:: json

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|magnitude": 65.9,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|unit": "unit"
  }

RM-Attributes
--------------
Some attributes are not defended by the template but by the RM-Model. If those are optional there are not part of the `Web-template`_ and are selected by "_attributeName"

.. code-block:: json

 {
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|magnitude": 65.9,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/lower|magnitude": 20.5,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/lower|unit": "unit",
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/upper|magnitude": 66.6,
  "conformance-ehrbase.de.v0/conformance_section/conformance_observation/any_event:0/dv_quantity/_normal_range/upper|unit": "unit"
  }

See :doc:`/09_flat/01_data_types/index` for details.

Context
--------------
To simplify the input, the flat formate offers the option to set context values which set default values in the rm-tree.

.. code-block:: json

 {
  "ctx/language": "de",
  "ctx/territory": "US",
  "ctx/time": "2021-04-01T12:40:31.418954+02:00",
  "ctx/composer_name": "Silvia Blake"
  }

See :doc:`/09_flat/02_context/index` for details.