# Implementation Guide: Manual Solution Release for HPM 883

**Quick Reference**: How to set up and use the recommended simple manual push approach.

---

## One-Time Setup (5 minutes)

### Step 1: Create Solutions Branch in Template Repo

```bash
# Navigate to your lab template repo
cd ~/Documents/Repos/hpm883-lab-template

# Create and switch to solutions branch
git checkout -b solutions

# Create initial solutions file (if doesn't exist)
# Or add content to existing analysis-sols.qmd

git add analysis-sols.qmd
git commit -m "Initialize solutions branch"
git push origin solutions
```

### Step 2: Protect Solutions Branch on GitHub

1. Go to repo on GitHub: `https://github.com/HPM-883-2026/hpm883-lab-template`
2. Navigate to **Settings** → **Branches**
3. Click **Add branch protection rule**
4. Configure:
   - **Branch name pattern**: `solutions`
   - Check: **Require a pull request before merging**
   - Check: **Require approvals** (optional, prevents accidental merges)
   - Click **Create**

This prevents accidental deletion or direct pushes to solutions branch.

### Step 3: Set Up Working Branch Structure

```bash
# Switch back to main
git checkout main

# Verify branches
git branch -a
# Should see:
#   main
#   solutions
```

---

## Weekly Workflow (2 minutes per lab)

### Before Class: Add Solutions to Solutions Branch

```bash
# Switch to solutions branch
git checkout solutions

# Add/update solutions for this week's lab
# Edit analysis-sols.qmd with solutions

git add analysis-sols.qmd
git commit -m "Add Lab X solutions"
git push origin solutions

# Switch back to main for class
git checkout main
```

### After Class: Release Solutions to Students

```bash
# Ensure you're on main
git checkout main

# Pull latest changes (good habit)
git pull origin main

# Merge solutions branch (creates merge commit)
git merge solutions --no-ff -m "Release Lab X solutions"

# Push to students
git push origin main
```

### Notify Students

Post announcement in Canvas/Slack:
```
Lab X solutions are now available!

To access:
1. Open your lab repo in RStudio/terminal
2. Run: git pull
3. Open analysis-sols.qmd

Solutions are now in your repo for comparison with your work.
```

---

## Shell Alias (Optional Time-Saver)

Add to your `~/.zshrc` or `~/.bashrc`:

```bash
# HPM 883 Lab Solution Release
alias hpm-release-lab='git checkout main && git pull origin main && git merge solutions --no-ff && git push origin main && echo "✅ Solutions released! Notify students."'
```

Then after class, just run:
```bash
hpm-release-lab
# Optionally pass commit message:
# git merge solutions --no-ff -m "Release Lab 3 solutions"
```

---

## Troubleshooting

### Problem: Merge Conflict

**Cause**: Changes on main conflict with solutions branch.

**Solution**:
```bash
# During merge conflict
git status  # See conflicting files

# Edit conflicting files to resolve
# Look for <<<<<<< markers

git add <resolved-files>
git commit -m "Release Lab X solutions (resolved conflicts)"
git push origin main
```

### Problem: Forgot to Create Solutions Before Class

**Solution**:
```bash
# After class, quickly add solutions
git checkout solutions
# Add solutions to analysis-sols.qmd
git add analysis-sols.qmd
git commit -m "Add Lab X solutions (post-class)"
git push origin solutions

# Then release as normal
git checkout main
git merge solutions --no-ff -m "Release Lab X solutions"
git push origin main
```

### Problem: Released Solutions Too Early (Oops!)

