---
title: Contributing
parent: Secure Chain
nav_order: 6
---

# Contributing to SecureChain

We welcome contributions from the community! Whether you're fixing bugs, improving documentation, or developing new features, your help is appreciated. Also, we apreciate reports about inconsistences in our graph and vulnerability datasets.

## Repositories

This guide applies to all repositories under the [securechaindev organization](https://github.com/securechaindev). Read the README.md of the repository you're contributing to.

Check open issues or create one if you're proposing something new.

##  How to Contribute

### 1. Fork & Clone

Click the Fork button on GitHub and clone your copy:
```bash
git clone https://github.com/your-username/tool-name.git
```

### 2. Create a Local Enviroment

The project uses Python 3.13 and **uv** as the package manager for faster and more reliable dependency management.

#### Setting up the development environment with uv

1. **Install uv** (if not already installed):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Install dependencies**:
   ```bash
   uv sync
   ```

3. **Activate the virtual environment** (uv creates it automatically):
   ```bash
   source .venv/bin/activate
   ```

### 3. Create a Branch

Use a descriptive name:
```bash
git checkout -b fix/missing-dependency-warning
```

### 4. Make Changes

Focus on clarity and modularity. Each repository have a deployment guide in README.md to check your changes, but typically is running the command:
```bash
docker compose -f dev/docker-compose.yml up --build
```

### 5. Lint & Code Quality

The repositories support using **ruff** following PEP8 with command:
```bash
# Install ruff
uv sync -- extra dev
# Linting
uv ruff check app/

# Formatting
uv ruff format app/
```

### 6. Run Tests

```bash
# Install testing dependencies
uv sync -- extra test

# Run all tests
uv run pytest

# Run tests with coverage report
uv run pytest --cov=app --cov-report=term-missing --cov-report=html

# Run specific test file
uv run pytest tests/unit/controllers/test_graph_controller.py -v

# Run only unit tests
uv run pytest tests/unit/ -v
```

### 7. Commit Changes

Follow [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) when possible:
```bash
git commit -m "fix: Warn on missing indirect imports"
```

### 8. Push & Open Pull Request

Once you have linted and tested your code you can push your changes:
```bash
git push origin fix/missing-dependency-warning
```

Then go to GitHub and open a pull request from your branch.

## Communication

Ask questions via GitHub Discussions or issues.

Tag a maintainer when needed.

Be kind, constructive, and respectful to all contributors.

## License

By contributing, you agree that your contributions will be licensed under the same license as the project (GNU General Public License v3.0).

## Thank You!

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
