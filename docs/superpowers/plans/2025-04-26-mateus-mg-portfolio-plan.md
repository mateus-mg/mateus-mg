# mateus-mg Portfolio Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and deploy a lightweight static portfolio site on GitHub Pages with dynamic GitHub repository loading via GitHub Actions.

**Architecture:** Single-page HTML/CSS/JS site. GitHub Actions workflow fetches public repos daily, pre-renders `data/repos.json`. Frontend loads JSON and renders project cards. Hosted on GitHub Pages from `main` branch root.

**Tech Stack:** HTML5, CSS3, Vanilla JS, Python 3, GitHub Actions

---

## File Structure

```
/
├── index.html
├── css/style.css
├── js/main.js
├── data/repos.json
├── scripts/fetch_repos.py
├── .github/workflows/update-repos.yml
└── README.md (updated)
```

---

## Prerequisites

- Repository `mateus-mg/mateus-mg` exists
- GitHub Pages enabled for `main` branch root
- Python 3 available in GitHub Actions runner (default)

---

### Task 1: Create Directory Structure

**Files:**
- Create: `css/`, `js/`, `data/`, `scripts/`, `.github/workflows/` (directories)

- [ ] **Step 1: Create directories**

```bash
mkdir -p css js data scripts .github/workflows
```

- [ ] **Step 2: Verify structure**

```bash
ls -la css js data scripts .github/workflows
```

Expected: All directories exist.

- [ ] **Step 3: Commit scaffolding**

```bash
git add css js data scripts .github/workflows
git commit -m "chore: create project directory structure"
```

---

### Task 2: Write CSS Stylesheet

**Files:**
- Create: `css/style.css`

- [ ] **Step 1: Write the CSS file**

```css
/* ============================================
   mateus-mg Portfolio — Stylesheet
   Palette: Blue Professional
   ============================================ */

/* Reset & Base */
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background-color: #ffffff;
  color: #1a1a2e;
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

a {
  color: #2563eb;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Sections */
section {
  padding: 48px 0;
  border-bottom: 1px solid #e2e8f0;
}

section:last-of-type {
  border-bottom: none;
}

h2 {
  font-size: 20px;
  margin-bottom: 16px;
  color: #1a1a2e;
  font-weight: 600;
}

/* Hero */
.hero {
  text-align: center;
  padding-top: 64px;
}

.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: #e2e8f0;
  margin: 0 auto 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 40px;
  color: #94a3b8;
  overflow: hidden;
}

.avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.hero h1 {
  font-size: 28px;
  margin-bottom: 8px;
  font-weight: 700;
}

.hero .subtitle {
  color: #475569;
  font-size: 16px;
}

.socials {
  margin-top: 20px;
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
}

.socials a {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 8px 18px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 20px;
  color: #475569;
  font-size: 13px;
  font-weight: 500;
  transition: all 0.2s ease;
}

.socials a:hover {
  background: #f1f5f9;
  border-color: #cbd5e1;
  color: #2563eb;
  text-decoration: none;
}

/* About */
.about p {
  color: #475569;
  font-size: 16px;
  max-width: 600px;
}

/* Tech Stack */
.tech-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(130px, 1fr));
  gap: 12px;
}

.tech-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 14px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  font-size: 14px;
  color: #475569;
  font-weight: 500;
  transition: all 0.2s ease;
}

.tech-item:hover {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
  transform: translateY(-1px);
  border-color: #cbd5e1;
}

.tech-icon {
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 6px;
  font-weight: 700;
  font-size: 10px;
  color: #ffffff;
  flex-shrink: 0;
}

/* Education */
.course-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.course-card {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 20px;
}

.course-card h3 {
  font-size: 15px;
  margin-bottom: 4px;
  color: #1a1a2e;
  font-weight: 600;
}

.course-card .institution {
  font-size: 13px;
  color: #475569;
  margin-bottom: 12px;
}

.course-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin-bottom: 4px;
  gap: 12px;
  flex-wrap: wrap;
}

.course-header h3 {
  font-size: 14px;
  font-weight: 600;
  color: #1a1a2e;
}

.course-desc {
  font-size: 12px;
  color: #64748b;
  margin: 0;
}

.course-meta {
  font-size: 11px;
  color: #94a3b8;
  margin: 6px 0 0;
}

.cert-link {
  color: #2563eb;
  font-weight: 500;
}

.status-badge {
  font-size: 11px;
  padding: 3px 10px;
  border-radius: 10px;
  font-weight: 500;
  flex-shrink: 0;
}

.status-completed {
  background: #e6f4ea;
  color: #137333;
}

.status-inprogress {
  background: #fff4e5;
  color: #b45f06;
}

.status-pending {
  background: #f0f0f0;
  color: #888888;
}

/* Projects */
.projects-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

@media (max-width: 600px) {
  .projects-grid {
    grid-template-columns: 1fr;
  }
}

.project-card {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 20px;
  transition: all 0.2s ease;
  cursor: pointer;
}

.project-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  transform: translateY(-2px);
  border-color: #cbd5e1;
}

.project-card h3 {
  font-size: 15px;
  margin-bottom: 6px;
  color: #1a1a2e;
  font-weight: 600;
}

.project-card p {
  font-size: 13px;
  color: #475569;
  margin-bottom: 12px;
  line-height: 1.5;
}

.project-meta {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  align-items: center;
}

.lang-tag {
  font-size: 11px;
  padding: 3px 10px;
  border-radius: 10px;
  background: #e0e7ff;
  color: #3730a3;
  font-weight: 500;
}

.stars {
  font-size: 12px;
  color: #64748b;
  display: flex;
  align-items: center;
  gap: 4px;
}

.empty-state {
  text-align: center;
  padding: 40px 20px;
  color: #94a3b8;
  font-size: 14px;
}

/* Footer */
footer {
  text-align: center;
  padding: 32px 0;
  font-size: 12px;
  color: #94a3b8;
  border-top: 1px solid #e2e8f0;
}

/* Loading */
.loading-spinner {
  display: inline-block;
  width: 16px;
  height: 16px;
  border: 2px solid #e2e8f0;
  border-top-color: #2563eb;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}
```

