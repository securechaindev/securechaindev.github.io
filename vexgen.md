---
title: VEXGen
parent: Secure Chain
nav_order: 3
---

<p align="center">
  <img src="/assets/securechain/logos/vexgen-logo.png" alt="VEXGen Logo" width="180"/>
</p>

# VEXGen

## What is VEXGen?

**VEXGen** is an automated tool for generating **VEX (Vulnerability Exploitability eXchange)** and **TIX (Threat Intelligence eXchange)** documents from GitHub repositories.

### Key Features

- 🔍 **Automatic SBOM Discovery** - Finds and processes Software Bill of Materials files
- 🧠 **Smart Code Analysis** - Multi-language analyzer detects actual component usage
- 📊 **Vulnerability Assessment** - Determines exploitability using package affected artefacts
- 📦 **VEX/TIX Generation** - Creates standards-compliant security documents

## Development requirements

1. [Docker](https://www.docker.com/) to deploy the tool.
2. [Docker Compose](https://docs.docker.com/compose/) for container orchestration.
3. It is recommended to use a GUI such as [MongoDB Compass](https://www.mongodb.com/en/products/compass).
4. The Neo4J browser interface to visualize the graph built from the data is in [localhost:7474](http://0.0.0.0:7474/browser/) when the container is running.
5. Python 3.13 or higher.

## Deployment with docker

### 1. Clone the repository
Clone the repository from the official GitHub repository:
```bash
git clone https://github.com/securechaindev/securechain-vexgen.git
cd securechain-vexgen
```

### 2. Configure environment variables
Create a `.env` file from the `template.env` file and place it in the `app/` directory.

#### Get API Keys

- How to get a *GitHub* [API key](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

- Modify the **Json Web Token (JWT)** secret key and algorithm with your own. You can generate your own secret key with the command **openssl rand -base64 32**.

### 3. Create Docker network
Ensure you have the `securechain` Docker network created. If not, create it with:
```bash
docker network create securechain
```

### 4. Databases containers

For graphs and vulnerabilities information you need to download the zipped [data dumps](https://doi.org/10.5281/zenodo.17692376) from **Zenodo**. Once you have unzipped the dumps, inside the root folder run the command:
```bash
docker compose up --build
```

The containerized databases will also be seeded automatically.

### 5. Start the application
Run the command from the project root:
```bash
docker compose -f dev/docker-compose.yml up --build
```

### 6. Access the application
The API will be available at [http://localhost:8002](http://localhost:8002). You can access the API documentation at [http://localhost:8002/docs](http://localhost:8002/docs). Also, in [http://localhost:8001/docs](http://localhost:8001/docs) you can access the auth API documetation.

## Endpoints Specification

### Vulnerability Exploitybility eXchange (VEX) endpoints

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /vex/user</strong>
  <p style="margin: 0;"><strong>Description:</strong> Fetches all VEX documents associated with a specific user.</p>
  <p style="margin: 0;"><strong>Response:</strong> List of VEX documents with metadata and JSON content.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /vex/show/{vex_id}</strong>
  <p style="margin: 0;"><strong>Description:</strong> Fetches a specific VEX document by its ID.</p>
  <p style="margin: 0;"><strong>Path Parameters:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>VEX ID:</strong> The ID of the VEX document to retrieve.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> VEX document metadata and content in JSON format.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /vex/download/{vex_id}</strong>
  <p style="margin: 0;"><strong>Description:</strong> Downloads a VEX document as a ZIP file using a specific VEX ID.</p>
  <p style="margin: 0;"><strong>Path Parameters:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>VEX ID:</strong> The ID of the VEX document to retrieve.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> A downloadable ZIP file containing the VEX document.</p>
</div>

### Thread Intelligence eXchange (TIX) endpoints

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /tix/user</strong>
  <p style="margin: 0;"><strong>Description:</strong> Fetches all TIX documents associated with a specific user.</p>
  <p style="margin: 0;"><strong>Response:</strong> List of TIX documents with metadata and JSON content.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /tix/show/{tix_id}</strong>
  <p style="margin: 0;"><strong>Description:</strong> Fetches a specific TIX document by its ID.</p>
  <p style="margin: 0;"><strong>Path Parameters:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>TIX ID:</strong> The ID of the TIX document to retrieve.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> TIX document metadata and content in JSON format.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /tix/download/{tix_id}</strong>
  <p style="margin: 0;"><strong>Description:</strong> Downloads a TIX document as a ZIP file using a specific TIX ID.</p>
  <p style="margin: 0;"><strong>Path Parameters:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>TIX ID:</strong> The ID of the TIX document to retrieve.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> A downloadable ZIP file containing the TIX document.</p>
</div>

### Generation endpoints

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /vex_tix/generate</strong>
  <p style="margin: 0;"><strong>Description:</strong> Generates VEX and TIX for a specific GitHub repository.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>owner:</strong> The owner of the GitHub repository.</li>
    <li style="margin: 0;"><strong>name:</strong> The name of the GitHub repository.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> A downloadable ZIP file containing generated VEX and TIX documents.</p>
</div>


## License
[GNU General Public License 3.0](https://www.gnu.org/licenses/gpl-3.0.html)

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
