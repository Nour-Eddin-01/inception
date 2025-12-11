# Developer Documentation

## 1. Setting Up the Environment
To deploy this project on a fresh machine:

1.  **Prerequisites:** Install Docker Engine, Docker Compose, and Make.
2.  **DNS:** Add `127.0.0.1 nait-bou.42.fr` to your `/etc/hosts` file.
3.  **Secrets:** Create a `.env` file in `srcs/` containing the required database and WordPress variables (refer to `USER_DOC.md` for keys).
4.  **Folders:** Ensure the directory `/home/nait-bou/data` exists or can be created by the Makefile.

## 2. Building and Launching
The project uses `docker-compose.yml` located in `srcs/` but is orchestrated via the root `Makefile`.

* **Launch:** Run `make`. This executes `docker compose up -d --build`.
* **Rebuild:** Run `make re`. This stops containers, removes Docker images, and rebuilds them from scratch using the Dockerfiles.

## 3. Container Management
* **Logs:** `make logs` (Follows log output for all services).
* **Shell Access:**
    * WordPress: `docker exec -it wordpress /bin/bash`
    * MariaDB: `docker exec -it mariadb /bin/bash`
    * NGINX: `docker exec -it nginx /bin/bash`

## 4. Data Persistence
Data is persisted using **Bind Mounts** to the host machine.
* **Database Data:** Stored in `/home/nait-bou/data/mariadb`. Maps to `/var/lib/mysql` inside the container.
* **Website Files:** Stored in `/home/nait-bou/data/wordpress`. Maps to `/var/www/html` inside the container.

Running `make down` does **not** delete this data. To verify persistence, you can stop the project and restart it; your WordPress posts and users will remain.