### YT-DLP Web UI Deployment Instructions

1.  **Create Volume Directories:**
    Run the following command on your Proxmox VM to create the necessary directories for persistent storage:
    ```bash
    mkdir -p /mnt/nas4/dockers/yt-dlp-webui/config /mnt/nas4/dockers/yt-dlp-webui/downloads
    ```

2.  **Save the Docker Compose File:**
    Save the provided `compose_content` as `compose.yaml` (or `docker-compose.yaml`) in a directory on your Proxmox VM, for example, `/opt/yt-dlp-webui/compose.yaml`.

3.  **Configure Parameters:**
    Edit the `compose.yaml` file and adjust the following parameters under the `environment` section to match your system and preferences:
    *   `PUID`: Set this to the User ID (UID) that Docker should use for the container processes. You can find your user's UID by running `id -u yourusername` on your VM.
    *   `PGID`: Set this to the Group ID (GID) that Docker should use. You can find your user's GID by running `id -g yourusername` on your VM.
    *   `TZ`: Set your desired timezone (e.g., `America/New_York`, `Asia/Tokyo`). A comprehensive list can be found [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).
    *   `ports`: If port `8080` is already in use on your host, change the `HOST_PORT` part (e.g., `9080:8080`) to an available port.

4.  **Deploy the Stack:**
    Navigate to the directory where you saved the `compose.yaml` file (e.g., `/opt/yt-dlp-webui/`). Then, deploy the stack using Docker Swarm:
    ```bash
    docker stack deploy -c compose.yaml yt-dlp-webui
    ```

5.  **Access YT-DLP Web UI:**
    Once the stack is deployed, you can access the YT-DLP Web UI in your web browser at `http://<YOUR_VM_IP_ADDRESS>:8080` (or the `HOST_PORT` you configured).

6.  **To Remove the Stack:**
    ```bash
    docker stack rm yt-dlp-webui
    ```