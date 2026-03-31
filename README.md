# Task 3: Undoing Changes in Git

## Objective

Learn how to safely undo changes in Git using different approaches:

- Undo changes in a tracked file before committing
- Undo commits using revert and reset strategies

## Setup Instructions

This task is part of the git-fundamentals project. Ensure you have:

- Git installed on your system
- A local git repository initialized in this directory
- Basic knowledge of git commits and tracked files

## Exercise Overview

This task covers three key scenarios for undoing work in Git:

### 1. Undoing Tracked File Changes (Before Commit)

**Scenario:** You've modified a tracked file but haven't committed yet and want to discard changes.

**Methods:**

#### `git checkout -- <file>`

```bash
git checkout -- styles.css
```

- Discards all changes to the specified file in the working directory
- Restores the file to the version in the staging area or HEAD commit
- Works for single files or multiple files
- Legacy command (still widely used)

#### `git restore <file>` (Recommended)

```bash
git restore styles.css
```

- Modern replacement for `git checkout -- <file>`
- Cleaner and more intuitive syntax
- Better for restoring files specifically
- Available in Git 2.23 and later

### 2. Undoing Commits

#### `git revert <commit>`

```bash
git revert HEAD
```

- Creates a **new commit** that undoes the changes from a previous commit
- Preserves commit history (non-destructive)
- Safe to use on shared/public branches
- Best practice: Use revert for published commits
- Example: If commit ABC added a file, revert creates commit XYZ that removes it

#### `git reset <commit>`

```bash
git reset --soft HEAD~1    # Keep changes, unstage them
git reset --mixed HEAD~1   # Keep changes in working directory
git reset --hard HEAD~1    # Discard all changes
```

- Moves the current branch to a previous commit
- Modifies commit history (destructive)
- Three variants:
  - `--soft`: Keeps changes in staging area
  - `--mixed` (default): Keeps changes in working directory
  - `--hard`: Discards all changes completely
- Should only be used on local branches, not shared history

## Key Differences: Revert vs Reset

| Aspect              | `git revert`                     | `git reset`                    |
| ------------------- | -------------------------------- | ------------------------------ |
| **Safety**          | Safe - creates new commit        | Destructive - rewrites history |
| **History**         | Preserves all history            | Removes commits from history   |
| **Shared Branches** | ✅ Safe to use                   | ❌ Avoid if pushed             |
| **Use Case**        | Undo published commits           | Undo local commits             |
| **Command Type**    | Non-destructive                  | Destructive                    |
| **Result**          | New commit with opposite changes | Branch pointer moves back      |

## Checkout vs Restore

| Aspect             | `git checkout`                   | `git restore`                   |
| ------------------ | -------------------------------- | ------------------------------- |
| **Purpose**        | Switch branches & restore files  | Restore files only              |
| **Syntax**         | `git checkout -- <file>`         | `git restore <file>`            |
| **Clarity**        | Can be confusing (multi-purpose) | Clear intent (file restoration) |
| **Version**        | Available in all Git versions    | Git 2.23+                       |
| **Recommendation** | Legacy, still works              | Modern choice                   |

## Best Practices

1. **For uncommitted changes:** Use `git restore` or `git checkout --`
2. **For committed changes on local branches:** Use `git reset`
3. **For committed changes on shared branches:** Use `git revert`
4. **Before destructive operations:** Check `git status` and ensure you have backups
5. **Use `git log`** to review commit history before undoing
6. **Use `git reflog`** as a safety net - it shows all HEAD movements even after reset

## Practical Workflow

```bash
# 1. Make changes to a tracked file
echo "new content" >> styles.css

# 2. Check status
git status

# 3. Undo the changes
git restore styles.css
# OR
git checkout -- styles.css

# 4. Make and commit changes
echo "final content" >> styles.css
git add styles.css
git commit -m "Add content to styles"

# 5. Undo the commit safely (creates new commit)
git revert HEAD

# 6. For local-only commits, reset instead
git reset --hard HEAD~1
```

## Resources

- [Git restore documentation](https://git-scm.com/docs/git-restore)
- [Git revert documentation](https://git-scm.com/docs/git-revert)
- [Git reset documentation](https://git-scm.com/docs/git-reset)
- [Git reflog documentation](https://git-scm.com/docs/git-reflog)

## Summary

Understanding how to undo changes safely is crucial for effective version control. Remember:

- **git restore/checkout**: For uncommitted work
- **git revert**: For published commits (collaborative work)
- **git reset**: For local-only commits (solo work)
