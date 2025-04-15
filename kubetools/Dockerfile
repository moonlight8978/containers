FROM alpine:3.21.3 AS builder

ARG SOPS_VERSION=v3.10.2
ARG KUBE_VERSION=v1.32.3
ARG HELM_VERSION=v3.17.3

ARG BUILD_OS
ARG BUILD_ARCH

WORKDIR /app/bin

RUN apk add --no-cache curl

RUN curl -L "https://get.helm.sh/helm-${HELM_VERSION}-${BUILD_OS}-${BUILD_ARCH}.tar.gz" -o helm.tar.gz \
    && tar -xvzf helm.tar.gz --strip-components=1 \
    && chmod +x helm \
    && mv helm /app/bin/ \
    && rm helm.tar.gz

RUN curl -LO "https://dl.k8s.io/release/${KUBE_VERSION}/bin/${BUILD_OS}/${BUILD_ARCH}/kubectl" \
    && chmod +x kubectl \
    && mv kubectl /app/bin/

RUN curl -L "https://github.com/getsops/sops/releases/download/${SOPS_VERSION}/sops-${SOPS_VERSION}.${BUILD_OS}.${BUILD_ARCH}" -o sops \
    && chmod +x sops \
    && mv sops /app/bin/

FROM alpine:3.21.3

RUN apk add --no-cache \
    gnupg=2.4.7-r0

RUN adduser -u 1001 -D -S alpine
USER alpine

COPY --chown=alpine:alpine --from=builder /app/bin /usr/local/bin
