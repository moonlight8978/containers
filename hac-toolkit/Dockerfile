FROM alpine:3.22.1

ARG TARGETOS
ARG TARGETARCH

RUN apk add --upgrade --no-cache \
  ca-certificates \
  yq \
  jq \
  tar \
  ripgrep \
  duplicity \
  curl

# renovate: datasource=github-releases depName=getsops/sops
ARG SOPS_VERSION=v3.10.2
# renovate: datasource=github-releases depName=FiloSottile/age
ARG AGE_VERSION=v1.2.1
# renovate: datasource=github-releases depName=containrrr/shoutrrr
ARG SHOUTRRR_VERSION=v0.8.0

COPY --from=hairyhenderson/gomplate:v4.3.3-alpine /bin/gomplate /bin/gomplate

RUN curl -fSsL https://github.com/getsops/sops/releases/download/${SOPS_VERSION}/sops-${SOPS_VERSION}.${TARGETOS}.${TARGETARCH} -o /bin/sops \
  && chmod +x /bin/sops

RUN curl -fSsL https://github.com/FiloSottile/age/releases/download/${AGE_VERSION}/age-${AGE_VERSION}-${TARGETOS}-${TARGETARCH}.tar.gz | tar -xz -C /tmp/ \
  && mv /tmp/age/age /bin/age \
  && mv /tmp/age/age-keygen /bin/age-keygen \
  && chmod +x /bin/age /bin/age-keygen \
  && rm -rf /tmp/*

RUN curl -fSsL https://github.com/containrrr/shoutrrr/releases/download/${SHOUTRRR_VERSION}/shoutrrr_${TARGETOS}_${TARGETARCH}.tar.gz | tar -xz -C /tmp \
  && mv /tmp/shoutrrr /bin/shoutrrr \
  && chmod +x /bin/shoutrrr \
  && rm -rf /tmp/*
