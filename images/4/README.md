## Apache Karaf 4.0.5

A simple docker build for installing a vanilla Karaf 4.0.5 below */opt/karaf*. It comes out of the box and is intended for use for integration testing.

During startup a directory specified by the environment variable `DEPLOY_DIR` (*/maven* by default) is checked for .war files. If there are any, they are linked into the *webapps/* directory for automatic deployment. This plays nicely with the Docker maven plugin from https://github.com/rhuss/docker-maven-plugin/ and its 'assembly' mode which can automatically create Docker data container with Maven artifacts exposed from a directory */maven*.

### Agent Bond

For this image [Agent Bond](https://github.com/fabric8io/agent-bond) is enabled. Agent Bond exports metrics from [Jolokia](http://www.jolokia.org) and [jmx_exporter](https://github.com/prometheus/jmx_exporter).

Please refer to the base image's [documentation](https://github.com/fabric8io-images/java/tree/master/images/centos/openjdk8/jre) for available options to tune the Java JVM.

Features:

* Karaf Version: **4.0.5**
* Java Base image: **fabric8/java-centos-openjdk8-jre:1.1.4**
* Port: **8080**
* User **admin** (Password: **admin**) has been added to access the admin
  applications */host-manager* and */manager*)
* Documentation and examples have been removed
* Command: `/opt/karaf/bin/deploy-and-run.sh` which links .kar files from */maven* to
  */opt/karaf/deploy* and then calls `karaf run`
* Sets `-Djava.security.egd=file:/dev/./urandom` for faster startup times
  (though a bit less secure)
