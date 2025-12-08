- [[Tech Fundamentals]]

## Advanced Commands

### Fix-up Commits
```bash
git commit --fixup={commit hash}
git rebase HEAD~7 -i --autosquash
```

### Interactive Rebase
```bash
# Rebase last 5 commits
git rebase -i HEAD~5

# Rebase from specific commit
git rebase -i <commit-hash>

# Edit, squash, reorder commits
# pick, edit, squash, fixup, drop
```

### Stash Advanced
```bash
# Stash with message
git stash push -m "WIP: feature"

# Stash specific files
git stash push -m "message" -- file1.txt file2.txt

# Stash including untracked files
git stash push -u

# List stashes
git stash list

# Apply stash without removing
git stash apply stash@{0}

# Apply and remove
git stash pop stash@{0}

# Drop stash
git stash drop stash@{0}
```

### Cherry-pick
```bash
# Pick specific commit
git cherry-pick <commit-hash>

# Pick range of commits
git cherry-pick <start-commit>^..<end-commit>

# Pick without committing
git cherry-pick -n <commit-hash>

# Pick with no commit (just changes)
git cherry-pick --no-commit <commit-hash>
```

### Reflog
```bash
# View reflog
git reflog

# Recover lost commit
git reflog
git checkout <commit-hash>

# Reset to specific reflog entry
git reset --hard HEAD@{2}
```

### Reset vs Revert
```bash
# Soft reset (keeps changes staged)
git reset --soft HEAD~1

# Mixed reset (keeps changes unstaged) - default
git reset HEAD~1
git reset --mixed HEAD~1

# Hard reset (discards changes)
git reset --hard HEAD~1

# Revert (creates new commit undoing changes)
git revert <commit-hash>
```

### Amend
```bash
# Amend last commit message
git commit --amend -m "New message"

# Amend last commit with new changes
git add file.txt
git commit --amend --no-edit

# Amend author
git commit --amend --author="Name <email>"
```

### Bisect
```bash
# Find bug-introducing commit
git bisect start
git bisect bad                    # current commit is bad
git bisect good <commit-hash>     # known good commit
# Git checks out middle commit, test it
git bisect good                   # if this commit is good
git bisect bad                    # if this commit is bad
# Repeat until found
git bisect reset                  # return to original branch
```

### Worktree
```bash
# Create new worktree (multiple checkouts)
git worktree add ../feature-branch feature-branch

# List worktrees
git worktree list

# Remove worktree
git worktree remove ../feature-branch
```

### Submodules
```bash
# Add submodule
git submodule add <repository-url> <path>

# Clone repo with submodules
git clone --recurse-submodules <repo-url>

# Update submodules
git submodule update --init --recursive

# Update submodule to latest
git submodule update --remote
```

### Clean
```bash
# Preview files to be removed
git clean -n

# Remove untracked files
git clean -f

# Remove untracked files and directories
git clean -fd

# Interactive removal
git clean -i
```

### Log Advanced
```bash
# One line per commit
git log --oneline

# Graph view
git log --oneline --graph --all

# Search commits by message
git log --grep="keyword"

# Search commits by author
git log --author="name"

# Search commits touching file
git log -- file.txt

# Show changes in commits
git log -p

# Last N commits
git log -n 5
```

### Diff Advanced
```bash
# Diff staged changes
git diff --staged
git diff --cached

# Diff specific file
git diff HEAD -- file.txt

# Diff between branches
git diff branch1..branch2

# Diff between commits
git diff <commit1> <commit2>

# Word-level diff
git diff --word-diff
```

### Branch Advanced
```bash
# Rename branch
git branch -m old-name new-name

# Delete remote branch
git push origin --delete branch-name

# Track remote branch
git branch --set-upstream-to=origin/branch-name branch-name

# List merged branches
git branch --merged

# List unmerged branches
git branch --no-merged
```

### Fetch vs Pull
```bash
# Fetch without merging
git fetch origin

# Fetch specific branch
git fetch origin branch-name

# Fetch all remotes
git fetch --all

# Pull with rebase
git pull --rebase origin main
```

### Tagging
```bash
# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Release version 1.0.0"

# Push tags
git push origin v1.0.0
git push --tags

# Delete tag
git tag -d v1.0.0
git push origin --delete v1.0.0
```

### Aliases
```bash
# Create alias
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
```

### Useful Shortcuts
```bash
# Short status
git status -s

# Show what changed in last commit
git show

# Show file at specific commit
git show <commit>:<file-path>

# Find when bug was introduced
git log -S "bug text" --source --all

# Find file in history
git log --all --full-history -- <file-path>
```