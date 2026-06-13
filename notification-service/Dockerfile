FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /workspace

COPY pom.xml ./
# grpc-common-lib УБРАЛИ!
COPY notification-service ./notification-service

RUN mvn -q -DskipTests -pl notification-service -am clean package

FROM eclipse-temurin:21-jre
WORKDIR /app

COPY --from=build /workspace/notification-service/target/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app/app.jar"]