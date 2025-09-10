---
title: Supporting Entities
parent: Secure Chain
nav_order: 9
---

# Supporting Entities

The development of the Secure Chain tools has been made possible thanks to the support of the following institutions:

<div style="display: flex; align-items: center; gap: 1rem; margin-bottom: 1rem;">
  <img src="/assets/supporters/logos/idea-logo.png" alt="IDEA Logo" width="70" />
  <a href="https://www.idea.us.es/home/" target="_blank">IDEA Research Group, University of Seville</a>
</div>

<div style="display: flex; align-items: center; gap: 1rem;">
  <img src="/assets/supporters/logos/i3us-logo.png" alt="I3US Logo" width="170" />
  <a href="https://i3us.us.es/" target="_blank">Institute of Computer Engineering, University of Seville</a>
</div>
<p></p>

Their guidance, research environment, and institutional backing have helped shape the mission of Secure Chain to advance security in the software supply chain.

We gratefully acknowledge their contributions.

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
