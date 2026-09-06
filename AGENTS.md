# bellen

This repository is the **public template** and **GitHub Pages initializer** for thin control repos named `*-org`.

It is **not** a product monorepo. Do not clone random product repositories into this checkout. Product code stays in separate repos. Git is the shared disk.

- **Use this template** → `OWNER/something-org` (a control repo).
- **https://e-st.github.io/bellen/?repo=OWNER/something-org** opens a PR on that control repo with the Codespace (`Agent Canvas` + `grok agent stdio`).
- **`.github/workflows/after-mastart-pr.yml`** is inherited by copies of the template. It comments a human checklist on `init/*` PRs. The gh-aw stub is written by the initializer (user PAT with `workflows: write`); Actions `GITHUB_TOKEN` cannot create workflow files.

When you are working **inside a control repo** copied from this template, follow **that** repo’s `AGENTS.md`, not this one.
