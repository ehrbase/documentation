Template Endpoint
-----------------

.. _sdk-reference-client_module-client-template_endpoint-label:

The ``DefaultRestClient`` includes a ``DefaultRestTemplateEndpoint`` 
with the following functionalities.

.. warning:: WIP

Ensure existence of a template
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Uploading necessary operational templates is vital for many other client functions.

The ``ensureExistence(String templateId)`` method combines a check 
and the upload, if the template is not already available on the server.

The following example utilizes a ``TemplateProvider`` 
(see :ref:`Client reference <sdk-reference-client_module-client-template_provider-label>`)
to go through all templates of the client's scope and ensure their existence:

.. code-block:: java

    templateProvider.listTemplateIds().forEach(
            t -> client.templateEndpoint().ensureExistence(t)
    );
