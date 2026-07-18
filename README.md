# content-compose-example

A worked example of a **composed content project** built with
[throughline-compose](https://github.com/timebacksolutions/throughline-compose).

It models one real writing task — a council **rent-statement help page** — that has to
be readable, correctly styled **and** in the right register. Those are independent
concerns, so the project adopts three orthogonal content sources by reference and cites
their clauses side by side on the requirements they satisfy:

| Namespace | Source | Axis |
|---|---|---|
| `plain` | [throughline-plain-language](https://github.com/timebacksolutions/throughline-plain-language) `@v2026-07` | readability |
| `conventions` | [throughline-conventions-uk](https://github.com/timebacksolutions/throughline-conventions-uk) `@v2026-07` | British-English conventions |
| `tone` | [throughline-tone-formal](https://github.com/timebacksolutions/throughline-tone-formal) `@v2026-07` | register (formal) |

This is the orthogonality payoff: the page takes exactly the three axes it needs and
combines them. Each source numbers its items `SR-0001` upward, yet they never collide
because each is imported under its own namespace (`plain:SR-0004`,
`conventions:SR-0008`, `tone:SR-0001`) — the same way a security project composes
`asvs` + `gds` + `wcag`.

The register axis is a family of **mutually-exclusive sibling sources** —
`throughline-tone-formal`, `-neutral` and `-informal`. A government rent-statement page
composes the **formal** sibling; a chatty community-newsletter page would swap that one
`url`/`ref` for `-informal` without touching the readability or conventions axes.

## How it's wired

- The project's own graph lives under `intents/`, `user-requirements/` and
  `system-requirements/`. Each page requirement **grounds** through `implements` →
  `UR-0001` → `derives_from` → `INT-0001` (its own throughline), and **separately**
  `satisfies` the borrowed `plain:`/`conventions:`/`tone:` clause it honours.
- `satisfies` is a traceability link, not a grounding link — so a page requirement
  still justifies itself through its own intent, not through a borrowed standard.

## Running it

```sh
tl-compose check --strict     # fetches both pinned sources, merges, validates
tl-compose trace SR-0004      # show a requirement's throughline across both axes
```

Drive this project with `tl-compose`, never bare `tl`: bare `tl` fails fast the moment
it meets a namespace-qualified reference (`plain:SR-0004`) it cannot resolve, because
only the composition-aware tool fetches and merges the sources.
