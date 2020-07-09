SDK as dependency
=================

.. warning:: WIP

To make use of the SDK's features it needs to be included as dependency.

For instance, to build a simple client, include the ``client`` module as dependency.

Depending on the project structure and used dependency management tools this might look like the following ``pom.xml`` snipped in a Maven example:

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