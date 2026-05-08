# GIT_WORKFLOW.md

## Git Workflow for OpenSIS California Extension Project

**Project**: OpenSIS Classic - California TK–8 Extension  
**Repository**: GitHub (OS4ED/openSIS-Classic or your fork)  
**Primary Branch**: `ca-extension` (project-specific branch)

---

## Branch Strategy

### Main Branches

#### `main`
- **Purpose**: Production-ready code (after successful pilot)
- **Protection**: Requires PR review before merge
- **Who merges**: Project lead only
- **Merge source**: `release/` or `hotfix/` branches

#### `ca-extension` (PROJECT BRANCH)
- **Purpose**: Integration branch for California extension work
- **Protection**: Requires PR review before merge
- **Who merges**: Tech lead reviews and merges
- **Merge source**: Feature branches (`feature/phase-*`)
- **Current**: Where Phase 1–13 development happens

### Supporting Branches

#### Feature Branches: `feature/phase-{X}-{name}`

**Purpose**: Develop a specific phase

**Naming Convention**:
```
feature/phase-1-baseline
feature/phase-2-schema
feature/phase-3-code-sets
feature/phase-4-demographics
... etc
```

**Creation**:
```bash
git checkout ca-extension
git pull origin ca-extension
git checkout -b feature/phase-1-baseline
```

**Work**:
1. Make code changes
2. Commit regularly with semantic messages
3. Push to remote: `git push -u origin feature/phase-1-baseline`

**Merge Back**:
1. Create Pull Request (PR) from `feature/phase-1-baseline` → `ca-extension`
2. Get code review
3. Address feedback
4. Merge when approved

**After Merge**:
```bash
git checkout ca-extension
git pull origin ca-extension
git branch -d feature/phase-1-baseline  # Delete local
git push origin --delete feature/phase-1-baseline  # Delete remote
```

#### Hotfix Branches: `hotfix/{issue-name}`

**Purpose**: Urgent bug fixes in production code

**Naming**:
```
hotfix/critical-enrollment-bug
hotfix/data-validation-crash
```

**Creation** (only after Phase 13 pilot starts):
```bash
git checkout main
git checkout -b hotfix/urgent-fix-name
```

**Merge to**: `main` AND `ca-extension` (if still in development)

---

## Commit Message Convention

### Format

```
[Phase X] Component: Brief description

Detailed explanation of what changed and why.

Fixes: #123 (if closing an issue)
Related: Context.MD Section X.Y (if relevant)
Test: Describe manual test performed
```

### Examples

**Good**:
```
[Phase 2] Database: Create ca_student_profile table

Added California extension table for student demographics with:
- Effective dating for history tracking
- Foreign key to students table
- CALPADS-aligned fields

Migration script: migrate_phase2_up.sql
Rollback: migrate_phase2_down.sql

Test: Schema created successfully, FK constraints enforced
```

**Good**:
```
[Phase 4] Student UI: Add California demographic fields

Extended student form (/modules/students/Student.php) to include:
- Primary language (code set lookup)
- Gender code (CA-specific)
- Birth country/state
- Parent education level

Added functions in CAStudentFnc.php for profile sync.

Test: Form renders, saves to ca_student_profile, validates codes
Related: Context.MD Section 4.1
```

**Bad**:
```
updated student stuff
```

**Bad**:
```
fixed bug
```

### Commit Size

- **Too Small**: Avoid micro-commits like "add semicolon"
- **Too Large**: Avoid 100+ file changes in one commit
- **Right Size**: One logical feature or task per commit

**Example sequence**:
```
[Phase 4] Database: Create ca_student_demographic_history table
[Phase 4] Student UI: Add demographic form fields
[Phase 4] Functions: Add validation for demographic codes
[Phase 4] Tests: Add test cases for demographic workflows
```

---

## Pull Request (PR) Workflow

### Before Creating PR

1. **Sync with main branch**:
   ```bash
   git checkout ca-extension
   git pull origin ca-extension
   git rebase ca-extension  # Rebase feature branch on latest
   ```

2. **Run local tests**:
   ```bash
   # Run any linting/testing
   php -l /path/to/modified/files.php
   # Manual testing in browser
   ```

3. **Check commit history**:
   ```bash
   git log --oneline origin/ca-extension..HEAD
   # Should see 3–10 logical commits, not 50+ small ones
   ```

### Create PR

1. **Push feature branch**:
   ```bash
   git push -u origin feature/phase-1-baseline
   ```

