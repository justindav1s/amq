FROM docker-registry.default.svc:5000/openshift/amq-broker-73-openshift:7.3

# Temporary switch to root
USER root

# Prometheus JMX exporter agent
RUN mkdir -p /opt/prometheus/etc \
    && curl http://central.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.11.0/jmx_prometheus_javaagent-0.11.0.jar \
        -o /opt/prometheus/jmx_prometheus_javaagent.jar
ADD ./prometheus/prometheus-config.yml /opt/prometheus/prometheus-config.yml
RUN chmod 444 /opt/prometheus/jmx_prometheus_javaagent.jar \
    && chmod 444 /opt/prometheus/prometheus-config.yml \
    && chmod 775 /opt/prometheus/etc \
    && chgrp root /opt/prometheus/etc

EXPOSE 9779


COPY ./bin/custom-launch.sh /opt/amq/bin/custom-launch.sh
RUN chmod  777 /opt/amq/bin/custom-launch.sh

# Override default launch.
CMD [ "/opt/amq/bin/custom-launch.sh" ]


## S2I requires a numeric, non-0 UID. This is the UID for the jboss user in the base image
USER 185

