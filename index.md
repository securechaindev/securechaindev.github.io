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

<button id="theme-toggle" style="position: fixed; bottom: 1rem; right: 1rem; padding: 0.5rem 1rem;">
  Toggle Theme
</button>

<script>
// Elements
const toggleBtn = document.getElementById('theme-toggle');

// Initialize theme from localStorage or default to light
let theme = localStorage.getItem('theme') || 'light';
document.documentElement.setAttribute('data-theme', theme);
toggleBtn.textContent = theme === 'dark' ? 'Light Mode' : 'Dark Mode';

// Theme toggle logic
toggleBtn.addEventListener('click', () => {
  theme = theme === 'dark' ? 'light' : 'dark';
  document.documentElement.setAttribute('data-theme', theme);
  localStorage.setItem('theme', theme);
  toggleBtn.textContent = theme === 'dark' ? 'Light Mode' : 'Dark Mode';
});
</script>
