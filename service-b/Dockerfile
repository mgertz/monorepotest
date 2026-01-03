# Build stage
FROM gradle:8-jdk21-alpine AS build

WORKDIR /app

# Kopier hele projektet ind
COPY . .

# Byg projektet
RUN gradle build --no-daemon -x test

# Runtime stage
FROM amazoncorretto:21-alpine-jdk AS runtime

RUN apk --no-cache add curl

WORKDIR /app

# Kopier JAR filen fra build stage
COPY --from=build /app/build/libs/service-b.jar /app/service-b.jar

EXPOSE 8002

HEALTHCHECK --interval=30s --timeout=3s --start-period=30s --retries=3 \
	CMD curl --fail http://localhost:8002/actuator/health || exit 1

CMD ["java", "-jar", "/app/service-b.jar"]