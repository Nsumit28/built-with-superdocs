# The "built with SuperDocs" badge

![built with SuperDocs](badge.svg)

One badge, 133 × 20 px, 7 KB. It links to the gallery entry list, so the claim it
makes is checkable by whoever clicks it.

**Add it only if it's true.** The badge says a real project used SuperDocs to do
real work. It is not a logo you display because you like the product — the
gallery's entry bar is in `CURATION.md`, and entries that don't meet it are
declined with a reason.

---

## Copy-paste

### Markdown — README.md, GitHub, GitLab, most static-site generators

```markdown
[![built with SuperDocs](https://nsumit28.github.io/built-with-superdocs/badge.svg)](https://nsumit28.github.io/built-with-superdocs/)
```

### HTML — docs sites, project pages, anywhere raw HTML is allowed

```html
<a href="https://nsumit28.github.io/built-with-superdocs/">
  <img src="https://nsumit28.github.io/built-with-superdocs/badge.svg"
       alt="built with SuperDocs" width="133" height="20">
</a>
```

Keep `width` and `height`. They stop the page reflowing while the image loads,
and GitHub sizes the element from them.

### reStructuredText — PyPI long descriptions, Sphinx, Read the Docs

```rst
.. image:: https://nsumit28.github.io/built-with-superdocs/badge.svg
   :alt: built with SuperDocs
   :target: https://nsumit28.github.io/built-with-superdocs/
```

Verified on PyPI's side: PyPI runs its own image proxy
(`pypi-camo.freetls.fastly.net`) and does proxy `.svg` badges — checked against a
live project page, not assumed from documentation.

**Don't change the alt text.** "built with SuperDocs" is what a screen reader
announces and what appears when images are blocked or the proxy is down. It is
the badge's only fallback.

---

## Why it looks the same on every machine

The badge contains no live text. Every letter is an outline path, so it needs no
font, no stylesheet, and no network request beyond the image itself.

That isn't a stylistic preference. GitHub serves every README image through its
camo proxy, and camo returns this policy with the file:

```
content-security-policy: default-src 'none'; img-src data:; style-src 'unsafe-inline'
content-type: image/svg+xml;charset=utf-8
x-content-type-options: nosniff
```

`default-src 'none'` with no `font-src` means an `@font-face` of any kind is
blocked. A badge with live text falls back to whatever the reader happens to
have installed — and SuperDocs' own typeface, Space Grotesk, ships on no
operating system, so a live-text badge could never be on-brand. Outlines are the
only way to be both correct and branded. `verify_badge.py` enforces this
mechanically, so a later edit can't quietly reintroduce a font dependency.

Camo was also measured to pass the origin bytes through **byte-for-byte** — a
camo-served badge diffed identical to its origin file. So nothing is stripped
after the fetch; everything that can break has to be inside the file, which is
what the verifier checks.

## Colours and contrast

Read out of superdocs.app's own stylesheet, not sampled by eye: coral `#f97766`,
near-black `#110b0b`, cream `#fbfaf6`.

| | ratio | |
|---|---:|---|
| cream on near-black | 18.7:1 | passes AA and AAA |
| near-black on coral | 7.3:1 | passes AA and AAA |
| ~~white on coral~~ | 2.67:1 | fails AA — rejected for this reason |

A hairline at 25% cream keeps the dark half from merging into the page on
GitHub's dark theme, where the background (`#0d1117`) is close to the badge's own
near-black. Checked by rendering the badge over both theme backgrounds rather
than trusting that it would be fine.

## Rebuilding it

```bash
pip install fonttools brotli uharfbuzz
python3 make_badge.py          # writes badge.svg
python3 verify_badge.py        # 12 checks against the camo contract
python3 test_verify.py         # 10 negative tests, so the checker isn't decoration
```

Fonts are downloaded on demand against a pinned SHA-256 rather than committed.
Space Grotesk and Inter are both SIL OFL 1.1, which permits shipping outlines
derived from them.

## If SuperDocs adopts this

Every URL above resolves through one base constant. Moving the gallery to a
SuperDocs-owned address — `https://superdocs.app/built-with/` — is a one-line
change plus a repository transfer, which preserves history and issue threads.
The badge was designed to be handed over, not to stay on a candidate's account.
