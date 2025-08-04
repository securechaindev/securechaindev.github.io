---
title: Local Deployment
parent: Secure Chain
nav_order: 4
---

# Local Deployment

You can try **Secure Chain** tools in your local machine following the next steps.

## Enviroment File

The first you need is to create an enviroment `(.env)` file from this template:
```bash
# For dockerized backend and database
GRAPH_DB_URI='bolt://neo4j:7687'
VULN_DB_URI='mongodb://mongoSecureChain:mongoSecureChain@mongodb:27017/admin'
ALLOWED_ORIGINS='["http://securechain-gateway:8000"]'

# Databases settings
GRAPH_DB_USER='neo4j'
GRAPH_DB_PASSWORD='neoSecureChain'
VULN_DB_USER='mongoSecureChain'
VULN_DB_PASSWORD='mongoSecureChain'

# Secrets for JWT
ALGORITHM='your_preferred_algorithm'  # e.g., 'HS256'
ACCESS_TOKEN_EXPIRE_MINUTES='access_token_expire_minutes'
REFRESH_TOKEN_EXPIRE_DAYS='refresh_token_expire_days'
JWT_ACCESS_SECRET_KEY='your_access_secret_key'
JWT_REFRESH_SECRET_KEY='your_refresh_secret_key'

# Api key for github services
GITHUB_GRAPHQL_API_KEY='add_your_api_key'
```

## Docker Deployment

The second step only needs as requirements Docker with docker compose. 

### Docker Network

Create a docker network wirh command:
```bash
docker network create securechain
```

### Databases containers

For graphs and vulnerabilities information you need to download the zipped [data dumps]() from **Zenodo**. Once you have unzipped the dumps, inside the root folder copy the `.env` file and run the command:
```bash
docker compose up --build
```

The containerized databases will also be seeded automatically.

### Tools containers

To deploy all **Secure Chain** tools in your local machine as demo, all you need is run this the command `docker compose up --build` with the `.env` file and this docker compose file in the same folder:
```yml
services:
  securechain-gateway:
    container_name: securechain-gateway
    image: ghcr.io/securechaindev/securechain-gateway:latest
    env_file:
      - .env
    ports:
      - '8000:8000'
    networks:
      - securechain
    depends_on:
      securechain-auth:
        condition: service_healthy
      securechain-depex:
        condition: service_healthy

  securechain-depex:
    container_name: securechain-depex
    image: ghcr.io/securechaindev/securechain-depex:latest
    env_file:
      - .env
    ports:
      - '8000'
    networks:
      - securechain
    healthcheck:
      test: ["CMD", "python", "-c", "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')"]
      interval: 5s
      timeout: 3s
      retries: 5

  securechain-auth:
    container_name: securechain-auth
    image: ghcr.io/securechaindev/securechain-auth:latest
    env_file:
      - .env
    ports:
      - '8000'
    networks:
      - securechain
    healthcheck:
      test: ["CMD", "python", "-c", "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')"]
      interval: 5s
      timeout: 3s
      retries: 5

networks:
  securechain:
    name: securechain
    external: true
    driver: bridge
```

<button class="btn js-toggle-dark-mode" style="
  position: fixed;
  top: 1rem;
  right: 1rem;
  z-index: 1000;
">
  🌕
</button>

<script>
  const toggleDarkMode = document.querySelector('.js-toggle-dark-mode'); jtd.addEvent(toggleDarkMode, 'click', function(){ if (jtd.getTheme() === 'dark') { jtd.setTheme('light'); toggleDarkMode.textContent = '🌕'; } else { jtd.setTheme('dark'); toggleDarkMode.textContent = '☀️'; } });
</script>
