To set up Memos, follow these steps:

1.  **Create the data directory**:
    Execute the following command on your Docker host to create the necessary persistent storage directory:
    ```bash
    mkdir -p /mnt/nas4/dockers/memos/data
    ```

2.  **Review the `compose.yaml` file**:
    *   **`HOST_PORT`**: By default, Memos will be accessible on port `5230` of your Docker host. If this port is already in use, or you prefer a different port, modify the `ports` section in the `compose.yaml` file:
        ```yaml
        ports:
          - "YOUR_DESIRED_HOST_PORT:5230"
        ```

3.  **Deploy the stack**:
    Save the provided `compose_content` as `compose.yaml` and deploy the stack using Docker Swarm:
    ```bash
    docker stack deploy -c compose.yaml memos
    ```

After deployment, Memos should be accessible via your browser at `http://YOUR_DOCKER_HOST_IP:HOST_PORT` (e.g., `http://your-server-ip:5230`).