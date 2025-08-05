---
title: VEXGen
parent: Secure Chain
nav_order: 3
---

<p align="center">
  <img src="/assets/securechain/logos/vexgen-logo.png" alt="VEXGen Logo" width="180"/>
</p>

# VEXGen

## Description

**VEXGen** analyzes software dependencies and generates VEX documents that communicate whether known vulnerabilities are actually exploitable in a given context. It integrates data from multiple sources, including OSV.dev, SBOMs, and Git commit history.

## Purpose

**VEXGen** is a tool for generating [VEX](https://www.cisa.gov/resources-tools/resources/vulnerability-exploitability-exchange-vex) (Vulnerability Exploitability eXchange) documents, which indicate whether specific vulnerabilities affect software artifacts.

It helps organizations and developers:

- Track and analyze vulnerabilities in dependencies
- Communicate exploitability status via VEX
- Store and visualize data using dependency graphs and SBOMs
- Integrate vulnerability data from OSV.dev and Git

## Video tutorial

<video controls width="600">
  <source src="https://github.com/user-attachments/assets/5750712e-8429-410b-b697-ce8414fe5063" type="video/mp4">
  Your browser does not support the video tag.
</video>

## Prerequisites

Before deploying VEXGen, ensure the following are installed:

- Docker
- Git
- Git LFS (Git Large File Storage)

## Deployment requirements

1. [Docker](https://www.docker.com/) to deploy the tool.

2. [Git Large Files Storage](https://git-lfs.com/) (git-lfs) for cloning correctly the seeds of the repository.

## Deployment with docker

### Step 1
 Create a .env file from template.env

#### Get API Keys

- How to get a GitHub [API key](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens).

- Modify the **Json Web Token (JWT)** secret key with your own. You can generate your own with the command **node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"**.

### Step 2
Create the **graphs** folder inside the **seeds** folder in the root of the project, download the graphs seed from this [link](https://goo.su/YjuzmQ), and insert it into the **graphs** folder.

### Step 3
Run command *docker compose up --build*.

### Step 4
Enter [here](http://0.0.0.0:3000) for the frontend Web API.

#### Other tools
1. It is recommended to use a GUI such as [MongoDB Compass](https://www.mongodb.com/en/products/compass) to see what information is being indexed in vulnerability database

2. You can see the created graph built for [here](http://0.0.0.0:7474/browser/), using the Neo4J browser interfaces.

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
