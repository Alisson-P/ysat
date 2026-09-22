# YSAT

### You Sure About That?

**An agent skill that disagrees with you, on purpose, with evidence.**

> 🇧🇷 Leia em [português](README.pt-BR.md)

Most assistants are trained to be agreeable. That is exactly wrong at the moment you are about to
commit to an architecture, a vendor, a deadline or a migration. YSAT reads your real context, code,
infrastructure, email, chat, shared documents and calendar, and gives you the argument nobody made
in the meeting: what can break, how badly, when, and the cheapest test to run before you commit.

It is deliberately one sided. The burden of proof sits on the decision, not on the objection.

```
You:  "I am moving the client database to serverless at the end of the month, thoughts?"

YSAT: Decision as I understand it ...
      What I looked at ...
      Where this can break      (risk | why | evidence | severity | when it bites)
      Blind spot ...
      Strongest case for it ... and what would have to be true
      Verdict: would do it with guardrails
      Mitigations ... / Cheapest test ...
```

## On determinism

An assistant that is helpful gets rewarded, and agreement is the cheapest form of helpfulness
available to it. That pressure never announces itself. It shows up as a model that mirrors your
framing back at you: the longer the thread runs, the more your assumptions become its premises,
until the answer you get is your own opinion with better formatting.

Call the effect what it is. The drift is deterministic. Give an agent a user with a position and a
conversation long enough, and it converges on that position. You can count on that happening. What
you cannot count on is the agent volunteering the thing you did not want to hear, because nothing
in the loop rewards it for doing so.

This matters most exactly where the stakes are highest. The decisions that hurt are rarely the ones
where somebody named the risk and you accepted it anyway. They are the ones where nobody said
anything: the room agreed, the date was close, and the person who could see the problem was not the
person being asked. An agent that agrees by default joins that room instead of breaking it.

YSAT is built to break it, and it does that structurally rather than tonally. A preference can be
argued away in a single turn of pressure. A rule cannot. That is why the invariants live in their
own section, outside the configuration surface: risk leads, agreement has to be earned, and the
verdict moves on new evidence and on nothing else. Tone, depth, language, domains and format are
yours to set. The part that refuses to fold is not.

And the most common outcome is not that YSAT stops you. It is that it puts on the table the thing
you were not looking at, which is what the blind spot line of every answer exists for. Sometimes you
read it, disagree, and go ahead anyway. That is a good outcome too, because the decision now carries
the counterargument in hand instead of discovering it in production.

## Why people stay quiet

There is a second reason the room goes silent, and this one has nothing to do with models. Every
organisation runs on the same unwritten rule: all proactivity will be rewarded with more work. Raise
the risk nobody had seen and you just volunteered for it. The investigation is yours now, and so are
the mitigation, the follow up, and the slightly cold conversation with the person whose plan you
questioned. People learn that lesson exactly once. After that the observation stays in their head,
and it stays there rationally. The most expensive silence in a project is not ignorance, it is
arithmetic.

An agent has no career to protect. It does not inherit the workstream it just created, it is not up
for the same promotion, and it will not sit next to the architect it contradicted for the following
six months. That is the one structural advantage it holds over everybody in the room, and YSAT
exists to spend it. Bora.

## Why it is not just "be critical"

Two failure modes kill a critic agent. This skill is built against both:

| Failure | Guard |
|---|---|
| **Sycophancy**: agreeing because agreement feels helpful | Risk first output, agreement has to be earned, verdict changes only on new evidence |
| **Fearmongering**: inventing risks to look rigorous | Fixed severity scale, every claim carries a source, hard cap on the number of risks |

The pressure sensitive behaviours sit in a **locked** section that configuration cannot reach. Tone,
depth, language, domains and format are fully yours. Calibration is not.

## Install

Bora. The skill is a single folder that follows the [Agent Skills specification](https://agentskills.io/specification)
(`SKILL.md` with `name` and `description` frontmatter), so it works in any agent that supports it.

**Microsoft 365 Copilot (Cowork personal skills)**

```
Documents/Cowork/skills/ysat/
```

Copy the folder there, or ask Copilot: *"create a skill from this repository"*. It appears within
about 35 seconds.

**Any other Agent Skills compatible runtime**

```bash
git clone https://github.com/<your-user>/ysat.git ~/.agent/skills/ysat
```

Point it at whatever skills directory your runtime reads.

**No skill support at all**

Paste the body of `SKILL.md` into your system prompt or custom instructions. Everything below the
frontmatter works standalone.

## Use it

Say any of these, in English or Portuguese:

- "challenge this decision"
- "play devil's advocate"
- "what could go wrong with this"
- "what are the risks of moving to X"
- "discorda dessa decisao"
- "por que isso pode dar errado"
- "estou pensando em fazer X, o que pode falhar"

Add an inline override for a single run: *quick mode*, *deep mode*, *top 3 only*, *focus on cost*,
*brutal*, *as a document*.

## Customize it

Edit `config.yaml`. Every key is optional.

```yaml
language: auto          # auto | pt-BR | en-US | any locale
depth: standard         # quick | standard | deep
max_risks: 5            # 3 to 8
bluntness: direct       # plain | direct | blunt
focus_domains: [architecture, security, cost, delivery]
sources: [m365, code, web, calendar]
output_format: chat     # chat | document | card
avoid_dashes: true
house_rules:
  - Any customer data leaving the tenant is reported even at Low severity.
custom_domains:
  - name: field delivery
    questions:
      - Does this change what was written in the signed statement of work?
```

`house_rules` and `custom_domains` are the ones people end up living in: your team's non negotiables
and your own sweep categories, applied on every run without repeating them.

Full reference, inline overrides and ready made recipes: [references/customization.md](references/customization.md).

## What you cannot turn off

By design, and this is the point of the skill:

1. Risk first. No praise opening.
2. Agreement must be earned: "no strong reason to disagree" requires naming the disconfirming checks,
   the residual risks and the pre mortem.
3. The verdict changes only on new evidence. Repetition, authority and urgency are not evidence.
4. The pre mortem always runs, at every depth.
5. Every claim about your context carries a source. Unsourced items are labelled as domain pattern.
6. Read only. It never sends, posts, edits or runs anything.
7. No evaluation of people. Capacity risk is framed as process and dependency.
8. You decide. It offers to help execute even when you go ahead against the recommendation.

A `house_rule` may make it stricter. One that tries to make it softer is ignored and reported.

## What is in the box

```
ysat/
├── SKILL.md                        the skill itself
├── config.yaml                     your settings
├── README.md                       this file
├── README.pt-BR.md                 Portuguese version
├── LICENSE                         MIT
├── CHANGELOG.md
└── references/
    ├── risk-checklists.md          9 domains plus the biases that prop up bad decisions
    ├── customization.md            every knob, every override, and what is locked
    └── examples.md                 three worked runs, including one that pushes back
```

## When not to use it

- You want a balanced pros and cons write up. This one argues the against.
- The decision is already executed. That is a post mortem.
- You want line by line code review, or the announcement written for stakeholders.
- You want an opinion on a person. It will refuse and offer process analysis instead.

## Other languages

- [Português (Brasil)](README.pt-BR.md)

Triggers work in both languages out of the box, and `language: pt-BR` in `config.yaml` forces the
answer into Portuguese regardless of the language of the request.

## Contributing

Issues and pull requests welcome, especially new checklist domains and new anti-sycophancy cases.
Keep the locked invariants locked: a pull request that makes the skill easier to agree with is the
one thing that will not be merged.

## License

MIT. See [LICENSE](LICENSE).
