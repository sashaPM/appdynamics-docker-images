FROM alpine AS builder

ARG APPD_AGENT_VERSION 
ARG APPD_AGENT_SHA256


RUN apk --no-cache add unzip

COPY AppServerAgent-${APPD_AGENT_VERSION}.zip /
RUN if [ "x${APPD_AGENT_SHA256}" != "x" ]; then \ 
 echo "${APPD_AGENT_SHA256} *AppServerAgent-${APPD_AGENT_VERSION}.zip" >> appd_checksum \
    && sha256sum -c appd_checksum \
    && rm appd_checksum \
    && unzip -oq AppServerAgent-${APPD_AGENT_VERSION}.zip -d /tmp; \
   else \
     unzip -oq AppServerAgent-${APPD_AGENT_VERSION}.zip -d /tmp; \
   fi

ADD LICENSE /licenses/

FROM alpine

ARG APPD_AGENT_VERSION 

MAINTAINER support@appdynamics.com

LABEL name="AppDynamics Java Agent" \
      vendor="AppDynamics" \
      version="${APPD_AGENT_VERSION}" \
      release="1" \
      url="https://www.appdynamics.com" \
      summary="AppDynamics solution for monitoring Java applications" \
      description="AppDynamics agent for monitoring Java applications"


COPY --from=builder /tmp /opt/appdynamics
COPY --from=builder /licenses /opt/appdynamics/licenses

CMD [ "echo", "I am just a messenger delivering the Java Agent." ]