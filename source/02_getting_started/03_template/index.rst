.. _upload_a_template:

Step 2: Upload a Template
=========================

After we created the Template, it's time to put upload it to EHRbase. The data format that is uploaded to an openEHR server 
like EHRbase is called an Operational Template (OPT-File). You can find an example of an OPT here.

Please see https://specifications.openehr.org/releases/ITS-REST/latest/definitions.html#definitions-adl-1.4-template-post
for the REST endpoint that you can use in EHRbase to POST a template. On a local instance of EHRbase, the URL should 
be like localhost:8080/ehrbase/rest/openehr/v1/definition/template/adl1.4 

Copy the content of the OPT file into the body of the REST call. Make sure that the Content-Type attribute is set to application/XML.
After the successful call, you should receive a 200 response code.


Client Library
^^^^^^^^^^^^^^

As an alternative to directly using the REST API, the EHRbase Client Library provides functionality to provide a Template.

.. code-block:: Java

        OPERATIONALTEMPLATE template = TemplateDocument.Factory.parse(OperationalTemplateTestData.BLOOD_PRESSURE_SIMPLE.getStream()).getTemplate();
        String templateId = "ehrbase_blood_pressure_simple.de.v" + RandomStringUtils.randomNumeric(10);
        template.getTemplateId().setValue(templateId);
        String actual = new DefaultRestTemplateEndpoint(cut).upload(template);