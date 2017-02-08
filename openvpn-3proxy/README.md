### Description

OpenVPN + 3proxy bundle

Used to create client-2-client VPN with access to the external network only from proxy server.

No config files provided - use own.

```bash
docker run -v $(pwd)/openvpn:/etc/openvpn -v $(pwd)/3proxy.cfg:/etc/3proxy.cfg -p <OPENVPN_PORT>:<OPENVPN_PORT> --cap-add=NET_ADMIN openvpn-3proxy
```
