FROM alpine:3.22.1 AS builder

ARG TARGETOS
ARG TARGETARCH

# renovate: datasource=github-releases depName=helm/helm
ARG HELM_VERSION=v3.18.4
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
    # renovate: datasource=repology depName=alpine_3_22/gnupg versioning=loose
    gnupg=2.4.7-r0 \
    # renovate: datasource=repology depName=alpine_3_22/jq versioning=loose
    jq=1.8.0-r0 \
    # renovate: datasource=repology depName=alpine_3_22/yq-go versioning=loose
    yq-go=4.45.4-r0 \
    git

RUN adduser -u 1001 -D -S alpine
USER alpine

COPY --chown=alpine:alpine --from=builder /app/bin /usr/local/bin

# renovate: datasource=github-releases depName=jkroepke/helm-secrets
ARG HELM_SECRETS_VERSION=v4.6.5
RUN helm plugin install https://github.com/jkroepke/helm-secrets --version "${HELM_SECRETS_VERSION}"
