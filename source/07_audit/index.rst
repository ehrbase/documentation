.. _audit:

============
Audit
============

This section provides information about the Audit Trail and Node Authentication Profile implemented by EHRbase.

Introduction
------------

The audit messages sent by EHRbase are conformant with the schema specified by the `DICOM Audit Trail Profile <http://dicom.nema.org/medical/Dicom/2016b/output/chtml/part15/sect_A.5.html>`_.
Regarding the implementation side, the application relies on ``ipf-commons-audit`` and ``ipf-atna-spring-boot-starter`` modules
provided by `IPF Open eHealth Integration Platform <https://oehf.github.io/ipf-docs/docs/audit/>`_.

As an example, the following audit trail message is generated from the Composition endpoint when a new composition is created:

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>

    <AuditMessage>
        <EventIdentification EventActionCode="C" EventDateTime="2021-02-23T10:29:09.788302Z" EventOutcomeIndicator="0">
            <EventID csd-code="composition" codeSystemName="http://www.openehr.org/api/v1/ehr" originalText="composition"/>
            <EventTypeCode csd-code="249" codeSystemName="openehr" originalText="creation"/>
            <EventOutcomeDescription>204 No Content</EventOutcomeDescription>
        </EventIdentification>
        <ActiveParticipant UserID="demo-user" UserIsRequestor="true" NetworkAccessPointID="192.168.1.100" NetworkAccessPointTypeCode="2">
            <RoleIDCode csd-code="110153" codeSystemName="DCM" originalText="Source Role ID"/>
        </ActiveParticipant>
        <ActiveParticipant UserID="ehrbase" UserIsRequestor="false" NetworkAccessPointID="192.168.1.236" NetworkAccessPointTypeCode="2">
            <RoleIDCode csd-code="110152" codeSystemName="DCM" originalText="Destination Role ID"/>
        </ActiveParticipant>
        <AuditSourceIdentification AuditSourceID="ehrbase">
            <AuditSourceTypeCode csd-code="4" codeSystemName="DCM" originalText="Application Server Process or Thread"/>
        </AuditSourceIdentification>
        <ParticipantObjectIdentification ParticipantObjectID="ehr/3043ebd5-0f27-4a32-b5f8-93b1fe107fa9/composition/1b9f55fa-7307-4c8f-8e6b-54b9116dd2c2"
                                         ParticipantObjectTypeCode="2"
                                         ParticipantObjectDataLifeCycle="1">
            <ParticipantObjectIDTypeCode csd-code="12" codeSystemName="RFC-3881" originalText="URI"/>
            <ParticipantObjectName>Schwangerschaftsstatus</ParticipantObjectName>
        </ParticipantObjectIdentification>
        <ParticipantObjectIdentification ParticipantObjectID="3043ebd5-0f27-4a32-b5f8-93b1fe107fa9"
                                         ParticipantObjectTypeCode="1"
                                         ParticipantObjectTypeCodeRole="1">
            <ParticipantObjectIDTypeCode csd-code="2" codeSystemName="RFC-3881" originalText="Patient Number"/>
        </ParticipantObjectIdentification>
    </AuditMessage>

Configuration
-------------

The following subsections provide information on the configuration of EHRbase and the installation of a simple Audit Repository
using the `Elastic Stack <https://www.elastic.co/elastic-stack>`_ and `Docker <https://www.docker.com/>`_.

Configuring EHRbase
^^^^^^^^^^^^^^^^^^^

EHRbase supports the following properties in order to properly configure the audit feature:

+---------------------------------------+--------------------------------+---------------------------------------------------------+
| Key                                   | Default Value                  | Description                                             |
+=======================================+================================+=========================================================+
| ``ipf.atna.audit-enabled``            | ``false``                      | Whether to enable audit feature.                        |
+---------------------------------------+--------------------------------+---------------------------------------------------------+
| ``ipf.atna.audit-enterprise-site-id`` |                                | Enterprise Site ID                                      |
+---------------------------------------+--------------------------------+---------------------------------------------------------+
| ``ipf.atna.audit-repository-host``    | ``localhost``                  | Audit repository host.                                  |
+---------------------------------------+--------------------------------+---------------------------------------------------------+
| ``ipf.atna.audit-repository-port``    | ``514``                        | Audit repository port.                                  |
+---------------------------------------+--------------------------------+---------------------------------------------------------+
| ``ipf.atna.audit-source-id``          | ``${spring.application.name}`` | Audit source ID.                                        |
+---------------------------------------+--------------------------------+---------------------------------------------------------+
| ``ipf.atna.audit-value-if-missing``   | ``UNKNOWN``                    | Value used for mandatory elements that are empty.       |
+---------------------------------------+--------------------------------+---------------------------------------------------------+

