FROM maven:3.6-jdk-8 as builder

COPY . .
RUN mvn install


FROM openliberty/open-liberty:javaee8-ubi-min

ENV INSTALL_DIR /opt/ol/wlp/
ENV SERVER_DIR /opt/ol/wlp/usr/servers/defaultServer/

# Add MySQL-Java-Connector to container
RUN curl -O ${INSTALL_DIR}lib/mysql-connector-java-8.0.16.jar https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.16/mysql-connector-java-8.0.16.jar

# Copy build artefacts
COPY --from=builder target/health-api.war ${SERVER_DIR}apps

# Copy configs
COPY liberty-mysql/mysql-driver.xml ${SERVER_DIR}configDropins/defaults/
COPY liberty/server.xml $SERVER_DIR
COPY liberty/jvm.options $SERVER_DIR

EXPOSE 9080
