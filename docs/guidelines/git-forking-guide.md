# Forking and Contribution Guide

This guide explains how to fork the Steel Browser project and maintain your own repository while staying synchronized with the upstream project.

## Overview

When you want to contribute to Steel Browser or maintain your own version, you'll need to:
1. Fork the repository to your GitHub account
2. Set up your local repository to push to your fork
3. Configure upstream tracking to receive updates from the original repository

## Prerequisites

- Git installed and configured
- GitHub account
- Local clone of the steel-browser repository

## 1. Fork the Repository

### Step 1: Create a Fork on GitHub
1. Navigate to https://github.com/steel-dev/steel-browser
2. Click the "Fork" button in the top-right corner
3. Select your GitHub account as the destination
4. Wait for GitHub to create your fork

### Step 2: Update Your Local Repository Remote

Currently, your local repository points to the original Steel repository:
```bash
# Check current remote configuration
git remote -v
# Output: origin https://github.com/steel-dev/steel-browser (fetch/push)
```

Update it to point to your fork:
```bash
# Replace YOUR_USERNAME with your actual GitHub username
git remote set-url origin https://github.com/YOUR_USERNAME/steel-browser
```

### Step 3: Verify the Change
```bash
git remote -v
# Output should now show your fork:
# origin https://github.com/YOUR_USERNAME/steel-browser (fetch)
# origin https://github.com/YOUR_USERNAME/steel-browser (push)
```

### Step 4: Push to Your Fork
```bash
git push origin main
```

## 2. Set Up Upstream Tracking

To keep your fork synchronized with the original repository, you need to add the original repository as an "upstream" remote.

### Step 1: Add Upstream Remote
```bash
git remote add upstream https://github.com/steel-dev/steel-browser
```

### Step 2: Verify Remote Configuration
```bash
git remote -v
```

You should now see both remotes:
```
origin    https://github.com/YOUR_USERNAME/steel-browser (fetch)
origin    https://github.com/YOUR_USERNAME/steel-browser (push)
upstream  https://github.com/steel-dev/steel-browser (fetch)
upstream  https://github.com/steel-dev/steel-browser (push)
```

## 3. Handling Existing Changes During Fork Setup

If you already have uncommitted changes or staged files on your main branch when setting up your fork, you need to handle them properly to maintain clean git version control.

### Check Your Current Status
```bash
git status
```

### Scenario: You Have Changes on Main Branch

When you have existing changes on main (staged or unstaged), you have two recommended approaches:

#### Option A: Clean Commit to Main (Recommended for Documentation/Non-Feature Changes)
If your changes are documentation, configuration, or other non-feature modifications:

```bash
# Add any untracked files you want to keep
git add CLAUDE.md  # or other files

# Commit all staged changes
git commit -m "Add documentation and configuration files"

# Push to your fork
git push origin main
```

#### Option B: Feature Branch Approach (Recommended for Features/Code Changes)
If your changes are feature implementations or significant code modifications:

```bash
# Create and switch to a feature branch
git checkout -b feature/add-your-feature-name

# Add your changes
git add .

# Commit with a descriptive message
git commit -m "Add feature: description of your changes"

# Push to your fork
git push origin feature/add-your-feature-name

# Switch back to main for clean state
git checkout main
```

### Choosing the Right Option

- **Use Option A** for:
  - Documentation updates (README, guides, etc.)
  - Configuration files (CLAUDE.md, etc.)
  - Build configuration changes
  - Non-functional improvements

- **Use Option B** for:
  - New features or functionality
  - Bug fixes
  - Code refactoring
  - API changes
  - Any changes you might want to contribute back via PR

### Current Situation Example

If you're currently seeing something like:
```bash
git status
# On branch main
# Changes to be committed:
#   new file:   .gitmodules
#   new file:   docs/workplan
# Untracked files:
#   .claude/
#   CLAUDE.md
#   docs/guidelines/
```

For this scenario, **Option A is recommended** since these are documentation and configuration files.

### After Handling Your Changes

