services:
  scrutiny:
    image: ghcr.io/analogj/scrutiny:master-omnibus
    container_name: scrutiny
    ports:
      - $WEB_PORT:8080
      - $DB_PORT:8086
    volumes:
      - /run/udev:/run/udev:ro
      - $VOLUMES_PATH/scrutiny/config:/opt/scrutiny/config
      - $VOLUMES_PATH/scrutiny/influxdb:/opt/scrutiny/influxdb
      - $NOTIFICATIONS_PATH:/notifications
    cap_add:
      - SYS_RAWIO
    devices: [ $DEVICES ]
    healthcheck:
      test: curl -ILfSs http://localhost:8080/api/health || exit 1
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s
    restart: unless-stopped
    networks:
      - default

networks:
  default:
    name: $NETWORK_NAME
    external: true
