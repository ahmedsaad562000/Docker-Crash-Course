```markdown
# Exploring Bridge and User-Defined Networks in Docker

This guide demonstrates the differences between Docker's default `bridge` network and user-defined networks. The commands and explanations below will help you understand how networking works in Docker.

---

## Commands and Steps

### 1. List Available Networks
```bash
docker network ls
```
Displays all available Docker networks. The default `bridge` network is created by Docker.

---

### 2. Create Containers in the Default Bridge Network
```bash
docker run -dit --name alpine1 alpine ash
docker run -dit --name alpine2 alpine ash
```

#### Inspect the Networks
```bash
docker network inspect bridge
```

---

### 3. Connect to `alpine1` and Check Network Details
```bash
docker attach alpine1
ip addr show
```
Example output:
- `eth0` has an IP address in the `172.17.0.0/16` subnet.
  
#### Ping Tests
- **Ping Google**: `ping -c 2 google.com` (works)
- **Ping by IP**: `ping -c 2 172.17.0.3` (works)
- **Ping by Name**: `ping -c 2 alpine2` (doesn't work in the default bridge network)

---

### 4. Create a User-Defined Bridge Network
```bash
docker network create --driver bridge my-bridge-net
docker network ls
```

Inspect the network:
```bash
docker network inspect my-bridge-net
```

---

### 5. Create Containers in the User-Defined Network
```bash
docker run -dit --name alpine1 --network my-bridge-net alpine ash
docker run -dit --name alpine2 --network my-bridge-net alpine ash
docker run -dit --name alpine4 --network my-bridge-net alpine ash
```

#### Attach a Container to the Default Network
```bash
docker run -dit --name alpine3 alpine ash
docker network connect bridge alpine4
```

Inspect both networks:
```bash
docker inspect my-bridge-net
docker inspect bridge
```

---

### 6. Test Automatic Service Discovery
On user-defined networks, containers can resolve names to IP addresses.

#### Connect to `alpine1`
```bash
docker attach alpine1
```

#### Ping Containers
- **Ping Alpine2**: `ping -c 2 alpine2`
- **Ping Alpine4**: `ping -c 2 alpine4`
- **Ping Alpine1 (Self)**: `ping -c 2 alpine1`
- **Ping Alpine3**: `ping -c 2 alpine3` (fails as it is not in the same network)

#### Detach from Container
Use `CTRL + p` followed by `CTRL + q` to detach.

---

### 7. Disconnect Containers
```bash
docker network disconnect bridge alpine4
docker container inspect alpine4
```

---

## Key Differences

### Default `bridge` Network
- Containers can communicate via IP address.
- **Name resolution** (e.g., `ping alpine2`) does not work.

### User-Defined Bridge Network
- Containers can communicate via both **IP address** and **container name**.
- **Automatic service discovery** enables name resolution for containers within the same network.

---

## Notes
1. Automatic service discovery only works for containers in the same user-defined network.
2. Default network names (e.g., `bridge`) do not support name resolution.
3. Use `CTRL + p CTRL + q` to detach from a container without stopping it.
