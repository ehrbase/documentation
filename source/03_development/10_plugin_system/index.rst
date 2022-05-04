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
        enable: true                                   # false to disable all plugins
        plugin-dir: ./my-ehrbase-plugins               # directory containing the plugin JARs
        plugin-config-dir: ./my-ehrbase-plugins-config # directory containing the plugin JARs
        plugin-context-path: /plugins                  # base context path for plugin endpoint URLs

Example plugins
==================
Two example plugins can be found in `Example Plugin Repo <https://github.com/ehrbase/ExamplePlugin>`_

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

        <dependencyManagement>
           <dependencies>
           <!-- Include the ehrbase bom -->
            <dependency>
                <groupId>org.ehrbase.openehr</groupId>
                <artifactId>bom</artifactId>
                <version>0.21.0-SNAPSHOT</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
          </dependencies>
        </dependencyManagement>

        <dependencies>
            <!-- plugin SPI module -->
            <dependency>
                <groupId>org.ehrbase.openehr</groupId>
                <artifactId>plugin</artifactId>
                <scope>provided</scope>
            </dependency>
        </dependencies>

       <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>3.1.0</version>
                <configuration>
                    <descriptors>
                        <descriptor>src/main/assembly/assembly.xml</descriptor>
                    </descriptors>
                    <finalName>${project.artifactId}-${project.version}-plugin</finalName>
                    <appendAssemblyId>false</appendAssemblyId>
                    <attach>false</attach>
                    <archive>
                        <manifest>
                            <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
                            <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
                        </manifest>
                        <manifestEntries>
                            <Plugin-Id>${plugin.id}</Plugin-Id>
                            <Plugin-Version>${plugin.version}</Plugin-Version>
                            <Plugin-Provider>${plugin.provider}</Plugin-Provider>
                            <Plugin-Class>${plugin.class}</Plugin-Class>
                            <Plugin-Dependencies>${plugin.dependencies}</Plugin-Dependencies>
                        </manifestEntries>
                    </archive>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
       </build>
    </project>

also add under ``src/main/assembly/assembly.xml``

.. code-block:: xml

 <assembly>
    <id>plugin</id>
    <formats>
      <format>jar</format>
    </formats>
    <includeBaseDirectory>false</includeBaseDirectory>
    <dependencySets>
        <dependencySet>
            <useProjectArtifact>true</useProjectArtifact>
            <unpack>true</unpack>
            <scope>runtime</scope>
            <outputDirectory>/</outputDirectory>
            <useTransitiveDependencies>true</useTransitiveDependencies>
        </dependencySet>
    </dependencySets>
 </assembly>

Dependencies
-------------
The EHRbase bom provides the correct version for als Dependencies used by EHRbase and are provided by it. Thus any depandances in the bom has to be included with scope provided. Additional Dependencies need to be packed into the jar. This is done via the maven-assembly-plugin.

Plugin entrypoint
-----------------

The manifest entry ``Plugin-Class`` as the entrypoint of the plugin has two variants with respective base classes from the SPI.

* For plugins which will use the full WebApplicationContext (provide Controller endpoints) need to implement ``org.ehrbase.plugin.WebMvcEhrBasePlugin``
  as in the `web example plugin <https://github.com/ehrbase/ExamplePlugin/blob/master/web-plugin/src/main/java/org/ehrbase/example_web_plugin/ExampleWebPlugin.java>`_.
* If the full WebApplicationContext is not required - the simplified default ``org.ehrbase.plugin.NonWebMvcEhrBasePlugin`` must be implemented
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
and annotate it with ``@Extension`` and a specific ``@Order(#)`` annotation or implement the Order or PriorityOrder interface to define the execution order of the implementation.
Please be aware that the lowest order value has the highest precedence, meaning it can process the input value first, but will process the return value last (same as with around pointcuts in aspects).
If multiple extension point implementations of the same interface have the same order value (across plugins), PriorityOrdered takes precedence and after that precedence is determined by descending alphabetical order of the bean names.

The extension point interfaces define methods with semantics similar to around pointcuts in aspects in most cases.
When implementing an around method, you will receive an input object and a call chain function.
The input object contains all or parts of the arguments provided to the intercepted methods in the ehrbase service layer.
Input DTOs combining multiple arguments are usually not mutable, but if you need to modify the argument values before proceeding, you can create a new instance with modified values.
The call chain function argument acts as an analog to ProceedingJoinPoint in aspects.
To proceed with the service invocation just call apply with your (possibly) modified input object.
You may also modify the return value of the call chain.

As with aspects it is recommended to use the least powerful advice style necessary. To support that there are some helpers available in ``org.ehrbase.plugin.extensionpoints.ExtensionPointHelper`` to use if you do not need the full around scope.
Available helpers include:

* before: use this if your logic only works with the input object before proceeding (i.e. modifying the arguments)
* after: use this if your logic only works with the return value of the service call (i.e. send a success notification to some message queue)
* beforeAndAfter: use this if you need to do some logic before and after the call, but the two parts do not need to interact or share data

For a (very) simple example of how to use the helpers please refer to the `Example Plugin <https://github.com/ehrbase/ExamplePlugin/blob/master/simple-plugin/src/main/java/org/ehrbase/example_plugin/CompositionListener4.java>`_


Plugin configuration
--------------------

Plugins can also be configured externally to create plugins which can react to their environment.

Any file under ``${plugin-config-dir}\${plugin.id}`` of format ``XML,JSON,YAML`` will be accessible as property in the spring application context.
See `web example plugin <https://github.com/ehrbase/ExamplePlugin/blob/master/web-plugin/src/main/java/org/ehrbase/example_web_plugin/TestProperty.java>`_
