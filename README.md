# DOC-LAB-6-network-container-communication

Docker Container Communication Between Two Networks
# Step 1: Create Two Docker Networks
```bash
docker network create network1
```
```bash
docker network create network2
```

# Step 2: Check Existing Networks
```bash
docker network ls
```
# Step 3: Create Containers in Different Networks

Create Container 1 in network1

```bash
docker run --name=container1 --network=network1 -d -p 1000:80 nginx
```
Create Container 2 in network2

```bash
docker run --name=container2 --network=network2 -d -p 2000:80 nginx
```

# METHOD 1 — Connect Containers Using a Shared Network

# Step 4 : Create a Shared Network
```bash
docker network create shared
```
Check networks:
```bash
docker network ls
```
# Step 5: Connect Both Containers to Shared Network

```bash
docker network connect shared container1
```
```bash
docker network connect shared container2
```
# Step 6
Inspect the containers if it is connected and copy the ip from shared network

```bash
docker ps
```
```bash
docker inspect container1
```
GET IP OF SHARED NETWORK 
```bash
docker ps
```
```bash
docker inspect container2
```
GET IP OF SHARED NETWORK 

# Step 7: Login Inside the Containers

Enter Container 1
```bash
docker exec -it container1 /bin/bash
```
Install Ping Utility Inside Containers

Run these commands inside both containers:

```bash
apt update
```
```bash
apt install iputils-ping -y
```
# Step 8 : Test Communication Between Containers

# Login to Container 1
```bash
docker exec -it container1 /bin/bash
```
```bash
ping <container2-ip>
```
# Step 9: Login Inside the Container2

Enter Container 2
```bash
docker exec -it container2 /bin/bash
```
Install Ping Utility Inside Containers

```bash
apt update
```
```bash
apt install iputils-ping -y
```

# Step 10 : Test Communication Between Containers
```bash
docker exec -it container2 /bin/bash
```
```bash
ping <container1-ip>
```

# Remove Old Networks

```bash
docker network disconnect shared container1
```
```bash
docker network disconnect shared container2
```
```bash
docker network rm shared
```

# METHOD 2 — Directly Connect Containers to Each Other’s Networks

Instead of creating a shared network, connect each container to the other network.

# Step 1: Connect Container 2 to network1
```bash
docker network connect network1 container2
```
# Step 2: Connect Container 1 to network2
```bash
docker network connect network2 container1
```
# Step 3: Test Connectivity

Login to Container 1
```bash
docker exec -it container1 /bin/bash
```
```bash
ping <container2-ip>
```
Login to Container 2
```bash
docker exec -it container2 /bin/bash
```
```
ping <container1-ip>
```

# Remove Old Networks

Disconnect Containers from Networks
```bash
docker network disconnect network1 container2
```
```bash
docker network disconnect network2 container1
```

# Remove Networks
```bash
docker network rm network1
```
```bash
docker network rm network2
```

check containers 
```bash
docker ps
```
# remove containers
```bash
docker rm -f container1
```
```bash
docker rm -f container2
```
