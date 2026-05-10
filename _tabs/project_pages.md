---
title: Project Pages
icon: fas fa-rocket
order: 0
---

<div class="project-card-grid">
  <a href="/dsd-gs/" class="project-card">
    <div class="project-card-title">DSD-GS</div>
    <div class="project-card-desc">Dynamic-Static Decomposition of Gaussian Splatting for Efficient and High-Fidelity Dynamic Scene Reconstruction</div>
    <div class="project-card-venue">arXiv Preprint</div>
  </a>
</div>

<style>
  .project-card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
    gap: 1rem;
    margin-top: 1rem;
  }
  .project-card {
    display: block;
    border: 1px solid var(--tb-border-color);
    border-radius: 10px;
    padding: 1.2rem;
    text-decoration: none;
    background: var(--card-bg);
    transition: box-shadow 0.15s, border-color 0.15s;
  }
  .project-card:hover {
    box-shadow: var(--card-shadow);
    border-color: rgb(114, 224, 211);
  }
  .project-card-title {
    font-size: 1.05rem;
    font-weight: 700;
    color: var(--heading-color);
    margin-bottom: 0.4rem;
  }
  .project-card-desc {
    font-size: 0.83rem;
    color: var(--text-muted-color);
    line-height: 1.5;
    margin-bottom: 0.6rem;
  }
  .project-card-venue {
    display: inline-block;
    font-size: 0.75rem;
    font-weight: 600;
    background: rgb(114, 224, 211);
    color: #111;
    padding: 0.15rem 0.6rem;
    border-radius: 20px;
  }
</style>
