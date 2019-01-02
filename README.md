# Kotlin Microservices Springboot Template

### Basic Setup / Structure
You have to start the stack in the right order with following command in each root folder: `mvn spring-boot:run`
1. Start the [Configuration Server](config-server)
2. Start the Service [Discovery Server](discovery-server)
3. Start the [Example Microservice](example-ms)
4. Start the [Gateway Server](gateway)

### Docker
Each server has a dockerfile. They can be found in the `Docker` folder. They basically take the generated jar file and starting them. All of the servers also have a docker maven plugin to automatically build and run inside a container.

## Configuration-Server (Spring Cloud - Config Server)
A Configuration Server allows us to serve any microservice the configuration that it may need, so when a microservice starts, it will require no more configurations than just the one from where the Configuration Service is located.

#### Configuration
Stored in the [`resources/application.yml`](config-server/src/main/resources/application.yml) file
```yaml
server:
  port: 8888

spring:
  profiles:
    active: native
  cloud:
    config:
      server:
        native:
          search-locations: classpath:config/
```
The configuration: `search-locations` includes all property/yaml files under the `config` folder

## Discovery-Server (Spring Cloud - Netflix Eureka)
Service Discovery Servers allow us to dynamically register the instances of our microservices. Having a dynamic list of instances could be used either by applications or other microservices when they need to perform a request. They help us to dynamically manage when instances are starting or stopping to give an accurate view on the scaling of our microservices. Finally, they provide a mechanism to disconnect instances where they are not available using a Heart-beat mechanism.

#### Configuration
Stored in the [`resources/application.yml`](discovery-server/src/main/resources/application.yml) file
```yaml
server:
  port: 8761
spring:
  application:
    name: "discovery-server"
```

## Example Microservice
This microservice exposes a simple `GET /greetings` method responding with a String which is retrieved from the config server. See below:

#### Configuration
Stored in the [`resources/config/example-ms.yml`](config-server/src/main/resources/config/example-ms.yml) file at the Configuration-Server
```yaml
microservice:
  example:
    greetings: "hello for the example-ms application"
```
This just defines a custom text that will be shown as a response message

## Gateway (Spring Cloud - Netflix Zuul)
This Gateway allows us to simplify how we can expose a microservice to be used by applications or to other microservices, which simplifies how we can perform load balancing between the instances and expose a clear mechanism to access them.

In my template I use .

#### Configuration
Stored in the [`resources/config/gateway.yml`](config-server/src/main/resources/config/gateway.yml) file at the Configuration-Server
```yaml
zuul:
  ignoredServices: '*'
  routes:
    example-ms:
      path: /message/**
      serviceId: example-ms
```
I defined a custom route for the example microservice, normally the example service would be reachable via `http://localhost:8080/example-ms/greetings` now it's reachable via `http://localhost:8080/message/greetings`

# Disclaimer
To create this template I followed the book: "**_Hands-On Microservices with Kotlin_**" from **_Juan Antonio Medina Iglesias_**.
Some of the texts you can find above are copy/pasted from this book.
