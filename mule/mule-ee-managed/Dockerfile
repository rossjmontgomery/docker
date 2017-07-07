# Dockerizing Mule EE

# TODO: use a custom java image?
FROM          java:8u91-jdk

LABEL         version="ESB 3.8.4"
LABEL         description="Mule ESB Standalone that is named and automatically registered with the Runtime Manager"

ARG           MULE_ESB_VERSION=3.8.4
ARG           MULE_AGENT_VERSION=1.7.0
ARG           MULE_SERVER_NAME=""
ARG           ANYPOINT_PLATFORM_REGISTRATION_TOKEN=""

# Define environment variables.

ENV           MULE_HOME=/opt/mule
ENV           MULE_BASE=/opt/mule

# Install Mule EE
WORKDIR       /opt
RUN           wget https://s3.amazonaws.com/new-mule-artifacts/mule-ee-distribution-standalone-$MULE_ESB_VERSION.tar.gz
RUN           tar xvzf mule-ee-distribution-standalone-$MULE_ESB_VERSION.tar.gz
RUN           rm mule-ee-distribution-standalone-$MULE_ESB_VERSION.tar.gz
RUN           ln -s mule-enterprise-standalone-$MULE_ESB_VERSION/ mule

# Install Mule Licence
# Assumes that licence already exists in the current local directory
WORKDIR       /opt/mule
COPY          ./mule-ee-license.lic conf/
RUN           bin/mule -installLicense conf/mule-ee-license.lic

# Install Mule Agent
WORKDIR       /opt/mule/bin
RUN           wget http://mule-agent.s3.amazonaws.com/$MULE_AGENT_VERSION/agent-setup-$MULE_AGENT_VERSION.zip
RUN           rm amc_setup
RUN           rm amc_setup.bat
RUN           unzip agent-setup-$MULE_AGENT_VERSION.zip

# register server with name provided IFF a registration token has been provided
WORKDIR       /opt/mule/bin
RUN           if [-n $($ANYPOINT_PLATFORM_REGISTRATION_TOKEN | xargs)] then ./amc_setup --encrypt --hybrid $ANYPOINT_PLATFORM_REGISTRATION_TOKEN $MULE_SERVER_NAME fi

# TODO: add/edit mule-agent.yaml?

# Define mount points.
# VOLUME      ["/opt/mule/logs", "/opt/mule/conf", "/opt/mule/apps", "/opt/mule/domains"]
VOLUME        ["/opt/mule/logs"]

# TODO: configure wrapper.conf?

# TODO: configure directory permissions?

# Configure external access:

# HTTP Service Port
# Expose the necessary port ranges as required by the Mule Apps
EXPOSE        8081-8181
EXPOSE        8443-8543

# Mule JMX port (must match Mule config file)
EXPOSE        1098

# Environment and execution:
WORKDIR       /opt/mule
CMD           ["bin/mule"]
