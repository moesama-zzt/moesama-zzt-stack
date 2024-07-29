version: "3"

services:
  wireguard:
    #image: lscr.io/linuxserver/wireguard:latest
    image: linuxserver/wireguard:v1.0.20210914-ls33
    container_name: wireguard
    cap_add:
      - NET_ADMIN
    volumes:
      - ./config:/config
      - /etc/localtime:/etc/localtime:ro
    ports:
      # port for wireguard-ui. this must be set here as the `wireguard-ui` container joins the network of this container and hasn't its own network over which it could publish the ports
      - "5000:5000"
      # port of the wireguard server
      - "51820:51820/udp"
    environment:
      - PEERS=1
    restart: unless-stopped
      
  wireguard-ui:
    # image: ngoduykhanh/wireguard-ui:latest
    image: ngoduykhanh/wireguard-ui:0.6.2
    container_name: wireguard-ui
    depends_on:
      - wireguard
    cap_add:
      - NET_ADMIN
    # use the network of the 'wireguard' service. this enables to show active clients in the status page
    network_mode: service:wireguard
    environment:
      - SENDGRID_API_KEY
      - EMAIL_FROM_ADDRESS
      - EMAIL_FROM_NAME
      - SESSION_SECRET
      - WGUI_USERNAME=admin
      - WGUI_PASSWORD=Test.1122331
      - WG_CONF_TEMPLATE
      - WGUI_MANAGE_START=true
      - WGUI_MANAGE_RESTART=true
      - WGUI_DNS=223.5.5.5
      - WGUI_PERSISTENT_KEEPALIVE=30
      - WGUI_SERVER_INTERFACE_ADDRESSES=10.0.8.1/24
      - WGUI_DEFAULT_CLIENT_ALLOWED_IPS=10.0.8.0/24
      - WGUI_SERVER_POST_UP_SCRIPT=iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
      - WGUI_SERVER_POST_DOWN_SCRIPT=iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
    logging:
      driver: json-file
      options:
        max-size: 50m
    volumes:
      - ./db:/app/db
      - ./config:/etc/wireguard
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped