# Git Commands Cheat Sheet

Essential Git commands for version control.

## ⚙️ Configuration

```bash
# Set user info
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# View config
git config --list
git config user.name

# Set default editor
git config --global core.editor "code --wait"

# Aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --graph --oneline --all"
```

## 🚀 Getting Started

```bash
# Initialize repository
git init

# Clone repository
git clone https://github.com/user/repo.git
git clone -b branch_name url
```

## 📝 Basic Operations

```bash
# Status
git status
git status -s                    # Short format

# Stage changes
git add file.txt                 # Add specific file
git add .                        # Add all changes
git add *.js                     # Add all JS files

# Unstage
git reset file.txt               # Unstage file
git reset                        # Unstage all

# Commit
git commit -m "Message"
git commit -am "Message"         # Stage and commit
git commit --amend               # Amend last commit

# View history
git log
git log --oneline
git log --graph --oneline --all
git log -n 5                     # Last 5 commits
git log --author="Name"

# Show changes
git diff                         # Unstaged changes
git diff --staged               # Staged changes
git diff commit1 commit2
```

## 🌿 Branching

```bash
# List branches
git branch                       # Local
git branch -r                    # Remote
git branch -a                    # All

# Create branch
git branch feature
git checkout -b feature          # Create and switch
git switch -c feature            # Modern syntax

# Switch branch
git checkout main
git switch main                  # Modern syntax

# Merge
git merge feature
git merge --no-ff feature        # Create merge commit

# Delete branch
git branch -d feature            # Safe delete
git branch -D feature            # Force delete

# Rename branch
git branch -m old_name new_name
```

## 🌐 Remote Operations

```bash
# View remotes
git remote -v

# Add remote
git remote add origin https://github.com/user/repo.git

# Fetch
git fetch
git fetch origin

# Pull
git pull
git pull origin main
git pull --rebase

# Push
git push
git push origin main
git push -u origin main          # Set upstream
git push --all                   # All branches
git push --tags                  # Push tags
```

## 🏷️ Tags

```bash
# List tags
git tag

# Create tag
git tag v1.0.0
git tag -a v1.0.0 -m "Version 1.0.0"

# Push tags
git push origin v1.0.0
git push --tags

# Delete tag
git tag -d v1.0.0                # Local
git push origin --delete v1.0.0  # Remote
```

## 🔧 Undoing Changes

```bash
# Discard changes
git checkout -- file.txt         # Discard file changes
git restore file.txt             # Modern syntax
git restore .                    # All files

# Reset commits
git reset --soft HEAD~1          # Keep changes staged
git reset HEAD~1                 # Keep changes unstaged
git reset --hard HEAD~1          # Discard changes

# Revert
git revert commit_hash           # Create reverting commit
```

## 🎯 Stashing

```bash
# Stash changes
git stash
git stash save "Work in progress"

# List stashes
git stash list

# Apply stash
git stash apply
git stash pop                    # Apply and remove

# Drop stash
git stash drop stash@{0}
git stash clear                  # Remove all
```

## 🗑️ Cleaning

```bash
# Preview
git clean -n

# Remove untracked files
git clean -f
git clean -fd                    # Files and directories
git clean -fX                    # Ignored files only
```

## 🔍 Inspection

```bash
# Show commit
git show commit_hash
git show HEAD
git show HEAD~3

# Blame
git blame file.txt               # Who changed what

# Log with patches
git log -p
git log -p -2                    # Last 2 commits with diff

# Search history
git log --grep="keyword"
git log -S "function_name"       # Search code changes
```

## 🍒 Advanced

```bash
# Cherry-pick
git cherry-pick commit_hash

# Rebase
git rebase main
git rebase -i HEAD~3             # Interactive

# Squash commits
git rebase -i HEAD~3             # Change 'pick' to 'squash'

# Submodules
git submodule add url path
git submodule update --init --recursive
```

## 💡 Useful Commands

```bash
# Find deleted file
git log --all --full-history -- path/to/file

# List files in commit
git show --name-only commit_hash

# Compare branches
git diff branch1..branch2

# Show file from specific commit
git show commit_hash:path/to/file

# Count commits
git rev-list --count branch_name

# List contributors
git shortlog -sn

# Find commit by message
git log --grep="pattern"

# View file history
git log --follow -- file.txt
```

## 🔄 Workflows

### Feature Branch
```bash
git checkout -b feature/new-feature
# Make changes
git add .
git commit -m "Add feature"
git push -u origin feature/new-feature
# Create PR
# After merge
git checkout main
git pull
git branch -d feature/new-feature
```

### Hotfix
```bash
git checkout -b hotfix/bug-fix main
# Fix bug
git commit -am "Fix critical bug"
git checkout main
git merge --no-ff hotfix/bug-fix
git tag -a v1.0.1 -m "Hotfix 1.0.1"
git push --follow-tags
git branch -d hotfix/bug-fix
```

## 📊 .gitignore

```bash
# Patterns
*.log                            # All log files
*.tmp
node_modules/                    # Directories
build/
dist/
.env                             # Environment files
.DS_Store                        # macOS
Thumbs.db                        # Windows
```

## 🚨 Emergency Commands

```bash
# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Recover deleted branch
git reflog
git checkout -b branch_name commit_hash

# Abort merge
git merge --abort

# Abort rebase
git rebase --abort

# Recover lost commits
git reflog
git checkout commit_hash
```

## Related Topics

- [[Development/Git Workflow|Git Workflow (Detailed)]]
- [[Development/GitHub|GitHub]]
- [[Development/Development Index|Development]]

---

#git #versioncontrol #cheatsheet #reference
