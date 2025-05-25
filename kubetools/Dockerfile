FROM alpine:3.21.3 AS builder

# renovate: datasource=github-releases depName=kubernetes/kubernetes
ARG KUBE_VERSION=v1.33.1
# renovate: datasource=github-releases depName=helm/helm
ARG HELM_VERSION=v3.18.0
# renovate: datasource=github-releases depName=getsops/sops
ARG SOPS_VERSION=v3.10.2
# renovate: datasource=github-releases depName=kubernetes-sigs/kustomize extractVersion=^kustomize\/(?<version>.+)$
ARG KUSTOMIZE_VERSION=v5.6.0

ARG BUILD_OS
ARG BUILD_ARCH

WORKDIR /app/bin

RUN apk add --no-cache curl

RUN curl -L "https://get.helm.sh/helm-${HELM_VERSION}-${BUILD_OS}-${BUILD_ARCH}.tar.gz" -o helm.tar.gz && \
    tar -xvzf helm.tar.gz --strip-components=1 && \
    chmod +x helm && \
    mv helm /app/bin/ && \
    rm helm.tar.gz

RUN curl -LO "https://dl.k8s.io/release/${KUBE_VERSION}/bin/${BUILD_OS}/${BUILD_ARCH}/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /app/bin/

RUN curl -L "https://github.com/getsops/sops/releases/download/${SOPS_VERSION}/sops-${SOPS_VERSION}.${BUILD_OS}.${BUILD_ARCH}" -o sops && \
    chmod +x sops && \
    mv sops /app/bin/

RUN curl -L "https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2F${KUSTOMIZE_VERSION}/kustomize_${KUSTOMIZE_VERSION}_${BUILD_OS}_${BUILD_ARCH}.tar.gz" -o kustomize.tar.gz && \
    tar -xvzf kustomize.tar.gz && \
    chmod +x kustomize && \
    mv kustomize /app/bin/ && \
    rm kustomize.tar.gz

FROM alpine:3.21.3

RUN apk add --no-cache \
    # renovate: datasource=repology depName=alpine_3_21/gnupg versioning=loose
    gnupg=2.4.7-r0 \
    # renovate: datasource=repology depName=alpine_3_21/jq versioning=loose
    jq=1.7.1-r0

RUN adduser -u 1001 -D -S alpine
USER alpine

COPY --chown=alpine:alpine --from=builder /app/bin /usr/local/bin
