# Git Worktree Workflow for Research Projects

**Purpose:** Organize parallel research using git worktrees for clean, isolated work environments.

**Key Benefit:** Multiple research streams can work simultaneously without branch switching chaos.

---

## Why Worktrees for Research?

### The Problem with Traditional Branching

**Traditional workflow:**
```bash
git checkout -b research/critical-path
# Work on critical research
git commit -m "feat: critical findings"
git checkout main
git merge research/critical-path

git checkout -b research/high-priority
# Work on high priority research
git commit -m "feat: high priority findings"
git checkout main
git merge research/high-priority
```

**Issues:**
- Constant branch switching
- Risk of uncommitted changes when switching
- File system state changes with each checkout
- Can't work on multiple branches simultaneously
- IDE/editor gets confused by changing files

### The Worktree Solution

**With worktrees:**
```bash
# Each branch gets its own directory
project/
├── main/                    # Main worktree
├── research-critical/       # Critical path worktree
├── research-high/           # High priority worktree  
└── research-medium/         # Medium priority worktree
```

**Benefits:**
- âœ… Each research stream has its own directory
- âœ… No branch switching needed
- âœ… Work on multiple branches simultaneously
- âœ… IDE can have multiple windows open
- âœ… No risk of mixing changes
- âœ… Clean, isolated environments

---

## Setup: Project Initialization

### Step 1: Create Main Repository

```bash
# Create project directory
mkdir research-project
cd research-project

# Initialize git
git init

# Create initial commit
echo "# Research Project" > README.md
git add README.md
git commit -m "chore: initialize project"

# This is your "main" worktree
```

### Step 2: Create Worktree Structure

```bash
# Create directories for worktrees
mkdir -p worktrees

# Optional: Add to .gitignore (worktrees are managed by git)
echo "worktrees/" >> .gitignore
git add .gitignore
git commit -m "chore: setup worktree structure"
```

### Step 3: Verify Setup

```bash
# List worktrees
git worktree list

# Should show:
# /path/to/research-project  [main]
```

---

## Workflow: Research Session

### Phase 1: Critical Path Research

#### Create Critical Path Worktree

```bash
# From main worktree
git worktree add worktrees/critical research/critical-path

# This creates:
# - New directory: worktrees/critical/
# - New branch: research/critical-path
# - Automatically checks out the branch in that directory
```

#### Work in Critical Path Worktree

```bash
# Navigate to worktree
cd worktrees/critical/

# Create research findings
mkdir -p research-findings
# ... research agents create documents ...

# Commit findings
git add research-findings/
git commit -m "feat: complete critical path research

- HW-01: Hardware compatibility validated
- SW-01: Software stack feasible
- SEC-01: No showstopper security issues
- OPS-01: Operational requirements reasonable

ðŸ¤– Generated with Claude Code
Co-Authored-By: Atlas, Nova, Cipher, Sage"

# Return to main worktree
cd ../../
```

#### Merge to Main

```bash
# From main worktree
git merge --no-ff research/critical-path -m "chore: merge critical path research

All critical questions answered with high confidence.
Project feasibility: CONFIRMED ✅"

# The worktree still exists, can be reused or cleaned up later
```

### Phase 2: High Priority Research

#### Create High Priority Worktree

```bash
# Create new worktree from current main
git worktree add worktrees/high-priority research/high-priority

cd worktrees/high-priority/

# This worktree includes all critical path findings!
```

#### Work in High Priority Worktree

```bash
# Research agents create more documents
# ... parallel work happens ...

# Commit
git add research-findings/
git commit -m "feat: complete high-priority research

- SW-02: Monitoring stack selected
- HW-02: Peripheral requirements validated
- OPS-02: Deployment strategy designed

ðŸ¤– Generated with Claude Code
Co-Authored-By: Nova, Atlas, Sage"

cd ../../
```

#### Merge to Main

```bash
# From main worktree
git merge --no-ff research/high-priority -m "chore: merge high-priority research"
```

### Phase 3: Cleanup

#### Option A: Keep Worktrees (for later reference)

```bash
# List all worktrees
git worktree list

# Worktrees remain accessible for future work
# Useful if you might add more research later
```

