# Deployment Manifests (Source of Truth)

This repository serves as the single source of truth for all Docker Compose files, environment variable templates, Dockerfiles, and Nginx configurations required to deploy the microservices within the portfolio.

The primary goal is to provide repeatable, verifiable, and version-controlled infrastructure for local development and staging environments.

## 🚀 Getting Started

Deployment instructions are intentionally kept minimal and stable. The full, secure, step-by-step deployment command is anchored in a dedicated Gist (the public entry point).

*   **Locate the Gist:** Refer to the public Gist for the latest `curl` commands.
*   **Execute:** The steps download the files from the relevant project directory (e.g., `/mtd-itsa-compliance`) into a local directory for execution.

## 🗂️ Structure

Each top-level directory corresponds to a unique microservice or application stack:

````
deployment-manifests/
├── mtd-itsa-compliance/
│   ├── docker-compose.yml
│   ├── .env
│   ├── backend/Dockerfile
│   └── frontend/
│       ├── Dockerfile
│       └── nginx.conf
└── user-auth-service/
    └── ... (Future project files)
````

## ⚙️ Configuration Protocol

All service configurations adhere to the **Manual Port Assignment Protocol (MPAP)** to prevent host port collisions:

*   **Host Port Range:** All services run in the clean `42xxx` port range.
*   **Project Allocation:** Projects are assigned a unique, dedicated `100-port` block (e.g., `42000-42099` for `mtd-itsa-compliance`).
*   **Service Ports:** External services within a project increment backwards by `10` from the base port, ensuring clean, predictable addresses (e.g., `42060`, `42050`, `42040`).


## Building Images

1.  **Configure Variables:** Define the `CONFIG_URL` and `APP_DIR` environment variables.

    ````bash
    CONFIG_URL="https://raw.githubusercontent.com/eozgit/deployment-manifests/main/mtd-itsa-compliance"
    APP_DIR="mtd-itsa-compliance-config"
    ````

2.  **Prepare Directory Structure:** Create the local configuration directory and necessary subdirectories for the Docker build context.

    ````bash
    mkdir -p $APP_DIR/backend $APP_DIR/frontend
    cd $APP_DIR
    ````

3.  **Download Core Configuration Files:** Download the main orchestration (`docker-compose.yml`) and environment (`.env`) files.

    ````bash
    curl -sO $CONFIG_URL/docker-compose.yml
    curl -sO $CONFIG_URL/.env
    ````

4.  **Download Docker Build Context Files:** Download the necessary Dockerfiles and the Nginx configuration, ensuring they land in the correct subdirectories.

    ````bash
    curl -sO $CONFIG_URL/backend/Dockerfile -o backend/Dockerfile
    curl -sO $CONFIG_URL/frontend/Dockerfile -o frontend/Dockerfile
    curl -sO $CONFIG_URL/frontend/nginx.conf -o frontend/nginx.conf
    ````

5.  **Edit Secrets:** **MANUALLY** open the downloaded `.env` file and replace the placeholder values (especially `SQL_SA_PASSWORD` and `ConnectionStrings__DefaultConnection`) with your actual secrets.

    ````bash
    # Example to open the file for editing:
    nano .env 
    # OR 
    code .env
    ````

6.  **Build and Run the Stack:** Start the services in detached mode (`-d`) and explicitly build the images (`--build`) using the downloaded configuration files.

    ````bash
    docker compose up --build -d
