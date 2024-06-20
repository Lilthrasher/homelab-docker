# Media Stack

## Docker Compose (`docker-compose.yaml`)
``` yaml
services:
  gluetun:
    image: qmcgaw/gluetun
    container_name: gluetun
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    ports:
      - 58846:58846 #optional
      - 6881:6881
      - 6881:6881/udp
      - 9696:9696 # prowlarr
      - 7878:7878 # radarr
      - 8989:8989 # sonarr
      - 8112:8112 # deluge
    volumes:
      - ${CONFIG_DIR}/gluetun:/gluetun
    environment:
      - VPN_SERVICE_PROVIDER=private internet access
      - OPENVPN_USER=${VPN_USER}
      - OPENVPN_PASSWORD=${VPN_PASS}
      - SERVER_REGIONS=${VPN_REGION}
      - PORT_FORWARD_ONLY=true
      - VPN_PORT_FORWARDING=on
      - VPN_PORT_FORWARDING_STATUS_FILE=/gluetun/forwarded_port

  overseerr: #default port is 5055
    image: lscr.io/linuxserver/overseerr:latest
    container_name: overseerr
    network_mode: host
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ${CONFIG_DIR}/overseerr:/config
    restart: unless-stopped
    depends_on:
      gluetun:
        condition: service_healthy

  prowlarr: #default port is 9696
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    network_mode: "service:gluetun"
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ${CONFIG_DIR}/prowlarr:/config
    restart: unless-stopped
    depends_on:
      gluetun:
        condition: service_healthy

  radarr: #default port is 7878
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    network_mode: "service:gluetun"
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ${CONFIG_DIR}/radarr:/config
      - ${DATA_DIR}:/data
    restart: unless-stopped
    depends_on:
      gluetun:
        condition: service_healthy

  sonarr: #default port is 8989
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    network_mode: "service:gluetun"
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
    volumes:
      - ${CONFIG_DIR}/sonarr:/config
      - ${DATA_DIR}:/data
    restart: unless-stopped
    depends_on:
      gluetun:
        condition: service_healthy

  deluge:
    image: lscr.io/linuxserver/deluge:latest
    container_name: deluge
    network_mode: "service:gluetun"
    environment:
      - PUID=${PUID}
      - PGID=${PGID}
      - TZ=${TZ}
      - DELUGE_LOGLEVEL=error #optional
    volumes:
      - ${CONFIG_DIR}/gluetun:/pia:ro
      - ${CONFIG_DIR}/deluge:/config
      - ${DL_DIR}:/data/downloads
    restart: unless-stopped
    depends_on:
      gluetun:
        condition: service_healthy

  tdarr:
    image: ghcr.io/haveagitgat/tdarr:latest
    container_name: tdarr
    network_mode: host
    environment:
      - TZ=${TZ}
      - PUID=${PUID}
      - PGID=${PGID}
      - UMASK_SET=002
      - serverIP=0.0.0.0
      - serverPort=8266
      - webUIPort=8265
      - internalNode=true
      - inContainer=true
      - ffmpegVersion=6
      - nodeName=MyInternalNode
    volumes:
      - ${CONFIG_DIR}/tdarr/server:/app/server
      - ${CONFIG_DIR}/tdarr/configs:/app/configs
      - ${CONFIG_DIR}/tdarr/logs:/app/logs
      - ${MEDIA_DIR}:/media
      - /transcode_cache:/temp
    restart: unless-stopped
```

## Environment File (`.env`)
```
PUID=1000
PGID=1000
TZ=path/to/timezone
VPN_USER=vpn_user
VPN_PASS=vpn_pass
VPN_REGION=vpn_region
CONFIG_DIR=path/to/config/dir
DL_DIR=path/to/dl/dir
MEDIA_DIR=path/to/media/dir
```