# Nginx Proxy Manager with Environment Variables

This repository contains a refined Docker Compose configuration for Nginx Proxy Manager that implements best practices for configuration management by using environment variables.

## Purpose

The purpose of this project is to demonstrate how to refine GitHub repositories by implementing proper configuration management. Specifically, this example shows how to:

1. Move sensitive and configurable data from hardcoded values in Docker Compose files to environment variables
2. Improve maintainability and portability of Docker configurations
3. Follow security best practices for Docker deployments

## Components

The setup consists of:

- **Nginx Proxy Manager**: A web interface for managing Nginx proxy hosts
- **MariaDB**: Database backend for storing proxy configurations
- **Environment Variables**: Centralized in a `.env` file for easier management

## Prerequisites

- Docker and Docker Compose installed on your system
- Basic understanding of networking concepts
- Port 80, 443, and 81 available on your host machine (or customized in the `.env` file)

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/nginx-proxy-manager-env.git
cd nginx-proxy-manager-env
```

### 2. Configure Environment Variables

The `.env` file contains all configurable parameters. Review and adjust according to your needs:

```properties
# Container configuration
NGINX_CONTAINER_NAME=Nginx
MARIADB_CONTAINER_NAME=MariaDB

# Network ports
HTTP_PORT=80
HTTPS_PORT=443
ADMIN_PORT=81

# Database configuration
DB_HOST=db
DB_PORT=3306
DB_USER=npm
DB_PASSWORD=npm
DB_NAME=npm
MYSQL_ROOT_PASSWORD=npm

# IPv6 configuration
DISABLE_IPV6=true

# Database auto upgrade
MARIADB_AUTO_UPGRADE=1
```

> **Important Security Note**: For production environments, change the default database passwords to strong, unique values.

### 3. Start the Services

```bash
docker compose up -d
```

### 4. Access the Admin Interface

Once containers are running, access the admin interface at:

```
http://your-server-ip:81
```

Default login credentials:
- Email: `admin@example.com`
- Password: `changeme`

> **Important**: Change the default admin credentials immediately after first login.

## Environment Variables Explained

### Container Names
- `NGINX_CONTAINER_NAME`: Name for the Nginx Proxy Manager container
- `MARIADB_CONTAINER_NAME`: Name for the MariaDB container

### Network Ports
- `HTTP_PORT`: Public HTTP port (default: 80)
- `HTTPS_PORT`: Public HTTPS port (default: 443)
- `ADMIN_PORT`: Admin web interface port (default: 81)

### Database Configuration
- `DB_HOST`: Database hostname (default: db)
- `DB_PORT`: Database port (default: 3306)
- `DB_USER`: Database username
- `DB_PASSWORD`: Database user password
- `DB_NAME`: Database name
- `MYSQL_ROOT_PASSWORD`: MariaDB root password

### Additional Settings
- `DISABLE_IPV6`: Disable IPv6 support if needed (default: true)
- `MARIADB_AUTO_UPGRADE`: Enable automatic database upgrades (default: 1)

## Customization

### Changing Ports

If ports 80, 443, or 81 are already in use on your system, you can modify the corresponding variables in the `.env` file:

```properties
HTTP_PORT=8080
HTTPS_PORT=8443
ADMIN_PORT=8081
```

### Persistent Data

All data is stored in Docker volumes:
- `data`: Nginx Proxy Manager data
- `letsencrypt`: SSL certificates
- `mysql`: Database files

## Security Recommendations

1. **Change Default Passwords**: Modify all default passwords in the `.env` file
2. **Restrict Admin Port**: Consider using a firewall to restrict access to port 81
3. **Add .env to .gitignore**: Never commit your `.env` file to version control
4. **Regular Backups**: Implement backup procedures for the Docker volumes

## Troubleshooting

### Database Connection Issues

If Nginx Proxy Manager cannot connect to the database:
1. Ensure the MariaDB container is running: `docker ps`
2. Check logs for errors: `docker logs MariaDB`
3. Verify environment variables are correctly set

### Port Conflicts

If you see errors like "port is already allocated":
1. Identify which application is using the port: `sudo lsof -i :80`
2. Either stop the conflicting application or change the port in the `.env` file

## Contributing

Contributions to improve this configuration are welcome. Please follow these steps:
1. Fork the repository
2. Create a feature branch
3. Submit a pull request with a clear description of your changes

## License

This project is licensed under the MIT License - see the LICENSE file for details.
