# Stage 1: Build
FROM gradle:8.5-jdk21 AS build
WORKDIR /app

# Copy Gradle wrapper and root files
COPY gradlew .
COPY gradle/ gradle/
COPY settings.gradle .
COPY build.gradle .

# Make gradlew executable
RUN chmod +x gradlew

# Copy all backend modules (so dependencies can be resolved)
COPY obelix-common/ obelix-common/
COPY obelix-basket-api/ obelix-basket-api/
COPY obelix-basket-impl/ obelix-basket-impl/

# Build only this module
RUN ./gradlew :obelix-basket-impl:build -x test

# Stage 2: Runtime
FROM eclipse-temurin:21-jdk
WORKDIR /app

# Copy built jar
COPY --from=build /app/obelix-basket-impl/build/libs/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
