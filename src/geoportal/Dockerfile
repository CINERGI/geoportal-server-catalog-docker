FROM maven:3-jdk-8 AS gptbuild

MAINTAINER David Valentine dwvalentine@usdc.edu


# Install wget and install/updates certificates
# not going to use this in production, so don't do it.
#RUN apt-get update \
# && apt-get install -y -q --no-install-recommends \
#    ca-certificates \
#    git \
#    maven \
#    ca-certificates-java="$CA_CERTIFICATES_JAVA_VERSION" \
# && apt-get clean \
# && rm -r /var/lib/apt/lists/*

# Install geoportal web application
ENV GITURL=https://github.com/cinergi/geoportal-server-catalog.git

WORKDIR /tmp
RUN git clone ${GITURL} geoportal-server-catalog
WORKDIR /tmp/geoportal-server-catalog/geoportal
RUN mvn package -DskipTests

# Install harvester application
ENV GITURL=https://github.com/cinergi/geoportal-server-harvester.git

WORKDIR /tmp
RUN git clone ${GITURL} harvester
WORKDIR /tmp/harvester
RUN mvn package -DskipTests

WORKDIR $CATALINA_HOME/webapps/

#######################
# just tomcat
#######################
FROM tomcat:9-jre8

RUN apt-get update \
 && apt-get install -y -q --no-install-recommends \
    ca-certificates \
#   git \
#   maven \
    ca-certificates-java="$CA_CERTIFICATES_JAVA_VERSION" \
 && apt-get clean \
 && rm -r /var/lib/apt/lists/*
 COPY tomcat-users.xml $CATALINA_HOME/conf

WORKDIR $CATALINA_HOME/webapps/

COPY --from=gptbuild /tmp/geoportal-server-catalog/geoportal/target/*.war $CATALINA_HOME/webapps/geportal.war
COPY --from=gptbuild /tmp/harvester/geoportal-application/geoportal-harvester-war/target/*.war $CATALINA_HOME/webapps/harvester.war

WORKDIR $CATALINA_HOME/webapps/
# RUN cp /tmp/geoportal-server-catalog/geoportal/target/*.war geportal.war

# RUN cp /tmp/harvester/geoportal-application/geoportal-harvester-war/target/*.war harvester.war
