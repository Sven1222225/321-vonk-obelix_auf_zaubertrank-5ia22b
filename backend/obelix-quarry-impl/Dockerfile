FROM gradle:8.5-jdk21 AS build
WORKDIR /app

# Copy Gradle wrapper + module files
COPY gradlew .
COPY gradle/ gradle/
COPY build.gradle.kts .
COPY settings.gradle.kts .
COPY src/ src/

# Build this module only
RUN ./gradlew build -x test

# Runtime
FROM eclipse-temurin:21-jdk
WORKDIR /app

# Copy built JAR
COPY --from=build /app/build/libs/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
