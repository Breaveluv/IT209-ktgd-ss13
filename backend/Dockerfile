# ========================================================
# Production Dockerfile for Campus Hub Backend
# Lightweight & Secure JRE 21 Runtime Environment
# ========================================================
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# Install curl for Actuator healthcheck and tzdata for accurate local timestamps
RUN apk add --no-cache curl tzdata

# Configure Vietnam timezone
ENV TZ=Asia/Ho_Chi_Minh

# Create and switch to non-root user for security best practice
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Copy built executable Spring Boot JAR into container
COPY --chown=appuser:appgroup build/libs/*.jar app.jar

# Expose Spring Boot server port
EXPOSE 8080

# JVM container optimization flags
ENV JAVA_OPTS="-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0 -Djava.security.egd=file:/dev/./urandom"

# Healthcheck configuration via Spring Boot Actuator
HEALTHCHECK --interval=15s --timeout=5s --start-period=30s --retries=3 \
  CMD curl -f http://localhost:8080/actuator/health || exit 1

# Start Spring Boot application
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
