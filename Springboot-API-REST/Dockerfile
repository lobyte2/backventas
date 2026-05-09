# Etapa 1: Build
FROM maven:3.8.5-openjdk-17 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Etapa 2: Runtime (Seguridad y Optimización)
FROM openjdk:17-jdk-slim
WORKDIR /app

# Usuario no root (Requisito de seguridad)
RUN addgroup --system springuser && adduser --system springuser --ingroup springuser
USER springuser

COPY --from=build /app/target/*.jar app.jar
EXPOSE 8081 
# Nota: He puesto 8081 por si quieres correr ambos backends a la vez localmente
ENTRYPOINT ["java", "-jar", "app.jar"]