# Watchtower ([Website]{:target="_blank"})

[Website]: https://containrrr.dev/watchtower/

![Watchtower](../assets/images/watchtower/logo-watchtower.png)

## About Watchtower

Watchtower is a self-hosted application that automates the process of updating Docker containers. It monitors your running containers and automatically pulls and applies updates for the images, ensuring that your containers are always up-to-date with the latest versions. This helps maintain security, stability, and access to new features without manual intervention. Watchtower is particularly useful for managing multiple Docker containers efficiently, reducing the overhead associated with container maintenance.

## Docker Compose (`docker-compose.yaml`)
``` yaml
services:
  watchtower:
    image: containrrr/watchtower
    container_name: watchtower
    restart: unless-stopped
    ports:
      - ${WATCH_PORT}:8080
    volumes:
      - /etc/timezone:/etc/timezone
      - ${SECRET_DIR}
    environment:
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_SCHEDULE=0 0 0 * * *
      - WATCHTOWER_HTTP_API_METRICS=true
      - WATCHTOWER_HTTP_API_TOKEN=${API_TOKEN}
```

## Environment File (`.env`)
```
WATCH_PORT=8080
API_TOKEN=api_token
```