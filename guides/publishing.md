# Review and publishing

Your schema becomes public only when you, signed in on ramoira.com, review a version and press **Make public**. Uploading is a separate step anyone with the credential can do (you, your coding agent, CI), and it never makes anything public. Both are free; they need a free account so your slug belongs to you.

It works like git: a push is a branch waiting for review, the review is the pull request, and the version you make public is the merge.

---

## Quick start

```sh
ramoira login                     # opens your browser: sign in with GitHub or an emailed link, then approve
ramoira push --slug your-brand    # the slug only the first time
# review on ramoira.com/dashboard, then:
ramoira pull
```

For CI, create a token on your dashboard (or `ramoira create-token ci`) and set it as `RAMOIRA_TOKEN`. A token can push and pull; it can never make a version public.

---

## Push: a private version for review

1. The CLI checks `ramoira/brand.schema.json` locally: a valid 3.x full schema with an up-to-date `content_hash`. If not, it says why and stops, with no network call.
2. It sends the full schema, and the reviewed version it is based on, to `ramoira.com/api/brands/<slug>/proposals`.
3. Ramoira keeps it privately and shows it on your dashboard: *A version is waiting for your review.*

```
✔ Pushed. Nothing is public.
  The slug "your-brand" is now yours. It is never given to anyone else.

  The owner reviews it here, signed in: https://ramoira.com/dashboard/your-brand/review
  Version 0.1.0 · sha256:eaed480d372b…
```

A push is refused when:

- **it is out of date:** you have reviewed a newer version than the one this file is based on. Run `ramoira pull` and make the change again on top of it;
- **it claims an affirmation you never made:** a rule marked `"affirmed": true`, an example judged by the brand, or a field marked affirmed, that you did not affirm in a review. Only you affirm, signed in. Upload them unaffirmed; a rule your agent changed goes back to `"affirmed": false`.

The first push to a slug no one holds claims it for your account. A newer push replaces a version still waiting for review. A 2.0.0 schema is refused, with a pointer to the [migration guide](https://github.com/ramoira/brand-schema-spec/blob/main/migrations/2.0.0-to-3.0.0.md).

Without the CLI, upload `brand.schema.json` on your dashboard, or finish a draft at [ramoira.com/draft](https://ramoira.com/draft): both go to the same review.

## Review: only you, in your words

At `ramoira.com/dashboard/<slug>/review`:

- each **rule**: *Affirm*, *Edit* (your wording, recorded as yours) or *Delete*. A rule counts once you affirm it; an undecided rule stays in the schema, unaffirmed;
- each **proposed field**: *Keep* it or *Rewrite* it in your words; fields already yours can be rewritten too;
- a **version label** (`schema_version`): when a changed version would repeat the public one's label, it goes up by one (`0.1.0` → `0.1.1`); type another if you like. The label is for people; `content_hash` identifies the version;
- then **Make public**, or **Send back to your agent** with your requests, one per line.

Ramoira applies your decisions as your words, without a model, and adds nothing of its own. After the first review, you see only what changed since the version you reviewed last.

## Pull: bring the reviewed version down

```sh
ramoira pull
```

`pull` writes the reviewed version into `ramoira/brand.schema.json`, with your decisions, and records it as the base in `ramoira/sync.json`. If you sent it back, your requests are in `ramoira/REVIEW.md`: ask your agent *"Make the changes in ramoira/REVIEW.md"*, then push again. `pull` refuses to overwrite changes that were never pushed, unless you pass `--force`.

**Always pull after a review.** Otherwise your next push is refused as out of date.

## Where your file stands

```sh
ramoira status
```

Besides the public facts about the brand, it says whether your file is the public version, waiting for review, changed since the last push, or behind a newer version you reviewed or made public. The file's own label, `ramoira.workflow_state`, follows along (`draft`, `in_review`, `published`), but what ramoira.com says is what counts.

---

## Changing a public schema

A version you made public never changes, like a commit. You make a new one; the public one stays until you make the new one public.

- **With your agent:** pull, ask for the change, push, review.
- **Yourself, in the browser:** **Revise** on your dashboard opens your last reviewed version: edit or delete rules, rewrite fields, then make it public, or write requests and send it back to your agent. Afterwards, run `ramoira pull` in your project. A revision you change your mind about can be discarded.

A public version can't be taken down yet, only replaced by a newer one.

---

## What becomes public

| | Public summary |
|:---|:---|
| Rules with `visibility: public` | ✓ |
| `identity`, `narrative`, `voice` | ✓ in full: approved claims, examples (rejected ones too), context variants, rails, myth evolution, pillars |
| `identity.prism.culture.sacredBoundary` | Only if you opt in |
| `commercial`, `governance` | Only if you opt in (`ramoira.summary_opt_in`) |
| Rules with `visibility: private` | Never |
| `draft_provenance` | Never |
| Versions you have not made public | Never |

Choose what stays private per rule, with `visibility`. The defaults are the same for every brand.

## After making it public

```
https://ramoira.com/brands/<slug>/schema.summary.json    the public summary
https://ramoira.com/brands/<slug>/status                 facts: published, ratified, checked
https://ramoira.com/brands/<slug>                        the brand's page
```

## Renaming

Rename your slug on your dashboard. The old slug redirects to the new one and is never given to another brand, and your fingerprints don't change. A push to the old slug is refused and names the new one: set `ramoira.brand_id` to it before you push again.

---

## Publishing is not ratification

A public schema is still a **candidate**. Making it public serves the summary; it does not mean the brand has [ratified](../concepts/ratification.md) the schema, and it certifies nothing.
