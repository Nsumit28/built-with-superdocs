# Built with SuperDocs — an attribution badge, and a gallery with a bar

![built with SuperDocs](badge.svg)

A badge projects can put in their README, a gallery it links to, and — the part
that actually matters — **a written rule for who gets in and why anyone is turned
down.**

A badge is worth exactly what stands behind it. Hand it to anyone who asks and it
says nothing. So the interesting half of this build is not the SVG; it is
[CURATION.md](CURATION.md): three tests, eight numbered decline reasons published
before anyone had to be declined, a review path with a clock on it, and declines
recorded in public rather than closed in silence.

## What's here

| | |
|---|---|
| [`badge.svg`](badge.svg) | The badge. 133 × 20, 7 KB, no external dependency of any kind |
| [`SNIPPETS.md`](SNIPPETS.md) | Copy-paste Markdown / HTML / reStructuredText |
| [`CURATION.md`](CURATION.md) | The bar, the decline reasons, the review path |
| [`SUBMITTING.md`](SUBMITTING.md) | How to get listed, and what happens after |
| [`gallery/manifest.json`](gallery/manifest.json) | The only place an entry exists |
| [`index.html`](index.html) | The page, generated from the manifest |

## How to run it

```bash
python3 make_gallery.py     # regenerate the page from the manifest
python3 lint_manifest.py    # every entry has evidence, a date and a reason
python3 verify_gallery.py   # the page requests nothing on the reader's behalf
python3 verify_badge.py     # the badge survives GitHub's image proxy
python3 test_lint.py        # 9 negative tests
python3 test_verify.py      # 10 negative tests
```

Those six have **no third-party dependencies** — they are the review gate, and a
gate that needs an install is a gate people skip. Rebuilding the badge itself
needs `fonttools`, `brotli` and `uharfbuzz`:

```bash
pip install fonttools brotli uharfbuzz
python3 make_badge.py
```

Only the SuperDocs script talks to the network with credentials. It reads one
environment variable:

```bash
export SUPERDOCS_API_KEY=your-key-here   # placeholder — never commit a real key
pip install markdown
python3 b3_superdocs.py --check          # offline: re-report the precision measurement
python3 b3_superdocs.py --edit           # spends one operation
```

Verify a key with `GET /v1/sessions`. Not `/v1/users/me` — that returns 401 for a
valid `sk_` key and looks like a bad credential when it isn't.

### Where the CI gate does and doesn't run

`.github/workflows/curation.yml` is a real gate **in a standalone gallery repo**,
where it sits at the root. Vendored inside someone else's repository as a project
folder it does not execute — GitHub only runs workflows from the repository root,
and this project deliberately modifies nothing outside its own folder. Read it
there as the review process written down and runnable by hand, not as CI that is
currently running. The six commands above are that gate.

## Two things that were measured rather than assumed

**The badge contains no live text.** Every letter is an outline path. That is not
a stylistic choice — GitHub serves README images through its camo proxy, and camo
returns `default-src 'none'` with no `font-src`, so `@font-face` is blocked
outright. A live-text badge falls back to whatever font the reader happens to
have, and SuperDocs' own typeface ships on no operating system. `camo_sim.py`
serves the badge under camo's exact headers and lets a browser decode it, so the
policy is applied rather than reasoned about.

**Contrast was computed, not eyeballed.** Cream on near-black is 18.7:1;
near-black on coral is 7.3:1. White on coral — the obvious first instinct —
measures 2.67:1 and was rejected on the number.

## Which SuperDocs features this build uses

- **Editing inside the document, via `POST /v1/chat`** — used on `CURATION.md`,
  this project's own rule. Section precision held: nine of ten sections came back
  byte-identical, the decline table untouched, across two independent sessions.
- **Export, via `POST /v1/documents/export`** — returns a file, not JSON.

Both sentences the AI drafted were **rejected on review** — one on the wrong
argument entirely, one containing a capability claim that had never been tested.
The sentence that shipped was written by hand. That is in the rule document too,
because a rule that misstates its own history is worth nothing.

## A note on the one entry

The gallery opens with a single entry, and it is the curator's own project, which
is disclosed on the entry itself. Padding it with builds whose owners never asked
to be listed would break the precondition the whole thing rests on — entries come
from their owners, nothing is harvested.

And this project is **listed publicly as declined under R5**. It is the gallery;
being the gallery is disqualifying on its own. A curator whose own work would be
declined should say so before anyone else has to.

---

Built by **Sumit Negi** as a Round 2 candidate submission for
[@superdocsapp](https://twitter.com/superdocsapp). Not an official SuperDocs
property.
