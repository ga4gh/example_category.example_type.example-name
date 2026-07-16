# Licensing in this template

This repo intentionally hosts more than one license because it contains more
than one kind of content: original code, vendored dependencies, and reference
data/schemas. Rather than picking one license for the whole repo, we license
each *thing* correctly and track it in a machine-readable way.

## Structure

```
LICENSE                  <- explains the multi-license setup, points here
LICENSES/                <- full, unmodified text of every license in use
  MIT.txt
  Apache-2.0.txt
  GPL-3.0-or-later.txt
  CC-BY-4.0.txt
REUSE.toml                <- machine-readable map of path -> license
src/                       <- your code (Apache-2.0 by default)
schemas/                   <- GA4GH-style API/schema defs (Apache-2.0)
data/                       <- reference data / docs (CC-BY-4.0)
vendor/                     <- third-party code, keeps ITS license (e.g. GPL)
```

Every source file should carry a two-line SPDX header, e.g.:

```python
# SPDX-FileCopyrightText: 2021-2026 Your Org / GA4GH Contributors
# SPDX-License-Identifier: Apache-2.0
```

This is what actually determines a file's license — REUSE.toml just fills in
defaults for files/filetypes where you can't or don't want to add a header
(images, generated code, etc).

## Why this instead of just one LICENSE file

- **Your own code**: pick one primary license (this template defaults to
  Apache-2.0, since it's common for GA4GH-style specs/APIs and adds an
  explicit patent grant — swap for MIT if you want maximum permissiveness).
- **Vendored/bundled dependencies**: must keep their original license.
  Never relicense someone else's code just because it lives in your repo.
  Keep it isolated in its own directory (see `vendor/`) so obligations don't
  bleed into your own code.
- **Reference data / datasets**: code licenses (MIT/Apache/GPL) aren't
  written for data and don't fit well. CC-BY-4.0 (attribution required) or
  CC0 (public domain) are the standard choices in genomics.

## Keeping it correct over time

Nothing here needs "updating" because a license *changed* — MIT and
Apache-2.0 text is frozen, and GPL only gets new major versions rarely and
deliberately. What actually needs upkeep, and what the CI workflow in
`.github/workflows/licensing.yml` automates:

1. **Compliance check** — `reuse lint` runs on every push/PR and fails if a
   new file is added without a license annotation (from either a header or
   REUSE.toml).
2. **Copyright year** — a small scheduled job bumps the end year in
   SPDX-FileCopyrightText headers and REUSE.toml once a year.

Install the linter locally with `pip install reuse` and run:

```
reuse lint
```

## Adding a new dependency or vendored component

1. Check its license is compatible with what you're bundling it into
   (e.g. you generally cannot bundle GPL code inside something you ship
   as MIT-only without the whole combined work becoming GPL).
2. Put it in its own directory under `vendor/`.
3. Add its license text to `LICENSES/` if not already present.
4. Add a path entry to `REUSE.toml` (or header its files directly).
