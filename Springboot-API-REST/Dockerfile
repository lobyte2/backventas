# Etapa 1: Build
FROM maven:3.8.5-eclipse-temurin-17 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Etapa 2: Runtime
FROM eclipse-temurin:17-jdk-jammy
WORKDIR /app

# Ejecución con usuario no root (Seguridad)
RUN addgroup --system springuser && adduser --system springuser --ingroup springuser
USER springuser

COPY --from=build /app/target/*.jar app.jar

# IMPORTANTE: Cambiamos el puerto al 8081 para que no choque con el otro microservicio
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "app.jar", "--server.port=8081"]