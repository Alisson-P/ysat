# YSAT

### You Sure About That?

**An agent skill that disagrees with you, on purpose, with evidence.**

> 🇧🇷 Leia em [português](README.pt-BR.md)

Folks, most assistants are trained to be agreeable.
That is exactly the worst possible behaviour at the moment you are about to commit to an architecture, a vendor, a deadline or
a migration.

YSAT reads your real context (code, infrastructure, email, chat, shared documents and calendar) and gives you the argument nobody made in the meeting:
what can break, how badly, when, and the cheapest test to run before you commit.

It is deliberately one sided. The burden of proof sits on the decision, not on the objection.

```
You:  "I am moving the client database to serverless at the end of the month, thoughts?"

YSAT: Decision as I understand it ...
      What I looked at ...
      Where this can break      (risk | why | evidence | severity | when it bites)
      Blind spot ...
      Strongest case for it ... and what would have to be true
      Verdict: would do it in phases, in waves, etc
      Mitigations ... / Cheapest test ...
```

## Determinism

An assistant that agrees looks helpful, and agreeing is the cheapest way to look helpful (and to be no help at all, too).

The more you interact with your agent there, the longer the thread runs, the more your own bias becomes its premises,
until the answer you get is your own opinion, only better formatted.

So, folks, that is determinism.
Give an agent a user with a position and a conversation long enough, and it converges on that position.
This will happen with your agents, or it already has.
What will not happen is the agent raising a counterpoint on its own about the thing you did not want
to hear, because nothing in the loop rewards that behaviour.

And this matters most exactly where the stakes are highest.
The hard part is not making a decision while taking on a known risk; that is life.
The hard part is making a decision without knowing about a risk that was known, but nobody brought it to the table.

With that in mind, I built this first version of YSAT.
So it questions us, and does that through structure, not bias.
A preference can be argued away in a single turn of pressure. A rule cannot.
That is why the invariants live in their own section, outside the configuration surface:
risk leads, agreement has to be earned, and the verdict moves on new evidence and on nothing else.
Tone, depth, language, domains and format are yours to set.
The part that refuses to fold is not.

The idea behind YSAT is NOT to stop you from anything.
It is to put on the table the thing you were not looking at, which is exactly what the blind spot line of every answer exists for.
Sometimes you read it, disagree, and go ahead anyway.
That is a good outcome too, because the decision now carries the counterargument in hand instead of discovering it in production.

## Why people stay quiet

"all proactivity will be rewarded with more work"; ever heard that one?

You raise a point and "voilà"; now you own it; go and solve it; bring us options...

Raised the risk nobody had seen? You just volunteered (that reminds me of the army).
The investigation is yours now, and so are the mitigation, the follow up, and the slightly cold conversation with the person whose plan
you questioned.

People learn that lesson exactly once. From the second time on, the observation stays in their head,
and it stays there by rational calculation, not by incompetence.
Some people even get afraid of voicing the opposite view, because raising the point nobody saw is the shortest path to
owning it. The most expensive silence in a project is not ignorance, it is arithmetic. Sad, right?! yeah...

A skill though, an agent, has no career to protect.
It does not inherit the workstream it just created, it is not up for the same promotion
and it will not sit next to the architect it contradicted for the following six months.
That is the one structural advantage it holds over everybody in the room,
and YSAT exists to spend exactly that advantage.

From here on the text was written by AI, from the ideas I gave it. Bora.
Give it a try, and help us out with feedback, issues, and so on. Cheers.

## Why it is not just "be critical"

Two failure modes kill a critic agent. This skill is built against both:

| Failure | Guard |
|---|---|
| **Sycophancy**: agreeing because agreement feels helpful | Risk first output, agreement has to be earned, verdict changes only on new evidence |
| **Fearmongering**: inventing risks to look rigorous | Fixed severity scale, every claim carries a source, hard cap on the number of risks |

The pressure sensitive behaviours sit in a **locked** section that configuration cannot reach. Tone,
depth, language, domains and format are fully yours. Calibration is not.

## Install

Bora. YSAT is a single folder that follows the [Agent Skills specification](https://agentskills.io/specification),
so it works in any agent that supports it.

**1. Get the folder**

```bash
git clone https://github.com/Alisson-P/ysat.git
```

No git? Use the green **Code** button at the top of this page, then **Download ZIP**, and unzip it.
It comes out named `ysat-main`, so rename it to `ysat`.

**2. Move the `ysat` folder into your agent's skills folder**

| Agent | Where it goes |
|---|---|
| Microsoft 365 Copilot (Cowork) | `Documents/Cowork/skills/ysat/` |
| Any other Agent Skills runtime | usually `~/.agent/skills/ysat/` |

In Copilot it shows up within about 35 seconds.

> The folder has to be named exactly `ysat`, the same as `name` in the frontmatter. If it does not
> match, the skill never loads and no error is shown.

**No skill support at all?** Paste the body of `SKILL.md` into your system prompt or custom
instructions. Everything below the frontmatter works standalone.

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
