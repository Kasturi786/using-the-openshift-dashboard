FROM maven:3.8.4-openjdk-11-slim AS builder
WORKDIR application
#ARG JAR_FILE= target\backend-service-0.0.1-SNAPSHOT.jar
COPY . .
RUN mvn package
#COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar target\backend-service-0.0.1-SNAPSHOT.jar list
RUN java -Djarmode=layertools -jar target\backend-service-0.0.1-SNAPSHOT.jar extract

FROM adoptopenjdk:11-jre-hotspot
WORKDIR application
COPY --from=builder application/dependencies/ ./
COPY --from=builder application/snapshot-dependencies/ ./
COPY --from=builder application/spring-boot-loader/ ./
COPY --from=builder application/application/ ./
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
