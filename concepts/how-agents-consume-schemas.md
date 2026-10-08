# How agents consume schemas

An agent writing for a brand should load what the task needs, in order, rather than the whole schema every time.

## 1. Find the surface

Where will the content appear? The surface (`OutputSurface`, 17 values: `product_detail_page`, `social_organic`, `email_retention`, …) decides which rules apply and which voice variant to use. Optionally, the user intent (`UserIntent`, 8 values) refines it. Both lists are in the spec's [Appendix A](https://github.com/ramoira/brand-schema-spec/blob/main/SPEC.md#appendix-a--enumerations).

## 2. Load, in this order

1. **The rules** for that surface: `rules` where `surfaces` is `"all"` or includes it. `absolute` rules are never broken; `strong` ones need the brand's sign-off to break.
2. **`voice.examples`**: approved and rejected lines, each with the brand's reason. The rejected ones show what the brand never sounds like.
3. **`voice.approvedTones`** and the brand's humour and vocabulary (`voice.base`).
4. **`voice.contextVariants`** for the surface: openings, closings, formality and warmth shifts.
5. **Rails** (`voice.rails`, and those on pillars, surfaces and situations): what to do instead in a given context.
6. **`narrative.semiotic.denotative.claims`**: the only product claims the brand makes.

From the public summary, private rules and the commercial and governance layers are absent unless the brand opted them in. Work from what is there; do not invent the rest.

A shorter route: `ramoira init` writes `ramoira/agents.md`, a plain-language brief built from the public summary, for tools that read Markdown better than JSON.

## 3. Check your own output

Run the brand's rules over the draft before using it:

```sh
ramoira check --surface product_detail_page draft.txt
```

Exact and structural rules run without a model; judged rules use your own model key. Every finding names the rule and quotes the span; nothing suggests a rewrite. See the [CLI reference](../reference/cli.md).

A check you run on your own work is a **self-check**: useful tooling, not an independent check and not a certification. A schema the brand has not ratified is a candidate, and content is not "approved" by having been checked against it.
