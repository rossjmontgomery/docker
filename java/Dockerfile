FROM ubuntu:16.04

ARG JAVA_ARCHIVE_NAME

ENV JAVA_HOME=/opt/java/java
ENV PATH=$JAVA_HOME/bin:$PATH

WORKDIR /opt/java

COPY $JAVA_ARCHIVE_NAME .
RUN tar xvzf $JAVA_ARCHIVE_NAME
RUN rm $JAVA_ARCHIVE_NAME

RUN ln -s /opt/java/$(ls) java
