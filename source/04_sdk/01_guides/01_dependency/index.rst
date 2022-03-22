SDK as dependency
=================

.. warning:: WIP

To make use of the SDK's features it needs to be included as dependency.

Release-artifacts should be taken from `maven central <https://search.maven.org/search?q=g:org.ehrbase.openehr.sdk>`_.

If you need a specific non-release-version, you can get it from `jitpack.io <https://jitpack.io/#ehrbase/openEHR_SDK>`_. Consider that there is a different *groupId* comparing to maven central.

For instance, to build a simple client, include the ``client`` module as dependency.

Depending on the project structure and used dependency management tools this might look like the following ``pom.xml`` snipped in a Maven example:

**Release-Version** (recommended)

.. code-block:: xml

    <properties>
        <!-- ... -->
        <ehrbase.sdk.version>$RELEAE_VERSION</ehrbase.sdk.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- ... -->
            <dependency>
                <groupId>org.ehrbase.openehr.sdk</groupId>
                <artifactId>client</artifactId>
                <version>${ehrbase.sdk.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <!-- ... -->
        <dependency>
            <groupId>org.ehrbase.openehr.sdk</groupId>
            <artifactId>client</artifactId>
        </dependency>
    </dependencies>

**Any non-release-version**

.. code-block:: xml

    <properties>
        <!-- ... -->
        <ehrbase.sdk.version>$VERSION_TAG_OR_LATEST_COMMIT_HASH</ehrbase.sdk.version>
    </properties>

    <repositories>
        <!-- ... -->
        <!-- external -->
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>

    <dependencyManagement>
        <dependencies>
            <!-- ... -->
            <dependency>
                <groupId>com.github.ehrbase.openEHR_SDK</groupId>
                <artifactId>client</artifactId>
                <version>${ehrbase.sdk.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <!-- ... -->
        <dependency>
            <groupId>com.github.ehrbase.openEHR_SDK</groupId>
            <artifactId>client</artifactId>
        </dependency>
    </dependencies>
