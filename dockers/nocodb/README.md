### NocoDB Docker Swarm Deployment Instructions

1.  **Create Volume Directory**: Before deploying the stack, ensure the persistent storage directory exists on your Docker host. Run the following command:
    ```bash
    mkdir -p /mnt/nas4/dockers/nocodb/data
    ```

2.  **Save the Compose File**: Save the provided `compose_content` as `nocodb.yaml` (or any other `.yaml` file name).

3.  **Configure Parameters**: Review the `environment` section under the `nocodb` service in the `nocodb.yaml` file. You may want to:
    *   Set initial `NC_ROOT_EMAIL` and `NC_ROOT_PASSWORD` for the NocoDB administrator user. If not set, NocoDB will prompt you to create an account on first access.
    *   Configure external database connection details (e.g., `NC_DB`, `NC_PG_HOST`, etc.) if you wish to use a database other than the default SQLite (which is stored in the `nocodb_data` volume).

4.  **Deploy the Stack**: Navigate to the directory where you saved `nocodb.yaml` and deploy the stack using Docker Swarm:
    ```bash
    docker stack deploy -c nocodb.yaml nocodb
    ```

5.  **Access NocoDB**: Once deployed, NocoDB will be accessible via your Docker host's IP address on port `8080`. For example, `http://YOUR_DOCKER_HOST_IP:8080`.

6.  **Verify Deployment**: You can check the status of your stack and services with:
    ```bash
    docker stack ls
    docker stack services nocodb
    docker service ps nocodb_nocodb
    ```
