******************************
Plugin system (WIP)
******************************

EHRbase at its core is an implementation of the openEHR specifications with as minimal as possible additions on top of that.
In general, those unspecified additions may be classified in two categories:

* technical: features that are required for operation, such as logging, actuator endpoints, etc.
* functional: features that are generalizable, such as authentication

While additions like those are not explicitily specified in openEHR, they are also not working against the specifications and 
are usually part of every service in the wild.

The plugin system provides additional ways to extend the openEHR workflows provided by EHRbase.

Concept
=======

EHRbase uses `pf4j <https://github.com/pf4j/pf4j>`_ and `pf4j-spring <https://github.com/pf4j/pf4j-spring>`_ to load 
built plugins at startup from a defined directory, and add the code from those plugins to the main application context.

Plugins are in the same application context as the main EHRbase code, therefore any type of additional custom beans may be added,
as well as using any service from EHRbase itself.

.. warning::
    Plugin management and security is not in scope of the plugin system implementation in EHRbase.
    Provisioning of plugins and security implications of possibly using broken or rogue plugins is the responsibility of the operator of a customized EHRbase. 

Using plugins
=============

.. note::
    Precondition: The plugins to be used are built, available as a JAR file, and placed in a directory readable by the target EHRbase instance.

To enable plugins to be loaded, additional configuration parameters need to be set in the application configuration of EHRbase:

.. code-block:: yaml

    plugin-manager:
        enable: true                     # false to disable all plugins
        plugin-dir: ./my-ehrbase-plugins # directory containing the plugin JARs
        plugin-context-path: /plugins    # base context path for plugin endpoint URLs

Developing plugins
==================

The following section assumes a standard Maven and Java workflow, but is also possible with Gradle or Kotlin (not documented here).
Please advise the respective documentation on how to create a JAR with your compiled JVM code and manifest. 

Plugin project setup
--------------------

Create a standard Maven project, and insert the following snippets into the ``pom.xml``

.. code-block:: xml

    <project>
        <properties>
            <!-- compiler target level 11 required -->
            <maven.compiler.source>11</maven.compiler.source>
            <maven.compiler.target>11</maven.compiler.target>

            <!-- plugin metainformation -->
            <plugin.id>my-plugin-a</plugin.id>
            <plugin.class>com.acme.plugin.a.MyPluginA</plugin.class>
            <plugin.version>0.0.1</plugin.version>
            <plugin.provider>acme</plugin.provider>
            <plugin.dependencies />
        </properties>

        <dependencies>
            <!-- plugin SPI module -->
            <dependency>
                <groupId>org.ehrbase.openehr</groupId>
                <artifactId>plugin</artifactId>
                <version>0.20.0-SNAPSHOT</version>
            </dependency>
        </dependencies>

        <build>
            <plugins>
                <plugin>
                    <!-- add the required manifest entries to the plugin JAR -->
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-jar-plugin</artifactId>
                    <version>2.4</version>
                    <configuration>
                        <archive>
                            <manifestEntries>
                                <Plugin-Id>${plugin.id}</Plugin-Id>
                                <Plugin-Class>${plugin.class}</Plugin-Class>
                                <Plugin-Version>${plugin.version}</Plugin-Version>
                                <Plugin-Provider>${plugin.provider}</Plugin-Provider>
                                <Plugin-Dependencies>${plugin.dependencies}</Plugin-Dependencies>
                            </manifestEntries>
                        </archive>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </project>

Plugin entrypoint
-----------------

The manifest entry ``Plugin-Class`` as the entrypoint of the plugin has two variants with respective base classes from the SPI.

* For plugins which will use the full WebApplicationContext (provide Controller endpoints) need to implement ``org.ehrbase.plugin.EhrBasePlugin``
  as in the `web example plugin <https://github.com/ehrbase/ExamplePlugin/blob/master/web-plugin/src/main/java/org/ehrbase/example_web_plugin/ExampleWebPlugin.java>`_.
* If the full WebApplicationContext is not required - the simplified default ``org.pf4j.spring.SpringPlugin`` must be implemented
  as in the `simple example plugin <https://github.com/ehrbase/ExamplePlugin/blob/master/simple-plugin/src/main/java/org/ehrbase/example_plugin/ExamplePlugin.java>`_.

For both versions, it is recommended to create a spring configuration class triggering a ComponentScan on the plugin package. 
With this setup, the boilerplate of the plugin implementation is done and the specific plugin logic may be implemented.

Extension points
----------------

While it might be enough to have additional code running in parallel and based on EHRbase, but the plugin use-case may also 
require hooking into processes and services, and react on or modify data.

To handle this requirements additional extension point interfaces are provided by the plugin SPI, which provide aspect-like hooks on specific functionalities.

Available extension points are in the package ``org.ehrbase.plugin.extensionpoints`` or in 
`GitHub <https://github.com/ehrbase/ehrbase/tree/develop/plugin/src/main/java/org/ehrbase/plugin/extensionpoints>`_,
and the respective JavaDoc should be consulted on the specific extension point hook.

To implement an extension point, create an implementation of the specific extension point interface as a component of the plugin, 
and annotate it with ``@Extension`` and a specific ``@Order(#)`` annotation to define the execution order of the implementation.

Plugin configuration
--------------------

Plugins can also be configured externally to create plugins which can react to their environment.

.. todo:: 
    TODO: Document configuration.
