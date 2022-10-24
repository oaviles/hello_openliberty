# DevSquad Open Liberty Accelerator

## Overview
The sample application provides a simple example of how to get started with Open Liberty. It provides a REST API that retrieves the system properties in the JVM and a web based UI for viewing them. It also uses MicroProfile Config, MicroProfile Health and MicroProfile Metrics to demonstrate how to use these specifications in an application that maybe deployed to kubernetes.

Reference Documentation: [Open Liberty / WebSphere Liberty on AKS](https://learn.microsoft.com/en-us/azure/developer/java/ee/websphere-family)
![Open Liberty logo](https://learn.microsoft.com/en-us/azure/developer/java/ee/media/websphere-family/websphere-family.svg)

## Project structure

- `src/main/java` - the Java code for the Project
  - `io/openliberty/sample`
    - `config`
      - `ConfigResource.java` - A REST Resource that exposes MicroProfile Config via a /rest/config GET request
      - `CustomConfigSource.java` - A MicroProfile Config ConfigSource that reads a json file.
    - `system`
      - `SystemConfig.java` - A CDI bean that will report if the application is in maintenance. This supports the config variable changing dynamically via an update to a json file.
      - `SystemHealth.java` - A MicroProfile Health check that reports DOWN if the application is in maintenance and UP otherwise.
      - `SystemResource.java` - A REST Resource that exposes the System properties via a /rest/properties GET request. Calls to this GET method have MicroProfile Timer and Count metrics applied.
      - `SystemRuntime.java` - A REST Resource that exposes the version of the Open Liberty runtime via a /rest/runtime GET request.
    - `SystemApplication.java` - The Jakarta RESTful Web Services Application class
  - `liberty/config/server.xml` - The server configuration for the liberty runtime
  - `META-INF` - Contains the metadata files for MicroProfile Config including how to load CustomConfigSource.java
  - `webapp` - Contains the Web UI for the application.
  - `test/java/it/io/openliberty/sample/health`
    - `HealthIT.java` - Test cases for a sample application running on `localhost`
    - `HealthUtilIT.java` - Utility methods for functional tests
- `resources/CustomConfigSource.json` - Contains the data that is read by the MicroProfile Config ConfigSource.
- `Dockerfile` - The Dockerfile for building the sample
- `pom.xml` - The Maven POM file

## Run the Sample in Azure Kubernetes Service

Clone the repor and create all the necesario Github Secrets to run all the Github Workflows. To run the sample using Azure Kubertenes Service run all the [Github Worflows](https://github.com/oaviles/hello_openliberty/actions) based on numeric order or create your own Github Workflow based on code provided in this repo. 


### Access the application
Open a browser to http://[your on IP address] deployed by GitHub Workflows called ["Deploy API"](https://github.com/oaviles/hello_openliberty/blob/main/.github/workflows/deploy-api.yml)

![image](https://user-images.githubusercontent.com/3076261/117993383-4f34c980-b305-11eb-94b5-fa7319bc2850.png)

## Run the functional tests

The test cases uses [JUnit 5](https://junit.org/junit5/) and 
[Maven Failsafe Plugin](https://maven.apache.org/surefire/maven-failsafe-plugin/index.html) defined 
in [`pom.xml`](pom.xml).

> Note: Sample appplication must be running on `http://[your on IP address]` before running the test cases. 
> <br>
> See [`HealthUtilIT.java`](src/test/java/it/io/openliberty/sample/health/HealthUtilIT.java) to change 
> the change the sample application target URL.

To run the test cases against a running sample application, use the following command
```
mvnw failsafe:integration-test
```

To view the test results, look at the console output or look under 
directory  `target/failsafe-reports`
