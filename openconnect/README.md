## Description

OpenConnect VPN client.

### Notes

- Most likely you want to run this in `--network=host` mode. Otherwise, other VPN clients won't be able to access resources on the host machine.
- Host network driver has serious limitations in non-Linux environments. Most likely it won't work on macOS and Windows. See https://docs.docker.com/engine/network/drivers/host/#limitations for details.

### Build

```bash
docker build . -t openconnect
```

### Run

1. Create a `openconnect.conf` file with OpenConnect configuration.
2. Start the container:

```bash
docker run -v ./openconnect.conf:/usr/local/etc/openconnect/openconnect.conf --cap-add=NET_ADMIN openconnect
```

or

```bash
docker-compose up -d
```

### Configuration

`openconnect.conf`:

```
server vpn.example.com
form-entry main:username=user
form-entry main:password=password
```
