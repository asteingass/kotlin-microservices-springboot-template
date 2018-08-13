FROM openjdk:8-jdk-alpine

ADD maven/*.jar discovery-server.jar

ENTRYPOINT ["java","-jar", "discovery-server.jar"]