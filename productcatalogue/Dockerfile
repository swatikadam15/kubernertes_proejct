# ============================
# 1) Build stage - compile & package with Maven
# ============================
FROM maven:3.9.5-eclipse-temurin-17 AS build

WORKDIR /app

# Copy pom to download dependencies first (build optimization)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code and build
COPY src ./src
RUN mvn package -DskipTests -B

# ============================
# 2) Runtime stage
# ============================
FROM eclipse-temurin:11-jre-jammy


WORKDIR /app

# Copy built jar
COPY --from=build /app/target/*.jar app.jar

# Copy Dropwizard config file into container
COPY config.yml .

EXPOSE 8080 8081

# Run Dropwizard properly
CMD ["java", "-jar", "app.jar", "server", "config.yml"]
