FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /workspace

COPY pom.xml ./
COPY grpc-common-lib ./grpc-common-lib
COPY api-gateway ./api-gateway

RUN mvn -q -DskipTests -pl api-gateway -am clean package

FROM eclipse-temurin:21-jre
WORKDIR /app

COPY --from=build /workspace/api-gateway/target/*.jar app.jar

EXPOSE 8080 9090
ENTRYPOINT ["java", "-jar", "/app/app.jar"]