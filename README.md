# Nginx Proxy Manager Docker Setup

This repository contains a Docker Compose configuration for easily deploying Nginx Proxy Manager with a MariaDB database backend. The setup uses environment variables for all configuration options, making it flexible and secure.

## What is Nginx Proxy Manager?

Nginx Proxy Manager (NPM) is a Docker container that enables you to easily forward domains and subdomains to your various Docker containers and other services, with free SSL management via Let's Encrypt certificates. It includes a beautiful web interface to easily manage your proxies, hosts, and certificates.

## Features

- **Easy deployment** with Docker Compose
- **Environment variable configuration** via `.env` file
- **MariaDB backend** for configuration storage
- **Automatic SSL certificate generation** via Let's Encrypt
- **Web UI** for easy management

## Requirements

- Docker Engine (20.10+)
- Docker Compose (2.0+)
- Open ports for HTTP (80), HTTPS (443), and Admin Panel (81)

## Quick Start

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/nginx-proxy-manager.git
   cd nginx-proxy-manager
   ```

2. Customize your configuration by editing the `.env` file:
   ```bash
   cp .env.example .env
   nano .env
   ```

3. Start the containers:
   ```bash
   docker-compose up -d
   ```

4. Access the admin interface at http://localhost:81 (or your server IP)

5. Login with the default credentials:
   - Email: `admin@example.com`
   - Password: `changeme`

   **Important**: Change these default credentials after your first login!

## Configuration

The `.env` file contains all configurable options:

| Variable | Description | Default |
|----------|-------------|---------|
| NGINX_IMAGE | Nginx Proxy Manager image | jc21/nginx-proxy-manager:latest |
| HTTP_PORT | HTTP port | 80 |
| HTTPS_PORT | HTTPS port | 443 |
| ADMIN_PORT | Admin interface port | 81 |
| DB_MYSQL_HOST | Database host | db |
| DB_MYSQL_PORT | Database port | 3306 |
| DB_MYSQL_USER | Database user | npm |
| DB_MYSQL_PASSWORD | Database password | npm |
| DB_MYSQL_NAME | Database name | npm |
| DISABLE_IPV6 | Disable IPv6 | true |
| DB_IMAGE | MariaDB image | jc21/mariadb-aria:latest |
| MYSQL_ROOT_PASSWORD | MySQL root password | npm |
| MYSQL_DATABASE | MySQL database name | npm |
| MYSQL_USER | MySQL user | npm |
| MYSQL_PASSWORD | MySQL user password | npm |
| MARIADB_AUTO_UPGRADE | Auto upgrade MariaDB | 1 |

## Data Volumes

The setup creates three Docker volumes:

1. `data` - Stores Nginx Proxy Manager configuration
2. `letsencrypt` - Stores SSL certificates from Let's Encrypt
3. `mysql` - Stores the MariaDB database

## Common Tasks

### Adding a Proxy Host

1. Access the admin interface at http://localhost:81
2. Go to "Proxy Hosts" and click "Add Proxy Host"
3. Fill in the details:
   - Domain Name: your-domain.com
   - Scheme: http or https
   - Forward Hostname/IP: IP or hostname of your service
   - Forward Port: Port of your service
4. Configure SSL if needed (Let's Encrypt can be set up automatically)
5. Save

### Updating the Application

To update to the latest version:

```bash
docker-compose pull
docker-compose up -d
```

## Troubleshooting

### Cannot access the admin interface

- Verify that all containers are running: `docker-compose ps`
- Check container logs: `docker-compose logs -f app`
- Ensure port 81 is not being used by another service
- Check firewall settings to allow traffic on ports 80, 443, and 81

### Database connection issues

- Verify that the database container is running: `docker-compose ps db`
- Check database logs: `docker-compose logs -f db`
- Ensure that the environment variables in the `.env` file match the configuration

## Security Recommendations

1. Change default database credentials in the `.env` file
2. Change the admin user credentials after first login
3. Do not expose port 81 to the internet (use a local network or VPN to access)
4. Configure a strong firewall to only allow necessary traffic
