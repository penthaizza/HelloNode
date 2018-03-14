FROM node:8.9.4 as build
WORKDIR /app
COPY . ./
RUN yarn install && yarn build

FROM node:8.9.4-alpine as run

ENV LANG C.UTF-8
RUN { \
                echo '#!/bin/sh'; \
                echo 'set -e'; \
                echo; \
                echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
        } > /usr/local/bin/docker-java-home \
        && chmod +x /usr/local/bin/docker-java-home
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk/jre
ENV PATH $PATH:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin
ENV JAVA_VERSION 8u131
ENV JAVA_ALPINE_VERSION 8.131.11-r2
RUN set -x \
        && apk add --no-cache \
                openjdk8-jre="$JAVA_ALPINE_VERSION" \
        && [ "$JAVA_HOME" = "$(docker-java-home)" ]

WORKDIR /app
COPY --from=build /app/dist dist
COPY --from=build /app/node_modules node_modules
COPY --from=build /app/package.json package.json
COPY --from=build /app/yarn.lock yarn.lock
RUN yarn install --prod

CMD node dist