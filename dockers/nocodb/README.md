## NocoDB Docker Swarm Deployment Instructions

1.  **Create Volume Directories:**
    Before deploying, create the necessary directories on your Proxmox host (or NFS mount) for persistent storage:
    ```bash
    mkdir -p /mnt/nas4/dockers/nocodb/data
    mkdir -p /mnt/nas4/dockers/nocodb/postgres
    ```

2.  **Configure `compose.yaml`:**
    Open the generated `compose.yaml` file and configure the following parameters:
    *   **NocoDB Port (`ports`):** By default, NocoDB is exposed on port `8080`. If you need to use a different host port, modify `- "8080:8080"` in the `nocodb` service. For example, to use port `8081` on the host: `- "8081:8080"`.
    *   **PostgreSQL User (`POSTGRES_USER`):** Change `postgres_user` to a strong, unique username for your PostgreSQL database.
    *   **PostgreSQL Password (`POSTGRES_PASSWORD`):** **CRITICAL!** Change `postgres_password` to a very strong, unique password.
    *   **PostgreSQL Database Name (`POSTGRES_DB`):** You can change `nocodb_db` if you wish, but ensure it matches in both `postgres` and `nocodb` services.
    *   **NocoDB Database Connection (`NC_DB`):** Update the `NC_DB` environment variable in the `nocodb` service to reflect your chosen `POSTGRES_USER`, `POSTGRES_PASSWORD`, and `POSTGRES_DB`. The format is `pg://<POSTGRES_USER>:<POSTGRES_PASSWORD>@postgres:5432/<POSTGRES_DB>`.
    *   **Swarm Placement Constraints (`node.labels.nocodb == true`):** The `placement` section in each service specifies that the service should run on a node with the label `nocodb=true`. Adjust this label according to your Docker Swarm node labeling strategy, or remove the `placement` section if you don't use specific node constraints.

3.  **Deploy the Stack:**
    Navigate to the directory containing your `compose.yaml` file and deploy the stack using Docker Swarm:
    ```bash
    docker stack deploy -c compose.yaml nocodb
    ```

4.  **Access NocoDB:**
    Once deployed, NocoDB should be accessible in your web browser at `http://<YOUR_DOCKER_SWARM_MANAGER_IP>:<NocoDB_Port>`.

