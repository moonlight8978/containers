FROM alpine:3.22.1 AS builder

ARG TARGETOS
ARG TARGETARCH

# renovate: datasource=github-releases depName=kubernetes/kubernetes
ARG KUBE_VERSION=v1.33.4
# renovate: datasource=github-releases depName=helm/helm
ARG HELM_VERSION=v3.18.6
# renovate: datasource=github-releases depName=getsops/sops
ARG SOPS_VERSION=v3.10.2
# renovate: datasource=github-releases depName=kubernetes-sigs/kustomize extractVersion=^kustomize\/(?<version>.+)$
ARG KUSTOMIZE_VERSION=v5.7.1

WORKDIR /app/bin

RUN apk add --no-cache curl

RUN curl -L "https://get.helm.sh/helm-${HELM_VERSION}-${TARGETOS}-${TARGETARCH}.tar.gz" -o helm.tar.gz && \
    tar -xvzf helm.tar.gz --strip-components=1 && \
    chmod +x helm && \
    mv helm /app/bin/ && \
    rm helm.tar.gz

RUN curl -LO "https://dl.k8s.io/release/${KUBE_VERSION}/bin/${TARGETOS}/${TARGETARCH}/kubectl" && \
    chmod +x kubectl && \
    mv kubectl /app/bin/

RUN curl -L "https://github.com/getsops/sops/releases/download/${SOPS_VERSION}/sops-${SOPS_VERSION}.${TARGETOS}.${TARGETARCH}" -o sops && \
    chmod +x sops && \
    mv sops /app/bin/

RUN curl -L "https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2F${KUSTOMIZE_VERSION}/kustomize_${KUSTOMIZE_VERSION}_${TARGETOS}_${TARGETARCH}.tar.gz" -o kustomize.tar.gz && \
    tar -xvzf kustomize.tar.gz && \
    chmod +x kustomize && \
    mv kustomize /app/bin/ && \
    rm kustomize.tar.gz

FROM alpine:3.22.1

RUN apk add --no-cache \
    gnupg \
    jq \
    yq-go \
    git

RUN adduser -u 1001 -D -S alpine
USER alpine

COPY --chown=alpine:alpine --from=builder /app/bin /usr/local/bin
