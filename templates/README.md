## Apache Karaf {{= fp.config.version.version}}

A simple docker build for installing a vanilla Karaf {{= fp.config.version.version}} below */opt/karaf*. It comes out of the box and is intended for use for integration testing.

During startup a directory specified by the environment variable `DEPLOY_DIR` (*/deployments* by default) is checked for .war files. If there are any, they are linked into the *webapps/* directory for automatic deployment. This plays nicely with the [docker-maven-plugin](https://github.com/fabric8io/docker-maven-plugin/) and its 'assembly' mode which can automatically create Docker data container with Maven artifacts exposed from a directory */deployments*.

### Agent Bond

For this image [Agent Bond](https://github.com/fabric8io/agent-bond) is enabled. Agent Bond exports metrics from [Jolokia](http://www.jolokia.org) and [jmx_exporter](https://github.com/prometheus/jmx_exporter).

The agent is installed as `/opt/agent-bond/agent-bond.jar` and enables the following agents by default:

* [Jolokia](http://www.jolokia.org) : version **{{= fp.jolokiaVersion }}** and port **8778**
* [jmx_exporter](https://github.com/prometheus/jmx_exporter): version **{{= fp.jmxExporterVersion }}** and port **9779**

You can influence the behaviour of `agent-bond-opts` by setting various environment variables:

{{= fp.block('agent-bond','readme',{ 'fp-no-files' : true }) }}

Features:

* Karaf Version: **{{= fp.config.version.version}}**
* Java Base image: **{{= fp.config.version.from.image }}**
* Port: **8080**
* User **admin** (Password: **admin**) has been added to access the admin
  applications */host-manager* and */manager*)
* Documentation and examples have been removed
* Command: `/opt/karaf/bin/deploy-and-run.sh` which links .kar files from */maven* to
  */opt/karaf/deploy* and then calls `karaf run`
* Sets `-Djava.security.egd=file:/dev/./urandom` for faster startup times
  (though a bit less secure)

### Debugging

You can enable remote debugging by setting `JAVA_DEBUG` to any value:

  * **JAVA_DEBUG** If set remote debugging will be switched on
  * **JAVA_DEBUG_PORT** Port used for remote debugging. Default: 5005