#### Option B: Remove Worktrees

```bash
# Remove worktree
git worktree remove worktrees/critical/

# Or prune all disconnected worktrees
git worktree prune

# Delete branches (optional)
git branch -d research/critical-path
git branch -d research/high-priority
```

---

## Advanced: Parallel Research with Multiple Worktrees

### Scenario: 4 Research Waves Simultaneously

```bash
# Create all worktrees at once
git worktree add worktrees/wave1-critical research/wave1-critical
git worktree add worktrees/wave2-high research/wave2-high
git worktree add worktrees/wave3-medium research/wave3-medium
git worktree add worktrees/wave4-low research/wave4-low

# Now you have:
research-project/
├── main/                          # Main worktree
├── worktrees/
    ├── wave1-critical/           # Critical path
    ├── wave2-high/               # High priority
    ├── wave3-medium/             # Medium priority
    └── wave4-low/                # Low priority

# Each research agent can work in its own worktree!
```

### Parallel Work Session

```bash
# Terminal 1: Scout in main
cd research-project/
# Scout runs permission initialization

# Terminal 2: Critical research
cd research-project/worktrees/wave1-critical/
# Atlas, Nova, Cipher, Sage create documents

# Terminal 3: High priority research
cd research-project/worktrees/wave2-high/
# More agents work here

# Terminal 4: Medium priority research
cd research-project/worktrees/wave3-medium/
# Even more agents!

# No branch switching! No conflicts! Pure parallel work!
```

### Sequential Merging

```bash
# From main worktree
cd research-project/

# Merge in order
git merge --no-ff research/wave1-critical
git merge --no-ff research/wave2-high
git merge --no-ff research/wave3-medium
git merge --no-ff research/wave4-low

# Or merge all at once (if no dependencies)
git merge --no-ff research/wave1-critical research/wave2-high \
                   research/wave3-medium research/wave4-low
```

---

## Worktree Commands Reference

### Creating Worktrees

```bash
# Create worktree with new branch
git worktree add <path> <new-branch>

# Create worktree from existing branch
git worktree add <path> <existing-branch>

# Create worktree with specific commit
git worktree add <path> <commit-hash>

# Create worktree and checkout different branch
git worktree add --track -b <new-branch> <path> <remote>/<branch>
```

### Managing Worktrees

```bash
# List all worktrees
git worktree list

# Detailed list
git worktree list --porcelain

# Move worktree to new location
git worktree move <worktree> <new-path>

# Remove worktree
git worktree remove <worktree>

# Remove worktree (force)
git worktree remove --force <worktree>

# Prune stale worktrees
git worktree prune

# Lock worktree (prevent removal)
git worktree lock <worktree>

# Unlock worktree
git worktree unlock <worktree>
```

### Working in Worktrees

```bash
# Each worktree is independent
cd worktrees/research-wave1/

# Normal git commands work
git status
git add .
git commit -m "message"
git log

# But branch operations affect the repository
git branch      # Shows all branches (shared)
git checkout    # Can't checkout branch that's checked out elsewhere!
```

---

## Best Practices

### 1. Naming Convention

Use consistent naming for worktrees and branches:

```bash
# Pattern: worktrees/<wave>-<priority>/ → research/<wave>-<priority>
git worktree add worktrees/wave1-critical research/wave1-critical
git worktree add worktrees/wave2-high research/wave2-high
git worktree add worktrees/synthesis research/synthesis

# This makes it easy to see which worktree corresponds to which branch
```

### 2. One Worktree Per Wave

```bash
# Good: One worktree per research wave
worktrees/
├── critical/      # All critical research
├── high/          # All high priority
└── medium/        # All medium priority

# Avoid: One worktree per agent (too many!)
worktrees/
├── atlas/
├── nova/
├── cipher/
└── sage/         # Don't do this!
```

### 3. Clean Up After Merging

```bash
# After merging to main
git merge --no-ff research/critical-path

# Remove worktree
git worktree remove worktrees/critical/

# Optionally delete branch
git branch -d research/critical-path

# Keep git clean!
```

### 4. Don't Nest Worktrees

