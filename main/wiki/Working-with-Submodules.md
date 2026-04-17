---
title: "Working with Submodules in Git"
type: "Deep Dive"
source: "https://github.blog/open-source/git/working-with-submodules/"
created: 2026-04-13
---

# Working with Submodules in Git

## 🧩 Executive Synthesis
Git submodules enable embedding one or more repositories as sub-folders within another repository, facilitating dependency management. This comprehensive guide covers adding submodules, joining projects with them, converting existing folders to submodules, advice on usage (including alternatives), common commands, best practices, and troubleshooting. It emphasizes balancing consistency and convenience, preferring consistency for static dependencies while noting challenges for dynamic ones.

## 🗂 Detailed Breakdown (Proportional to Source)
### Overview
Submodules allow including or embedding repositories as sub-folders inside another, useful for projects depending on external libraries or frameworks. While effective for simple cases, alternatives may be better for complex dependencies.

### Adding a Submodule
Example: Adding 'rock' to 'slingshot' project.
- Command: `git submodule add https://github.com/<user>/rock rock`
- This creates a 'rock' folder in 'slingshot'.
- If empty (older Git versions): `git submodule update --init --recursive`
- Commit the change.
- On GitHub, the folder shows as a submodule; clicking navigates to its repo.
- Commands in parent folders affect the parent repo; inside submodule, they affect the submodule.

### Joining a Project Using Submodules
- Clone with: `git clone --recursive <project url>` to fetch everything.
- If not: Run `git submodule update --init --recursive` post-clone.
- Without update, submodule folders appear empty.

### Switching to Submodules
Example: Extract 'rubber-band' from 'slingshot' to standalone repo and add as submodule.
1. Copy repo: `cp -r slingshot rubber-band`
2. Filter history: `git filter-branch --subdirectory-filter rubber-band -- --all`
3. Create new GitHub repo for 'rubber-band'.
4. Update remote: `git remote set-url origin https://github.com/<user>/rubber-band`
5. Push: `git push`
6. In original 'slingshot': `git rm -r rubber-band` and commit.
7. Add submodule: `git submodule add https://github.com/<user>/rubber-band rubber-band` and commit.
8. Update: `git submodule update --init --recursive`
- History is preserved, but can cause confusion due to duplicated commits.
- Remind collaborators to update submodules.
- Repeat for other projects (e.g., adding to 'magic roll-back can').

### Advice on Using Submodules (or Not)
- Alternatives: Go's built-in system, RubyGems, npm, CocoaPods, Carthage, Bower.
- Git doesn't download submodules by default; require explicit updates, including in scripts/automation.
- Balances consistency (lock to SHA) vs. convenience; favors consistency, complicating contributions back.
- Updates not automatic; remind colleagues.
- Frustrating for dynamic/evolving repos; focused on simple, static relationships.
- Future post (per source) on complex workflows.

### Further Reading
- [Pro Git: Git Tools – Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- [`git filter-branch` reference](https://git-scm.com/docs/git-filter-branch)

## 🛠 Technical Implementation / Skills
### Common Commands
- Pull with submodules: `git pull --recurse-submodules`
- Update to latest: `git submodule update --remote --merge`
- Add: `git submodule add <repository-url> <path>` then commit.
- Remove: 
  1. `git submodule deinit -f <path>`
  2. `rm -rf .git/modules/<path>`
  3. `git rm --cached <path>`
  4. Commit.
- Foreach: `git submodule foreach git pull origin main`
- Sync: `git submodule sync --recursive` then update.

### Best Practices
- Use `--recurse-submodules` for clone/pull.
- Commit/push in submodule first, then update parent.
- Create branches in submodules to avoid detached HEAD.

### Troubleshooting
- Detached HEAD: `git checkout -b <branch-name>`
- Permissions: Ensure access.
- Sync issues: `git submodule sync --recursive` then update.

## 🔗 Knowledge Graph Connections
- **Context**: [[Git Version Control]]
- **Related Topics**: [[Git Filter-Branch]], [[Dependency Management]], [[Repository Embedding]], [[OpenClaw Setup]]
