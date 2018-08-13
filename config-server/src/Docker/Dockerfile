FROM openjdk:8-jdk-alpine

ADD maven/*.jar config-server.jar

ENTRYPOINT ["java","-jar", "config-server.jar"]