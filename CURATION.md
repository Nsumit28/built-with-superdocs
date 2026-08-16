# How entries get in, and why they get turned down

A badge is worth exactly as much as the thing standing behind it. If any project
that wants the badge can have it, the badge says nothing, and a gallery that
lists everything is a directory, not a signal.

So this is the bar, written down before anything was submitted, and the reasons
for turning an entry down, written down before anyone had to be turned down. If
your entry is declined you will get one of the numbered reasons below, not a
silence and not a matter of taste.

---

## The bar

Three tests. An entry has to pass **all three**. A reviewer applies them in
order and stops at the first failure.

### 1. There is a real document, and it can be opened

Something a reviewer can click: a published page, a repository, a file in a
release, a public export. Not a screenshot of it. Not a private link. Not a
description of a document that exists somewhere.

*Why this one first:* it is the only test that can be settled in ten seconds,
and everything after it depends on the artifact being real.

### 2. SuperDocs did work that mattered to the outcome

Name the surface used and what it changed — editing inside the document,
tracked changes, export, the API, the MCP surface. Then the counterfactual:

> If the same artifact could have been produced, at the same risk, by typing
> into a plain text editor, the entry does not qualify.

A worked example, taken from this rule's own revision history: a change that
rewrote the section you are reading while every other section came back
byte-identical — the decline table below included — qualifies, because it is
checkable and because doing it by hand is exactly the thing that goes wrong
quietly. Opening the same file, reading it, and saving it unchanged does not.

This is the test most submissions will fail, and it is meant to be. "I opened
it in SuperDocs" is not use. "Section-precision edits let me revise one clause
of a contract without touching the eleven around it" is.

### 3. The claim is evidenced, not asserted

A second link that lets a reviewer check the claim in test 2: a commit, a diff,
an exported file, a public write-up, a PR. "We use it internally" is not
checkable, and unverifiable praise is worth less to SuperDocs than no entry.

### And two preconditions

- **The owner submits it.** Nothing is harvested. A project is not listed
  because a curator found it and thought it looked good — being written about
  without asking is not a favour.
- **It is safe to link.** No credentials, no personal data, nothing under an NDA,
  nothing that names a private individual.

---

## Reasons an entry is declined

The reviewer records one of these in the manifest, in public, next to the entry.

| | reason |
|---|---|
| **R1** | **Not submitted by its owner.** Someone else's project, however good. |
| **R2** | **Nothing to open.** The evidence is a screenshot, a dead link, or private. |
| **R3** | **SuperDocs was incidental.** A plain text editor would have produced the same thing at the same risk. |
| **R4** | **Not real work.** A hello-world, or a document created to qualify for the badge. |
| **R5** | **Self-referential.** The project is the badge, the gallery, or exists to be listed in it. |
| **R6** | **Unsafe to link.** Credentials, personal data, confidential or NDA'd material, or a named private individual. |
| **R7** | **Makes a claim about SuperDocs that isn't true.** Including capabilities it doesn't have. Correct it and resubmit. |
| **R8** | **Unverifiable in the review window.** Not a judgement on the project — resubmit when the evidence is public. |

R7 and R8 are explicitly re-submittable. So is anything else, once the reason is
fixed: the bar is about evidence, not about taste, and an entry that failed on
Tuesday can pass on Thursday with a better link.

---

## The review path

1. **The owner opens a pull request** adding one object to
   `gallery/manifest.json`. One entry per PR.
2. **`lint_manifest.py` runs first.** An entry with no evidence URL, no review
   date, or no recorded reason cannot merge — the rule is enforced by the tooling,
   not by the reviewer remembering.
3. **A human applies the three tests in order**, opening both links.
4. **The verdict is written into the manifest** — `listed` or `declined`, with
   the reason and the reviewer's one line — and merged either way. **A decline is
   recorded, not closed in silence.**
5. **Within seven days**, or it is declined R8 with an invitation to resubmit.
6. **Listed entries are re-checked.** A dead link is delisted with the reason
   kept visible, because a gallery of broken links is worse than a short one.

---

## Where the curator is compromised

The gallery currently holds one entry, and it is the curator's own page. That is
the weakest thing about it and it would be dishonest to bury it, so it is
disclosed on the entry itself.

The rule that keeps this from rotting: **the curator's own work is held to the
same three tests, and the conflict is stated on the entry, every time.** If the
gallery is ever handed to SuperDocs, that disclosure line stops being necessary
and should be dropped — but not before.

And the gallery is not in the gallery. It is listed publicly as declined under
R5, and the reason is worth stating precisely rather than flatteringly:
SuperDocs *was* used on this project — the section above was revised through the
API and the document exported — but both sentences it drafted were rejected on
review, and the one that shipped was written by hand. Being the gallery is
disqualifying on its own. A curator whose own work would be declined should say
so before anyone else has to.

---

## What this is not

It is not a quality bar. Nobody is judging whether the project is good, whether
the writing is clean, or whether the idea is clever. The three tests ask one
question: **did SuperDocs do real work here, and can a stranger check it?**

A rough project with an honest diff gets in. A beautiful project with an
unverifiable claim does not.
