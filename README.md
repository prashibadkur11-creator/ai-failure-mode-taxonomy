# AI Failure Mode Taxonomy

A structured, forkable catalog of the ways AI/LLM products fail at the **product level** —
not stack traces and bugs, but the failure modes that arise *because there is a model in the
loop*: hallucination, persona drift, silent degradation, prompt injection, and more.

It exists to be **used**, not just read. Fork it, walk your own product against the catalog,
and you come away with a risk map: which failure modes can occur in your product, where they
matter most, what to instrument, and what to do about them.

## Who this is for

AI product managers, founders, and eng leads who need a systematic way to reason about how
their AI product can fail — during design review, pre-launch risk assessment, or incident
retrospectives.

## What's here

```
.
├── schema.yaml              # The fixed schema every failure mode conforms to
├── failure-modes/           # One YAML file per failure mode (the source of truth)
├── generate_catalog.py  # Generates CATALOG.md from the YAML
├── CATALOG.md               # Readable, generated catalog (browse this first)
└── product-audit.md     # Fillable template to audit your own product
```

## The schema

Every failure mode is described with the same fields, which is what makes the catalog
filterable, comparable, and extensible:

| Field | Purpose |
|---|---|
| `id` | Stable slug for cross-referencing |
| `name` | Human-readable name |
| `category` | accuracy · consistency · calibration · safety · reliability · interaction |
| `summary` | What the failure is, in a sentence or two |
| `cause` | The mechanism that produces it |
| `user_experience` | How a user actually encounters it |
| `stakes_context` | Where it matters most vs. least |
| `detection` | Concrete, opinionated ways to catch it |
| `mitigation` | Concrete, opinionated ways to reduce it |
| `example` | A realistic instance of the failure |
| `related` | Failure modes that tend to co-occur |

See [`schema.yaml`](schema.yaml) for the full field definitions.

## How to use it

1. **Browse** [`CATALOG.md`](CATALOG.md) to get the lay of the land.
2. **Fork** the repo.
3. **Audit your product** using [`templates/product-audit.md`](templates/product-audit.md):
   for each failure mode, decide whether it can occur in your product, rate its severity in
   your context, and note what you'll instrument and mitigate.
4. **Extend** the catalog: add YAML files for failure modes specific to your domain, following
   the schema, and regenerate the catalog.

## Regenerating the catalog

The catalog is generated from the YAML — never edit `CATALOG.md` by hand.

```bash
pip install pyyaml
python scripts/generate_catalog.py
```

This validates every failure-mode file against the required schema fields and rewrites
`CATALOG.md`.

## Design principles

- **Universal core.** The catalog applies to any LLM-powered product. Severity is
  context-dependent (captured in `stakes_context`); the failure modes themselves are general.
- **Opinionated guidance.** Detection and mitigation name specific techniques and take a
  stance, because vague advice ("test thoroughly") helps no one.
- **Structured, not prose.** The value is in the consistent schema you can filter, compare,
  and build on — not in essay-style write-ups.

## Contributing

Add or refine failure modes via pull request. New entries must conform to `schema.yaml` and
pass `generate_catalog.py`. Counterexamples and sharper detection/mitigation techniques are
especially welcome.

## License

MIT — use it, fork it, adapt it for your team.
