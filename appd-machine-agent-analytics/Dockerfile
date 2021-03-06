
FROM openjdk:8-jre-slim AS builder

ARG APPD_AGENT_SHA256
ARG APPD_AGENT_VERSION 


RUN apt-get update -y; \
 apt-get install -y unzip

COPY MachineAgent-${APPD_AGENT_VERSION}.zip /
RUN if [ "x${APPD_AGENT_SHA256}" != "x" ]; then \ 
	echo "${APPD_AGENT_SHA256} *MachineAgent-${APPD_AGENT_VERSION}.zip" >> appd_checksum \
    && sha256sum -c appd_checksum \
    && rm appd_checksum \
    && unzip -oq MachineAgent-${APPD_AGENT_VERSION}.zip -d /tmp; \
    else \
     unzip -oq MachineAgent-${APPD_AGENT_VERSION}.zip -d /tmp; \
    fi

ADD LICENSE /licenses/

FROM openjdk:8-jre-slim

ARG APPD_AGENT_VERSION 

MAINTAINER support@appdynamics.com

LABEL name="AppDynamics MachineAgent" \
      vendor="AppDynamics" \
      version="${APPD_AGENT_VERSION}" \
      release="1" \
      url="https://www.appdynamics.com" \
      summary="AppDynamics solution for monitoring infrastructure" \
      description="The MachineAgent monitors resource utilization, server load and processes on the host."


RUN apt-get update -y; \
 apt-get install -y --no-install-recommends iproute2 procps dumb-init coreutils sed; \
	rm -rf /var/lib/apt/lists/*

COPY --from=builder /tmp /opt/appdynamics
COPY --from=builder /licenses /licenses

ENV MACHINE_AGENT_HOME /opt/appdynamics

WORKDIR ${MACHINE_AGENT_HOME}

COPY updateAnalyticsAgent.sh ./updateAnalyticsAgent.sh
RUN chmod +x ./updateAnalyticsAgent.sh

COPY startup.sh ./startup.sh
RUN chmod +x ./startup.sh


EXPOSE 9090

CMD "./startup.sh"