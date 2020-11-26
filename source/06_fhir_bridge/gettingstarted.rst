Installation
============

-  Install Java 11 (preferably https://adoptopenjdk.net/)

   -  Check for correct version:

   ::

       java[c] -version

   -  for Linux:
      https://adoptopenjdk.net/installation.html#linux-pkg

   ::

      sudo apt-get install adoptopenjdk-11-hotspot
      sudo apt-get install adoptopenjdk-11-hotspot-jre 

-  Install Maven 3

   -  Instructions: https://maven.apache.org/install.html
   -  Check for correct version:

   ::

      mvn --version

   -  for Linux:

   ::

      # in (.bashrc)
      # 2020-08-05 add maven to path
       export PATH="/home/birgit/local/apache-maven-3.6.3/bin:$PATH"
       $ source ~/.bashrc

-  install git / git bash 

-  install docker

   ::

      sudo apt-get install docker

-  Install IntelliJ (or similar) if necessary

   ::

      sudo snap install intellij-idea-community --classic

-  Linux: don't forget to set path variables

::

   # in (.bashrc)
   # 2020-08-05 add maven to path
   export PATH="/home/USERNAME/local/apache-maven-3.6.3/bin:$PATH"
   export PATH="/home/USERNAME/Desktop/num/Installer/jdk-11.0.8+10/bin:$PATH"
   export JAVA_HOME="/home/USERNAME/Desktop/num/Installer/jdk-11.0.8+10"

-  Windows: set path (if not automatically done):

::

   PATH="C:\Program Files\AdoptOpenJDK\jdk-11.0.7.10-hotspot\bin"
   PATH="C:\Program Files\apache-maven-3.6.3-bin\apache-maven-3.6.3\bin"
   JAVA_HOME="C:\Program Files\AdoptOpenJDK\jdk-11.0.7.10-hotspot\"
   JAVA_TOOL_OPTIONS="-Dfile.encoding=UTF8"

Database for Audit Logs in FHIR Bridge
======================================

1. add config/application.yml to the root folder of fhir-bridge
2. with the following properties:

::

   ehrbase:
     address:              localhost
     port:                 8080
     path:                 /ehrbase/rest/openehr/v1/
   spring:
     datasource:
       url:                jdbc:postgresql://localhost:9999/fhir_bridge
       username:           fhir_bridge
       password:           fhir_bridge

3. run
   ``docker run --name fhirdb -e POSTGRES_PASSWORD=fhir_bridge -e POSTGRES_USER=fhir_bridge -d -p 9999:5432 postgres``
4. run fhir-bridge