```bash
# Bad: Worktree inside worktree
research-project/
└── worktrees/
    └── critical/
        └── worktrees/    # Don't do this!
            └── nested/

# Good: Flat structure
research-project/
├── worktrees/
    ├── critical/
    └── high/
```

### 5. Use Absolute Paths for Scripts

```bash
# When scripting, use absolute paths
MAIN_REPO=$(git rev-parse --show-toplevel)

# Then reference worktrees
cd "$MAIN_REPO/worktrees/critical/"
```

---

## Integration with Research Orchestrator

### Conductor's Worktree Strategy

```markdown
# Conductor's Workflow

## Phase 1: Setup

I'll create a worktree for each research wave:

```bash
git worktree add worktrees/wave1-critical research/wave1-critical
git worktree add worktrees/wave2-high research/wave2-high
```

## Phase 2: Research

Each wave works in its own worktree:
- 🟤 Atlas → worktrees/wave1-critical/
- 🟢 Nova → worktrees/wave1-critical/
- 🔴 Cipher → worktrees/wave2-high/
- 🟡 Sage → worktrees/wave2-high/

## Phase 3: Merge

After each wave completes:

```bash
cd main-worktree/
git merge --no-ff research/wave1-critical
```

## Phase 4: Cleanup

```bash
git worktree remove worktrees/wave1-critical/
git branch -d research/wave1-critical
```
```

---

## Common Scenarios

### Scenario 1: Agent Needs to See Previous Wave's Work

```bash
# Wave 1 completed and merged to main
git merge --no-ff research/wave1-critical

# Create Wave 2 worktree from updated main
git worktree add worktrees/wave2-high research/wave2-high

# Wave 2 agents automatically have Wave 1's findings!
cd worktrees/wave2-high/
ls research-findings/
# Shows all previous findings
```

### Scenario 2: Parallel Independent Waves

```bash
# Create all worktrees at once
git worktree add worktrees/hardware research/hardware
git worktree add worktrees/software research/software
git worktree add worktrees/security research/security

# Each is completely independent
# Can work on all simultaneously
# Merge in any order later
```

### Scenario 3: Hotfix During Research

```bash
# Research in progress in worktrees
cd worktrees/wave2-high/

# Need to make urgent fix in main
cd ../../  # Back to main worktree

# Create hotfix worktree
git worktree add worktrees/hotfix hotfix/urgent-fix

cd worktrees/hotfix/
# Make fix
git commit -m "fix: urgent issue"

cd ../../
git merge --no-ff hotfix/urgent-fix

# Research continues unaffected!
```

### Scenario 4: Visualization in Separate Worktree

```bash
# Research complete, now create visualizations
git worktree add worktrees/visualization research/visualization

cd worktrees/visualization/

# Prism works here
mkdir -p research-viz/
python create_visualizations.py

git add research-viz/
git commit -m "feat: add research visualizations"

cd ../../
git merge --no-ff research/visualization
```

---

## Worktree vs. Branch vs. Clone

### Worktree

**Use for:**
- Parallel work on different features
- Research waves
- Independent experimentation

**Advantages:**
- Shares .git directory (efficient)
- Quick to create
- Proper git integration

**Disadvantages:**
- Can't checkout same branch twice
- Requires git 2.5+

### Branch (Traditional)

**Use for:**
- Simple sequential work
- Single person, single feature

**Advantages:**
- Simple, well-understood
- Works everywhere

**Disadvantages:**
- Requires branch switching
- Risk of uncommitted changes
- Can't work on multiple branches simultaneously

### Clone (Separate Repository)

**Use for:**
- Complete isolation
- Different teams
- Long-running forks

**Advantages:**
- Complete independence
- Can have same branch in multiple clones

**Disadvantages:**
- Duplicates entire .git (storage cost)
- Slower to create
- Harder to merge back

---

## Troubleshooting

### Error: "Branch is already checked out"

```bash
# You tried to checkout a branch that's active in another worktree
$ git checkout research/critical-path
fatal: 'research/critical-path' is already checked out at 'worktrees/critical'

# Solution: Go to that worktree instead
cd worktrees/critical/
```

### Error: "Worktree already exists"

