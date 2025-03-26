# GitHub Cheat Sheet

## Initial Setup

```bash
# Configure user information
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# Configure default editor
git config --global core.editor "code --wait"  # For VS Code

# Verify configuration
git config --list
```

## Basic Commands

```bash
# Initialize a repository
git init

# Clone an existing repository
git clone https://github.com/username/repository.git
git clone https://github.com/username/repository.git directory-name

# Repository status
git status

# Add files to staging area
git add filename
git add .                  # Add all changes
git add *.py               # Add all .py files

# Commit changes
git commit -m "Descriptive commit message"
git commit -am "Message"   # Add and commit in one step (tracked files only)

# View commit history
git log
git log --oneline          # Summarized format
git log --graph            # Visual format of branches
```

## Working with Branches

```bash
# List branches
git branch                 # Local
git branch -r              # Remote
git branch -a              # All

# Create branch
git branch branch-name

# Switch to a branch
git checkout branch-name
git switch branch-name     # Modern Git

# Create and switch to a branch in one step
git checkout -b branch-name
git switch -c branch-name  # Modern Git

# Delete branch
git branch -d branch-name  # Safe deletion
git branch -D branch-name  # Force deletion

# Merge branches
git merge branch-name      # Merge branch-name into current branch
```

## Working with Remote Repositories

```bash
# List remotes
git remote -v

# Add remote
git remote add origin https://github.com/username/repository.git

# Change remote URL
git remote set-url origin https://github.com/username/new-repo.git

# Download changes without merging
git fetch origin

# Download changes and merge
git pull origin branch-name

# Send changes to remote
git push origin branch-name
git push -u origin branch-name  # Set upstream branch
```

## Undoing Changes

```bash
# Discard changes in unstaged files
git restore filename
git checkout -- filename  # Old Git

# Undo staged changes (remove from staging)
git restore --staged filename
git reset HEAD filename   # Old Git

# Modify last commit
git commit --amend -m "New message"

# Revert a commit (creates a new commit that undoes changes)
git revert commit-hash

# Reset to a specific commit
git reset --soft commit-hash    # Keeps changes in staging
git reset --mixed commit-hash   # Keeps changes in working directory
git reset --hard commit-hash    # Removes all changes (dangerous!)
```

## Stash (Temporary Storage)

```bash
# Save changes temporarily
git stash

# Save with message
git stash save "Descriptive message"

# List stashes
git stash list

# Apply latest stash
git stash apply

# Apply specific stash
git stash apply stash@{n}

# Apply and delete latest stash
git stash pop

# Delete latest stash
git stash drop

# Delete specific stash
git stash drop stash@{n}

# Delete all stashes
git stash clear
```

## Pull Requests

1. Fork the repository on GitHub
2. Clone your fork: `git clone https://github.com/your-username/repository.git`
3. Create branch for changes: `git checkout -b feature/new-functionality`
4. Make changes, commit, and push: `git push origin feature/new-functionality`
5. Go to GitHub and create Pull Request from your branch

## Working with Tags

```bash
# List tags
git tag

# Create lightweight tag
git tag v1.0.0

# Create annotated tag (recommended)
git tag -a v1.0.0 -m "Version 1.0.0"

# Create tag for a specific commit
git tag -a v0.9.0 -m "Beta Version" commit-hash

# Push tags to remote
git push origin v1.0.0        # Specific tag
git push origin --tags        # All tags
```

## Git Flow (Workflow)

```bash
# Install Git Flow
apt-get install git-flow      # Debian/Ubuntu
brew install git-flow         # macOS

# Initialize Git Flow in a repository
git flow init

# Work with features
git flow feature start feature-name
git flow feature finish feature-name

# Work with releases
git flow release start v1.0.0
git flow release finish v1.0.0
```

## Troubleshooting

```bash
# View reflog (reference history)
git reflog

# Verify repository integrity
git fsck

# Clean untracked files
git clean -n                   # Simulation (shows what would be removed)
git clean -f                   # Remove untracked files
git clean -fd                  # Include directories

# Maintenance and optimization
git gc                         # Garbage collection
git prune                      # Remove inaccessible objects
```

## .gitignore Configuration

Example of a `.gitignore` file:

```
# System files
.DS_Store
Thumbs.db

# Virtual environment directories
venv/
env/
.env/

# Python files
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
*.egg-info/

# Log files
*.log

# Dependency directories
node_modules/
vendor/

# Local configuration files
config.local.js
.env.local
```

## GitHub CLI

```bash
# Install GitHub CLI
# Visit: https://cli.github.com/

# Authenticate
gh auth login

# Clone repository
gh repo clone username/repository

# Create repository
gh repo create repo-name

# Create issue
gh issue create --title "Title" --body "Description"

# Create pull request
gh pr create --title "Title" --body "Description"
```

## Advanced Git Commands

```bash
# Interactive rebase
git rebase -i HEAD~3  # Rebase last 3 commits

# Cherry-pick commits
git cherry-pick commit-hash

# Bisect to find bugs
git bisect start
git bisect bad        # Current version is bad
git bisect good v1.0  # v1.0 was good
# Git will help you find the bad commit

# Create and apply patches
git format-patch -1 commit-hash
git apply patch-file.patch

# Show changes in commit
git show commit-hash
```

## GitHub Actions (CI/CD)

Basic workflow example (`.github/workflows/main.yml`):

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: 3.9
        
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest
        pip install -r requirements.txt
        
    - name: Run tests
      run: |
        pytest
```
