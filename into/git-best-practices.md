# Git Best Practices

### ✅ 1. Git Workflow
Always use the `dev` or `development` branch as the base.

**Directorist**
```
development (or dev)
└─── feature/branch-name → Pull Request → development
```

**Directorist Extensions**
```
development (or dev)
└─── feature/branch-name → Pull Request → development
```

**HelpGent**
```
development (or dev)
└─── feature/branch-name → Pull Request → development
```

**FormGent**
```
development (or dev)
└─── feature/branch-name → Pull Request → development
```

### ✅ 2. Use Feature Branches

- Create a new branch for every task, bug fix, or feature.
- Name branches clearly: `feature/webhook-integration`, `fix/search-bar-broken-number-field`.

```bash
git checkout -b feature/improve-csv-import
```

### ✅ 3. Pull Before You Push
Always pull the latest changes from the base branch before you push your branch or open a PR.

```bash
git checkout dev # dev or development
git pull origin dev
git checkout feature/my-work
git merge dev   # or rebase, rebase gives cleaner history btw
```

### ✅ 4. Resolve Conflicts Immediately and Carefully
When conflicts happen:

- Use visual merge tools like VS Code’s Git interface or `git mergetool`.
- Discuss with the author if unsure.
- Re-test thoroughly after resolving.

### ✅ 5. Code Review Before Merging
Use Pull Requests (PRs) and require:

- At least one code review approval.
- CI/CD checks to pass (like unit tests or PHPCS/WPCS linting).
- Clear PR descriptions: **what**, **why**, **screenshots** (if UI), **test instructions**.

### ✅ 6. Keep Commits Small & Meaningful
One commit = one logical change.

Use clear commit messages:

```
fix(api): prevent timeout on bulk import
feat(block): add color picker for listing header
```

Please do not write commit messages like update, fix, wip etc.

### ✅ 7. Avoid Committing Generated or Built Files
Add `node_modules`, `vendor`, `.env`, `.DS_Store`, and any build files to `.gitignore`.

### ✅ 8. Use `.gitattributes` for Merge Strategy
Prevent conflicts in files like `composer.lock` or `package-lock.json`:

```
*.lock merge=ours
*.min.js merge=ours
*.min.css merge=ours
```

### ✅ 9. Run `git status` and `git diff` Often
Before every commit:

```
git status
git diff
```

This helps you know what you're committing and catch mistakes early.

### ✅ 10. Automate with Pre-Commit Hooks
Use tools like Husky to auto-run:

- Linting (`phpcs`, `eslint`)
- Tests
- Formatting (`phpcbf`, `prettier`)

This helps keep the codebase clean.

### ✅ 11. Periodically Rebase to Avoid Long-Lived Branch Conflicts
If your feature branch stays open for days:

```
git fetch origin
git rebase origin/main
```

This keeps it up-to-date and avoids massive conflicts during PR merge. `rebase` is a powerful tool so use it wisely.


### ✅ 12. Tag Your Releases
Tag stable versions:

```
git tag -a v2.1.0 -m "Add AI-powered form generator"
git push origin v2.1.0
```

### ✅ 13. Use GitHub Settings to Enforce Rules
Require PR reviews before `merge`.

- Require status checks (e.g., WPCS, unit tests).
- Protect `dev` or `master` from direct pushes.

---

### 🏋️‍♀️ Team Habits Matter More Than Tools
- Do frequent small merges.
- Must communicate on bigger changes.
