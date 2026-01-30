
## YT-DLP Web UI Docker Swarm Setup

This guide will walk you through setting up the YT-DLP Web UI using Docker Swarm.

### 1. Create Persistent Storage Directories

First, you need to create the directories on your host machine that will be used for persistent storage for the YT-DLP Web UI configuration and downloads.

Execute the following command on your Docker host:

```bash
mkdir -p /mnt/nas4/dockers/ytdlp-webui/config /mnt/nas4/dockers/ytdlp-webui/downloads
```

### 2. Configure the `compose.yaml` file

Review the generated `compose.yaml` content. You might want to adjust the following parameters:

*   **Ports**: By default, the web UI is accessible on port `8080`. If this port is already in use or you prefer a different port, modify the `ports` section under the `ytdlp-webui` service:
    ```yaml
        ports:
          - "YOUR_HOST_PORT:8080" # Change YOUR_HOST_PORT to your desired port
    ```
*   **PUID and PGID**: If you encounter permission issues with the downloaded files or configuration, you may need to specify the User ID (PUID) and Group ID (PGID) that the container should run as. Uncomment and modify the `PUID` and `PGID` environment variables with the appropriate values for your system. You can find your user's PUID and PGID by running `id -u` and `id -g` respectively on your host machine.
    ```yaml
        environment:
          # ... other environment variables
          - PUID=1000 # Replace 1000 with your user ID
          - PGID=1000 # Replace 1000 with your group ID
    ```

### 3. Deploy the Stack

Save the `compose.yaml` content to a file named `compose.yaml` (or any other name, e.g., `ytdlp-webui.yaml`) on your Docker manager node.

Navigate to the directory containing your `compose.yaml` file in your terminal and deploy the stack using Docker Swarm:

```bash
docker stack deploy -c compose.yaml ytdlp-webui
```

### 4. Access the YT-DLP Web UI

Once the stack is deployed, the YT-DLP Web UI will be accessible in your web browser at `http://YOUR_DOCKER_HOST_IP:8080` (replace `YOUR_DOCKER_HOST_IP` with the IP address of your Docker host and `8080` with the port you configured if you changed it).

