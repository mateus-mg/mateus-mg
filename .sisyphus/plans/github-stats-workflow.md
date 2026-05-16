# Plan: Implement GitHub Actions Workflow for GitHub Stats Images

**Context:**
User wants to implement a GitHub Actions workflow to generate GitHub Readme Stats images locally to bypass rate limits from github-readme-stats.vercel.app service.

**Goal:**
Create a workflow that runs automatically every Sunday at 00:00 UTC to fetch GitHub stats images and save them to the repository, which can then be referenced locally in the README.

**Schedule:** Weekly (Sundays at 00:00 UTC)
**Frequency:** Once per week
**Trigger:** Schedule + Manual dispatch

---

## Tasks

### Task 1: Create GitHub Actions Workflow File
**Location:** `.github/workflows/generate-stats.yml`

**What to do:**
Create a workflow with the following configuration:
- Schedule: `0 0 * * 0` (Sundays at 00:00)
- Manual trigger: `workflow_dispatch`
- Push trigger on main branch (optional)

**Workflow Steps:**
1. Checkout repository
2. Create `assets/` directory
3. Download GitHub Stats card using GITHUB_TOKEN
4. Download Top Languages card using GITHUB_TOKEN
5. Commit and push images to repository

**Code to implement:**
```yaml
name: Generate GitHub Stats Images

on:
  schedule:
    - cron: '0 0 * * 0'  # Every Sunday at 00:00 UTC
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        
      - name: Create assets directory
        run: mkdir -p assets
        
      - name: Generate GitHub Stats Card
        run: |
          curl -H "Authorization: token ${{ secrets.GITHUB_TOKEN }}" \
               "https://github-readme-stats.vercel.app/api?username=mateus-mg&show_icons=true&theme=tokyonight&include_all_commits=true&count_private=true&hide_border=true" \
               --output assets/github-stats.svg
          
      - name: Generate Top Languages Card
        run: |
          curl -H "Authorization: token ${{ secrets.GITHUB_TOKEN }}" \
               "https://github-readme-stats.vercel.app/api/top-langs/?username=mateus-mg&layout=compact&langs_count=7&theme=tokyonight&hide_border=true" \
               --output assets/github-languages.svg
          
      - name: Commit and push images
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add assets/
          if git diff --cached --quiet; then
            echo "No changes to commit"
          else
            git commit -m "chore: update GitHub stats images [skip ci]"
            git push
          fi
```

### Task 2: Update README to Reference Local Images
**Location:** `README.md`

**What to do:**
Update the GitHub Stats section to reference the locally generated images instead of the external API.

**Changes:**
Replace external URLs with relative paths:
- `assets/github-stats.svg` for main stats
- `assets/github-languages.svg` for top languages

**Code to add to README:**
```markdown
## 📊 GitHub Stats

<p align="center">
  <img height="180em" src="assets/github-stats.svg" />
  <img height="180em" src="assets/github-languages.svg" />
</p>

<p align="center">
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=mateus-mg&theme=tokyonight">
</p>
```

### Task 3: Create Initial Assets Directory and Run Workflow
**What to do:**
1. Create `assets/` directory locally
2. Commit and push via PR
3. Trigger the workflow manually once to generate initial images
4. Verify images are generated and committed

### Task 4: Verify Weekly Schedule
**What to verify:**
- Workflow runs automatically every Sunday at 00:00 UTC
- Images are updated and committed
- README displays images correctly

---

## Acceptance Criteria

- [ ] Workflow file exists at `.github/workflows/generate-stats.yml`
- [ ] Schedule is set to `0 0 * * 0` (Sundays at 00:00)
- [ ] Workflow has `workflow_dispatch` for manual trigger
- [ ] README references local image paths
- [ ] Images are generated and saved to `assets/` folder
- [ ] Initial run completes successfully
- [ ] Images display correctly in README

---

## Benefits

1. **No Rate Limits:** Uses GITHUB_TOKEN with higher limits
2. **Offline Access:** Images are static files in the repo
3. **Reliability:** Doesn't depend on external service availability
4. **Speed:** Loads faster (no external API calls)
5. **Automatic Updates:** Runs weekly without manual intervention

---

## Notes

- The GITHUB_TOKEN is automatically provided by GitHub Actions
- No additional secrets needed
- Images will be SVG format for quality and small size
- [skip ci] in commit message prevents infinite loops
