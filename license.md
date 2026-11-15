# Standard: License

Every repository must contain an MIT license file at the repository root,
named exactly `LICENSE` (a `LICENSE.md` variant is tolerated but the canonical
name is `LICENSE`).

## Required content

The file must contain the standard MIT license text with:

- Copyright holder: `Garret Patten`
- Copyright year: the year of first publication (e.g. `2025`, `2026`)

## Canonical text

```text
MIT License

Copyright (c) 2026 Garret Patten

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Audit checks

1. A file named `LICENSE` (or `LICENSE.md`) exists at the repository root.
2. The text matches the canonical version above, ignoring:
   - the copyright year (any year is acceptable);
   - the copyright holder line if it correctly reads `Garret Patten` or
     `Garret Patten <garret.patten@proton.me>`.

To fetch the canonical text programmatically:

```bash
gh api repos/garretpatten/github-standards/contents/license.md \
  --jq '.content' | base64 -d
```

Detecting the recognized MIT variant (works for `LICENSE` or `LICENSE.md`):

```bash
gh api repos/OWNER/REPO/contents/LICENSE --jq '.content' | base64 -d |
  grep -q 'Permission is hereby granted, free of charge'
```
