# NTFY ([Website]{:target="_blank"})

[Website]: https://ntfy.sh/

![NTFY](../assets/images/ntfy/logo-ntfy.png)

## About NTFY

NTFY is a self-hosted notification service that allows you to send real-time notifications to your devices. It supports multiple platforms and can be integrated with various applications and services to provide alerts and updates. Users can customize notifications, set up channels, and manage delivery methods, ensuring timely and relevant information. Being self-hosted, NTFY gives you full control over your data and notification settings, making it a flexible and privacy-focused solution for personal or organizational use.

## Screenshots

![NTFY](../assets/images/ntfy/screenshot.png)

## Docker Compose (`docker-compose.yaml`)
``` yaml
services:
  ntfy:
    image: binwiederhier/ntfy
    container_name: ntfy
    restart: unless-stopped
    ports:
      - ${NTFY_PORT}:80
    volumes:
      - /etc/timezone:/etc/timezone
      - ${NTFY_DIR}/var/cache/ntfy:/var/cache/ntfy
      - ${NTFY_DIR}/etc/ntfy:/etc/ntfy
    command:
      - serve
    healthcheck: # optional: remember to adapt the host:port to your environment
        test: ["CMD-SHELL", "wget -q --tries=1 http://ip:port/v1/health -O - | grep -Eo '\"healthy\"\\s*:\\s*true' || exit 1"]
        interval: 60s
        timeout: 10s
        retries: 3
        start_period: 40s
```

## Environment File (`.env`)
```
NTFY_PORT=80
NTFY_DIR=path/to/ntfy/dir
```