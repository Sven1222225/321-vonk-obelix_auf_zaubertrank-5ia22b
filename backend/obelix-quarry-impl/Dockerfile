# Stage 1: build
FROM gradle:8.5-jdk21 AS build
WORKDIR /app

# Copy Gradle wrapper and root files
COPY ../gradlew .
COPY ../gradle/ gradle/
COPY ../settings.gradle .
COPY build.gradle .

# Copy only this module's code
COPY obelix-quarry-impl/ obelix-quarry-impl/

# Build only this module
RUN ./gradlew :obelix-quarry-impl:build -x test

# Stage 2: runtime
FROM eclipse-temurin:21-jdk
WORKDIR /app

# Copy built jar
COPY --from=build /app/obelix-quarry-impl/build/libs/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
