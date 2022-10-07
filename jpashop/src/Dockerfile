# build gradle
FROM gradle:7.5-jdk17 AS build
COPY --chown=gradle:gradle ./settings.gradle /home/gradle/src/settings.gradle
COPY --chown=gradle:gradle ./build.gradle /home/gradle/src/
COPY --chown=gradle:gradle src /home/gradle/src/api
WORKDIR /home/gradle/src
RUN gradle build -x test --stacktrace

# production deployment
FROM openjdk:17-jdk-slim
ENV MODE=application
ENV EXPOSE_PORT=8787

EXPOSE ${EXPOSE_PORT}

RUN mkdir /app
COPY --from=build /home/gradle/src/build/libs/*.jar /app/api.jar
CMD ["java", "-XshowSettings:vm", "-XX:+UnlockExperimentalVMOptions", "-XX:+UseContainerSupport", "-jar", "/app/api.jar", "--spring.config.location=classpath:/${MODE}.properties"]