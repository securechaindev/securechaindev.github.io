---
title: Depex
parent: Secure Chain
nav_order: 2
---

<p align="center">
  <img src="/assets/securechain/logos/depex-logo.png" alt="Depex Logo" width="180"/>
</p>

# Depex

## What is Depex?

Depex is a tool that allows you to reason over the entire configuration space of the Software Supply Chain of an open-source software repository.

### Key Features

- 🔍 **Multi-ecosystem support:** Analyzes Python, JavaScript, Ruby, Rust, Java, and PHP dependencies
- 🧮 **SMT-based reasoning:** Uses Z3 solver to find optimal dependency configurations
- 📊 **Graph analysis:** Visualize and query dependency graphs using Neo4j
- ⚡ **High performance:** Async architecture with Redis caching for SSC ingestion with Dagster

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
git clone https://github.com/securechaindev/securechain-depex.git
cd securechain-depex
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

### 7. Visualize the graph database
Access Neo4j browser interface at [http://localhost:7474](http://localhost:7474/browser/) to visualize and query the dependency graphs.

### 8. Monitor databases
- **MongoDB Compass:** Connect to MongoDB at `mongodb://localhost:27017` to browse documents
- **Redis:** Connect to `localhost:6379` to monitor cache

## Endpoints Specification

### Graph endpoints

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /graph/repositories</strong>
  <p style="margin: 0;"><strong>Description:</strong> Retrieve a list of repositories for a specific user.</p>
  <p style="margin: 0;"><strong>Response:</strong> List of user repositories</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /graph/package/status</strong>
  <p style="margin: 0;"><strong>Description:</strong> Retrieve the status of a specific package.</p>
  <p style="margin: 0;"><strong>Query Parameters:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Package Name:</strong> The name of the package.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Package vulnerability status information.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00aaffff; margin: 0;">GET /graph/version/status</strong>
  <p style="margin: 0;"><strong>Description:</strong> Retrieve the status of a specific version.</p>
  <p style="margin: 0;"><strong>Query Parameters:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Package Name:</strong> The name of the package.</li>
    <li style="margin: 0;"><strong>Version Name:</strong> The name of the version.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Version vulnerability status information.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /graph/package/init</strong>
  <p style="margin: 0;"><strong>Description:</strong> Initialize a specific package into the graph.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Package Name:</strong> The name of the package.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Initialization of the package background extraction process.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /graph/repository/init</strong>
  <p style="margin: 0;"><strong>Description:</strong> Initialize a specific repository into the graph.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Owner:</strong> The owner of the repository.</li>
    <li style="margin: 0;"><strong>Name:</strong> The name of the repository.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Initialization of the repository background extraction process.</p>
</div>

### SSC Operations endpoints

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/scc/file_info</strong>
  <p style="margin: 0;"><strong>Description:</strong> Retrieve Software Supply Chain information about a specific requirement file.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Requirement File ID:</strong> The ID of the requirement file from which its supply chain will be extracted.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Detailed requirement file supply chain information including direct and indirect dependencies.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/scc/package_info</strong>
  <p style="margin: 0;"><strong>Description:</strong> Retrieve Software Supply Chain information about a specific package.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Package Name:</strong> The name of the package from which its supply chain will be extracted.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Detailed package supply chain information including direct and indirect dependencies.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/scc/version_info</strong>
  <p style="margin: 0;"><strong>Description:</strong> Retrieve Software Supply Chain information about a specific version.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Package Name:</strong> The name of the parent package of the version.</li>
    <li style="margin: 0;"><strong>Version Name:</strong> The name of the version from which its supply chain will be extracted.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Detailed version supply chain information including direct and indirect dependencies.</p>
</div>

### SMT Operations endpoints

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/smt/valid_graph</strong>
  <p style="margin: 0;"><strong>Description:</strong> Validate the graph of a requirement file up to a specified level.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Requirement File ID:</strong> The ID of the requirement file where the config have been selected.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Graph validation result.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/smt/minimize_impact</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the configurations with the minimized impact of a specific requirement file.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Requirement File ID:</strong> The ID of the requirement file where the config have been selected.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Agregator:</strong> Aggragation method (tipically mean of weighted mean).</li>
    <li style="margin: 0;"><strong>Limit:</strong> The number of configurations to return ordered by impact.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Minimized impact configurations.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/smt/maximize_impact</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the configurations with the maximize impact of a specific requirement file.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Requirement File ID:</strong> The ID of the requirement file where the config have been selected.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Agregator:</strong> Aggragation method (tipically mean of weighted mean).</li>
    <li style="margin: 0;"><strong>Limit:</strong> The number of configurations to return ordered by impact.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Maximize impact configurations.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/smt/filter_configs</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the filtered configurations of a specific requirement file.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Requirement File ID:</strong> The ID of the requirement file where the config have been selected.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Agregator:</strong> Aggragation method (tipically mean of weighted mean).</li>
    <li style="margin: 0;"><strong>Max Threshold:</strong> Maximum threshold for filtering between 0 and 10.</li>
    <li style="margin: 0;"><strong>Min Threshold:</strong> Minimum threshold for filtering between 0 and 10.</li>
    <li style="margin: 0;"><strong>Limit:</strong> The number of configurations to return ordered by impact.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Filtered configurations.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/smt/valid_config</strong>
  <p style="margin: 0;"><strong>Description:</strong> Validate the configuration based on a requirement file.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Requirement File ID:</strong> The ID of the requirement file where the config have been selected.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Agregator:</strong> Aggragation method (tipically mean of weighted mean).</li>
    <li style="margin: 0;"><strong>Config:</strong> Configuration of dependencies versions to validate.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Validation result or message if no dependencies found.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/smt/complete_config</strong>
  <p style="margin: 0;"><strong>Description:</strong> Complete a partial configuration.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Requirement File ID:</strong> The ID of the requirement file where the config have been selected.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Agregator:</strong> Aggragation method (tipically mean of weighted mean).</li>
    <li style="margin: 0;"><strong>Config:</strong> Partial configuration of dependencies versions to complete.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Completed configuration with the min impact.</p>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #00ff37ff; margin: 0;">POST /operation/smt/config_by_impact</strong>
  <p style="margin: 0;"><strong>Description:</strong> Give a configuration based on impact.</p>
  <p style="margin: 0;"><strong>Request Body:</strong></p>
  <ul style="margin: 0;">
    <li style="margin: 0;"><strong>Requirement File ID:</strong> The ID of the requirement file where the config have been selected.</li>
    <li style="margin: 0;"><strong>Max Level:</strong> The max level of depth in the graph.</li>
    <li style="margin: 0;"><strong>Node Type:</strong> The type of package manager node.</li>
    <li style="margin: 0;"><strong>Agregator:</strong> Aggragation method (tipically mean of weighted mean).</li>
    <li style="margin: 0;"><strong>Config:</strong> Impact criteria between 0 and 10.</li>
  </ul>
  <p style="margin: 0;"><strong>Response:</strong> Configuration result based on impact.</p>
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
