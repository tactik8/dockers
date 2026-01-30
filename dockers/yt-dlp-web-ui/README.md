### YT-DLP Web UI Docker Compose Instructions

1.  **Create Volumes**: Execute the `volume_command` to create the necessary directories for persistent storage on your NFS mount.
2.  **Configure Parameters**:
    *   **`PUID` and `PGID`**: Change `1000` to your user ID and group ID respectively. You can find these by running `id <your_username>` on your Proxmox server or Docker VM.
    *   **`TZ`**: Update `Etc/UTC` to your local timezone (e.g., `Europe/London`, `America/New_York`).
    *   **`PORT`**: If port `8010` is already in use or you prefer a different port, change the host port (`8010` on the left side of the colon in `ports: - "8010:8010"`) to an available port.
3.  **Deploy the Stack**: Save the `compose_content` as `compose.yaml` and deploy it using Docker Swarm.
    ```bash
    docker stack deploy -c compose.yaml yt-dlp-web-ui
    ```
4.  **Access the UI**: Once deployed, you can access the YT-DLP Web UI by navigating to `http://<YOUR_SERVER_IP>:8010` in your web browser.