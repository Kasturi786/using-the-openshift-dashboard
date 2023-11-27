FROM adoptopenjdk:11-jre-hotspot AS builder
WORKDIR application
ARG JAR_FILE= target\backend-service-0.0.1-SNAPSHOT.jar
RUN apt-get install maven -y
COPY . .
RUN mvn package
COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar application.jar extract

FROM adoptopenjdk:11-jre-hotspot
WORKDIR application
COPY --from=builder application/dependencies/ ./
COPY --from=builder application/snapshot-dependencies/ ./
COPY --from=builder application/spring-boot-loader/ ./
COPY --from=builder application/application/ ./
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
