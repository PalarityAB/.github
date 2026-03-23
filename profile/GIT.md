# Git Cheat Sheet

## Status & Inspect
```bash
git status           # Show modified/staged/untracked files
git diff             # Show unstaged changes
git diff --staged    # Show staged changes
```

## Add / Stage
```bash
git add image.png                 # Stage specific file
git add .                         # Stage everything (careful)
git restore --staged image.png    # Unstage specific file
```

## Stash (Temporary Save)
Use when switching branches or pulling.

```bash
git stash                    # Stash tracked changes
git stash -u                 # Include untracked files
git stash list               # Show stashes
git stash pop                # Apply + remove latest stash
git stash apply              # Apply but keep stash
git stash drop               # Delete latest stash
```

Recommended safe usage:
```bash
git stash -u
git pull
git stash pop
```

Pitfall: `stash pop` may create conflicts.

## Checkout / Switch Branch
```bash
git checkout branch_name
git switch branch_name            # preferred (newer git)
```

## Pull (Always Rebase)
Avoid merge commits `git pull --rebase`

If conflicts:
```bash
# fix files
git add .
git rebase --continue
```

Abort rebase:
```bash
git rebase --abort
```

## Push
```bash
git push
git push origin branch
```

Set upstream (first push):
```bash
git push -u origin branch
```

## Force Push (SAFE WAY)
Only use when rebasing your own branch `git push --force-with-lease`

Why NOT `--force`?
- `--force` overwrites others work
- `--force-with-lease` checks remote first (safe)

Rule:
- OK: your feature branch
- NOT OK: main / develop / shared branches

## Undo
Undo last commit (keep changes): `git reset --soft HEAD~1`

Undo last commit (unstage changes): `git reset HEAD~1`

Discard everything (danger): `git reset --hard`

## Common Safe Workflow

### Update branch
```bash
git status
git stash -u
git pull --rebase
git stash pop
```

### Commit work
```bash
git add <files>
git commit -m "message"
git push
```

### Rebase feature branch
```bash
git fetch origin
git rebase origin/main
git push --force-with-lease
```

## Common Pitfalls

### "I can't switch branch"
You have local changes - `git stash -u`

### "Godot changed 100 files"
You committed `.import/` or `.godot/` - `git reset HEAD~1`
Also, add any temp files to `.gitignore`

### "stash pop broke everything"
First do `git reset --hard`, then `git stash apply`

### "force push rejected"
Someone pushed before you.
```bash
git pull --rebase
git push --force-with-lease
```

# Golden Rules

- Always `git status` first
- Prefer `git add -p`
- Use `git pull --rebase`
- Never `push --force` (use `--force-with-lease`)
- Never commit `.godot` or `.import`
- Stash before risky operations

# One‑line Lifesavers

Update safely: `git stash -u && git pull --rebase && git stash pop`

Fix wrong commit: `git reset --soft HEAD~1`

Rebase + safe force push: `git fetch origin && git rebase origin/main && git push --force-with-lease`
