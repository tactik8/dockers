# YT-DLP Web UI Docker Stack Setup

This document provides instructions for setting up the YT-DLP Web UI using Docker Swarm.

## 1. Create Persistent Storage Directories

Before deploying the stack, create the necessary directories on your Proxmox host that will store the application's configuration and downloaded files.

```bash
mkdir -p /mnt/nas4/dockers/yt-dlp-webui/config /mnt/nas4/dockers/yt-dlp-webui/downloads
```

## 2. Configure the `compose.yaml` File

Review the `compose.yaml` content provided and make any necessary adjustments:

*   **Ports:** The default port is `8000`. If this port is already in use on your host, change the `Host_Port` part of `- "8000:8000"` (e.g., `- "8080:8000"`).
*   **Timezone (`TZ`):** The timezone is set to `America/New_York`. Adjust this if you are in a different timezone. You can find a list of valid timezones [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).
*   **User/Group ID (`PUID`, `PGID` - Optional):** If you experience permission issues with files created by the container, uncomment and set `PUID` and `PGID` to match the user and group IDs on your host system that should own the files. You can find your user ID with `id -u` and group ID with `id -g`.
*   **Placement Constraints (`node.role`):** In a Docker Swarm, you can control which type of node (manager or worker) the service will run on. Adjust `node.role == manager` as needed.

## 3. Deploy the Stack

Navigate to the directory containing your `compose.yaml` file and deploy the stack using the following command:

```bash
docker stack deploy -c compose.yaml yt-dlp-webui
```

## 4. Access YT-DLP Web UI

Once the stack is deployed, you can access the YT-DLP Web UI in your web browser at `http://<YOUR_PROXMOX_VM_IP>:8000` (replace `8000` with your chosen host port if you changed it).
