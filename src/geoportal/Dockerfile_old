#FROM fabric8/java-centos-openjdk8-jdk:1.5.1
FROM tomcat:9-jre8
#MAINTAINER Mathias Kühn kuehn@synerva.de
MAINTAINER David Valentine dwvalentine@usdc.edu

# Install wget and install/updates certificates
RUN apt-get update \
 && apt-get install -y -q --no-install-recommends \
    ca-certificates \
    git \
    maven \
    openjdk-8-jdk="$JAVA_DEBIAN_VERSION" \
#    ca-certificates-java="$CA_CERTIFICATES_JAVA_VERSION" \
 && apt-get clean \
 && rm -r /var/lib/apt/lists/*

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

# Install geoportal web application
ENV GITURL=https://github.com/cinergi/geoportal-server-catalog.git

ADD $GPTURL /tmp/geoportal.zip

WORKDIR /tmp
RUN git clone ${GITURL} geoportal-server-catalog
WORKDIR /tmp/geoportal-server-catalog/geoportal
RUN mvn package -DskipTests

WORKDIR $CATALINA_HOME/webapps/
RUN cp /tmp/geoportal-server-catalog/geoportal/target/*.war geportal.war

COPY tomcat-users.xml $CATALINA_HOME/conf

# Install harvester application
ENV GITURL=https://github.com/cinergi/geoportal-server-harvester.git

WORKDIR /tmp
RUN git clone ${GITURL} harvester
WORKDIR /tmp/harvester
RUN mvn package -DskipTests

WORKDIR $CATALINA_HOME/webapps/

RUN cp /tmp/harvester/geoportal-application/geoportal-harvester-war/target/*.war harvester.war
