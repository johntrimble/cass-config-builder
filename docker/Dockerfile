# Copyright DataStax, Inc.
# Please see the included license file for details.
FROM --platform=linux/amd64 maven:3.6.3-adoptopenjdk-8 as builder

COPY . .

RUN ./gradlew copyDockerBuildCtx

FROM openjdk:8u212-jre-alpine3.9

ENV USER_UID=1001 \
    USER_NAME=cass-operator \
    HOME=/home/$USER_NAME

# Install the uber jar
COPY --from=builder ./build/docker/*.jar /usr/local/bin/

# Install definition files
COPY --from=builder ./build/docker/definitions /definitions

COPY --from=builder ./build/docker/bin/* /usr/local/bin/

RUN  /usr/local/bin/user_setup

ENV PATH=$PATH:/usr/local/bin

ENTRYPOINT ["/usr/local/bin/entrypoint"]

USER ${USER_UID}