Once you've handled your existing changes, proceed with the regular workflow for future development.

## 4. Long-Term Branch Management Strategy

After setting up your fork and handling initial changes, you need a sustainable strategy for managing different types of changes over time. This section provides strategic guidance for maintaining clean, efficient git workflows.

### 4.1 Types of Changes Analysis

Understanding the nature of your changes helps determine the best management approach:

#### **Personal/Local Changes (Keep in Your Fork)**

- **CLAUDE.md** - Personal documentation for Claude Code
- **.claude/** - Personal configuration directories
- **Local environment configs** - IDE settings, personal scripts
- **Private notes and documentation** - Internal development notes

#### **Upstream-Worthy Changes (Contribute Back)**

- **Bug fixes** - Fixes that benefit the community
- **Feature enhancements** - New functionality or improvements
- **Documentation improvements** - Better guides, examples, or clarity
- **Performance optimizations** - Code improvements

#### **Ambiguous Changes (Evaluation Needed)**

- **Configuration changes** - May be environment-specific or broadly useful
- **Build improvements** - Could be personal workflow or project enhancement
- **Documentation updates** - Might be personal notes or community-beneficial

### 4.2 Recommended Hybrid Strategy

The most sustainable approach uses different branch strategies for different change types:

#### **Strategy Overview**

- **`main` branch**: Clean, synced with upstream only
- **`personal/config` branch**: Permanent branch for personal changes
- **`feature/name` branches**: Temporary branches for upstream contributions

#### **Personal Branch Management**
Create a permanent personal branch for your configurations:

```bash
# Create personal branch from current feature branch
git checkout feature/add-claude-documentation
git checkout -b personal/config

# Keep only personal files
git reset --soft main
git add CLAUDE.md .claude/
git commit -m "Add personal configuration and documentation"

# Push to your fork
git push origin personal/config
```

#### **Feature Branch Management**

Use temporary feature branches for upstream contributions:

```bash
# Create feature branch from main
git checkout main
git checkout -b feature/improve-documentation

# Add only upstream-worthy changes
git add docs/guidelines/
git commit -m "Improve git workflow documentation"

# Push to your fork
git push origin feature/improve-documentation
```

### 4.3 Sustainable Workflow Patterns

#### **Regular Sync Pattern**

Keep your main branch up-to-date with upstream:

```bash
# Fetch latest changes from upstream
git fetch upstream

# Switch to main and merge upstream changes
git checkout main
git merge upstream/main

# Push updates to your fork
git push origin main
```

#### **Personal Branch Maintenance**

Keep your personal branch updated with main:

```bash
# Switch to personal branch
git checkout personal/config

# Rebase onto updated main (preserves your personal changes)
git rebase main

# Force push with lease (safer than regular force push)
git push origin personal/config --force-with-lease
```

#### **Feature Development Cycle**

Standard workflow for contributing features:

```bash
# 1. Start from updated main
git checkout main
git pull upstream main

# 2. Create feature branch
git checkout -b feature/your-feature-name

# 3. Work on feature
git add .
git commit -m "Implement feature: description"

# 4. Push to your fork
git push origin feature/your-feature-name

# 5. Create Pull Request on GitHub
# 6. After merge, clean up
git checkout main
git pull upstream main
git branch -d feature/your-feature-name
git push origin --delete feature/your-feature-name
```

### 4.4 Branch Lifecycle Management

#### **Long-Term Branches (Keep)**

- **`main`** - Always keep, sync with upstream
- **`personal/config`** - Keep permanently, rebase regularly

#### **Short-Term Branches (Delete After Use)**

- **`feature/*`** - Delete after PR merge
- **`bugfix/*`** - Delete after PR merge
- **`hotfix/*`** - Delete after PR merge

#### **Branch Cleanup Commands**

```bash
# List all branches
git branch -a

# Delete local feature branch
git branch -d feature/branch-name

# Delete remote feature branch
git push origin --delete feature/branch-name

# Prune deleted remote branches
git remote prune origin
```

### 4.5 Handling Mixed Changes

When you have both personal and upstream-worthy changes in one branch:

#### **Split Approach**

```bash
# Current mixed branch
git checkout feature/mixed-changes

# Create personal branch for personal changes
git checkout -b personal/config
git reset --soft main
git add CLAUDE.md .claude/
git commit -m "Personal: Add configurations"
git push origin personal/config

# Create clean feature branch for upstream changes
git checkout main
git checkout -b feature/clean-upstream
git cherry-pick <commit-hash-for-upstream-changes>
git push origin feature/clean-upstream
```

#### **Interactive Rebase Approach**

```bash
# Rewrite history to separate commits
git rebase -i main

# In the rebase editor, separate mixed commits
# Then proceed with branch separation as above
```

### 4.6 Best Practices for Long-Term Success

#### **Repository Hygiene**

- Keep main branch clean and synced
- Delete merged feature branches promptly
- Use descriptive branch names
- Regular personal branch maintenance

#### **Commit Organization**

- Separate personal and upstream changes
- Use clear, descriptive commit messages
- Atomic commits (one logical change per commit)
- Regular commits (don't let changes accumulate)

#### **Collaboration Etiquette**

- Fork-first approach for contributions
- Keep PRs focused and small
- Follow project contribution guidelines
- Respect upstream maintainer decisions

## 5. Regular Workflow

### Staying Updated with Upstream

Periodically sync your fork with the original repository:

```bash
# Fetch the latest changes from upstream
git fetch upstream

# Switch to your main branch
git checkout main

# Merge upstream changes into your main branch
git merge upstream/main

# Push the updates to your fork
git push origin main
```

### Working on Features

1. **Create a feature branch:**
   
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes and commit:**
   
   ```bash
   git add .
   git commit -m "Add your feature description"
   ```

3. **Push the feature branch to your fork:**
   
   ```bash
   git push origin feature/your-feature-name
   ```

4. **Create a Pull Request:**
   - Go to your fork on GitHub
   - Click "New Pull Request"
   - Select the feature branch to merge into the original repository

### Best Practices

- **Keep main branch clean:** Only use your main branch for syncing with upstream
- **Use feature branches:** Create separate branches for each feature or bug fix
- **Regular updates:** Sync with upstream regularly to avoid conflicts
- **Meaningful commits:** Write clear, descriptive commit messages

## 6. Project Structure

The Steel Browser project consists of:

- **`api/`** - Node.js/TypeScript backend with Fastify
- **`ui/`** - React/TypeScript frontend with Vite
- **`docs/`** - Documentation files
- **`repl/`** - REPL package for testing
- **Docker configurations** - For containerized deployment

### Development Commands

```bash
# Install dependencies
npm install

# Run development server
npm run dev

# Run with Docker (development)
docker compose -f docker-compose.dev.yml up --build

# Run with Docker (production)
docker compose up
```

## 7. Contributing Back to Steel

When you're ready to contribute your changes back to the original Steel Browser project:

1. Ensure your changes are in a feature branch
2. Push the branch to your fork
3. Create a Pull Request from your fork to the original repository
4. Follow the project's contribution guidelines and code review process

## 8. Troubleshooting

### Remote Already Exists Error

If you get "remote upstream already exists":

```bash
git remote remove upstream
git remote add upstream https://github.com/steel-dev/steel-browser
```

### Merge Conflicts

If you encounter merge conflicts when syncing:

1. Resolve conflicts in the affected files
2. Add the resolved files: `git add .`
3. Complete the merge: `git commit`
4. Push to your fork: `git push origin main`

### Force Push Warning
Avoid using `git push --force` on shared branches. If you need to force push, use `--force-with-lease` for safety.

## 9. Support

- **Project Issues:** <https://github.com/steel-dev/steel-browser/issues>
- **Discord Community:** <https://discord.gg/steel-dev>
- **Documentation:** <https://docs.steel.dev/>

---

This guide ensures you can maintain your own version of Steel Browser while staying synchronized with the upstream project for the latest features and bug fixes.
