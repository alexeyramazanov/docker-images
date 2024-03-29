![Build & publish openvpn-3proxy](https://github.com/alexeyramazanov/docker-images/workflows/Build%20&%20publish%20openvpn-3proxy/badge.svg)

## Description

OpenVPN + 3proxy bundle

Used to create client-2-client VPN with access to the external network only from proxy server.

### Build

```bash
docker build . -t openvpn-3proxy
```

### Run

```bash
docker run -v $(pwd)/openvpn:/etc/openvpn -v $(pwd)/3proxy/3proxy.cfg:/etc/3proxy.cfg -p <OPENVPN_PORT>:<OPENVPN_PORT> --cap-add=NET_ADMIN openvpn-3proxy
```

or

```bash
docker-compose up -d
```

### Configuration example

#### 3proxy (3proxy.cfg)

```
internal 10.8.0.1

auth iponly
allow * 10.8.0.0/24 * * *

proxy -p3128 -a

setgid 65534
setuid 65534
```

#### OpenVPN (openvpn.conf)

Use [easy-rsa](https://github.com/OpenVPN/easy-rsa) to generate keys.

```
### core
dev tun
proto udp
port 1194
server 10.8.0.0 255.255.255.0
topology subnet

### keys
ca ca.crt
cert server.crt
key server.key
dh dh.pem
tls-auth ta.key 0

# security
cipher AES-256-GCM
auth SHA256

### user/group
user nobody
group nogroup

### avoid accessing certain resources on restart
persist-key
persist-tun

### routes
client-to-client

### other
keepalive 5 20
# a long-term association between clients and the virtual IP address assigned to them
ifconfig-pool-persist ipp.txt
# Renegotiate data channel key after n seconds
reneg-sec 86400

### debug
#verb 5
#log-append server.log
```

### Notes

Runit logic partially taken from [phusion/baseimage-docker](https://github.com/phusion/baseimage-docker).
