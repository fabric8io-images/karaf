{{
  var base = "apache-karaf";
  var majorVersion = fp.config.version.version.replace(/^([^.]+).*$/,"$1");
  var toRemove = fp.config.version.toRemove;
  var roleFile = fp.config.version.roleFile;
}}
FROM {{= fp.config.version.from.image }}

MAINTAINER {{= fp.maintainer }}

EXPOSE 8181 8101

ENV KARAF_VERSION {{= fp.config.version.version }}

USER root

{{= fp.block("agent-bond", "install", { dest: "/opt/run-java-options" }) }}

RUN curl {{= fp.config.version.url }} -o /tmp/karaf.tar.gz \
 && tar xzf /tmp/karaf.tar.gz -C /opt/ \
 && ln -s /opt/{{= base }}-${KARAF_VERSION} /opt/karaf \
 && chown -R jboss /opt/karaf /opt/{{= base }}-${KARAF_VERSION} \
 && rm /tmp/karaf.tar.gz

# Add roles
ADD {{= roleFile.file}} /opt/{{= base}}-${KARAF_VERSION}/{{= roleFile.dir}}

# Startup script
ADD deploy-and-run.sh /opt/karaf/bin/

RUN chmod a+x /opt/karaf/bin/deploy-and-run.sh \
 && rm -rf {{~toRemove :value:index}}/opt/karaf/deploy/{{=value}} {{~}} \
 && sed -i 's/^\(.*rootLogger.*\)out/\1stdout/' /opt/karaf/etc/org.ops4j.pax.logging.cfg

 {{= fp.block("run-java-sh","copy-options",{dest: "/opt/"}) }}

ENV PATH $PATH:/opt/karaf/bin

CMD /opt/karaf/bin/deploy-and-run.sh

USER jboss