2. **Create PR on GitHub**:
   - Title: `[Phase 1] Install & Baseline - Setup OpenSIS and Document Schema`
   - Description:
     ```markdown
     ## Phase 1: Install & Baseline

     ### Changes
     - Installed OpenSIS Classic Community Edition
     - Created sample TK–8 data
     - Documented baseline schema

     ### Test Cases
     - [x] OpenSIS login works
     - [x] Students module accessible
     - [x] Sample students created
     - [x] Database schema verified

     ### Deliverables
     - SCHEMA_BASELINE.md
     - WORKFLOW_BASELINE.md
     - test_data_phase1.sql

     ### Related
     Closes: #123 (if applicable)
     Related: PROJECT_PLAN.md Phase 1
     ```

   - Link to `PROJECT_PLAN.md` Phase 1 section

3. **Request reviewers**:
   - Tech lead
   - 1–2 peer developers

### Review Process

**Reviewer**:
1. Read PR description and context
2. Review code changes
3. Check against success criteria from `PROJECT_PLAN.md`
4. Run tests locally if possible
5. Comment with feedback or approve

**Author**:
1. Read feedback carefully
2. Make requested changes
3. Commit and push (PR updates automatically)
4. Reply to comments when addressed
5. Request re-review when done

### Merge PR

When approved:
```bash
# Reviewer merges on GitHub (or author if approved)
# Choose "Squash and merge" for cleaner history
# Delete branch after merge (GitHub offers this)
```

---

## Common Workflows

### Start a New Phase

```bash
# 1. Ensure ca-extension is up to date
git checkout ca-extension
git pull origin ca-extension

# 2. Create feature branch for this phase
git checkout -b feature/phase-X-name

# 3. Make changes, commit regularly
git add .
git commit -m "[Phase X] Component: Description"

# 4. When ready, push and create PR
git push -u origin feature/phase-X-name
# Create PR on GitHub
```

### Sync with Latest Changes

```bash
# Someone else finished a phase and it's merged
git checkout ca-extension
git pull origin ca-extension

# If you're on a feature branch, rebase on latest
git rebase ca-extension
# Or merge if prefer: git merge ca-extension
```

### Undo Last Commit (Before Push)

```bash
# Undo last commit, keep changes
git reset --soft HEAD~1

# Re-commit with better message
git commit -m "[Phase X] Better message"
```

### Fix Commit Message (Before Push)

```bash
# Amend last commit
git commit --amend -m "Better message"
git push  # Force push to feature branch if already pushed
```

### Discard Changes (Be Careful!)

```bash
# Discard changes in working directory
git checkout -- filename.php

# Discard all changes in current branch
git reset --hard HEAD
```

---

## Version Tags

### Tag Naming

Tags mark phase completion:

```
v0.1.0-phase-1          Phase 1 complete (Install & Baseline)
v0.2.0-phase-2          Phase 2 complete (Data Model)
v0.3.0-phase-3          Phase 3 complete (Code Sets)
...
v1.0.0-phase-13         Phase 13 complete (Production Ready)
```

### Create Tag

```bash
# After phase PR is merged to ca-extension
git checkout ca-extension
git pull origin ca-extension

# Create tag
git tag -a v0.1.0-phase-1 -m "Phase 1: Install & Baseline complete"

# Push tag
git push origin v0.1.0-phase-1

# List tags
git tag -l
```

### View Tag

```bash
# Show tag details
git show v0.1.0-phase-1

# Checkout code at tag (detached HEAD)
git checkout v0.1.0-phase-1
```

---

## Handling Merge Conflicts

If `ca-extension` changes while you're working on a feature:

```bash
# 1. Fetch latest
git fetch origin

# 2. Rebase feature on ca-extension (cleaner history)
git rebase origin/ca-extension

# 3. If conflicts occur, resolve them
# Edit conflicted files, remove conflict markers

# 4. Stage resolved files
git add resolved_file.php

# 5. Continue rebase
git rebase --continue

# 6. Or abort if too messy
git rebase --abort

# Alternative: Merge instead of rebase
git merge origin/ca-extension
# Resolve conflicts, commit
```

---

## Local vs. Remote

### Local Branches

View local branches:
```bash
git branch
```

Create local branch:
```bash
git checkout -b feature/phase-1-baseline
```

Delete local branch:
```bash
git branch -d feature/phase-1-baseline
```

### Remote Branches

View remote branches:
```bash
git branch -r
# or
git branch -a  # all branches
```

Track remote branch locally:
```bash
git checkout --track origin/ca-extension
# or
git checkout -b ca-extension origin/ca-extension
```

