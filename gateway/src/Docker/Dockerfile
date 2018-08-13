FROM openjdk:8-jdk-alpine

ADD maven/*.jar gateway.jar

ENTRYPOINT ["java","-jar", "gateway.jar"]