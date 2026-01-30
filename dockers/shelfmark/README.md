
To set up Shelfmark using this Docker Compose file, follow these steps:

### 1. Create Necessary Directories
Execute the following command on your Docker host to create the required persistent storage directories:

```bash
mkdir -p /mnt/nas4/dockers/shelfmark/db_data /mnt/nas4/dockers/shelfmark/redis_data /mnt/nas4/dockers/shelfmark/nginx_conf /mnt/nas4/dockers/shelfmark/app_code
```

### 2. Clone Shelfmark Repository
Clone the Shelfmark application code into the `app_code` directory:

```bash
git clone https://github.com/calibrain/shelfmark.git /mnt/nas4/dockers/shelfmark/app_code
```

### 3. Install PHP Dependencies
Navigate to the application code directory and install PHP dependencies using Composer. If Composer is not installed on your host, you can run it via a temporary Docker container.

```bash
cd /mnt/nas4/dockers/shelfmark/app_code
docker run --rm -v "$(pwd)":/app composer install --no-dev --optimize-autoloader
```

### 4. Create Nginx Configuration File
Create a file named `default.conf` in `/mnt/nas4/dockers/shelfmark/nginx_conf/` with the following content:

```nginx
server {
    listen 80;
    server_name localhost; # Replace with your domain name (e.g., shelfmark.yourdomain.com)

    root /var/www/html/public; # Shelfmark\'s public directory

    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass shelfmark_app:9000; # Corresponds to the PHP-FPM service name
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

### 5. Configure Environment Variables
You need to set several environment variables. You can do this directly in the `docker-compose.yaml` file (not recommended for sensitive data) or by creating a `.env` file in the same directory as your `docker-compose.yaml`.

**Example `.env` file:**
```env
# Database Credentials (REQUIRED: CHANGE THESE TO STRONG, UNIQUE VALUES)
POSTGRES_DB=shelfmark
POSTGRES_USER=shelfmark_user
POSTGRES_PASSWORD=YOUR_STRONG_POSTGRES_PASSWORD

# Application Key (REQUIRED: GENERATE THIS IN THE NEXT STEP)
APP_KEY=

# Application URL (Update if not localhost)
APP_URL=http://localhost
```

### 6. Generate Application Key
After installing dependencies and before starting the stack, generate a unique application key for Shelfmark. This is typically done by running an Artisan command.

```bash
docker run --rm -v /mnt/nas4/dockers/shelfmark/app_code:/app php:8.1-cli-alpine php /app/artisan key:generate --show
```
Copy the outputted key (e.g., `base64:Abc...`) and paste it as the `APP_KEY` value in your `.env` file or `docker-compose.yaml`.

### 7. Deploy the Stack
Once all files are in place and environment variables are configured, deploy the stack in Docker Swarm mode:

```bash
docker stack deploy -c docker-compose.yaml shelfmark
```

### 8. Run Database Migrations
After the stack is deployed and services are healthy, run the database migrations to set up the Shelfmark schema:

First, find the container ID or name of the `shelfmark_app` service instance (e.g., `docker ps | grep shelfmark_app`). Then execute the migration command:

```bash
docker exec $(docker ps -qf "name=shelfmark_shelfmark_app") php artisan migrate --force
```
Replace `$(docker ps -qf "name=shelfmark_shelfmark_app")` with the actual container ID/name if the command doesn\'t resolve it correctly.

### 9. Access Shelfmark
Shelfmark should now be accessible in your web browser at the `APP_URL` you configured (e.g., `http://localhost`).
