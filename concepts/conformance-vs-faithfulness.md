# Conformance and faithfulness

There are two different questions you can ask about brand content, and they must never be answered as one.

| | Question | Kind of answer |
|---|---|---|
| **Conformance** | Does this content item match the schema the brand ratified? | Mechanical: the same answer every time, with the rule and the span cited |
| **Faithfulness** | Does the schema the brand ratified still match the brand? | Judgment: an independent reading of what the brand means |

A conformance pass is never evidence of faithfulness. A faithfulness question is never answered by running the conformance check again.

> **Status.** Neither is available yet. No Ramoira surface reports a conformance or faithfulness result today. Ramoira does not certify schemas or content.

---

## Conformance: does the content match the schema?

Most of what makes content fail a brand is not a matter of taste: a forbidden term used, a required disclaimer dropped, a claim the brand never approved, a competitor named, a discount above the cap. Each of these can be checked the same way every time.

In spec 3.0.0 every checkable rule lives in the schema's `rules` registry, with a check class:

- **`deterministic_exact`**: a term or phrase that must not appear. The finding quotes the matched text.
- **`deterministic_structural`**: a condition such as "only approved claims" or "no discount above 15 percent". The finding names the condition that failed.
- **`judged_bounded`**: something that needs judgment ("never present the pan as a gadget"), decided only by comparison with examples the brand itself judged. If the judge cannot quote the content and cite the brand's examples, the result is void, not a pass.

A conformance result:

- is about **one content item** against **one schema version** (its `content_hash`);
- cites the **rule** and the **span** for every finding;
- has standing only against a **ratified** schema. A check against a candidate, or a producer checking its own work, is useful tooling, recorded as `tooling_only`;
- never comes with rewritten copy. A check flags the item, cites the rule and stops. Fixing the content is the producer's work.

The open format for these results is [`record.schema.json`](https://github.com/ramoira/brand-schema-spec/blob/main/record.schema.json). Results live in that record, never in the schema file.

---

## Faithfulness: does the schema still match the brand?

A schema is a compression of what a brand means. Brands move; schemas go stale. A brand can have a perfect conformance record against a schema that no longer says what it means.

Whether the schema is still faithful cannot be checked mechanically: comparing a schema with its own text proves nothing. It needs a fresh reading of the brand by someone with no stake in having written the schema or in producing the content. For the same reason, Ramoira does not assess the faithfulness of a schema it helped draft.

Faithfulness results do not exist yet. Until they do, no Ramoira surface will claim them.

---

## Why keeping them apart matters

Take the fictional cookware brand Corvane, from the [spec examples](https://github.com/ramoira/brand-schema-spec/tree/main/examples/corvane). Its schema says the warranty is 25 years and forbids "lifetime warranty".

- An ad that says "lifetime warranty" **fails conformance**. That is mechanical, and the finding quotes the phrase.
- Suppose Corvane later extends the warranty to 30 years but never updates its schema. An ad saying "25-year warranty" **passes conformance** and is wrong. Only a faithfulness question catches that: does the ratified schema still match the brand?

If a conformance pass were read as "this content is right for the brand", the second ad would look approved. It is only consistent with what the brand last ratified.

---

## Neither: diagnostics

Some measures describe a schema or a body of content without being either kind of result:

- similarity to an archetype;
- how dense or thin a schema is;
- citation audits (how AI systems describe a brand).

These are reported as **diagnostics**, each as its own labelled field. A diagnostic is never a quality score, a conformance result, a faithfulness result or a certification, and it never feeds one.

---

## See also

- [Ratification](ratification.md): what makes a schema the brand's own, and therefore something content can be checked against.
- [Tiers](tiers.md): what is free, and what is not available yet.
