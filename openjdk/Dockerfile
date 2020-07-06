# Image that will build the app
FROM maven:latest as builder

COPY pom.xml /tmp/app
COPY src /tmp/app/src/

WORKDIR /tmp/app

RUN mvn clean package

# Image that will run the app
FROM openjdk:latest

COPY --from=builder /tmp/target/app.jar /usr/app.jar

EXPOSE 8080

ENTRYPOINT [ "java", "-jar", "/usr/app.jar"]