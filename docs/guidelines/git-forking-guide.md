# Fork-Based Development Guide

This guide explains how to fork the Steel Browser project for independent development while staying synchronized with valuable upstream improvements.

## Overview

When you want to develop your own enhanced version of Steel Browser while benefiting from upstream improvements, you'll need to:
1. Fork the repository to your GitHub account
2. Set up your local repository to push to your fork
3. Configure upstream tracking to receive valuable updates from the original repository
4. Establish a unified development workflow on your main branch

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
  - Complex changes that benefit from isolated development

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

## 4. Fork-Based Development Strategy

After setting up your fork and handling initial changes, establish a unified development approach where your main branch evolves as your enhanced version of Steel Browser while selectively benefiting from upstream improvements.

### 4.1 Fork-First Philosophy

Your fork represents your enhanced version of Steel Browser. Unlike traditional contribution-focused workflows, your main branch contains ALL your improvements, customizations, and enhancements without artificial separation of change types.

#### **Core Principles**

- **Unified Development**: Your main branch is your primary development workspace
- **Selective Integration**: Cherry-pick valuable upstream improvements
- **Enhancement Tracking**: Maintain clear history of your improvements vs upstream changes
- **Simplified Workflow**: Minimize branching complexity, maximize development efficiency

#### **Strategic Benefits**

- **Independent Evolution**: Your fork can evolve according to your needs
- **Upstream Value**: Still benefit from bug fixes and valuable features
- **Reduced Complexity**: No need for complex branch coordination
- **Clear Ownership**: Your main branch represents your vision of Steel Browser

### 4.2 Unified Development Workflow

#### **Primary Development Pattern**

```bash
# Most development happens directly on main
git checkout main

# Make your improvements
git add .
git commit -m "Add feature: your enhancement description"

# Push to your fork
git push origin main
```

#### **Feature Branches (Optional for Complex Work)**

Use feature branches sparingly, only for complex multi-day work:

```bash
# For complex experimental features
git checkout -b feature/experimental-enhancement

# Develop the feature
git add .
git commit -m "Implement experimental feature"

# When ready, merge back to main
git checkout main
git merge feature/experimental-enhancement
git branch -d feature/experimental-enhancement
git push origin main
```

### 4.3 Selective Upstream Integration

#### **Regular Upstream Evaluation**

```bash
# Stay informed about upstream changes
git fetch upstream

# Review what's new upstream
git log --oneline main..upstream/main

# Check detailed changes
git diff main upstream/main
```

#### **Selective Integration Strategies**

**Option A: Full Merge (When Compatible)**
```bash
# If upstream changes are fully compatible
git checkout main
git merge upstream/main
git push origin main
```

**Option B: Cherry-Pick Specific Changes**
```bash
# Pick specific valuable commits
git cherry-pick <commit-hash-1>
git cherry-pick <commit-hash-2>
git push origin main
```

**Option C: Manual Integration**
```bash
# For complex conflicts, manually integrate changes
git checkout main
git fetch upstream
# Review changes and manually apply what you want
git add .
git commit -m "Integrate valuable upstream improvements"
git push origin main
```

### 4.4 Consolidating Existing Branches

If you're transitioning from a multi-branch setup to fork-based development:

#### **Branch Consolidation Process**

```bash
# 1. Switch to main branch
git checkout main

# 2. Merge your feature branches
git merge feature/enhance-project-workflow

# 3. Merge personal configurations
git merge personal/config

# 4. Resolve any conflicts, keeping your enhancements
git add .
git commit -m "Consolidate all enhancements into unified main branch"

# 5. Push unified main to your fork
git push origin main
```

#### **Clean Up Temporary Branches**

```bash
# Delete local branches
git branch -d feature/enhance-project-workflow
git branch -d personal/config

# Delete remote branches
git push origin --delete feature/enhance-project-workflow
git push origin --delete personal/config

# Clean up branch tracking
git remote prune origin
```

### 4.5 Fork Evolution Management

#### **Track Your Enhancements**

Document major enhancements in your fork:

```bash
# Use descriptive commit messages
git commit -m "feat: add intelligent work progress tracking system"
git commit -m "enhance: improve git workflow documentation for fork development"
git commit -m "config: add personal Claude Code integration"
```

#### **Maintain Enhancement History**

```bash
# Periodically review your fork's evolution
git log --oneline upstream/main..main

# See your unique commits since last upstream sync
git log --graph --oneline upstream/main..main
```

### 4.6 Best Practices for Fork Development

#### **Development Efficiency**

- **Main Branch Focus**: Develop primarily on main branch
- **Selective Branching**: Use feature branches only for complex experimental work
- **Regular Commits**: Commit frequently with clear, descriptive messages
- **Push Often**: Keep your fork synchronized with your local changes

