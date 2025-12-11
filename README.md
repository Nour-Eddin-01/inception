*This project has been created as part of the 42 curriculum by nait-bou.*

## Description
This project, titled **Inception**, aims to broaden system administration knowledge by using Docker to virtualize several services. Instead of using a single Virtual Machine or container, this project sets up a small infrastructure composed of different services (NGINX, WordPress, MariaDB) running in dedicated containers, linked via a Docker network, and managed by Docker Compose.

The goal is to understand containerization, service orchestration, and best practices for building custom Docker images from scratch (using Dockerfiles) rather than relying on pre-made images.

## Instructions

### Prerequisites
* Docker Engine
* Docker Compose
* Make utility

### Installation & Execution
1.  **Clone the repository:**
    ```bash
    git clone <repository_url> inception
    cd inception
    ```

2.  **Environment Setup:**
    Create a `.env` file in the `srcs` directory. While a template is not provided for security reasons, the system requires the following variables:
    * **Domain:** `DOMAIN_NAME` (e.g., nait-bou.42.fr)
    * **Database:** `MYSQL_DATABASE`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_ROOT_PASSWORD`
    * **WordPress Admin:** `WP_ADMIN_USER`, `WP_ADMIN_PASS`, `WP_ADMIN_EMAIL`
    * **WordPress User:** `WP_USER`, `WP_USER_PASS`, `WP_USER_EMAIL`

3.  **Host Configuration:**
    Ensure your `/etc/hosts` file maps the domain to your local loopback address:
    ```text
    127.0.0.1 nait-bou.42.fr
    ```

4.  **Run the Project:**
    Execute the Makefile at the root:
    ```bash
    make
    ```

### Access
* **Website:** `https://nait-bou.42.fr`
* **Admin Panel:** `https://nait-bou.42.fr/wp-admin`

## Project Overview & Design Choices

This infrastructure separates concerns into three distinct containers:
1.  **NGINX:** Handles SSL/TLS (port 443) and serves as the entry point.
2.  **WordPress:** Runs PHP-FPM to generate dynamic content.
3.  **MariaDB:** Stores persistent data.

### Virtual Machines vs Docker
* **Virtual Machines:** Emulate entire hardware stacks (CPU, RAM, Disk) and require a full Guest OS. They are heavy and slow to boot.
* **Docker:** Uses OS-level virtualization. Containers share the Host OS kernel but have isolated user spaces. This makes them lightweight, fast, and efficient.

### Secrets vs Environment Variables
* **Environment Variables:** Used in this project via a `.env` file. They are simple to implement but can be visible in inspection commands.
* **Docker Secrets:** A more secure method (often used in Swarm) where data is encrypted at rest and mounted as files. This project sticks to Environment Variables as per the mandatory subject requirements.

### Docker Network vs Host Network
* **Docker Network:** Used here. It creates an isolated bridge network where containers communicate by name (DNS). This is secure and keeps internal ports closed to the outside world.
* **Host Network:** The container shares the host's IP and port space directly. This offers less isolation and can cause port conflicts.

### Docker Volumes vs Bind Mounts
* **Docker Volumes:** Managed fully by Docker (in `/var/lib/docker/`).
* **Bind Mounts:** Used in this project (`/home/nait-bou/data`). They map a specific user-controlled host directory to the container. This was chosen to strictly satisfy the subject requirement for data location.

## Resources
### References
* [Docker Documentation](https://docs.docker.com/)
* [NGINX Documentation](https://nginx.org/en/docs/)
* [WordPress CLI Commands](https://developer.wordpress.org/cli/commands/)
* [MariaDB Knowledge Base](https://mariadb.com/kb/en/)

### AI Usage
Generative AI (ChatGPT/Gemini) was used as an assistant for:
* **Scripting:** Debugging the MariaDB bootstrap script to avoid forbidden loops (`while/sleep`).
* **Configuration:** Verifying correct NGINX `location` blocks and SSL syntax.
* **Understanding:** Clarifying the difference between PID 1 execution (`exec`) and child processes.