---
title: Secure Chain MCP Server
parent: Secure Chain
nav_order: 4
---

# Secure Chain MCP Server

An MCP server that provides tools for checking the status of your software supply chain within the context of Secure Chain.

## Development requirements

1. [Docker](https://www.docker.com/) to deploy the tool.
2. [Docker Compose](https://docs.docker.com/compose/) for container orchestration.
3. It is recommended to use a GUI such as [MongoDB Compass](https://www.mongodb.com/en/products/compass).
4. The Neo4J browser interface to visualize the graph built from the data is in [localhost:7474](http://0.0.0.0:7474/browser/) when the container is running.
5. Python 3.13 or higher.

## Use Secure Chain MCP with VSCode

### 1. Register on Secure Chain

Go to [Secure Chain](https://securechain.dev/) official lading page, and register yourself as a user.

### 2. Add mcp configuration

Finally, inside the folder `.vscode` add the file `mcp.json` with the next configuration, and start the mcp server:
```json
{
  "servers": {
    "Secure Chain MCP Server": {
      "type": "http",
      "url": "https://mcp.securechain.dev/mcp",
      "headers": {
        "X-Auth-Email": "your_email",
        "X-Auth-Pass": "your_super_secret_password"
      }
    }
  }
}
```

## Deployment with docker

### 1. Clone the repository

Clone the repository from the official GitHub repository:

```bash
git clone https://github.com/securechaindev/securechain-mcp-server.git
cd securechain-mcp-server
```

### 2. Configure environment variables

Create a `.env.local` file from the `.env.example` file and place it in the root directory.

#### Get API Keys

- How to get a _GitHub_ [API key](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

- Modify the **Json Web Token (JWT)** secret key and algorithm with your own. You can generate your own secret key with the command **openssl rand -base64 32**.

### 3. Create Docker network

Ensure you have the `securechain` Docker network created. If not, create it with:

```bash
docker network create securechain
```

### 4. Databases containers

For graphs and vulnerabilities information you need to download the zipped [data dumps](https://doi.org/10.5281/zenodo.16739081) from Zenodo. Once you have unzipped the dumps, inside the root folder run the command:

```bash
docker compose up --build
```

The containerized databases will also be seeded automatically.

### 5. Start the application

Run the command from the project root:

```bash
docker compose -f dev/docker-compose.yml up --build
```

### 6. Create a User in Secure Chain local deployment

Go [here](http://localhost:8000/docs#/Secure%20Chain%20Auth%20-%20User/signup_signup_post) and create an user, for example:

```json
{
  "email": "mcp-bot@example.com",
  "password": "supersecre3T*"
}
```

### 7. Configure the MCP with VSCode

Inside the folder `.vscode/` add the file `mcp.json` with this template:

```json
{
  "servers": {
    "Secure Chain MCP Server": {
      "type": "http",
      "url": "http://localhost:8005/mcp",
      "headers": {
        "X-Auth-Email": "mcp-bot@example.com",
        "X-Auth-Pass": "supersecre3T*"
      }
    }
  }
}
```

And then start the MCP server and begin use it with Copilot for example.

## Python Environment
The project uses Python 3.13 and the dependencies are listed in `requirements.txt`.

### Setting up the development environment

1. **Create a virtual environment**:
   ```bash
   python3.13 -m venv depex-env
   ```

2. **Activate the virtual environment**:
   ```bash
   source depex-env/bin/activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

## Tools Specification

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_package_status</strong>
  <p style="margin: 0;"><strong>Description:</strong> Check if a package exists and get its status in the dependency graph.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>node_type:</strong> Type of node (PyPIPackage, NPMPackage, MavenPackage, CargoPackage, RubyGemsPackage, NuGetPackage).</li>
    <li><strong>package_name:</strong> Name of the package.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_package_ssc</strong>
  <p style="margin: 0;"><strong>Description:</strong> Check the direct and transitive software supply chain of a package in the dependency graph of the overall SSC.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>node_type:</strong> Type of node (PyPIPackage, NPMPackage, MavenPackage, CargoPackage, RubyGemsPackage, NuGetPackage).</li>
    <li><strong>package_name:</strong> Name of the package.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_version_status</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the status of a specific version of a package in the dependency graph.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>node_type:</strong> Type of node (PyPIPackage, NPMPackage, MavenPackage, CargoPackage, RubyGemsPackage, NuGetPackage).</li>
    <li><strong>package_name:</strong> Name of the package.</li>
    <li><strong>version_name:</strong> Name of the version.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_version_ssc</strong>
  <p style="margin: 0;"><strong>Description:</strong> Check the direct and transitive SSC of a version in the dependency graph of the overall SSC.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>node_type:</strong> Type of node (PyPIPackage, NPMPackage, MavenPackage, CargoPackage, RubyGemsPackage, NuGetPackage).</li>
    <li><strong>package_name:</strong> Name of the package.</li>
    <li><strong>version_name:</strong> Name of the version.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_vulnerability</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the information of a vulnerability by the ID.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>vulnerability_id:</strong> The ID of the vulnerability to look for.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_vulnerabilities_by_cwe</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the information of vulnerabilities related to a CWE by the CWE-ID.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>cwe_id:</strong> The ID of the CWE to look for.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_vulnerabilities_by_exploit</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the information of vulnerabilities related to an exploit by the exploit ID.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>exploit_id:</strong> The ID of the exploit to look for.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_exploit</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the information of an exploit by the ID.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>exploit_id:</strong> The ID of the exploit to look for.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_exploits_by_vulnerability_id</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the information of exploits related to a vulnerability ID.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>vulnerability_id:</strong> The ID of the vulnerability to look for associated exploits.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_cwe</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the information of a CWE by the ID.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>cwe_id:</strong> The ID of the CWE to look for.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_cwes_by_vulnerability_id</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the information of CWEs related to a vulnerability ID.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>vulnerability_id:</strong> The ID of the vulnerability to look for associated CWEs.</li>
  </ul>
</div>

<br>

<div style="border: 3px solid #ccc; padding: 10px; border-radius: 20px;">
  <strong style="color: #8e5ca1ff; margin: 0;">TOOL: get_vexs</strong>
  <p style="margin: 0;"><strong>Description:</strong> Get the VEXs for a given repository owner and name.</p>
  <p style="margin: 0;"><strong>Input:</strong></p>
  <ul style="margin: 0;">
    <li><strong>owner:</strong> The owner of the repository.</li>
    <li><strong>name:</strong> The name of the repository.</li>
    <li><strong>sbom_name:</strong> The name of the SBOM file.</li>
  </ul>
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