**Solution 1** (within minutes, haven't notified students yet):
```bash
# Revert the merge commit
git revert -m 1 HEAD
git push origin main
# This creates new commit removing solutions
```

**Solution 2** (students already pulled):
```bash
# Accept it happened, use as teaching moment
# Send announcement:
# "Solutions released early - please attempt lab independently
# before reviewing solutions. Academic integrity policy applies."
```

### Problem: Students Don't See Solutions After Pull

**Diagnosis**:
```bash
# Student runs in their repo:
git log --oneline -5
# Should see merge commit: "Release Lab X solutions"

git branch -vv
# Should show they're on main and up-to-date

ls -la
# Should see analysis-sols.qmd present
```

**Solution**:
```bash
# Student may need to fetch first
git fetch origin
git pull origin main
```

---

## Verification Checklist

After releasing solutions, verify:

- [ ] Merge commit appears in `git log` on main
- [ ] `analysis-sols.qmd` visible on GitHub web interface (main branch)
- [ ] Test pull in a student repo clone to confirm solutions appear
- [ ] Post notification to students via Canvas/Slack
- [ ] (Optional) Check GitHub Insights → Network to visualize branch merge

---

## Calendar Integration

### Option 1: Apple Calendar/Google Calendar Reminder

Create recurring event:
- **Title**: "Release HPM 883 Lab Solutions"
- **Time**: 5 minutes after class ends
- **Recurrence**: Weekly on class day
- **Alert**: At time of event
- **Notes**:
  ```
  cd ~/Documents/Repos/hpm883-lab-template
  git checkout main
  git merge solutions --no-ff -m "Release Lab X solutions"
  git push origin main
  [Post Canvas announcement]
  ```

### Option 2: GitHub Issues Template

Create issue template `.github/ISSUE_TEMPLATE/weekly-solution-release.md`:
```markdown
---
name: Weekly Solution Release
about: Checklist for releasing lab solutions
---

## Lab X Solution Release

**Date**: [Class date]

### Pre-Class
- [ ] Solutions complete in `solutions` branch
- [ ] Solutions tested locally
- [ ] Committed and pushed to `solutions`

### Post-Class (After class ends)
- [ ] Run: `git checkout main && git merge solutions --no-ff && git push`
- [ ] Verify solutions visible on GitHub
- [ ] Post Canvas announcement
- [ ] Close this issue

### Notes
[Any special notes about this week's solutions]
```

---

## Best Practices

### DO:
- ✅ Commit solutions to `solutions` branch as you create them (ahead of class)
- ✅ Use descriptive merge commit messages ("Release Lab 3 solutions")
- ✅ Test solutions locally before pushing to `solutions` branch
- ✅ Set calendar reminder so you don't forget
- ✅ Verify release worked by checking GitHub web interface

### DON'T:
- ❌ Don't work directly on `solutions` branch during class (stay on `main`)
- ❌ Don't delete `solutions` branch (it's your source of truth)
- ❌ Don't force push to `main` (breaks student repos)
- ❌ Don't forget to notify students after releasing

---

## Recovery Scenarios

### Accidentally Deleted Solutions Branch

```bash
# Find the commit SHA where solutions branch was
git reflog | grep solutions

# Recreate branch from that commit
git checkout -b solutions <commit-sha>
git push origin solutions
```

### Lost Local Repo

```bash
# Clone fresh from GitHub
git clone https://github.com/HPM-883-2026/hpm883-lab-template.git
cd hpm883-lab-template

# Fetch all branches
git fetch --all

# Set up local solutions branch
git checkout solutions
git checkout main
```

### Need to Update Released Solutions (Typo in Solutions)

```bash
# Fix on solutions branch
git checkout solutions
# Edit analysis-sols.qmd
git add analysis-sols.qmd
git commit -m "Fix typo in Lab X solutions"
git push origin solutions

# Merge fix to main
git checkout main
git merge solutions --no-ff -m "Update Lab X solutions (fix typo)"
git push origin main

# Notify students
# "Updated Lab X solutions available - git pull to update"
```

---

## Time Tracking

Expected time per lab:
- **Pre-class** (solution creation): Already part of lab prep (0 added time)
- **Post-class** (release): 2 minutes (commands + notification)
- **Verification**: 1 minute (check GitHub web)
- **Total added time per lab**: ~3 minutes

**Semester total** (12 labs): ~36 minutes

Compare to:
- GitHub Actions setup: 60-120 minutes + debugging time
- Separate repo approach: ~5 min/week = 60 minutes semester

---

## Questions?

If issues arise mid-semester:
1. Check git status: `git status` (identifies current state)
2. Check recent history: `git log --oneline -10` (see recent commits)
3. Check branches: `git branch -a` (verify branch structure)
4. Consult this guide's troubleshooting section
5. Test commands in a copy of the repo before running on production
