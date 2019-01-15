# ThreadFix

[![Build Status](https://travis-ci.org/ContinuousSecurityTooling/threadfix.svg?branch=master)](https://travis-ci.org/ContinuousSecurityTooling/threadfix)
[![Build Status](https://jenkins.martinreinhardt-online.de/buildStatus/icon?job=OSS/continuous-security-tooling/threadfix/master)](https://jenkins.martinreinhardt-online.de/blue/organizations/jenkins/OSS%2Fcontinuous-security-tooling%2Fthreadfix/branches/)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=com.denimgroup.threadfix%3Amaster-pom&metric=alert_status)](https://sonarcloud.io/dashboard?id=com.denimgroup.threadfix%3Amaster-pom)

**NOTE**: If you wish to download the latest build of ThreadFix please visit the [ThreadFix download page](http://www.threadfix.org/download/). Please **DO NOT** use the "Download ZIP" function from GitHub. If you DO use the "Download ZIP" function from GitHub you will just get a dump of the source code, but no ready-to-run Tomcat webserver and other facilities that make it really easy to get up and running with ThreadFix quickly. The normal [ThreadFix download](http://www.threadfix.org/download/) build comes pre-packaged and ready-to-run and is the preferred way to start using ThreadFix. You can set up your own [development environment](https://github.com/denimgroup/threadfix/wiki/Environment-Setup) but it is advised that first time users start with the [pre-packaged build](http://www.threadfix.org/download/).

ThreadFix is a software vulnerability aggregation and management system that reduces the time it takes to fix software vulnerabilities. ThreadFix imports the results from dynamic, static and manual testing to provide a centralized view of software security defects across development teams and applications. The system allows companies to correlate testing results and streamline software remediation efforts by simplifying feeds to software issue trackers. By auto generating application firewall rules, this tool allows organizations to continue remediation work uninterrupted. ThreadFix empowers managers with
vulnerability trending reports that show progress over time, giving them justification for their efforts.

ThreadFix is licensed under the Mozilla Public License (MPL) version 2.0.

The main GitHub site for ThreadFix can be found here:

https://github.com/denimgroup/threadfix/

The Google Group for ThreadFix can be found here:

https://groups.google.com/forum/#!forum/threadfix

Instructions on setting up a development environment can be found here:

https://github.com/denimgroup/threadfix/wiki/Development-Environment-Setup

Further documentation can be found online here:

https://github.com/denimgroup/threadfix/wiki

Submit bugs to the GitHub issue tracker:

https://github.com/denimgroup/threadfix/issues

ThreadFix is a platform with a number of components. Each subdirectory should have its own pom.xml files to support Maven builds. The major components in the repository include:

* **threadfix-cli-endpoints** - Command-line utility to calculate the attack surface of an application and print it to standard output. This relies on the Hybrid Analysis Mapping (HAM) capabilities in the threadfix-ham/ component.
* **theadfix-cli** - Command-line client for ThreadFix. This allows for scripting and automation of the ThreadFix platform.
* **threadfix-extras** - Experimental tools and ThreadFix proof-of-concept projects.
* **threadfix-ham** - Hybrid Analysis Mapping (HAM) technology used in ThreadFix that performs lightweight static analysis of application source code to calculate attack surfaces and map application attack surface endpoints to source code locations.
* **threadfix-ide-plugin** - IDE plugins for Eclipse and IntelliJ that pulls vulnerability data from ThreadFix and highlights these vulnerabilities in application source code.
* **threadfix-main** - Main ThreadFix server application. This is a Java-based Spring/Hibernate web application with associated web services. Other components of the ThreadFix platform call into the ThreadFix server.
* **threadfix-scanner-plugin** - Scanner plugins that can connect to a ThreadFix server and import an application's attack surface to improve the thoroughness of dynamic scanning. Also allows for exporting scan results directly into ThreadFix (rather than saving files and uploading them.)
* **threadfix-update** - Update scripts to upgrade the ThreadFix server database between versions.


## Development

If you run into issues with resolving jar, run the following:

```
./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/zap/2.2.2/zap-2.2.2.jar -DgroupId=com.owasp.zap -DartifactId=zap -Dversion=2.2.2 -Dpackaging=jar
./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/zaphelp/2.2.2/zaphelp-2.2.2.jar -DgroupId=com.owasp.zap -DartifactId=zaphelp -Dversion=2.2.2 -Dpackaging=jar
./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/xom/1.2.6/xom-1.2.6.jar -DgroupId=com.owasp.zap -DartifactId=xom -Dversion=1.2.6 -Dpackaging=jar
./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/java-getopt/1.0.13/java-getopt-1.0.13.jar -DgroupId=com.owasp.zap -DartifactId=java-getopt -Dversion=1.0.13 -Dpackaging=jar
./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/jgoodies-looks/2.4.0/jgoodies-looks-2.4.0.jar -DgroupId=com.owasp.zap -DartifactId=jgoodies-looks -Dversion=2.4.0 -Dpackaging=jar
./mvnw install:install-file -Dfile=threadfix-scanner-plugin/zaproxy/lib/com/owasp/zap/lablib-checkboxtree/3.2/lablib-checkboxtree-3.2.jar -DgroupId=com.owasp.zap -DartifactId=lablib-checkboxtree -Dversion=3.2 -Dpackaging=jar
./mvnw install:install-file -Dfile=lib/com/microsoft/tfs/sdk/com.microsoft.tfs.sdk/11.0.0/com.microsoft.tfs.sdk-11.0.0.jar -DgroupId=com.microsoft.tfs.sdk -DartifactId=com.microsoft.tfs.sdk -Dversion=11.0.0 -Dpackaging=jar
```