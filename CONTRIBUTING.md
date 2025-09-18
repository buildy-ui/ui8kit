# Contributing

Thanks for your interest in contributing! This project is organized as a Bun + Turbo monorepo. Beginners are welcome. This guide explains how to contribute via GitHub’s web interface or the command line, and how to comply with our sign‑off policy (DCO).

## Quick start (recommended flow)

1. Fork this repository to your GitHub account.
2. Clone your fork locally and initialize submodules:
   ```bash
   git clone <your-fork-url>
   cd <repo>
   git submodule update --init --recursive
   ```
3. Create a new branch for your change:
   ```bash
   git checkout -b feat/my-change
   ```
4. Make edits. If you run locally, install and start dev:
   ```bash
   bun install
   bun run dev
   ```
5. Commit with sign‑off (required):
   ```bash
   git add -A
   git commit -s -m "feat: describe your change"
   git push -u origin feat/my-change
   ```
6. Open a Pull Request from your branch to this repository’s default branch.

## Sign‑off policy (DCO) — what and why

We require a Developer Certificate of Origin (DCO) sign‑off on commits made through GitHub’s web UI, and we strongly encourage it for all commits. The sign‑off line looks like:

```
Signed-off-by: Your Name <your.email@example.com>
```

By adding this line, you certify you have the rights to contribute the code, per the DCO 1.1. This creates a clear, auditable trail of consent and licensing provenance.

### How to sign off

- Web (GitHub UI): When committing in the browser, check “Sign off on this commit”. GitHub will add the trailer for you. If the setting is enforced, you must check it to proceed.
- Command line: use the `-s` flag with `git commit`:
  ```bash
  git commit -s -m "feat: my change"
  ```
  Ensure your Git name/email match your author identity. If you hide email on GitHub, use your GitHub noreply email.

### Fixing a missing sign‑off

- Amend the last commit:
  ```bash
  git commit -s --amend --no-edit
  git push --force-with-lease
  ```
- For multiple commits, you can rebase and add sign‑off to each:
  ```bash
  git rebase -i --rebase-merges origin/main
  # or automatically add sign-off to each commit
  git rebase -i origin/main --exec "git commit -s --amend --no-edit"
  git push --force-with-lease
  ```

## Contributing via GitHub web UI (no local setup)

1. Navigate to the file you want to change and click “Edit”.
2. Make your changes.
3. In the commit form, check “Sign off on this commit”.
4. Propose changes and create a new Pull Request.

This path is ideal for small fixes (docs, typos, simple code tweaks). For larger changes, prefer local development so you can run and test.

## Local development tips

- This is a Turbo monorepo. Use the root scripts:
  - `bun run dev` to start development tasks
  - `bun run build` to build
- If you cloned without submodules, run:
  ```bash
  git submodule update --init --recursive
  ```
- Vite + SWC is used in apps. If importing from the UI kit, make sure path aliases and symlinks are respected (see README).

## Commit style (suggested)

Use clear, conventional messages:

```
feat(button): add loading state
fix(theme): correct spacing scale token name
docs(readme): clarify submodule setup

Signed-off-by: Your Name <your.email@example.com>
```

## Opening a Pull Request

1. Ensure your branch is up to date with the base branch.
2. Push your commits (with sign‑offs).
3. Open a PR with a clear title and description. Link related issues if any.
4. Be responsive to review comments. We may ask for small changes.

## Requests for new components

If you have a request for a new component, please open a GitHub Discussion or Issue with details (use cases, API sketch, examples). We’re happy to help.

## License and DCO

By contributing, you agree that your contributions are licensed under the repository’s license and that you certify the DCO by signing off your commits.