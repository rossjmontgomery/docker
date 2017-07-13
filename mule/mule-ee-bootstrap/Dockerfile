# Dockerizing Mule EE

# TODO: use a custom java image?
FROM          java:8u91-jdk

LABEL         version="ESB 3.8.4"
LABEL         description="Mule ESB Standalone 3.8.4. with server (re-)registration bootstrap app"

ARG           MULE_ESB_VERSION=3.8.4
ARG           MULE_AGENT_VERSION=1.7.0
ARG           MULE_TRUSTSTORE_PASSWORD=""

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

# TODO: add/edit mule-agent.yaml?

# Import Anypoint Platform certificate into truststore.
RUN           echo $MULE_TRUSTSTORE_PASSWORD
WORKDIR       /opt/mule
RUN           mkdir security
WORKDIR       /opt/mule/security
COPY          ./anypoint.mulesoft.com.cer .
RUN           keytool -import -noprompt -alias anypoint.mulesoft.com -file anypoint.mulesoft.com.cer -keystore truststore.jks -storepass $MULE_TRUSTSTORE_PASSWORD
RUN           rm anypoint.mulesoft.com.cer

# Copy the server registration bootstrap app to the apps directory.
COPY          ./mule-runtime-agent-bootstrapper.zip /opt/mule/apps

# Define mount points.
# VOLUME      ["/opt/mule/logs", "/opt/mule/conf", "/opt/mule/apps", "/opt/mule/domains"]
VOLUME        ["/opt/mule/logs"]

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

# Environment and execution: ensure only the bootstrapper runs on first execution.
WORKDIR       /opt/mule
COPY          ./mule-runner.sh bin/ && chmod u+x bin/mule-runner.sh
CMD           ["bin/mule-runner.sh"]