For EHRbase configuration, please read the documentation or visit the `Externalized Configuration <https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config>`_
documentation provided by Spring Boot.

For advanced configuration, please visit the IPF Open eHealth Integration Platform documentation:

  - Spring Boot support: https://oehf.github.io/ipf-docs/docs/boot-atna/
  - DICOM audit support: https://oehf.github.io/ipf-docs/docs/audit/

Setup an Audit Repository
^^^^^^^^^^^^^^^^^^^^^^^^^

The easiest way to install and configure your own Audit Record Repository instance is to used the Elastic Stack together with Docker.

Step 1: Create the Docker Compose file
""""""""""""""""""""""""""""""""""""""
The following Docker Compose file installs and configures Elasticsearch, Kibana and Logstash according to the default configuration
used in EHRbase.

.. code-block:: yaml
    :caption: docker-compose-audit.yml

    services:
      elasticsearch:
        image: elasticsearch:7.7.0
        hostname: elasticsearch
        environment:
          - "discovery.type=single-node"
        ports:
          - 9200:9200
          - 9300:9300
        networks:
          - elastic-network
      kibana:
        image: kibana:7.7.0
        hostname: kibana
        ports:
          - 5601:5601
        links:
          - elasticsearch:elasticsearch
        depends_on:
          - elasticsearch
        networks:
          - elastic-network
      logstash:
        image: logstash:7.7.0
        hostname: logstash
        ports:
          - 9600:9600
          - 8089:8089
          - 514:514/udp
        volumes:
          - ./logstash:/usr/share/logstash/pipeline/
        links:
          - elasticsearch:elasticsearch
        depends_on:
          - elasticsearch
        networks:
          - elastic-network
    networks:
      elastic-network: { }

Step 2: Configure Logstash
""""""""""""""""""""""""""

The second step is the configuration of Logstash to enable the support of the `Syslog Protocol <https://tools.ietf.org/html/rfc5424>`_
and DICOM Audit Trail Message Format Profile.

The following configuration file has to be stored in the directory referenced in the volume mounted in the ``logstash`` service
declared in the Docker Compose file above (i.e. ``./logstash/logstash.conf``).

.. code-block::
    :caption: logstash.conf

    input {
      udp {
        port => 514
      }
    }
    filter {
      grok {
        match => {
          "message" => "<%{NONNEGINT:syslog_header_pri}>%{NONNEGINT:syslog_header_version}%{SPACE}(?:-|%{TIMESTAMP_ISO8601})%{SPACE}(?:-|%{IPORHOST:syslog_header_hostname})%{SPACE}(?:%{SYSLOG5424PRINTASCII:syslog_header_app-name}|-)%{SPACE}(?:-|%{SYSLOG5424PRINTASCII:syslog_header_procid})%{SPACE}(?:-|%{SYSLOG5424PRINTASCII:syslog_header_msgid})%{SPACE}(?:-|(?<syslog_structured_data>(\[.*?[^\\]\])+))(?:%{SPACE}%{GREEDYDATA:syslog_message}|)"
        }
      }
      xml {
        store_xml => true
        target => "AuditMessage"
        force_array => false
        source => "syslog_message"
      }
      mutate {
        remove_field => [ "@timestamp", "@version", "message", "host" ]
      }
    }
    output {
      elasticsearch { hosts => ["elasticsearch:9200"] }
    }

Step 3: Start your Audit Repository instance
""""""""""""""""""""""""""""""""""""""""""""

Last step is to start the Audit Repository instance using the following command:

.. parsed-literal::

    $ docker-compose -f docker-compose-audit.yml up
