---
description: Branch, build, commit, and PR — full feature lifecycle
argument-hint: <feature-description>
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit]
---

# Git Feature Workflow

Manage the full lifecycle of a feature: branch → implement → commit → push → PR.

## Arguments

The user invoked this command with: $ARGUMENTS

## Instructions

### Step 1: Detect current state

Run `git status` and `git branch --show-current` to determine where we are in the lifecycle:

- **On `main` (or default branch):** This is a new feature — go to Step 2.
- **On a feature branch with no changes:** The previous iteration was committed — go to Step 3.
- **On a feature branch with uncommitted changes:** There's work in progress — go to Step 4.

### Step 2: Create a feature branch (new feature only)

1. Ensure `main` is up to date: `git pull origin main`
2. Derive a branch name from `$ARGUMENTS` using the format `feature/<short-slug>` (lowercase, hyphens, max 50 chars). Examples:
   - "add user settings page" → `feature/add-user-settings-page`
   - "fix login timeout bug" → `fix/login-timeout-bug`
   - "refactor auth module" → `refactor/auth-module`
3. Create and switch to the branch: `git checkout -b <branch-name>`
4. Tell the user the branch is ready and ask what to build.

### Step 3: Implement the feature

Work on what `$ARGUMENTS` describes (or what the user asks for if the branch already exists).

- Explore the project structure first
- Follow existing conventions
- Make incremental, focused changes

### Step 4: Commit and push

After completing a unit of work:

1. Run `git status` and `git diff --stat` to review changes.
2. Stage relevant files individually (never use `git add -A` or `git add .`). Avoid staging secrets or generated files.
3. Write a clear commit message following conventional commits (`feat:`, `fix:`, `refactor:`, `docs:`, `chore:`). Use a HEREDOC for the message.
4. Push the branch: `git push -u origin <branch-name>`

### Step 5: Create or update the PR

1. Check if a PR already exists for this branch: `gh pr list --head <branch-name>`
2. **If no PR exists:** Create one:
   ```
   gh pr create --title "<concise title>" --body "$(cat <<'EOF'
   ## Summary
   <bullet points describing what changed>

   ## Test plan
   - [ ] <testing steps>

   🤖 Generated with [Claude Code](https://claude.com/claude-code)
   EOF
   )"
   ```
3. **If a PR already exists:** The push already updated it. Optionally add a comment summarizing the new changes:
   ```
   gh pr comment <pr-number> --body "Updated: <brief summary of new changes>"
   ```
4. Return the PR URL to the user.

### Step 6: Next iteration

Tell the user:
- What was committed and pushed
- The PR URL
- That they can run `/git-feature` again to continue working on the same branch, or start a new feature by checking out `main` first
