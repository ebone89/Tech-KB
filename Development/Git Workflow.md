# Git Workflow

Essential Git commands and workflows for version control.

## 🚀 Git Basics

### Configuration
```bash
# Set user information
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# View configuration
git config --list
git config user.name

# Set default editor
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"          # Vim

# Set default branch name
git config --global init.defaultBranch main
```

### Initialize Repository
```bash
# Create new repository
git init

# Clone existing repository
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git myproject  # Custom name
git clone -b branch_name https://github.com/user/repo.git  # Specific branch
```

## 📝 Basic Operations

### Check Status
```bash
# View status
git status
git status -s                           # Short format
git status -sb                          # Short format with branch
```

### Staging Changes
```bash
# Add files to staging
git add filename.txt                    # Specific file
git add .                               # All files in current directory
git add -A                              # All files in repository
git add *.js                            # All JS files
git add src/                            # All files in directory

# Remove from staging
git reset filename.txt                  # Unstage file
git reset                               # Unstage all
```

### Committing
```bash
# Commit staged changes
git commit -m "Commit message"

# Commit with detailed message
git commit -m "Short description" -m "Detailed explanation"

# Commit all tracked changes (skip staging)
git commit -am "Commit message"

# Amend last commit
git commit --amend -m "New message"
git commit --amend --no-edit            # Keep same message
```

### Viewing History
```bash
# View commit history
git log
git log --oneline                       # Compact view
git log --graph                         # Graph view
git log --graph --oneline --all         # Full graph
git log -n 5                            # Last 5 commits
git log --author="Name"                 # Filter by author
git log --since="2 weeks ago"           # Time filter
git log --grep="keyword"                # Search commits
git log filename.txt                    # File history

# View changes
git diff                                # Unstaged changes
git diff --staged                       # Staged changes
git diff commit1 commit2                # Between commits
git diff branch1 branch2                # Between branches

# Show commit details
git show commit_hash
git show HEAD                           # Latest commit
git show HEAD~3                         # 3 commits ago
```

## 🌿 Branching

### Basic Branch Operations
```bash
# List branches
git branch                              # Local branches
git branch -r                           # Remote branches
git branch -a                           # All branches

# Create branch
git branch new_feature                  # Create only
git checkout -b new_feature             # Create and switch
git switch -c new_feature               # Modern alternative

# Switch branches
git checkout main
git switch main                         # Modern alternative

# Rename branch
git branch -m old_name new_name         # Rename
git branch -m new_name                  # Rename current branch

# Delete branch
git branch -d feature_branch            # Safe delete (merged only)
git branch -D feature_branch            # Force delete
```

### Merging
```bash
# Merge branch into current
git merge feature_branch

# Merge with no fast-forward (creates merge commit)
git merge --no-ff feature_branch

# Abort merge
git merge --abort

# View merged branches
git branch --merged
git branch --no-merged
```

### Rebasing
```bash
# Rebase current branch onto main
git rebase main

# Interactive rebase (edit history)
git rebase -i HEAD~3                    # Last 3 commits

# Continue/abort rebase
git rebase --continue
git rebase --abort
```

## 🌐 Remote Operations

### Remote Repositories
```bash
# View remotes
git remote
git remote -v                           # Verbose (show URLs)

# Add remote
git remote add origin https://github.com/user/repo.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream

# Change remote URL
git remote set-url origin https://new-url.git
```

### Fetch, Pull, Push
```bash
# Fetch updates (doesn't merge)
git fetch
git fetch origin
git fetch --all                         # All remotes

# Pull updates (fetch + merge)
git pull
git pull origin main
git pull --rebase                       # Rebase instead of merge

# Push changes
git push
git push origin main
git push -u origin main                 # Set upstream
git push --all                          # All branches
git push --tags                         # Push tags
git push -f                             # Force push (DANGEROUS!)
```

## 🏷️ Tags

```bash
# List tags
git tag
git tag -l "v1.*"                       # Filter tags

# Create tag
git tag v1.0.0                          # Lightweight tag
git tag -a v1.0.0 -m "Version 1.0.0"    # Annotated tag

# Push tags
git push origin v1.0.0                  # Single tag
git push origin --tags                  # All tags

# Delete tag
git tag -d v1.0.0                       # Local
git push origin --delete v1.0.0         # Remote

# Checkout tag
git checkout v1.0.0
```

## 🔧 Undoing Changes

