# Image that will build the app
FROM maven:latest as builder

RUN mkdir /tmp/app
COPY pom.xml /tmp/app
COPY src /tmp/app/src/

WORKDIR /tmp/app

RUN mvn clean package

# Image that will run the app
FROM openjdk:latest

ARG JAR_FILE=target/*.jar
COPY --from=builder /tmp/${JAR_FILE} /usr/app.jar

EXPOSE 8080

ENTRYPOINT [ "java", "-jar", "/usr/app.jar"]