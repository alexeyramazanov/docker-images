```bash
docker run -v $(pwd)/openvpn:/etc/openvpn -v $(pwd)/3proxy.cfg:/etc/3proxy.cfg -p 3128:3128 -p 1194:1194 --cap-add=NET_ADMIN openvpn-3proxy
```