```bash
# You tried to create a worktree that already exists
$ git worktree add worktrees/critical research/critical-path
fatal: 'worktrees/critical' already exists

# Solution: Remove old worktree first
git worktree remove worktrees/critical/
# Then create new one
git worktree add worktrees/critical research/critical-path
```

### Error: "Invalid path"

```bash
# Worktree path must not exist
$ git worktree add existing-directory/ research/branch
fatal: 'existing-directory' already exists

# Solution: Use non-existent path or remove directory
rmdir existing-directory/
git worktree add existing-directory/ research/branch
```

### Orphaned Worktrees

```bash
# List worktrees
git worktree list

# If some show as prunable:
# /path/to/worktree (prunable)

# Prune them
git worktree prune

# Or remove explicitly
git worktree remove <path>
```

---

## Performance Considerations

### Disk Usage

```bash
# Worktrees are efficient
Main repo:   .git/ (100MB)
Worktree 1:  .git file (tiny) + working files
Worktree 2:  .git file (tiny) + working files

# vs. Clones
Clone 1:     .git/ (100MB) + working files
Clone 2:     .git/ (100MB) + working files
```

**Result:** Worktrees use ~100MB total, clones use ~200MB

### Creation Speed

```bash
# Worktree creation
time git worktree add worktrees/test research/test
# Real: 0.05s

# Clone creation
time git clone . clones/test
# Real: 2.30s
```

**Result:** Worktrees are ~46Ã— faster to create

---

## Complete Example: Research Project

```bash
#!/bin/bash
# complete-research-workflow.sh

# Setup
echo "ðŸš€ Setting up research project with worktrees"

# Initialize main repository
git init
echo "# Research Project" > README.md
git add README.md
git commit -m "chore: initialize project"

# Create directory structure
mkdir -p research-findings research-viz tools

# Create permission init agent (Scout)
echo "✅ Scout: Setting up permissions..."
# ... Scout runs ...

# Create worktrees for each wave
echo "📁 Creating worktrees..."

git worktree add worktrees/wave1-critical research/wave1-critical
git worktree add worktrees/wave2-high research/wave2-high
git worktree add worktrees/wave3-medium research/wave3-medium
git worktree add worktrees/wave4-low research/wave4-low

# Wave 1: Critical Path
echo "🟤 Wave 1: Critical path research starting..."
cd worktrees/wave1-critical/

# Research agents work here
# Atlas, Nova, Cipher, Sage create documents

git add research-findings/
git commit -m "feat: complete critical path research

ðŸ¤– Generated with Claude Code
Co-Authored-By: Atlas, Nova, Cipher, Sage"

cd ../../

# Merge to main
git merge --no-ff research/wave1-critical -m "chore: merge critical path research"

echo "✅ Wave 1 complete and merged"

# Wave 2: High Priority
echo "🟢 Wave 2: High priority research starting..."
cd worktrees/wave2-high/

# More agents work
git add research-findings/
git commit -m "feat: complete high-priority research"

cd ../../

# Merge to main
git merge --no-ff research/wave2-high

echo "✅ Wave 2 complete and merged"

# Continue for remaining waves...

# Cleanup
echo "🧹 Cleaning up worktrees..."
git worktree remove worktrees/wave1-critical/
git worktree remove worktrees/wave2-high/
git worktree prune

echo "ðŸŽ‰ Research project complete!"
git log --oneline --graph --all
```

---

## Summary

**Use worktrees for research when:**
- âœ… Multiple research waves working in parallel
- âœ… Want to avoid branch switching
- âœ… Need isolated environments for each wave
- âœ… Want efficient disk usage
- âœ… Need to reference different branches simultaneously

**Use regular branches when:**
- Simple sequential research
- Single agent working alone
- No need for parallelization

**Worktrees give you:**
- Clean parallel workflows
- No branch switching chaos
- Efficient disk usage
- Proper git integration
- Professional repository organization

Start using worktrees for your next research project! ðŸš€

---

**Guide Version:** 1.0  
**Git Version Required:** 2.5+  
**Recommended for:** Parallel research projects with 3+ waves  
**Complexity:** Medium (worth learning!)
