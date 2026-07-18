# content-compose-example

A worked example of a **composed content project** built with
[throughline-compose](https://github.com/timebacksolutions/throughline-compose).

It models one real writing task — a council **rent-statement help page** — that has to
be readable, correctly styled, in the right register, doing the right communicative job
**and** pitched at the right reader. Those are independent concerns, so the project
adopts five orthogonal content sources by reference and cites their clauses side by
side on the requirements they satisfy:

| Namespace | Source | Axis |
|---|---|---|
| `plain` | [throughline-plain-language](https://github.com/timebacksolutions/throughline-plain-language) `@v2026-07` | readability |
| `conventions` | [throughline-conventions-uk](https://github.com/timebacksolutions/throughline-conventions-uk) `@v2026-07` | British-English conventions |
| `tone` | [throughline-tone-formal](https://github.com/timebacksolutions/throughline-tone-formal) `@v2026-07` | register (formal) |
| `purpose` | [throughline-purpose-instruct](https://github.com/timebacksolutions/throughline-purpose-instruct) `@v2026-07` | purpose (instruct) |
| `audience` | [throughline-audience-general](https://github.com/timebacksolutions/throughline-audience-general) `@v2026-07` | audience (general reader) |

This is the orthogonality payoff: the page takes exactly the five axes it needs and
combines them. Each source numbers its items `SR-0001` upward, yet they never collide
because each is imported under its own namespace (`plain:SR-0004`,
`conventions:SR-0008`, `tone:SR-0001`, `purpose:SR-0010`, `audience:SR-0003`) — the
same way a security project composes `asvs` + `gds` + `wcag`.

`plain` and `audience` look similar but are independent: `plain` is *universal* clarity
for any reader, while `audience` tunes how much prior knowledge and field vocabulary the
writing may assume for a *specific* reader — here, a lay tenant. The page composes both.

Three of the axes are families of **mutually-exclusive sibling sources**: register
(`throughline-tone-formal` / `-neutral` / `-informal`), purpose
(`throughline-purpose-inform` / `-instruct` / `-persuade`) and audience
(`throughline-audience-expert` / `-practitioner` / `-general`). A government
rent-statement help page composes the **formal** register, the **instruct** purpose and
the **general**-reader audience; a chatty expert-facing marketing page would swap those
`url`/`ref`s for `-informal`, `-persuade` and `-expert` without touching the readability
or conventions axes.

## The authored page

The requirements graph is the *spec*; the page it governs is
[`content/rent-statement-help-page.md`](content/rent-statement-help-page.md). throughline
does not lint prose, so the artifact is a plain file — but you can read it against the
graph and see each composed axis bite: the formal register (no contractions — "do not",
"cannot"), the general-reader glossary of *arrears*, *debit* and *credit*, the sentence-
case headings and GOV.UK number and date style ("42.50", "1 July 2026"), the numbered
pay steps, and the inverted-pyramid opening that leads with the balance due and payment
due date. Swap a sibling axis and the artifact would be rewritten to match; the graph is
what says how.

## How it's wired

- The project's own graph lives under `intents/`, `user-requirements/` and
  `system-requirements/`. Each page requirement **grounds** through `implements` →
  `UR-0001` → `derives_from` → `INT-0001` (its own throughline), and **separately**
  `satisfies` the borrowed `plain:`/`conventions:`/`tone:`/`purpose:`/`audience:` clause
  it honours.
- `satisfies` is a traceability link, not a grounding link — so a page requirement
  still justifies itself through its own intent, not through a borrowed standard.

## Running it

```sh
tl-compose check --strict     # fetches all five pinned sources, merges, validates
tl-compose trace SR-0004      # show a requirement's throughline across the axes
```

Drive this project with `tl-compose`, never bare `tl`: bare `tl` fails fast the moment
it meets a namespace-qualified reference (`plain:SR-0004`) it cannot resolve, because
only the composition-aware tool fetches and merges the sources.