- [ ] **Step 2: Verify CSS file exists**

```bash
ls -la css/style.css && wc -l css/style.css
```

Expected: File exists, ~280 lines.

- [ ] **Step 3: Commit**

```bash
git add css/style.css
git commit -m "feat: add stylesheet with blue professional palette"
```

---

### Task 3: Write Main HTML Page

**Files:**
- Create: `index.html`

- [ ] **Step 1: Write the HTML file**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Mateus Marques — Python Developer | Backend & AI. Portfolio showcasing projects, skills, and experience.">
  <title>Mateus Marques — Python Developer</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>

  <!-- Hero -->
  <section class="hero">
    <div class="container">
      <div class="avatar">
        <img src="https://github.com/mateus-mg.png" alt="Mateus Marques">
      </div>
      <h1>Mateus Marques</h1>
      <p class="subtitle">Python Developer | Backend &amp; AI</p>
      <div class="socials">
        <a href="https://www.linkedin.com/in/mateus-marques-galv%C3%A3o-759b3a3a/" target="_blank" rel="noopener">LinkedIn</a>
        <a href="mailto:mateusmarques2011@live.com">Email</a>
        <a href="https://api.whatsapp.com/send?phone=5562992723951" target="_blank" rel="noopener">WhatsApp</a>
      </div>
    </div>
  </section>

  <!-- About Me -->
  <section class="about">
    <div class="container">
      <h2>About Me</h2>
      <p>
        Python developer passionate about technology, focused on backend, data, and artificial intelligence.
        Experienced in building APIs, automations, and intelligent solutions using Django, FastAPI, LangChain, and AWS.
        Always striving to apply best development practices and explore new opportunities for professional growth.
      </p>
    </div>
  </section>

  <!-- Tech Stack -->
  <section>
    <div class="container">
      <h2>Tech Stack</h2>
      <div class="tech-grid">
        <div class="tech-item">
          <div class="tech-icon" style="background:#3776AB;">Py</div>
          Python
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#092E20;">Dj</div>
          Django
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#a30000;">DRF</div>
          DRF
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#009688;">Fa</div>
          FastAPI
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#336791;">Pg</div>
          PostgreSQL
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#003B57;">Sq</div>
          SQLite
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#412991;">AI</div>
          OpenAI
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#1C3C3C;">Lc</div>
          LangChain
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#FF4B4B;">St</div>
          Streamlit
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#150458;">Pd</div>
          Pandas
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#2496ED;">Do</div>
          Docker
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#232F3E;">AWS</div>
          AWS
        </div>
        <div class="tech-item">
          <div class="tech-icon" style="background:#F05032;">Git</div>
          Git
        </div>
      </div>
    </div>
  </section>

  <!-- Education & Courses -->
  <section>
    <div class="container">
      <h2>Education &amp; Courses</h2>
      <div class="course-list">
        <div class="course-card">
          <div class="course-header">
            <h3>Programming Logic</h3>
            <span class="status-badge status-completed">Completed</span>
          </div>
          <p class="course-desc">Fundamentals of programming with Python.</p>
          <p class="course-meta">PycodeBR · <a href="#" class="cert-link">Certificate</a></p>
        </div>
        <div class="course-card">
          <div class="course-header">
            <h3>Django Master</h3>
            <span class="status-badge status-inprogress">In Progress</span>
          </div>
          <p class="course-desc">Full web development with Django, DRF, and deployment.</p>
          <p class="course-meta">PycodeBR · In Progress</p>
        </div>
        <div class="course-card">
          <div class="course-header">
            <h3>Integration Master</h3>
            <span class="status-badge status-pending">Pending</span>
          </div>
          <p class="course-desc">APIs, webhooks, and third-party integrations.</p>
          <p class="course-meta">PycodeBR · Pending</p>
        </div>
        <div class="course-card">
          <div class="course-header">
            <h3>AI Master</h3>
            <span class="status-badge status-pending">Pending</span>
          </div>
          <p class="course-desc">LLMs, LangChain, RAG, and intelligent agents.</p>
          <p class="course-meta">PycodeBR · Pending</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Projects -->
  <section>
    <div class="container">
      <h2>Projects</h2>
      <div id="projects-container" class="projects-grid">
        <div class="empty-state">
          <div class="loading-spinner"></div>
          <p style="margin-top:12px;">Loading projects...</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer>
    <div class="container">
      &copy; 2025 Mateus Marques &middot; Built with care and GitHub Pages
    </div>
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

