---
title: Contributing
parent: Secure Chain
nav_order: 5
---

# 🤝 Contributing to SecureChain

We welcome contributions from the community! Whether you're fixing bugs, improving documentation, or developing new features, your help is appreciated.

##  📦 Repositories

This guide applies to all repositories under the securechaindev organization, including:

Depex – Dependency explorer & vulnerability detector

VEXGen – Automated VEX document generator

## ✅ Before You Start

Read the README.md of the repository you're contributing to.

Check open issues or create one if you're proposing something new.

Fork the repository and clone it locally.

Make sure you have the prerequisites installed.

## 🛠️ Development Setup

Example for Depex:

git clone https://github.com/your-username/depex.git
cd depex
python -m venv depex-venv
source depex-venv/bin/activate
pip install -r requirements.txt
docker compose up --build

## 🚀 How to Contribute

1. Fork & Clone
Click the Fork button on GitHub and clone your copy:
git clone https://github.com/your-username/depex.git

2. Create a Branch
Use a descriptive name:
git checkout -b fix/missing-dependency-warning

3. Make Changes
Focus on clarity and modularity.

4. Lint
The repos support using **ruff**

5. Commit Changes
Follow conventional commits when possible:
git commit -m "fix: Warn on missing indirect imports"

6. Push & Open Pull Request
git push origin fix/missing-dependency-warning
Then go to GitHub and open a pull request from your branch.

## 🗂️ Code Style

Python: follow PEP8

Use the linter ruff.

## 💬 Communication

Ask questions via GitHub Discussions or issues.

Tag a maintainer when needed.

Be kind, constructive, and respectful to all contributors.

## 📜 License

By contributing, you agree that your contributions will be licensed under the same license as the project (typically GNU GPL).

## 🙌 Thank You!

Your contributions help improve the security of the global software supply chain. We're glad to have you with us.

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
