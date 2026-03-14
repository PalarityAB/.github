# 🔁 Ticket Workflow Guide

This guide walks you through how we handle tickets from start to finish at PalarityAB. Follow these steps every time you pick up an issue.

> **Running example used throughout this guide:**
> `#42 – Block-breaking animation stutters on rapid hits` (Type: Bug)

---

## 1. Claim the ticket

Open the issue and click **"Assign yourself"** under the **Assignee** section. This signals to the team that you're actively working on it and prevents duplicate effort.

---

## 2. Set your status

Under the **Projects** section in the issue sidebar, change the status to **In progress**. This keeps the board up to date so everyone knows what's being worked on.

---

## 3. Create a branch

Scroll down to **Development** on the issue page and click **"Create a branch"**. GitHub will suggest a branch name — prepend it with the correct prefix based on the ticket **Type**:

| Type | Prefix |
|------|--------|
| Bug | `bug/` |
| Feature | `feat/` |

> **Example:** `bug/42-block-breaking-animation-stutter`

---

## 4. Check out the branch locally

Pull the latest changes, then switch to your new branch:

```bash
git pull
git checkout bug/42-block-breaking-animation-stutter
```

---

## 5. Commit your changes

Once you're happy with your solution, stage and commit your files.

### Grouping changes into commits

Each commit should represent **one coherent, atomic unit of work**. When in doubt, ask yourself: *"Could I describe this change in one sentence without using 'and'?"*

**Keep in one commit:**
- Files that implement the same fix or feature
- A source file and its corresponding test
- `package.json` and its lock file

**Split into separate commits when:**
- Changes address unrelated concerns (e.g. a bug fix and a new feature)
- Formatting/style changes are mixed with functional changes
- Documentation updates are unrelated to the code changes

### Writing the commit message

We follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) standard.

**Format:**
```
<type>[optional scope]: <description>

[optional body]

[optional footer]
```

**Types:**

| Type | When to use |
|------|-------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only |
| `refactor` | Code change that is neither a fix nor a feature |
| `style` | Formatting, whitespace — no logic change |
| `test` | Adding or updating tests |
| `chore` | Build process, dependencies, tooling |
| `ci` | CI/CD configuration |
| `perf` | Performance improvement |

**Rules:**
- Description: imperative mood, lowercase, no trailing period, under 72 characters
- Add a body (separated by a blank line) when the *why* isn't obvious
- Reference the issue in the footer with `Refs: #<issue-number>`
- Breaking changes: add `!` before `:` and/or a `BREAKING CHANGE:` footer

**Examples:**
```bash
# Simple fix
git commit -m "fix(gameplay): prevent stutter on rapid block hits (#42)"

# With body and footer
git commit -m "fix(gameplay): prevent stutter on rapid block hits" \
            -m "The animation queue was not cleared between hits, causing overlapping calls. Added a debounce guard before queuing the break animation." \
            -m "Refs: #42"

# Breaking change
git commit -m "feat(api)!: rename player health endpoint" \
            -m "BREAKING CHANGE: /player/hp is now /player/health"
```

### Using the smart-commit Claude command

If you have a Claude subscription, you can use the `/smart-commit` command in Claude Code. It will analyze all your uncommitted changes, group them into logical commits automatically, and ask for your confirmation before committing.

Teammates without a subscription should follow the grouping and message guidelines above manually.

---

## 6. Push your branch

> ⚠️ **Important:** Once you push, changes are visible to the team and it becomes harder to revert. Make sure you're happy with everything locally first.

```bash
git push
```

---

## 7. Open a pull request

On GitHub, click **"New pull request"**. The PR title must follow the same Conventional Commits format as your commit message.

In the description, explain what you changed and how you solved it. Then:

1. Move the ticket status to **In review**
2. Add at least one **reviewer**
3. Wait for approval, then **merge to main** 🎉

> **Example PR title:** `fix(gameplay): prevent stutter on rapid block hits (#42)`
>
> See [PR #2](https://github.com/PalarityAB/LevelShift/pull/2) for a real example of the expected format.

---

## Summary

```
Claim issue → In progress → Create branch → Checkout → Commit → Push → PR → Merge
```
