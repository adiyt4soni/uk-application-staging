---
name: uk-application-staging
description: Discover, verify, rank, and stage UK job applications in the jobs workspace without outbound submission; use for batch or unattended application sprints.
disable-model-invocation: true
user-invocable: true
---

# UK application staging

## When to use

Use for a UK job-search batch that needs employer-ATS verification, requirements screening, tailored CV/form artifacts, and a staged report. Do not use this skill to submit an application, send outreach, create an account, solve a CAPTCHA, or answer questions only the user can answer truthfully.

## Inputs to gather

1. Read `canonical-profile.md`, `daily-ops-spec.md`, `job-search-state.json`, prior batch files, and `tools/boards.json` in the jobs project folder.
2. Identify the current salary rule, the profile's standard form answers, and the explicit no-submit boundary.
3. Recheck which roles are already applied, evaluated, delegated, or held before proposing anything.

## Procedure

1. Sweep ATS boards with `tools/sweep.py`; record unresolved boards as instrument gaps. Use the hosted Ashby board and `window.__appData.jobBoard.jobPostings` if the public posting API returns 404.
2. Treat YC and LinkedIn results as leads. Confirm every candidate on its employer's own ATS and inspect the full JD before surfacing it.
3. Screen for UK location, fit, explicit gates, response probability, and salary tier.
4. Use the user's salary tiers (for example Tier A at or above a base threshold, Tier B a band below it). Lower salary needs exceptional career value. Label OTE, third-party figures, and base-versus-total ambiguity as uncertain.
5. For each selected role, create a separate CV and form-answer file. Use canonical facts only; state genuine gaps, and avoid unsupported claims.
6. Build and check each CV: `python3 tools/ats_check.py <cv.pdf> <cv.md> <jd.txt>`. Require `RESULT ALL PASS`, one page, 92–95% fill, and no one-word orphan. Render a page only if it can actually be viewed.
7. Inspect Ashby forms with `python3 tools/ashby_form.py org/job-id`. If browser staging is permitted, upload first, wait briefly, then use real clicks/keystrokes for fields that React may reset.
8. Update state as `APPLICATION DRAFTED` with the named approval required. Report exact user-only decisions, evidence, uncertainties, and the next approval phrase. Do not submit.

## Efficiency plan

- Reuse `tools/boards.json` and already-evaluated state to avoid rediscovery.
- Use a full ATS sweep as a positive control; only deep-read JDs that pass the first screen.
- Stop at a staged report when the next step changes external state or needs an answer only the user can truthfully give.

## Pitfalls and fixes

- A YC/LinkedIn listing is absent from the employer board -> do not stage it; log `lead-unconfirmed`.
- `ashby_form.py` fails with unpacking -> use `org/job-id`, not separate positional arguments; use `descriptionHtml`, not `descriptionPlain` in its GraphQL handling.
- `ats_check.py` finds a one-word orphan line -> reword/remove the trailing low-value line, rebuild, and rerun the check.
- A form question only the user can answer, or unclear compensation -> flag it for the user and retain uncertainty; do not guess.

## Verification checklist

- Every surfaced role is current and confirmed on the employer ATS.
- Eligibility conclusions retain their caveats.
- Each CV has `RESULT ALL PASS` and a demonstrably reviewed page render.
- Batch materials record real gaps and user-only decisions.
- `job-search-state.json` records staged—not submitted—status.
- No email, application, account, CAPTCHA, or other outbound action was performed.
