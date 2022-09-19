FROM tailscale/tailscale:latest as tailscale

FROM louislam/uptime-kuma:1 as uptimekuma

FROM alpine:latest
RUN apk update && \
  apk add ca-certificates iptables ip6tables nodejs && \
  rm -rf /var/cache/apk/*

# Copy binary to production image
WORKDIR /app
COPY --from=uptimekuma /app /app
COPY ./start.sh /app/start.sh
COPY --from=tailscale /usr/local/bin/tailscaled /app/tailscaled
COPY --from=tailscale /usr/local/bin/tailscale /app/tailscale
RUN mkdir -p /var/run/tailscale /var/cache/tailscale /var/lib/tailscale

# Run on container startup.
CMD ["/app/start.sh"]