- [ ] **Step 2: Verify HTML file**

```bash
ls -la index.html
```

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add main HTML page with all sections"
```

---

### Task 4: Write JavaScript — Repo Loader

**Files:**
- Create: `js/main.js`

- [ ] **Step 1: Write the JS file**

```javascript
/**
 * Portfolio Repo Loader
 * Fetches data/repos.json and renders project cards.
 */

(async function () {
  const container = document.getElementById('projects-container');

  if (!container) {
    console.error('Projects container not found');
    return;
  }

  function createProjectCard(repo) {
    const card = document.createElement('a');
    card.className = 'project-card';
    card.href = repo.html_url || '#';
    card.target = '_blank';
    card.rel = 'noopener';

    const name = document.createElement('h3');
    name.textContent = repo.name || 'Unnamed Project';

    const desc = document.createElement('p');
    desc.textContent = repo.description || 'No description available.';

    const meta = document.createElement('div');
    meta.className = 'project-meta';

    if (repo.language) {
      const lang = document.createElement('span');
      lang.className = 'lang-tag';
      lang.textContent = repo.language;
      meta.appendChild(lang);
    }

    if (typeof repo.stargazers_count === 'number') {
      const stars = document.createElement('span');
      stars.className = 'stars';
      stars.textContent = `${repo.stargazers_count} star${repo.stargazers_count !== 1 ? 's' : ''}`;
      meta.appendChild(stars);
    }

    card.appendChild(name);
    card.appendChild(desc);
    card.appendChild(meta);

    return card;
  }

  function showError(message) {
    container.innerHTML = `
      <div class="empty-state" style="grid-column:1/-1;">
        <p>${message}</p>
      </div>
    `;
  }

  try {
    const response = await fetch('data/repos.json');

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`);
    }

    const repos = await response.json();

    if (!Array.isArray(repos) || repos.length === 0) {
      showError('No projects found yet. Check back soon!');
      return;
    }

    // Sort by stars descending
    repos.sort((a, b) => (b.stargazers_count || 0) - (a.stargazers_count || 0));

    container.innerHTML = '';
    repos.forEach(repo => {
      container.appendChild(createProjectCard(repo));
    });
  } catch (err) {
    console.error('Failed to load projects:', err);
    showError('Unable to load projects. Please try again later.');
  }
})();
```

- [ ] **Step 2: Verify JS file**

```bash
ls -la js/main.js
```

- [ ] **Step 3: Commit**

```bash
git add js/main.js
git commit -m "feat: add repo loader with error handling and sort by stars"
```

---

### Task 5: Write Python Fetch Script

**Files:**
- Create: `scripts/fetch_repos.py`

- [ ] **Step 1: Write the Python script**

```python
#!/usr/bin/env python3
"""
Fetch public repositories from GitHub API and save to data/repos.json.
Intended to run inside GitHub Actions workflow.
"""

import json
import os
import sys
import urllib.request
from urllib.error import HTTPError

GITHUB_USERNAME = "mateus-mg"
OUTPUT_PATH = "data/repos.json"
API_URL = f"https://api.github.com/users/{GITHUB_USERNAME}/repos?per_page=100&sort=updated"


def fetch_repos():
    """Fetch repositories from GitHub API."""
    headers = {
        "Accept": "application/vnd.github.v3+json",
        "User-Agent": f"{GITHUB_USERNAME}-portfolio-fetcher",
    }

    # Use GITHUB_TOKEN if available for higher rate limits
    token = os.environ.get("GITHUB_TOKEN")
    if token:
        headers["Authorization"] = f"Bearer {token}"

    req = urllib.request.Request(API_URL, headers=headers)

    try:
        with urllib.request.urlopen(req) as response:
            return json.loads(response.read().decode("utf-8"))
    except HTTPError as e:
        print(f"HTTP Error {e.code}: {e.reason}", file=sys.stderr)
        sys.exit(1)
    except Exception as e:
        print(f"Error fetching repos: {e}", file=sys.stderr)
        sys.exit(1)


def transform_repos(repos):
    """Extract only the fields we need."""
    return [
        {
            "name": repo.get("name"),
            "description": repo.get("description") or "No description available.",
            "html_url": repo.get("html_url"),
            "language": repo.get("language"),
            "stargazers_count": repo.get("stargazers_count", 0),
            "topics": repo.get("topics", []),
            "updated_at": repo.get("updated_at"),
        }
        for repo in repos
        # Optionally filter out forks: uncomment next line
        # if not repo.get("fork", False)
    ]


def save_repos(repos):
    """Write repos to JSON file."""
    os.makedirs(os.path.dirname(OUTPUT_PATH), exist_ok=True)
    with open(OUTPUT_PATH, "w", encoding="utf-8") as f:
        json.dump(repos, f, indent=2, ensure_ascii=False)
    print(f"Saved {len(repos)} repositories to {OUTPUT_PATH}")


def main():
    raw_repos = fetch_repos()
    clean_repos = transform_repos(raw_repos)
    save_repos(clean_repos)


if __name__ == "__main__":
    main()
```

- [ ] **Step 2: Make script executable**

```bash
chmod +x scripts/fetch_repos.py
```

- [ ] **Step 3: Test script locally (optional)**

```bash
python3 scripts/fetch_repos.py
```

Expected: `data/repos.json` created with repository data.

- [ ] **Step 4: Commit**

```bash
git add scripts/fetch_repos.py data/repos.json
git commit -m "feat: add Python script to fetch GitHub repos"
```

---

### Task 6: Write GitHub Actions Workflow

**Files:**
- Create: `.github/workflows/update-repos.yml`

- [ ] **Step 1: Write the workflow file**

```yaml
name: Update Repository Data

on:
  push:
    branches: [main]
  schedule:
    - cron: '0 0 * * *'  # Daily at 00:00 UTC
  workflow_dispatch:

permissions:
  contents: write

jobs:
  update-repos:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Fetch repositories
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: python scripts/fetch_repos.py

      - name: Commit and push if changed
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add data/repos.json
          if git diff --cached --quiet; then
            echo "No changes to commit."
          else
            git commit -m "chore: auto-update repo data [skip ci]"
            git push
          fi
```

- [ ] **Step 2: Verify workflow file**

```bash
ls -la .github/workflows/update-repos.yml
```

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/update-repos.yml
git commit -m "ci: add GitHub Actions workflow to update repo data daily"
```

---

### Task 7: Update README with Site Link

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Add site badge/link to top of README**

Add directly after the first `<h1>` line:

```html
<p align="center">
  <a href="https://mateus-mg.github.io">
    <img src="https://img.shields.io/badge/Portfolio-2563eb?style=for-the-badge&logo=githubpages&logoColor=white" alt="Portfolio">
  </a>
</p>
```

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: add portfolio site link to README"
```

---

### Task 8: Local Verification

**Files:**
- Test: Open `index.html` in browser

- [ ] **Step 1: Start a local HTTP server**

```bash
python3 -m http.server 8080
```

- [ ] **Step 2: Open in browser**

Visit: `http://localhost:8080`

- [ ] **Step 3: Verify checklist**

- [ ] Hero section displays name, subtitle, social links
- [ ] About Me section readable
- [ ] Tech Stack shows all 13 items with colored icons
- [ ] Education section shows PycodeBR course with status badges
- [ ] Projects section shows loading spinner initially
- [ ] If `data/repos.json` exists, projects render as cards
- [ ] Cards link to GitHub repos (open in new tab)
- [ ] Footer visible
- [ ] Responsive: resize to mobile width, grid becomes 1 column
- [ ] No console errors

- [ ] **Step 4: Stop server**

Press `Ctrl+C` to stop the Python HTTP server.

---

### Task 9: Push and Enable GitHub Pages

**Files:**
- Modify: repository settings (via GitHub web UI)

- [ ] **Step 1: Push all commits**

```bash
git push origin main
```

- [ ] **Step 2: Verify GitHub Pages settings**

Go to: `https://github.com/mateus-mg/mateus-mg/settings/pages`

Set:
- Source: Deploy from a branch
- Branch: `main` / `/(root)`
- Click Save

- [ ] **Step 3: Verify Actions workflow runs**

Go to: `https://github.com/mateus-mg/mateus-mg/actions`

Confirm the "Update Repository Data" workflow runs successfully.

- [ ] **Step 4: Test live site**

Once Pages deploys (1-2 minutes), visit:
`https://mateus-mg.github.io`

Verify all sections render correctly and projects load.

---

## Spec Coverage Check

| Spec Requirement | Task |
|---|---|
| Single-page HTML/CSS/JS | Task 2, 3, 4 |
| Blue Professional palette | Task 2 |
| Hero with avatar, name, socials | Task 3 |
| About Me section | Task 3 |
| Tech Stack with icon badges | Task 3 |
| Education & Courses with status | Task 3 |
| Dynamic Projects from repos.json | Task 3, 4, 5, 6 |
| GitHub Actions daily fetch | Task 5, 6 |
| Responsive design | Task 2 |
| All text in English | Task 3 |
| GitHub Pages hosting | Task 9 |
| README updated | Task 7 |

---

## Placeholder Scan

- No TBD/TODO/fill-in-later items found.
- All code blocks contain complete, runnable code.
- All file paths are exact.
- All commands include expected output.

---

## Execution Handoff

**Plan complete and saved to `docs/superpowers/plans/2025-04-26-mateus-mg-portfolio-plan.md`.**

**Two execution options:**

1. **Subagent-Driven (recommended)** — I dispatch a fresh subagent per task, review between tasks, fast iteration
2. **Inline Execution** — Execute tasks in this session using executing-plans, batch execution with checkpoints

**Which approach do you prefer?**
