---
title: Depex
parent: Secure Chain
nav_order: 2
---

<p align="center">
  <img src="/assets/images/depex-logo.png" alt="Depex Logo" width="180"/>
</p>

# Depex

## Description

**Depex** builds complete dependency graphs from package manifest files (`package.json`, `requirements.txt`, `pom.xml`, etc.) and enriches them with vulnerability data using Neo4j.

## Purpose

**Depex** is a tool for building complete dependency graphs from package manifests and analyzing them for security risks.

It helps organizations and developers:

- Identify direct and transitive dependencies across multiple ecosystems (npm, pip, Maven, Cargo, etc.)
- Enrich dependency data with known vulnerabilities
- Visualize and explore relationships using a Neo4j graph database
- Audit software components for supply chain security
- Support impact analysis and decision-making during vulnerability response

## Video tutorial

<video controls width="600">
  <source src="https://github.com/user-attachments/assets/0dbb63f4-7bc5-4e4d-81d0-94444a61e386" type="video/mp4">
  Your browser does not support the video tag.
</video>

## Prerequisites

- Docker
- Git LFS
- GitHub API Key

## Deployment requirements

1. [Docker](https://www.docker.com/) to deploy the tool.

2. [Git Large Files Storage](https://git-lfs.com/) (git-lfs) for cloning correctly the seeds of the repository.

## Deployment with docker

### Step 1
Create a .env from *template.env* file.

#### Get API Keys

- How to get a *GitHub* [API key](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

- Modify the **Json Web Token (JWT)** secret key with your own. You can generate your own with the command **node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"**.

### Step 2
Create the **graphs** folder inside the **seeds** folder in the root of the project, download the graphs seed from this [link](https://goo.su/YjuzmQ), and insert it into the **graphs** folder.

### Step 3
Run command *docker compose up --build*.

### Step 4
Enter [here](http://0.0.0.0:3000) for the frontend Web API.

## Other tools
1. It is recommended to use a GUI such as [MongoDB Compass](https://www.mongodb.com/en/products/compass) to see what information is being indexed in vulnerability database.

2. You can see the graph built [here](http://0.0.0.0:7474/browser/), using the Neo4J browser interface.

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
