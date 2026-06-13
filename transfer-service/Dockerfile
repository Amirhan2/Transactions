# ==========================================
# ЭТАП 1: Станок (Сборка)
# ==========================================
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /workspace

# 1. Копируем родительский pom (обязательно для монорепы)
COPY pom.xml ./

# 2. Копируем локальные библиотеки, от которых зависит Transfer Service
COPY grpc-common-lib ./grpc-common-lib

# 3. Копируем исходники самого сервиса
COPY transfer-service ./transfer-service

# 4. Запускаем сборку ТОЛЬКО transfer-service и его зависимостей
RUN mvn -q -DskipTests -pl transfer-service -am clean package

# ==========================================
# ЭТАП 2: Среда выполнения (Продакшен)
# ==========================================
FROM eclipse-temurin:21-jre
WORKDIR /app

# 1. Забираем скомпилированный .jar из первого этапа "build"
COPY --from=build /workspace/transfer-service/target/*.jar app.jar

# 2. Документируем порты (замени на свои, если у тебя другие)
# 8080 - дефолтный порт Spring Boot (REST)
# 9090 - дефолтный порт gRPC сервера
EXPOSE 8080 9090

# 3. Команда, которая запустится при старте контейнера
ENTRYPOINT ["java", "-jar", "/app/app.jar"]