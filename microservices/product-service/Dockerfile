
# Docker image from the Temurin project is used and this stage is named builder
FROM eclipse-temurin:17.0.5_8-jre-focal as builder 
# Set the working directory to extracted
WORKDIR extracted
# adds the fat JAR file from the Gradle build library, build/libs, to that folder
ADD ./build/libs/*.jar app.jar
# Prform the extraction of the fat JAR file into its working directory, the extracted folder
RUN java -Djarmode=layertools -jar app.jar extract

# Use the same image from build stage
FROM eclipse-temurin:17.0.5_8-jre-focal
# Set the work directory to application
WORKDIR application
# Copy folder by folder from build stage to application
COPY --from=builder extracted/dependencies/ ./ 
COPY --from=builder extracted/spring-boot-loader/ ./
COPY --from=builder extracted/snapshot-dependencies/ ./
COPY --from=builder extracted/application/ ./
# Expose port 8080
EXPOSE 8080
# Start the microservice in the exploded format, that is, org.springframework. boot.loader.JarLauncher.
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]