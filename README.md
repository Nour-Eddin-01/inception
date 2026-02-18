# Inception

A system administration project using Docker to containerize a complete web infrastructure stack.

## Overview

This project sets up a small infrastructure composed of different services using **Docker Compose**. The goal is to build a group of Docker containers to serve a WordPress website running on Nginx and MariaDB.

### Services structure
- **NGINX**: Acts as the main entry point, handling HTTPS connections (TLSv1.2/TLSv1.3) and forwarding PHP requests to WordPress.
- **WordPress**: Not an image from Docker Hub, but built from scratch (using Alpine/Debian as base) configured with PHP-FPM.
- **MariaDB**: The database for WordPress, also built from a base image.

Each service runs in its own dedicated container.

## Prerequisites

- **OS**: Linux (Virtual Machine recommended)
- **Tools**: `docker`, `docker-compose`, `make`
- **Root privileges**: Required for creating data directories and managing Docker.

## Configuration

### 1. Host Setup
To access the website via the domain name `nait-bou.42.fr`, you must add it to your hosts file.

```bash
sudo vim /etc/hosts
# Add the following line:
127.0.0.1 nait-bou.42.fr
```

### 2. Environment Variables
Create a `.env` file in the `srcs/` directory to store your secrets.
Example variables typically required:
```env
DOMAIN_NAME=nait-bou.42.fr
MYSQL_ROOT_PASSWORD=rootpass
MYSQL_DATABASE=wordpress
MYSQL_USER=wpuser
MYSQL_PASSWORD=wppass
WP_ADMIN_USER=admin
WP_ADMIN_PASS=adminpass
WP_ADMIN_EMAIL=admin@example.com
```

## Usage

The project is managed via a **Makefile** at the root.

### Build and Run
Build the images and start the containers in detached mode:
```bash
make
```
*Creates data directories at `/home/nait-bou/data` automatically.*

### Stop
Stop the containers (preserves data volumes):
```bash
make down
```

### Clean
Stop containers and remove networks, images, and volumes (Deep clean):
```bash
make fclean
```
*(Note: Check Makefile for specific clean targets available)*

### Logs
View logs for all services:
```bash
docker compose -f srcs/docker-compose.yml logs -f
```

## Access

Once the containers are up:
- **Website**: https://nait-bou.42.fr
- **Admin Panel**: https://nait-bou.42.fr/wp-admin

*Note: You will encounter a security warning because the SSL certificate is self-signed. This is expected.*

## Directory Structure
```
.
├── Makefile                # Command center
├── srcs/
│   ├── docker-compose.yml  # Service orchestration
│   ├── .env                # Environment variables (not in git)
│   └── requirements/
│       ├── mariadb/        # Database setup
│       ├── nginx/          # Web server setup
│       └── wordpress/      # CMS setup
└── ...
```

## Data Persistence

Based on the configuration:
- **WordPress** data is stored in: `/home/nait-bou/data/wordpress`
- **MariaDB** data is stored in: `/home/nait-bou/data/mariadb`

These folders on the host machine ensure that your website data survives container restarts.
