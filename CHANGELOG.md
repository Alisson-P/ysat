# Changelog

All notable changes to this skill are documented here. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), versioning follows
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-09-19

### Added

- Initial release of the `ysat` counterpoint skill.
- Seven step workflow: load configuration, frame the decision, gather evidence in parallel, label
  evidence against assumption, pre mortem and risk table, steelman, verdict with mitigations and the
  cheapest test, plus a self check before answering.
- Locked invariants that configuration cannot override: risk first, agreement must be earned,
  verdict changes only on new evidence, pre mortem always runs, every claim carries a source, read
  only, no evaluation of people, the user decides.
- Anti-sycophancy protocol for pushback turns, separating new evidence from pressure.
- Customization layer in `config.yaml`: language, depth, maximum risks, bluntness, focus domains,
  allowed sources, output format, dash preference, house rules, custom domains.
- Inline per run overrides (quick mode, deep mode, top 3 only, focus on a domain, brutal, as a
  document, no web search).
- Risk checklists across nine domains plus the biases that usually prop up a bad decision.
- Three worked examples, including a pushback turn that does not move the verdict.
- Trigger phrases in English and Portuguese.
- Documentation in two languages: `README.md` (primary, English) and `README.pt-BR.md`.
