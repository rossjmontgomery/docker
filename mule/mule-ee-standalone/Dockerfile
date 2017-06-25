# Dockerizing Mule EE

FROM          java:8u91-jdk

# LABEL       version="ESB 3.8.4"
# LABEL       description="Mule ESB Standalone 3.8.4."

# MAINTAINER  x@y

# ARG           MULE_ESB_VERSION

# Define environment variables.

ENV           MULE_ESB_VERSION 3.8.4
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
COPY          ./license-mule.lic /opt/mule/conf/
RUN           bin/mule -installLicense conf/license-mule.lic
RUN           rm conf/license-mule.lic

# Install Mule Agent
WORKDIR       /opt/mule
COPY          ./agent-setup-$MULE_AGENT_VERSION.zip /opt/
RUN           rm bin/amc_setup
RUN           rm bin/amc_setup.bat
RUN           unzip /opt/agent-setup-$MULE_AGENT_VERSION.zip -d /opt/mule/bin/
RUN           bin/amc_setup -U
RUN           rm /opt/agent-setup-$MULE_AGENT_VERSION.zip

# Configure Mule Agent
# Run AMC Setup
# WORKDIR     /opt/mule-enterprise-standalone-3.7.2/bin/
# RUN         ./amc_setup -A http://52.74.225.250:8080/hybrid/api/v1 -W "wss://52.74.225.250:8443/mule" -F https://52.74.225.250/apiplatform -C https://52.74.225.250/accounts -H 61aa89ba-611a-449a-af83-f13921cee831---2 MULE-$HOSTNAME

# Define mount points.
# VOLUME      ["/opt/mule/logs", "/opt/mule/conf", "/opt/mule/apps", "/opt/mule/domains"]

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