### Discard Changes
```bash
# Discard working directory changes
git checkout -- filename.txt            # Specific file
git checkout .                          # All files
git restore filename.txt                # Modern alternative
git restore .                           # All files

# Discard staged changes
git reset HEAD filename.txt
git restore --staged filename.txt       # Modern alternative
```

### Reset Commits
```bash
# Soft reset (keep changes staged)
git reset --soft HEAD~1

# Mixed reset (keep changes unstaged) - DEFAULT
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset (discard all changes)
git reset --hard HEAD~1
git reset --hard origin/main            # Match remote
```

### Revert Commits
```bash
# Create new commit that undoes changes
git revert commit_hash
git revert HEAD                         # Revert last commit
git revert HEAD~3..HEAD                 # Revert range
```

## 🗑️ Cleaning

```bash
# Remove untracked files
git clean -n                            # Dry run (preview)
git clean -f                            # Remove files
git clean -fd                           # Remove files and directories
git clean -fX                           # Remove ignored files
git clean -fx                           # Remove all untracked
```

## 🎯 Stashing

```bash
# Stash changes
git stash
git stash save "Work in progress"

# List stashes
git stash list

# Apply stash
git stash apply                         # Apply latest
git stash apply stash@{2}               # Specific stash
git stash pop                           # Apply and remove

# Drop stash
git stash drop stash@{0}
git stash clear                         # Remove all stashes

# Create branch from stash
git stash branch new_branch
```

## 🔍 Advanced Operations

### Cherry-pick
```bash
# Apply specific commit to current branch
git cherry-pick commit_hash
git cherry-pick commit1 commit2         # Multiple commits
```

### Submodules
```bash
# Add submodule
git submodule add https://github.com/user/repo.git path/to/submodule

# Clone with submodules
git clone --recursive https://github.com/user/repo.git

# Update submodules
git submodule update --init --recursive
git submodule update --remote
```

### Worktrees
```bash
# Create worktree
git worktree add ../project-feature feature-branch

# List worktrees
git worktree list

# Remove worktree
git worktree remove ../project-feature
```

## 📋 .gitignore

```bash
# Common .gitignore patterns
*.log                                   # All log files
*.tmp                                   # All temp files
node_modules/                           # Directory
build/
dist/
.env                                    # Environment files
.DS_Store                               # macOS
Thumbs.db                               # Windows
*.swp                                   # Vim swap files
*.pyc                                   # Python compiled
__pycache__/
.idea/                                  # JetBrains IDEs
.vscode/                                # VS Code
```

## 🔄 Common Workflows

### Feature Branch Workflow
```bash
# 1. Create feature branch
git checkout -b feature/new-feature

# 2. Make changes and commit
git add .
git commit -m "Add new feature"

# 3. Push to remote
git push -u origin feature/new-feature

# 4. Create Pull Request on GitHub/GitLab

# 5. After PR approval, merge and delete branch
git checkout main
git pull origin main
git branch -d feature/new-feature
```

### Fixing Mistakes
```bash
# Forgot to add files to last commit
git add forgotten_file.txt
git commit --amend --no-edit

# Wrong commit message
git commit --amend -m "Correct message"

# Committed to wrong branch
git log -1                              # Copy commit hash
git checkout correct-branch
git cherry-pick commit_hash
git checkout wrong-branch
git reset --hard HEAD~1
```

## 📊 Git Aliases

Add to `~/.gitconfig`:
```ini
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = reset HEAD --
    last = log -1 HEAD
    lg = log --graph --oneline --all
    undo = reset --soft HEAD~1
```

Usage:
```bash
git st                                  # Instead of git status
git co main                             # Instead of git checkout main
git lg                                  # Pretty log graph
```

## 💡 Best Practices

1. **Commit Often**: Make small, logical commits
2. **Write Good Messages**: Clear, descriptive commit messages
3. **Use Branches**: Keep main/master clean, develop in branches
4. **Pull Before Push**: Always pull latest changes before pushing
5. **Review Before Commit**: Use `git diff` to review changes
6. **Don't Commit Secrets**: Use .gitignore for sensitive files
7. **Use .gitignore**: Exclude build artifacts, dependencies
8. **Tag Releases**: Use tags for version releases

## Related Topics

- [[GitHub]]
- [[GitLab]]
- [[Cheat Sheets/Git Commands|Git Commands Cheat Sheet]]

---

#git #versioncontrol #development
