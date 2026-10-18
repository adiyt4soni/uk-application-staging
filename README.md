# UK application staging (Codex)

Codex version of the application sprint: discover, verify, rank and stage UK job applications without ever submitting one.

**Type:** Codex skill · **Area:** Job search automation

## What it does

- Sweeps company job boards (Ashby, Greenhouse, Lever) and treats YC and LinkedIn results as leads until confirmed on the employer's own site.
- Screens each role for location, fit, explicit requirements, and salary tier.
- Builds a separate tailored CV and form-answer file per role and runs an ATS check that must print `RESULT ALL PASS`.
- Records each role as drafted, not submitted, and lists the exact decisions only the user can make.

## Guardrails

- Never submits, sends, creates accounts or solves CAPTCHAs.
- Anything only the user can truthfully answer (or unclear pay) is flagged, not guessed.

## When it triggers

> Discover, verify, rank, and stage UK job applications in the jobs workspace without outbound submission; use for batch or unattended application sprints.

## What's in this repo

- `SKILL.md`: the skill itself, the file Claude or Codex reads.
- Not included: The board sweeper, ATS checker and form inspector scripts are not published.
- Written to be generic: where the skill relies on the user's own files, it names them by role, not by content.

## Install

```bash
mkdir -p ~/.codex/skills/uk-application-staging
curl -fsSL https://raw.githubusercontent.com/adiyt4soni/uk-application-staging/main/SKILL.md -o ~/.codex/skills/uk-application-staging/SKILL.md
```
