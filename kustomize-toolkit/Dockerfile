FROM alpine:3.22.2 AS builder

ARG TARGETOS
ARG TARGETARCH

# renovate: datasource=github-releases depName=helm/helm
ARG HELM_VERSION=v3.19.1
# renovate: datasource=github-releases depName=getsops/sops
ARG SOPS_VERSION=v3.11.0
# renovate: datasource=github-releases depName=kubernetes-sigs/kustomize extractVersion=^kustomize\/(?<version>.+)$
ARG KUSTOMIZE_VERSION=v5.8.0

RUN apk add --no-cache curl git

RUN curl -L "https://get.helm.sh/helm-${HELM_VERSION}-${TARGETOS}-${TARGETARCH}.tar.gz" -o helm.tar.gz && \
    tar -xvzf helm.tar.gz --strip-components=1 && \
    chmod +x helm && \
    mv helm /usr/local/bin/ && \
    rm helm.tar.gz

RUN curl -L "https://github.com/getsops/sops/releases/download/${SOPS_VERSION}/sops-${SOPS_VERSION}.${TARGETOS}.${TARGETARCH}" -o sops && \
    chmod +x sops && \
    mv sops /usr/local/bin/

RUN curl -L "https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2F${KUSTOMIZE_VERSION}/kustomize_${KUSTOMIZE_VERSION}_${TARGETOS}_${TARGETARCH}.tar.gz" -o kustomize.tar.gz && \
    tar -xvzf kustomize.tar.gz && \
    chmod +x kustomize && \
    mv kustomize /usr/local/bin/ && \
    rm kustomize.tar.gz

FROM alpine:3.22.2

# Useful tools
RUN apk add --no-cache \
    gnupg \
    jq \
    yq-go

# Prefer non-root
RUN adduser -u 1001 -D -S alpine
USER alpine

# Binary
COPY --chown=alpine:alpine --from=builder /usr/local/bin /usr/local/bin
