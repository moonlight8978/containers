ARG BASE_IMAGE=containrrr/shoutrrr:0.8.0
FROM $BASE_IMAGE AS base

FROM alpine:3.23.3

COPY --from=base /shoutrrr /usr/local/bin/shoutrrr
COPY --from=base /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt

ENTRYPOINT ["shoutrrr"]
