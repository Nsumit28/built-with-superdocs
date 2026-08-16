# Submitting a build

Two ways in. Both end in the same place — a reviewed entry in
`gallery/manifest.json` — and both are answered in public.

**Read [CURATION.md](CURATION.md) first.** It is short, and it tells you whether
you will pass before you spend time on the submission.

---

## Route 1 — open an issue (easier)

Use the **Submit a build** issue template. Its fields are the three tests, so
filling it in honestly *is* the submission. A reviewer transcribes the accepted
entry into the manifest.

Pick this route if you would rather not touch JSON.

## Route 2 — open a pull request (faster)

Add one object to the `entries` array in `gallery/manifest.json`, then run:

```bash
python3 lint_manifest.py && python3 make_gallery.py
```

Commit both the manifest and the regenerated `gallery/index.html`. One entry per
pull request.

```json
{
  "slug": "your-project",
  "name": "Your project",
  "summary": "One sentence on what it is.",
  "link": "https://…",
  "superdocs_surfaces": ["Which surface did the work, and what it changed"],
  "evidence": {
    "what": "What a reviewer will see at this link.",
    "url": "https://…"
  },
  "submitted_by": "you",
  "reviewed_on": "",
  "decision": "",
  "reviewer_note": ""
}
```

Leave `reviewed_on`, `decision` and `reviewer_note` empty. **They are the
reviewer's fields, not yours** — the lint fails on a submission that fills in its
own verdict, which is the point.

---

## What the machine checks, and what it can't

`.github/workflows/curation.yml` runs on every pull request. It refuses:

- an entry with no evidence link, no review date, or no stated reason
- a decline citing a reason code that was never published to submitters
- a `gallery/index.html` that doesn't match what the manifest generates — so an
  entry cannot be added by hand-editing the page and skipping review entirely
- a badge that would break under GitHub's image proxy
- a page that fetches anything on the reader's behalf

It cannot check the thing that matters most: whether SuperDocs did work that
mattered. That is test 2, and it needs a human who opened both links. The
tooling exists to protect the paper trail, not to replace the judgement.

*(The checks have no third-party dependencies — plain Python. The build scripts
for the badge need `fonttools`, `brotli` and `uharfbuzz`, but nothing in the
review gate does.)*

---

## After you submit

- A human applies the three tests **in order**, stopping at the first failure.
- The verdict goes into the manifest — listed **or declined** — and is merged
  either way. A decline is recorded in public with a numbered reason.
- **Seven days.** If it can't be verified in that window it is declined **R8**,
  which means "resubmit when the evidence is public", not "no".
- Every reason is re-submittable once the reason is fixed. The bar is about
  evidence, not taste.

## If your entry is declined

You will get a number (R1–R8) and a sentence. Both are in the manifest, in
public, next to your entry. If you think the reviewer got it wrong, say so on
the same thread — a decline is a position someone has to defend, not a verdict
handed down.
