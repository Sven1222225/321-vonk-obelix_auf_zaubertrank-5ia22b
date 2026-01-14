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
COPY obelix-quarry-api/ obelix-quarry-api/
COPY obelix-quarry-impl/ obelix-quarry-impl/

# Build only this module
RUN ./gradlew :obelix-quarry-impl:build -x test

# Stage 2: Runtime
FROM eclipse-temurin:21-jdk
WORKDIR /app

# Copy built jar
COPY --from=build /app/obelix-quarry-impl/build/libs/*.jar app.jar

EXPOSE 8081
ENTRYPOINT ["java", "-jar", "app.jar"]
