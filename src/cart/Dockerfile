<<<<<<< HEAD
FROM public.ecr.aws/amazonlinux/amazonlinux:2023 as build
=======
FROM public.ecr.aws/amazonlinux/amazonlinux:2023 AS build
>>>>>>> 74699f6fad0d8e67f26c7f0fa99a5b4cf44ab762

RUN dnf --setopt=install_weak_deps=False install -q -y \
    maven \
    java-17-amazon-corretto-headless \
    which \
    tar \
    gzip \
    && \
    dnf clean all

ARG JAR_PATH

VOLUME /tmp

WORKDIR /

COPY .mvn .mvn
COPY mvnw .
COPY pom.xml .

RUN ./mvnw dependency:go-offline -B -q

COPY ./src ./src

RUN ./mvnw -DskipTests package -q && \
    mv /target/carts-0.0.1-SNAPSHOT.jar /app.jar

FROM public.ecr.aws/amazonlinux/amazonlinux:2023

RUN dnf --setopt=install_weak_deps=False install -q -y \
    java-17-amazon-corretto-headless \
    shadow-utils \
    && \
    dnf clean all

ENV APPUSER=appuser
ENV APPUID=1000
ENV APPGID=1000

RUN useradd \
    --home "/app" \
    --create-home \
    --user-group \
    --uid "$APPUID" \
    "$APPUSER"

ENV JAVA_OPTS=
ENV SPRING_PROFILES_ACTIVE=prod

WORKDIR /app
USER appuser

COPY ./ATTRIBUTION.md ./LICENSES.md

COPY --chown=appuser:appuser --from=build /app.jar .

EXPOSE 8080

<<<<<<< HEAD
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar /app/app.jar"]
=======
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar /app/app.jar"]


>>>>>>> 74699f6fad0d8e67f26c7f0fa99a5b4cf44ab762
