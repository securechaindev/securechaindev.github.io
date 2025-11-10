---
title: Local Deployment
parent: Secure Chain
nav_order: 5
---

# Local Deployment

You can try **Secure Chain** tool in your local machine for development purposes following the next steps.

## Enviroment File

The first you need is to create an enviroment `(.env and .env.local)` file from this template:

```bash
# For dockerized backend and database
GRAPH_DB_URI='bolt://neo4j:7687'
VULN_DB_URI='mongodb://mongoSecureChain:mongoSecureChain@mongo:27017/admin'
DOCS_URL='/docs' # Comment this on production
SERVICES_ALLOWED_ORIGINS='["http://securechain-gateway:8000"]'
GATEWAY_ALLOWED_ORIGINS='["http://securechain-frontend"]'
BACKEND_URL='http://securechain-gateway:8000'
NODE_ENV='development' # Change to 'production' for production builds

# Databases settings
GRAPH_DB_USER='neo4j' # Change in production
GRAPH_DB_PASSWORD='neoSecureChain' # Change in production
VULN_DB_USER='mongoSecureChain' # Change in production
VULN_DB_PASSWORD='mongoSecureChain' # Change in production

# Secrets for JWT
SECURE=False # Set to True in production
ALGORITHM='your_preferred_algorithm'  # e.g., 'HS256'
ACCESS_TOKEN_EXPIRE_MINUTES='access_token_expire_minutes'
REFRESH_TOKEN_EXPIRE_DAYS='refresh_token_expire_days'
JWT_ACCESS_SECRET_KEY='your_access_secret_key'
JWT_REFRESH_SECRET_KEY='your_refresh_secret_key'

# Api key for github services
GITHUB_GRAPHQL_API_KEY='add_your_api_key'

# Dockerized Redis Configuration (for Dagster integration)
REDIS_HOST=redis
REDIS_PORT=6379
REDIS_DB=0
REDIS_STREAM=package-extraction
REDIS_GROUP=extractors
REDIS_CONSUMER=package-consumer

# Git python environments
# Git python refresh required env variable. Use one of the following values:
# - quiet|q|silence|s|silent|none|n|0: for no message or exception
# - warn|w|warning|log|l|1: for a warning message (logging level CRITICAL, displayed by default)
# - error|e|exception|raise|r|2: for a raised exception
GIT_PYTHON_REFRESH='select_your option'
GIT_CONFIG_SYSTEM='/dev/null'
GIT_CONFIG_GLOBAL='/dev/null'
GIT_LFS_SKIP_SMUDGE='1'
GIT_TEMPLATE_DIR=''
```

## Nginx configuration

The second step is to create an Nginx configuration in the folder `nginx/templates/default.conf.template` following this template:

```nginx
# IP limit: 10 requests/second with a 10 MB shared memory zone
limit_req_zone $binary_remote_addr zone=api_ratelimit:10m rate=10r/s;

# Limit simultaneous connections per IP (not essential)
limit_conn_zone $binary_remote_addr zone=perip:10m;

server {
    listen 80;
    listen [::]:80;

    root /usr/share/nginx/html;
    index index.html;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    # Gzip compression
    gzip on;
    gzip_types
        text/plain
        text/css
        text/js
        text/xml
        text/javascript
        application/javascript
        application/json
        application/xml+rss;
    gzip_min_length 1000;

    # Cache static assets
    location /_next/static/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    location /images/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    location /assets/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }

    # Return 429 instead of 503 when the limit is exceeded
    limit_req_status 429;

    # Favicon for API routes
    location /api/favicon.ico {
        alias /usr/share/nginx/html/images/securechain-logo.ico;
        expires 1y;
        add_header Cache-Control "public, immutable";
        access_log off;
    }

    # API routes - proxy to backend
    location /api/ {
        proxy_pass $BACKEND_URL/;
        proxy_http_version 1.1;

        proxy_set_header Host              $host;
        proxy_set_header X-Real-IP         $remote_addr;
        proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_cookie_domain off;
        proxy_cookie_path   off;

        proxy_pass_header Set-Cookie;

        # Allows small bursts (burst 20) without rejection (no nodelay = delayed)
        limit_req zone=api_ratelimit burst=20;
    }

    # SPA fallback
    location / {
        try_files $uri $uri/ $uri.html /index.html;
    }

    # Health check
    location /health {
        access_log off;
        return 200 "healthy\n";
        add_header Content-Type text/plain;
    }

    # Deny access to hidden files
    location ~ /\. {
        deny all;
    }

    # Error pages
    error_page 404 /404.html;
    error_page 500 502 503 504 /500.html;
}
```

## Docker Deployment

The third step only needs as requirements Docker with docker compose. 

### Docker Network

Create a docker network with command:
```bash
docker network create securechain
```

### Databases containers

For graphs and vulnerabilities information you need to download the zipped [data dumps](https://doi.org/10.5281/zenodo.16739081) from **Zenodo**. Once you have unzipped the dumps, inside the root folder copy the `.env` file and run the command:
```bash
docker compose up --build
```

The containerized databases will also be seeded automatically.

### Tools containers

To deploy all **Secure Chain** tools in your local machine as demo, all you need is run this the command `docker compose up --build` with the `.env` file and this docker compose file in the same folder:
```yml
services:
  securechain-frontend:
    container_name: securechain-frontend
    image: ghcr.io/securechaindev/securechain-frontend:latest
    env_file:
      - .env.local
    ports:
      - "80:80"
    networks:
      - securechain
    depends_on:
      securechain-gateway:
        condition: service_healthy
    volumes:
      - ./nginx/templates:/etc/nginx/templates:ro

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
      securechain-vexgen:
        condition: service_healthy
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

  redis:
    container_name: securechain-redis
    image: redis:7-alpine
    ports:
      - '6379:6379'
    volumes:
      - redis-data:/data
    networks:
      - securechain
    command: redis-server --appendonly yes

  securechain-vexgen:
    container_name: securechain-vexgen
    image: ghcr.io/securechaindev/securechain-vexgen:latest
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

volumes:
  redis-data:
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
  const toggleDarkMode = document.querySelector('.js-toggle-dark-mode');
  jtd.addEvent(toggleDarkMode, 'click', function () {
    if (jtd.getTheme() === 'dark') {
      jtd.setTheme('light');
      toggleDarkMode.textContent = '🌕';
    } else {
      jtd.setTheme('dark');
      toggleDarkMode.textContent = '☀️';
    }
  });
</script>
