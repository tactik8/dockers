To deploy your Mealie stack:

1.  **Create the persistent storage directories**:
    Run the `volume_command` provided to create the necessary directories on your NFS mount.

2.  **Configure the `compose.yaml` file**:
    Before deploying, you **MUST** replace the placeholder values (`${POSTGRES_USER}`, `${POSTGRES_PASSWORD}`, `${POSTGRES_DB}`, `${SECRET_KEY}`) in the `compose.yaml` with your desired credentials and a strong, random secret key.

    *   `POSTGRES_USER`: Choose a username for your PostgreSQL database.
    *   `POSTGRES_PASSWORD`: Choose a strong password for your PostgreSQL database.
    *   `POSTGRES_DB`: Choose a name for your PostgreSQL database.
    *   `SECRET_KEY`: Generate a long, random alphanumeric string for Mealie's secret key. This is crucial for security.

3.  **Save the file**:
    Save the provided `compose_content` as `docker-compose.yaml` in a directory (e.g., `/opt/mealie`).

4.  **Deploy the stack**:
    Navigate to the directory where you saved `docker-compose.yaml` in your terminal and run the following command to deploy the stack in Docker Swarm mode:
    ```bash
    docker stack deploy -c docker-compose.yaml mealie
    ```

5.  **Access Mealie**:
    Once the services are up, you can access Mealie in your web browser at `http://YOUR_DOCKER_HOST_IP:9000`. The first time you access it, you will likely be prompted to create an admin user.