#### **Upstream Relationship**

- **Regular Evaluation**: Review upstream changes monthly or as needed
- **Selective Integration**: Only integrate changes that add value to your fork
- **Conflict Resolution**: Always preserve your enhancements when conflicts arise
- **Enhancement Documentation**: Keep clear history of your improvements

#### **Repository Hygiene**

- **Clean History**: Maintain readable commit history
- **Descriptive Messages**: Use clear commit messages that explain the value added
- **Regular Cleanup**: Remove unnecessary branches and keep repository organized
- **Backup Strategy**: Regular pushes to your fork serve as backup

## 5. Main-Branch Development Workflow

### Daily Development Routine

Your main branch is your primary workspace for building your enhanced version of Steel Browser:

```bash
# Start your development session
git checkout main
git pull origin main

# Make your improvements directly on main
git add .
git commit -m "enhance: improve browser session management"

# Push your progress regularly
git push origin main
```

### Staying Updated with Upstream

Evaluate and selectively integrate upstream improvements:

```bash
# Check for upstream updates
git fetch upstream

# Review what's new (optional but recommended)
git log --oneline main..upstream/main

# Option A: Selective integration (recommended)
git cherry-pick <specific-commit-hash>

# Option B: Full merge (when compatible)
git merge upstream/main

# Push your updated main branch
git push origin main
```

### Feature Development Patterns

#### Primary Pattern: Direct Main Development
```bash
# Most features developed directly on main
git checkout main

# Implement your enhancement
git add .
git commit -m "feat: add intelligent tab management"

# Push immediately for backup
git push origin main
```

#### Secondary Pattern: Complex Features (Optional)
For experimental or complex multi-day work:

```bash
# Create temporary feature branch
git checkout -b experiment/advanced-automation

# Develop the complex feature
git add .
git commit -m "experiment: advanced browser automation"

# When ready, integrate to main
git checkout main
git merge experiment/advanced-automation
git branch -d experiment/advanced-automation

# Push consolidated changes
git push origin main
```

### Fork Development vs Traditional Workflows

**❌ What You DON'T Need (Common Misconception):**

Fork-based development is NOT the same as traditional contribution workflows:

```bash
# ❌ Avoid this traditional pattern within your fork:
git checkout -b feature/my-feature        # Unnecessary branching
git push origin feature/my-feature        # Unnecessary feature branch
# Create PR to merge into your own main   # Unnecessary bureaucracy
```

**✅ What You DO Need (Fork-Based Approach):**

```bash
# ✅ Primary pattern - direct main development:
git checkout main
git add .
git commit -m "feat: add browser automation feature"
git push origin main

# ✅ Secondary pattern - only for complex experimental work:
git checkout -b experiment/complex-feature
# ... develop complex feature ...
git checkout main
git merge experiment/complex-feature      # Direct merge, no PR needed
git branch -d experiment/complex-feature
git push origin main
```

### Best Practices for Fork Development

- **Main-Branch Focus:** Develop primarily on main branch for simplicity
- **No Internal PRs:** You own your fork - merge directly, no PRs within your own repository
- **Regular Commits:** Commit frequently with clear, descriptive messages  
- **Selective Upstream Integration:** Only merge valuable upstream changes
- **Enhancement Documentation:** Use clear commit messages that describe value added
- **Regular Backups:** Push to your fork frequently to prevent data loss

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

## 7. Optional: Contributing Back to Steel (When Desired)

Since your fork is designed for independent development, contributing back to the original Steel Browser project is entirely optional. However, if you develop features that would benefit the broader community, you can choose to contribute:

### When to Consider Contributing

- **Bug fixes** that affect core functionality
- **Performance improvements** that benefit all users  
- **General features** that don't compromise your competitive advantages
- **Documentation improvements** for better project clarity

### Contribution Process (Optional)

If you decide to contribute specific improvements:

```bash
# Create a clean feature branch from latest upstream
git fetch upstream
git checkout -b contrib/feature-name upstream/main

# Cherry-pick or manually implement the specific feature
git cherry-pick <your-commit-hash>
# or manually implement the feature in isolation

# Push to your fork
git push origin contrib/feature-name

# Create Pull Request from your fork to upstream
```

### Contribution Strategy

- **Selective Sharing:** Only contribute features that align with your goals
- **Clean Implementation:** Ensure contributed code doesn't include your proprietary enhancements
- **Maintain Independence:** Keep your main development on your fork's main branch
- **Community Benefit:** Focus contributions on broadly useful improvements

Remember: Your primary development remains on your fork, and contribution is a secondary, optional activity.

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
