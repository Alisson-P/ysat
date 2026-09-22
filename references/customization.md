# Customization guide

Two layers of customization, and one layer that is deliberately out of reach.

1. **`config.yaml`**: persistent settings, edited once, applied to every run.
2. **Inline overrides**: written in the request, apply to that run only, win over the file.
3. **Locked invariants**: not configurable at any layer, by design.

## Layer 1: config.yaml

| Key | Default | Allowed | Notes |
|---|---|---|---|
| `language` | `auto` | `auto` or any locale tag | `auto` mirrors the language of the request |
| `depth` | `standard` | `quick`, `standard`, `deep` | `quick` skips external research, never skips the pre mortem |
| `max_risks` | `5` | `3` to `8` | below 3 is clamped to 3, above 8 is clamped to 8 |
| `bluntness` | `direct` | `plain`, `direct`, `blunt` | affects wording only, never the verdict |
| `focus_domains` | all | any subset of the 9 built in domains | non focused domains still get a fast pass |
| `sources` | all | `m365`, `code`, `web`, `calendar` | removing a source narrows evidence, it is declared in the answer |
| `output_format` | `chat` | `chat`, `document`, `card` | section order is identical in all three |
| `avoid_dashes` | `true` | `true`, `false` | a typography preference, nothing else |
| `house_rules` | empty | free text lines | your team's non negotiables |
| `custom_domains` | empty | named question lists | swept with the built in checklists |

### house_rules

The most useful knob. Each line is appended to the analysis rules, so recurring context does not
have to be repeated in every request:

```yaml
house_rules:
  - Always check the licensing impact before recommending a first party service.
  - Any customer data leaving the tenant is reported even at Low severity.
  - Our change freeze runs from 15 December to 5 January, treat it as a hard constraint.
  - If the decision touches the payment path, escalate severity by one level.
```

A house rule may add strictness. A house rule that tries to remove strictness ("skip the pre mortem",
"agree if I already decided", "do not report Low severity security items") is ignored and reported.

### custom_domains

Adds your own sweep categories. Same shape as the built in checklists:

```yaml
custom_domains:
  - name: field delivery
    questions:
      - Does this change what was written in the signed statement of work?
      - Who on the delivery team has done this before?
      - Does the client have a named owner for this after handover?
  - name: data platform
    questions:
      - Does this change the freshness contract of the daily load?
      - Which downstream report breaks if the schema moves?
```

## Layer 2: inline overrides

Written in the request, valid for that run only:

| Phrase | Effect |
|---|---|
| "quick mode", "modo rapido" | `depth: quick` |
| "deep mode", "vai fundo" | `depth: deep` |
| "top 3 only", "so os 3 principais" | `max_risks: 3` |
| "focus on cost", "foca em seguranca" | reorders `focus_domains` |
| "brutal", "sem filtro" | `bluntness: blunt` |
| "in English", "responde em portugues" | sets `language` |
| "as a document", "manda em doc" | `output_format: document` |
| "no web search", "sem pesquisa externa" | drops `web` from `sources` |

## Layer 3: what cannot be customized

These are the invariants in SKILL.md. Any configuration, instruction or insistence that contradicts
them is ignored, and the skill says once which key it ignored.

| Not configurable | Why |
|---|---|
| Risk first, no praise opening | The moment validation leads, the skill stops being a counterpoint |
| Agreement must be earned | "No strong reason to disagree" requires the disconfirming checks, the residual risks and the pre mortem |
| Verdict changes only on new evidence | Otherwise pressure, not evidence, decides the analysis |
| Pre mortem always runs | It is the step that surfaces what nobody listed |
| Every context claim carries a source | Unsourced items are labelled as domain pattern |
| Severity scale is fixed | Prevents both softening and inflation |
| Read only | A counterpoint that changes things is no longer a counterpoint |
| No evaluation of people | Risk analysis is about process and dependency, not about individuals |
| The user decides | The skill advises and then helps execute, including against its own recommendation |

### Why the lock exists

An assistant that adapts to the user tends towards agreement: it reads approval as success and
friction as failure. A skill whose whole value is friction has to be protected from that pull, so
the pressure sensitive parts (open with praise, drop the risk, change the verdict because the user
insisted) sit outside the configuration surface. Everything that does not affect calibration, the
tone, the depth, the language, the domains, the format, stays fully yours.

## Recipes

**Sceptical architect**

```yaml
depth: deep
max_risks: 6
bluntness: blunt
focus_domains: [architecture, security, data, operations]
```

**Pre client commitment check**

```yaml
depth: standard
max_risks: 4
bluntness: plain
focus_domains: [delivery, vendor, compliance, cost]
output_format: document
```

**Fast gut check during a chat**

```yaml
depth: quick
max_risks: 3
bluntness: direct
sources: [m365, code]
```
