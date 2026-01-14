# Stage 1: build
FROM gradle:8.5-jdk21 AS build

WORKDIR /app

# Copy Gradle wrapper + build files
COPY gradlew .
COPY gradle/ gradle/
COPY settings.gradle.kts .
COPY build.gradle.kts .
COPY obelix-quarry-impl/ obelix-quarry-impl/

# Build this module only
RUN ./gradlew :obelix-quarry-impl:build -x test

# Stage 2: runtime
FROM eclipse-temurin:21-jdk

WORKDIR /app

# Copy built JAR
COPY --from=build /app/obelix-quarry-impl/build/libs/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]