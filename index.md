---
title: Secure Chain
nav_order: 1
has_children: true
---

# Secure Chain

**Secure Chain** is an open-source initiative to strengthen the security of the Software Supply Chain (SSC).

We provide tools to:

- 📦 Analyze dependencies and configurations.
- ⚠️ Detect vulnerabilities and minimize its impact.
- 🧾 Generate security documents such as Vulnerability Exploitability eXchange (VEX) reports.
- 📊 Build and visualize dependency graphs.

## How it works?

<img id="overview-image" src="/assets/securechain/figs/overview_light.png" alt="Secure Chain Overview" width="1000" />

### What it receives (Left side)
SecureChain begins by consuming requirement files from different software ecosystems, such as *requirements.txt*, *package.json*, or *pom.xml*. These files describe the dependencies that a software project relies on, serving as the raw material for the rest of the system.

### How Depex and VEXGen work (Core)
At the core of SecureChain are two complementary components: **Depex** and **VEXGen**.

**Depex** is responsible for parsing the requirement files and resolving full dependency trees, including both direct and transitive dependencies. It also normalizes metadata across ecosystems, detects version constraints, and links packages to known vulnerabilities using precise matching logic. SecureChain ingests vulnerability data from the [OSV](https://osv.dev/) (Open Source Vulnerability) database, which provides standardized information about known vulnerabilities, affected version ranges, and fix metadata.

**VEXGen** enriches this dependency data with security threat intelligence. It analyzes the timelines and scopes of vulnerabilities, identifies potentially vulnerable dependency objects of dependencies, and generates contextual insights, such as which components are affected, and whether an exploit is likely.

Together, Depex and VEXGen form the analytical engine of SecureChain, transforming static requirement files and vulnerability feeds into a structured, semantically-rich model of software dependencies and risks.

### What it returns (Right side)
The output of the system is a unified knowledge graph that represents the software supply chain and its security posture. In this graph, software packages, versions, vendors, and vulnerabilities are modeled as interconnected entities. The relationships between them are encoded explicitly to allow for querying, visualization, and reasoning.

This knowledge graph enables advanced use cases such as automated supply chain impact analysis, configurations reasoning, SBOM enrichment, VEX generation, and maintainer-level risk assessments. It provides a foundation for understanding not only which components are vulnerable, but also how and why those vulnerabilities propagate through modern software stacks.

## How is it made?

<img id="architecture-image" src="/assets/securechain/figs/architecture_light.png" alt="Secure Chain Architecture" width="1000" />

Secure Chain software architecture is based on microservices. The application is divided into several layers, each with a specific role and associated technologies that allow for independent scaling, maintenance, and development of functionalities. The layers that make up this architecture are briefly described below:

1. **Frontend (Client Application):** Built with *Next.js*, this is the user-facing layer accessed through a browser or mobile device. It sends HTTP/HTTPS requests to the backend, handles user input, and displays the results dynamically.

2. **API Gateway (BFF - Backend for Frontend):** Implemented with *FastAPI*, this gateway manages routes client requests to the appropriate microservice, and aggregates data when necessary.

3. **Microservices Layer:** This layer consists of three separate microservices, all built with *FastAPI*. **Auth** manages authentication. **Depex** extracts and analyzes software dependencies from various package managers, and reason on dependency configurations. **VEXGen** generates vulnerability reports in VEX format.

4. **Database Layer:** Each microservice uses its own dedicated database. *MongoDB* stores the vulnerability data, and *Neo4j* stores the software supply chain graph used by our tools to compute SSC degrees and analyze dependency relationships.

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
    setTimeout(() => {
      const overview_img = document.getElementById('overview-image');
      const architecture_img = document.getElementById('architecture-image');
      const theme = jtd.getTheme();
      overview_img.src = theme === 'dark'
        ? '/assets/securechain/figs/overview_dark.png'
        : '/assets/securechain/figs/overview_light.png';
      architecture_img.src = theme === 'dark'
        ? '/assets/securechain/figs/architecture_dark.png'
        : '/assets/securechain/figs/architecture_light.png';
    }, 5);
  });
  document.addEventListener("DOMContentLoaded", function () {
    const overview_img = document.getElementById('overview-image');
    const architecture_img = document.getElementById('architecture-image');
    const theme = jtd.getTheme();
    overview_img.src = theme === 'dark'
      ? '/assets/securechain/figs/overview_dark.png'
      : '/assets/securechain/figs/overview_light.png';
    architecture_img.src = theme === 'dark'
      ? '/assets/securechain/figs/architecture_dark.png'
      : '/assets/securechain/figs/architecture_light.png';
  });
</script>
