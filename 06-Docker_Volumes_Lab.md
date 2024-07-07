## Hands-on Exercises for Docker Volumes

### Exercise 1: Creating and Inspecting Volumes

#### Steps:
1. **Create a volume named `training_volume`:**
   ```sh
   docker volume create training_volume
   ```

2. **Inspect the volume and note its properties:**
   ```sh
   docker volume inspect training_volume
   ```

#### Expected Output:
- Details of the volume, including its name, driver, mountpoint, and other metadata.

### Exercise 2: Persisting Data

#### Steps:
1. **Create a container that uses `training_volume` to store data:**
   ```sh
   docker run -d -v training_volume:/data --name data_container busybox
   ```

2. **Write data to the volume from within the container:**
   ```sh
   docker exec data_container sh -c "echo 'Persisted Data' > /data/persisted_data.txt"
   ```

3. **Remove the container:**
   ```sh
   docker rm -f data_container
   ```

4. **Create a new container to verify data persistence:**
   ```sh
   docker run -d -v training_volume:/data --name new_data_container busybox
   docker exec new_data_container cat /data/persisted_data.txt
   ```

#### Expected Output:
- The contents of `persisted_data.txt` should display "Persisted Data".

### Exercise 3: Volume Sharing

#### Steps:
1. **Set up a container to write data to a shared volume:**
   ```sh
   docker run -d -v shared_volume:/shared_data --name writer_container busybox sh -c "echo 'Shared Data' > /shared_data/shared.txt"
   ```

2. **Set up another container to read data from the shared volume:**
   ```sh
   docker run -d -v shared_volume:/shared_data --name reader_container busybox sh -c "cat /shared_data/shared.txt"
   ```

3. **Check the logs of the reader container:**
   ```sh
   docker logs reader_container
   ```

#### Expected Output:
- The logs should display "Shared Data".

### Exercise 4: Docker Compose and Named Volumes

#### Steps:
1. **Create a Docker Compose file (`docker-compose.yml`) that sets up a service using a named volume:**
   ```yaml
   version: '3.8'
   services:
     web:
       image: nginx
       volumes:
         - webdata:/usr/share/nginx/html
   volumes:
     webdata:
   ```

2. **Deploy the stack:**
   ```sh
   docker-compose up -d
   ```

3. **Verify volume usage:**
   - Check that the volume `webdata` has been created:
     ```sh
     docker volume ls
     ```

   - Inspect the volume:
     ```sh
     docker volume inspect webdata
     ```

   - Access the web server to verify that the data is being served from the volume:
     ```sh
     curl localhost
     ```



