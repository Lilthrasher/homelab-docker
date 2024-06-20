# Cloudflare DDNS ([Github]{:target="_blank"})

[Github]: https://github.com/oznu/docker-cloudflare-ddns

![Cloudflare](../assets/images/cloudflare/logo-cloudflare.png)

## About Cloudflare DDNS

Cloudflare DDNS (Dynamic DNS) is a self-hosted service that automatically updates DNS records in Cloudflare when your IP address changes. This is useful for users with a dynamic IP address, allowing them to maintain consistent access to their home network or services by updating the DNS records to point to the new IP address.

## Docker Compose (`docker-compose.yaml`)
``` yaml
services:
  cloudflare-ddns:
    image: oznu/cloudflare-ddns:latest
    restart: unless-stopped
    environment:
      - API_KEY=${API_KEY}
      - ZONE=${ZONE}
      - PROXIED=false
```

## Environment File (`.env`)
```
API_KEY=
ZONE=
```