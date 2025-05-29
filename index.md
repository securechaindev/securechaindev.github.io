---
title: Secure Chain
nav_order: 1
has_children: true
---

# Secure Chain

**Secure Chain** is an open-source initiative to strengthen the security of the Software Supply Chain (SSC).

We provide tools to:

- 📦 Analyze dependencies
- ⚠️ Detect vulnerabilities
- 🧾 Generate security documents
- 📊 Build and visualize dependency graphs

## Projects

- [Depex](depex.md)
- [VEXGen](vexgen.md)

## Contact

[📬 Contact Us](contact.md)

<button data-theme-toggle style="position: fixed; bottom: 1rem; right: 1rem; padding: 0.5rem 1rem;">
  Dark Mode
</button>

<script>
// Get the button
const themeToggle = document.querySelector('[data-theme-toggle]');

// Get the current theme from local storage or default to light
let currentThemeSetting = localStorage.getItem('theme') || 'light';
document.documentElement.setAttribute('data-theme', currentThemeSetting);
themeToggle.textContent = currentThemeSetting === 'dark' ? 'Light Mode' : 'Dark Mode';

// Toggle function
function setTheme(theme) {
  const newTheme = theme === 'light' ? 'dark' : 'light';
  document.documentElement.setAttribute('data-theme', newTheme);
  localStorage.setItem('theme', newTheme);
  themeToggle.textContent = newTheme === 'dark' ? 'Light Mode' : 'Dark Mode';
}

// Add listener
themeToggle.addEventListener('click', () => {
  setTheme(currentThemeSetting);
  currentThemeSetting = currentThemeSetting === 'light' ? 'dark' : 'light';
});
</script>