Delete remote branch:
```bash
git push origin --delete feature/phase-1-baseline
```

### Fetch vs. Pull

```bash
git fetch origin
# Downloads new branches/commits, doesn't change your work

git pull origin ca-extension
# Fetch + merge (equivalent to fetch + merge HEAD)

git pull --rebase origin ca-extension
# Fetch + rebase (cleaner history, use this)
```

---

## Code Review Checklist

Use this when reviewing PRs:

### Before Approving

- [ ] PR description is clear and links to PROJECT_PLAN.md
- [ ] Commits are logical and well-messaged
- [ ] Code follows project conventions (see CLAUDE.md)
- [ ] No obvious bugs or security issues
- [ ] Test cases from PROJECT_PLAN.md are addressed
- [ ] Documentation updated (if needed)
- [ ] No hardcoded credentials or sensitive data
- [ ] No commented-out code left behind
- [ ] Follows OpenSIS architecture (modules, functions, database)

### For Database Changes

- [ ] Migration scripts provided (up and down)
- [ ] Foreign keys properly configured
- [ ] No modifications to OpenSIS core tables (extension layer only)
- [ ] Code set mappings in place if needed

### For UI Changes

- [ ] Form validation present
- [ ] CSRF token in POST forms
- [ ] Error messages user-friendly
- [ ] Follows existing UI patterns in OpenSIS

---

## Stashing Changes (Temporary Save)

If you need to switch branches with uncommitted changes:

```bash
# Save changes temporarily
git stash

# Switch branches
git checkout other-branch

# Later, restore changes
git checkout original-branch
git stash pop
```

---

## Useful Commands Reference

```bash
# View commit history
git log --oneline --graph --all

# See what changed in last commit
git show HEAD

# See what changed in a file
git diff filename.php

# See who changed what line
git blame filename.php

# Search commit messages
git log --grep="Phase 1"

# See branches merged to ca-extension
git branch -a --merged ca-extension

# See branches NOT merged
git branch -a --no-merged ca-extension

# Revert a commit (create new commit undoing it)
git revert abc123def

# Interactive rebase (edit/reorder commits)
git rebase -i HEAD~5  # Last 5 commits
```

---

## Best Practices

1. **Commit Early, Commit Often**: Small, logical commits are easier to review and fix
2. **Write Good Messages**: Future you will thank present you
3. **Pull Before Push**: `git pull` before `git push` to avoid conflicts
4. **Feature Branches**: Keep `ca-extension` clean, work in feature branches
5. **No Force Push to Shared Branches**: Only force push to your own feature branches
6. **Review Your Own PR First**: Read diff before requesting review
7. **Sync With Latest**: Rebase feature on `ca-extension` before requesting merge
8. **Tag Phases**: Create tags when phases complete
9. **Document Changes**: Update PROJECT_PLAN.md and docs as you work
10. **Backup Regularly**: `git push` frequently

---

## Troubleshooting Git

### "fatal: not a git repository"

```bash
cd /path/to/opensis-dzsis
git status  # Should work now
```

### "rejected: updates were rejected"

```bash
# Someone pushed after you last pulled
git pull origin ca-extension
git push origin feature/phase-X
```

### "detached HEAD state"

```bash
# You checked out a commit instead of a branch
git checkout ca-extension  # Get back to a branch
```

### Lost Work / Want to Undo

```bash
# See recent actions
git reflog

# Recover lost commit
git checkout abc123def
```

---

## Continuous Integration (Optional)

When automated testing is set up:

- **GitHub Actions** can run tests on every PR
- Tests must pass before merge
- Configure in `.github/workflows/`

For now (Phase 1), tests are manual. See PROJECT_PLAN.md test cases.

---

## Phase-by-Phase Git Checklist

For each phase:

- [ ] Create feature branch: `feature/phase-X-name`
- [ ] Commit changes with semantic messages
- [ ] Push branch and create PR
- [ ] Get review, address feedback
- [ ] Merge to `ca-extension`
- [ ] Create tag: `v0.X.0-phase-X`
- [ ] Delete feature branch
- [ ] Update PROJECT_PLAN.md completion status
- [ ] Create PR from `ca-extension` to `main` (Phase 13 only)

---

## Summary

- **Main work**: Feature branches off `ca-extension`
- **Good commits**: Small, logical, well-messaged
- **Code review**: Every PR reviewed before merge
- **Tags**: Mark phase completion
- **Backup**: Push to remote frequently

---

**Last Updated**: 2026-05-08  
**Status**: Ready for Phase 1 Development
