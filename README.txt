This project assists in testing (SOAP) web services and web applications by providing an application to define and run tests. To this end it contains a baseline installation of FitNesse (an acceptance testing wiki framework) and some FitNesse fixture (base) classes.

The fixtures provided aim to assist in testing (SOAP) web services and web applications (using Selenium) minimizing the amount of (custom) Java code needed to define tests.

The baseline Fitnesse installation offers the following features:
1. Ability to easily create a standalone (no JDK or Maven required) Fitnesse environment.
2. Run Fitnesse tests on a build server, reporting the results in both JUnit XML format and HTML.
3. Fitnesse installation for fixture developers containing:
    - the fixture base classes (and Selenium drivers from Chrome and Internet Explorer),
    - Maven classpath plugin (such that tests can use all dependencies from pom.xml),
    - HSAC's fitnesse-plugin to add additional Wiki features (random values, calculating relative dates,
      Slim scenarios without need to specify all parameters, Slim scripts that take a screenshot after each step),
    - easy fixture debugging,

1. To create the standalone Fitnesse installation:
Execute 'mvn clean test dependency:copy-dependencies', the standalone installation is present in the wiki directory (and can be distributed by just copying this).
This standalone installation can be started using 'java -jar fitnesse-standalone.jar' from the wiki directory.

A zip file containing the standalone version of the latest version of this project can be downloaded from the Cloud Bees
build server (https://fhoeben.ci.cloudbees.com/job/hsac-fitnesse-fixtures/).


2. To run the tests on a build server:
Have the build server checkout the project and execute 'mvn clean test-compile failsafe:integration-test'.
The result in JUnit XML results can be found in: target/failsafe-reports (most build servers will pick these up automatically)
The HTML results can be found in: target/fitnesse-results/index.html

The Fitnesse suite to run can be specified by changing the value of the @Suite annotation in nl.hsac.fitnesse.fixture.FixtureDebugTest,
or (preferably) by adding a system property, called fitnesseSuiteToRun, specifying the suite to run to the build server's mvn execution.

The Selenium configuration (e.g. what browser on what platform) to use when testing websites can be overridden by using
system properties (i.e. seleniumGridUrl and either seleniumBrowser or seleniumCapabilities).
This allows different configurations on the build server to test with different browsers, without requiring different
Wiki content, but only requiring a different build configuration.

Example configurations for Windows (https://fhoeben.ci.cloudbees.com/job/hsac-fitnesse-fixtures-run-FitNesseTests/) and
OSX (https://fhoeben.ci.cloudbees.com/job/hsac-fitnesse-fixtures-run-FitNesseTests-Mac/) have been set up at Cloud Bees
showing what kind of report a run using a Sauce Labs selenium driver generates.


3. Fixture developer installation:
Import this project in your favorite Java IDE (with Maven support).

To start Fitnesse: have the IDE execute 'mvn compile exec:exec'. The port used by Fitnesse can be controlled by changing the fitnesse.port property's value in pom.xml.
Fitnesse will be available at http://localhost:<fitnesse.port>/, example usage of the symbols and fixtures can be seen in http://localhost:<fitnesse.port>/HsacExamples.

To debug a fixture used in a Fitnesse page: change the @Suite annotation's value to contain page name in nl.hsac.fitnesse.fixture.FixtureDebugTest, then just debug this test.
