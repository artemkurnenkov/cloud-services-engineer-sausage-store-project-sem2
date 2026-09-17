#Это так называемый мультистейдж docker-файл. Сначала делаем образ, который соберёт нам java-приложение,
#а затем из него берём собранный артефакт и кладём его в новый образ, чтобы оставить в нём меньше лишнего

# -- Образ для сборки
FROM maven:3.9-eclipse-temurin-17-alpine AS build

# задаём переменную VERSION, которая будет использоваться для сборки проекта
ARG VERSION=${VERSION}

WORKDIR /usr/src/app

COPY ./src ./src
COPY *.xml ./

RUN mvn package -Dversion.application=${VERSION}

# -- Runtime образ
FROM maven:3.9-eclipse-temurin-17-alpine

ARG VERSION=${VERSION}

WORKDIR /app

RUN addgroup --system jaruser \
    && adduser -S -s /bin/false -G jaruser jaruser -D -H \
    && apk add --no-cache dumb-init curl \
    && rm -rf /var/cache/apk/* \
    && mkdir -p /logs/reports \
    && chown -R jaruser:jaruser /logs

COPY --chown=jaruser:jaruser --from=build \
    /usr/src/app/target/sausage-store-${VERSION}.jar \
    ./sausage-store.jar

EXPOSE 8080

USER jaruser

ENTRYPOINT ["dumb-init", "java", "-jar", "-Dmyserver.bindPort=8080", "./sausage-store.jar"]
