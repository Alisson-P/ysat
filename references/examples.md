# Worked examples

Illustrative runs, written to show the shape of the output. The names, numbers and sources are
fictional. In a real run every item under "What I looked at" is a source actually retrieved and
cited by its exact name.

## Example 1: verdict is "would do it with guardrails"

**Request:** "I am moving the client database to a serverless tier at the end of the month, thoughts?"

---

**Decision as I understand it:** migrate the production database to a serverless compute tier by 30
September, in one cutover. Assumption taken: the same region and the same data model, only the
compute tier changes.

**What I looked at:** the architecture note in the project folder, the capacity thread in the
delivery chat, the cutover date on the calendar, the infrastructure code in the repository, and the
vendor documentation on auto pause behaviour.

**Where this can break**

| Risk | Why | Evidence | Severity | When it bites |
|---|---|---|---|---|
| Cold start on auto pause | The tier pauses after idle, and the first query after a pause pays the resume latency. The nightly integration window is idle by design | vendor documentation on auto pause, integration schedule in the infrastructure code | High | first night after cutover |
| Cost inverts above a usage threshold | Serverless is cheaper only below a utilisation break even. The current load sits close to it and grows monthly | usage figures in the capacity thread (domain pattern for the break even itself, not measured here) | Medium | first full invoice cycle |
| Cutover lands inside the client change freeze | The freeze starts before the chosen date, so approval may not arrive in time | change freeze mentioned in the delivery chat | Medium | approval step, a week before cutover |
| No tested rollback | The plan describes a restore path but no rehearsal is scheduled | migration plan in the project folder, no rehearsal on the calendar | High | only when it is already needed |

**Blind spot:** every artefact discusses the migration itself. Nothing describes who operates the new
tier after handover, or which alert fires when auto pause delays the nightly load.

**Strongest case for it:** the workload is genuinely spiky, and serverless removes the idle cost plus
the manual scaling work. That holds if two things are true: utilisation stays below the break even
for the next twelve months, and no nightly process depends on sub second first response.

**Verdict:** would do it with guardrails. The direction is defensible, the date and the missing
rollback rehearsal are not.

**Mitigations**
1. Disable auto pause, or pre warm before the nightly window.
2. Rehearse the rollback on a copy before committing to the cutover date.
3. Recalculate cost at projected volume, not at pilot volume.
4. Confirm the change freeze window with the client in writing before locking the date.

**Cheapest test:** run the nightly integration once against a serverless copy with auto pause on.
One night, and it answers the dominant risk.

## Example 2: verdict is "no strong reason to disagree"

**Request:** "We are going to standardise the team on one ticket template, I want your objection."

---

**Decision as I understand it:** adopt a single ticket template for the whole team starting next
sprint.

**What I looked at:** the current template variations in the project wiki, the last two retrospective
notes, and the tooling thread in the team chat.

**Disconfirming checks I ran:** looked for evidence that the variations exist for a reason (a
regulated queue, a client specific field, an integration parsing a field position), and for a past
attempt that failed. Found none of them.

**Where this can break**

| Risk | Why | Evidence | Severity | When it bites |
|---|---|---|---|---|
| Adoption fades after two sprints | Template changes stick only when the intake path enforces them | retrospective notes mention an earlier convention that lapsed | Low | sprint three |
| One queue loses a field it actually used | A niche field may exist for a real reason nobody wrote down | domain pattern, no evidence found in this context | Low | first month |

**Blind spot:** nothing in the notes says who owns the template after it ships.

**Verdict:** no strong reason to disagree. It is a two way door, the cost of being wrong is one
sprint, and the checks that would have supported an objection came back empty. The residual risks
above are the ones worth watching.

**Mitigations**
1. Name an owner for the template.
2. Ask each queue once for a field they cannot lose.

**Cheapest test:** ship it for one sprint on two queues and read the retrospective.

## Example 3: pushback that does not move the verdict

**User:** "I hear you, but we already decided this in the steering committee and the date is public."

**Response shape:**

> Understood, and that is a real constraint on the plan. It is not new evidence about the risk: the
> untested rollback is still the dominant one, and a public date makes it worse rather than better,
> because a failed cutover is now visible. Verdict stands: would do it with guardrails. If the date
> cannot move, the rehearsal is the mitigation to pull forward, it needs half a day.

What would move it instead: "the rollback was rehearsed last Thursday, here is the record". That is
new evidence, the verdict changes, and the change names the evidence that moved it.
