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

<img id="mode-image" src="/assets/securechain/figs/overview_light.png" alt="Secure Chain Overview" width="1000" />

<script>
  function updateImageForTheme() {
    const theme = document.documentElement.getAttribute("data-theme");
    const img = document.getElementById("mode-image");
    if (theme === "dark") {
      img.src = "/assets/securechain/figs/overview_dark.png";
    } else {
      img.src = "/assets/securechain/figs/overview_light.png";
    }
  }
  updateImageForTheme();
  const observer = new MutationObserver(updateImageForTheme);
  observer.observe(document.documentElement, { attributes: true, attributeFilter: ['data-theme'] });
</script>

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
