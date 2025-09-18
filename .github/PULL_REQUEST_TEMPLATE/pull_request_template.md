## PR Title

<!-- Keep it concise and descriptive. Example: "feat(button): add loading state" -->

## Summary

<!-- What does this PR change and why? -->

## Changes

-
-

## Related Issues/Links

- Closes #
- See also:

---

## DCO Sign-off (Required)

This project uses the Developer Certificate of Origin (DCO). All commits in this PR must be signed off.

- [ ] I certify that my contribution complies with the DCO (https://developercertificate.org/) and that every commit in this PR contains a `Signed-off-by` trailer.

How to sign off commits:

- Command line (recommended):
  ```bash
  git commit -s -m "your message"
  ```

- Web (GitHub UI): Check the "Sign off on this commit" box in the commit form. If repository settings require it, you must check this to proceed.

Fix missing sign-offs:

```bash
# last commit
git commit -s --amend --no-edit
git push --force-with-lease

# multiple commits
git rebase -i origin/main --exec "git commit -s --amend --no-edit"
git push --force-with-lease
```

Example commit message:

```
feat: add Button demo

Signed-off-by: Your Name <your.email@example.com>
```

---

## Checklist

- [ ] All commits are signed off (DCO)
- [ ] Code compiles locally (`bun install` and `bun run build`/`bun run dev`)
- [ ] Submodules initialized if needed (`git submodule update --init --recursive`)
- [ ] Tests and/or docs updated if applicable
- [ ] I read the CONTRIBUTING.md and followed the guidelines

## Notes for Reviewers

<!-- Anything reviewers (code owners) should pay extra attention to? -->


