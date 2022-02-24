Client
------

.. warning:: WIP

The openEHR Client is the foundation of all following functionalities.
It needs to be created and set up before the endpoints can be used.

Interface and implementation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All compatible clients need to implement the ``OpenEhrClient`` Interface.

Standard operation is possible with the included ``DefaultRestClient`` implementation.

Client setup
^^^^^^^^^^^^

To set up a default client it is necessary to create a new instance, 
which requires the following components as parameters:

- ``OpenEhrClientConfig`` containing the base URI of the openEHR REST API backend server
- Optional (if not given, templates will be queried from the remote system) add a ``TemplateProvider`` (see below)

Together a typical setup might look like:

.. code-block:: java

    DefaultRestClient client = new DefaultRestClient(
        new OpenEhrClientConfig(new URI("http://localhost:8080/ehrbase/")),
        templateProvider);

.. _sdk-reference-client_module-client-template_provider-label:

Template provider
^^^^^^^^^^^^^^^^^

The ``TemplateProvider`` interface needs an implementation that gives the SDK access to
the templates used.

Technically, it needs to provide a function ``Optional<OPERATIONALTEMPLATE> find(String s)``
which returns an object representation of a given template ID.

An example is the ``TestDataTemplateProvider`` from the SDK's own integration tests:

.. code-block:: java

    @Override
    public Optional<OPERATIONALTEMPLATE> find(String templateId) {
        return Optional.ofNullable(OperationalTemplateTestData.findByTemplateId(templateId))
                .map(OperationalTemplateTestData::getStream)
                .map(s -> {
                    try {
                        return TemplateDocument.Factory.parse(s);
                    } catch (XmlException | IOException e) {
                        throw new RuntimeException(e.getMessage(), e);
                    }
                })
                .map(TemplateDocument::getTemplate);
    }

Please also see the used ``OperationalTemplateTestData`` enum to understand how
actual .opt files can be made accessible.

Additional steps
^^^^^^^^^^^^^^^^

Now the client is set up and ready to be used.
Before going on it might make sense to sync your backend with the templates used by your client.

The template provider could have a ``listTemplateIds()`` method for that purpose.
So the following would use this method 
and the template endpoint (see :ref:`Template endpoint <sdk-reference-client_module-client-template_endpoint-label>`)
to make sure all templates of this clients scope are available in the backend.

.. code-block:: java

    templateProvider.listTemplateIds().forEach(
            t -> client.templateEndpoint().ensureExistence(t)
    );
