# Dockerizing Mule EE

# TODO: use a custom java image?
FROM          java:8u91-jdk

LABEL         version="ESB 3.8.4"
LABEL         description="Mule ESB Standalone 3.8.4."

# MAINTAINER  x@y

# ARG           MULE_ESB_VERSION

# Define environment variables.

ENV           MULE_ESB_VERSION 3.8.4
ENV           MULE_AGENT_VERSION 1.7.0
ENV           MULE_HOME /opt/mule
ENV           MULE_BASE /opt/mule

# Install Mule EE
WORKDIR       /opt
RUN           wget https://s3.amazonaws.com/new-mule-artifacts/mule-ee-distribution-standalone-$MULE_ESB_VERSION.tar.gz
RUN           tar xvzf mule-ee-distribution-standalone-$MULE_ESB_VERSION.tar.gz
RUN           rm mule-ee-distribution-standalone-$MULE_ESB_VERSION.tar.gz
RUN           ln -s mule-enterprise-standalone-$MULE_ESB_VERSION/ mule

# Install Mule Licence
WORKDIR       /opt/mule
COPY          ./mule-ee-license.lic /opt/mule/conf/
RUN           bin/mule -installLicense conf/mule-ee-license.lic
RUN           rm conf/mule-ee-license.lic

# Update Mule Agent
WORKDIR       /opt/mule
RUN           wget http://mule-agent.s3.amazonaws.com/$MULE_AGENT_VERSION/agent-setup-$MULE_AGENT_VERSION.zip
RUN           rm bin/amc_setup
RUN           rm bin/amc_setup.bat
RUN           unzip agent-setup-$MULE_AGENT_VERSION.zip -d bin/
RUN           bin/amc_setup --update
RUN           rm agent-setup-$MULE_AGENT_VERSION.zip

# Configure and Register Mule Agent for Hybrid Management
WORKDIR       /opt/mule/bin
RUN           ./amc_setup --encrypt --hybrid <token> $HOSTNAME

# TODO: add/edit mule-agent.yaml?

# Define mount points.
# VOLUME      ["/opt/mule/logs", "/opt/mule/conf", "/opt/mule/apps", "/opt/mule/domains"]

# TODO: configure wrapper.conf?

# TODO: configure directory permissions?

# Configure external access:

# HTTP Service Port
# Expose the necessary port ranges as required by the Mule Apps
EXPOSE        8081
EXPOSE        8443

# Mule remote debugger
EXPOSE        5000

# Mule JMX port (must match Mule config file)
EXPOSE        1098

# Environment and execution:
WORKDIR       /opt/mule
CMD           bin/mule
