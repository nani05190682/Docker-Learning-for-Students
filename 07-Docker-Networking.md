### Docker Container Networking Labs with Solutions

---

### **Lab Exercise 1: Basic Bridge Network**

**Objective:**  
Learn how to create a bridge network and connect containers to it for inter-container communication.

#### **Steps**
1. Create a new custom bridge network named `custom_bridge`.
2. Start two containers (`container1` and `container2`) connected to `custom_bridge`.
3. Verify that `container1` can ping `container2` by container name.

#### **Solution**
1. Create the bridge network:

   ```bash
   docker network create --driver bridge custom_bridge
   ```

2. Start the containers on `custom_bridge`:

   ```bash
   docker run -d --name container1 --network custom_bridge busybox sleep 3600
   docker run -d --name container2 --network custom_bridge busybox sleep 3600
   ```

3. Access `container1` and ping `container2` by name:

   ```bash
   docker exec -it container1 ping container2
   ```

**Expected Outcome**:  
You should see successful ping responses, confirming that the containers can communicate over the custom bridge network.

---

### **Lab Exercise 2: Network Isolation with Separate Bridge Networks**

**Objective:**  
Understand how to isolate containers by placing them in separate bridge networks.

#### **Steps**
1. Create two separate networks, `network1` and `network2`.
2. Start two containers, one in `network1` and one in `network2`.
3. Verify that containers cannot ping each other because they are on different networks.

#### **Solution**
1. Create two separate bridge networks:

   ```bash
   docker network create --driver bridge network1
   docker network create --driver bridge network2
   ```

2. Start one container in each network:

   ```bash
   docker run -d --name containerA --network network1 busybox sleep 3600
   docker run -d --name containerB --network network2 busybox sleep 3600
   ```

3. Attempt to ping `containerB` from `containerA`:

   ```bash
   docker exec -it containerA ping containerB
   ```

**Expected Outcome**:  
The ping should fail, as `containerA` and `containerB` are on separate networks and cannot communicate by default.

---

### **Lab Exercise 3: Connect a Container to Multiple Networks**

**Objective:**  
Learn how to connect a container to multiple networks to allow cross-network communication.

#### **Steps**
1. Create two networks, `network1` and `network2`.
2. Start a container (`multi_net_container`) and connect it to `network1`.
3. Connect `multi_net_container` to `network2`.
4. Start a new container in `network2` and verify that both containers can communicate with each other.

#### **Solution**
1. Create two networks:

   ```bash
   docker network create --driver bridge network1
   docker network create --driver bridge network2
   ```

2. Start a container connected to `network1`:

   ```bash
   docker run -d --name multi_net_container --network network1 busybox sleep 3600
   ```

3. Connect `multi_net_container` to `network2`:

   ```bash
   docker network connect network2 multi_net_container
   ```

4. Start a new container in `network2`:

   ```bash
   docker run -d --name containerC --network network2 busybox sleep 3600
   ```

5. Verify that `multi_net_container` can ping `containerC`:

   ```bash
   docker exec -it multi_net_container ping containerC
   ```

**Expected Outcome**:  
The ping should succeed, demonstrating that `multi_net_container` can communicate on both `network1` and `network2`.

---

### **Lab Exercise 4: Exposing a Container to the Host Network**

**Objective:**  
Use the `host` network mode to expose a container directly to the host’s network.

#### **Steps**
1. Start a container in `host` network mode.
2. Run a web server in this container, accessible via the host's IP.
3. Access the web server using the host IP to verify connectivity.

#### **Solution**
1. Start an Nginx container using the `host` network mode:

   ```bash
   docker run -d --network host --name host_network_container nginx
   ```

2. Verify that Nginx is accessible on the host’s IP (using `localhost` or the host’s IP address). Open a browser or use `curl`:

   ```bash
   curl http://localhost
   ```

**Expected Outcome**:  
You should see the default Nginx welcome page, indicating that the container is accessible directly via the host’s IP address.

---

### **Lab Exercise 5: Multi-Host Communication with Overlay Network (Docker Swarm)**

**Objective:**  
Set up a Docker Swarm and create an overlay network to enable container communication across multiple hosts.

#### **Steps**
1. Initialize a Docker Swarm on one host.
2. Join another host to the Swarm.
3. Create an overlay network.
4. Deploy a service to the overlay network and verify cross-host communication.

#### **Solution**
1. **Initialize Docker Swarm** on the first host (Manager node):

   ```bash
   docker swarm init --advertise-addr <Manager-IP>
   ```

2. **Join the second host** as a worker by running the command provided by the Swarm init output on the worker node:

   ```bash
   docker swarm join --token <Swarm-Join-Token> <Manager-IP>:2377
   ```

3. **Create an overlay network**:

   ```bash
   docker network create -d overlay multi_host_network
   ```

4. **Deploy a service** using this overlay network:

   ```bash
   docker service create --name overlay_service --network multi_host_network -p 8080:80 nginx
   ```

5. Verify that the service is accessible from both hosts using `curl`:

   ```bash
   curl http://<Worker-Node-IP>:8080
   ```

**Expected Outcome**:  
The Nginx welcome page should be accessible from both the Manager and Worker nodes, confirming that the overlay network enables cross-host communication.

