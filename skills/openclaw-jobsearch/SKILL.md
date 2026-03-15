---
name: openclaw-jobsearch
description: Match live jobs from a named employer's careers portal against the user's background and job preferences. Use when the user provides a single employer name and wants the top current roles from the matching careers site in EMPLOYERS.yaml, scored with USER.md plus PREFERENCES.md and returned as a ranked shortlist.
---

# Match Employer Jobs

Resolve the employer name to a careers portal key in `EMPLOYERS.yaml`.

Treat `USER.md` as a background on the job seeker.

Treat `PREFERENCES.md` as the user's job-search policy and ranking bias.

Read only the files needed for the current run:
- `EMPLOYERS.yaml`
- `PREFERENCES.md`
- `USER.md`
- `APPLICATIONS.yaml` only when avoiding duplicates or checking prior applications is relevant

Use the workflow below.

## Workflow

1. Normalize the employer name.
2. Find the matching employer key and portal URL in `EMPLOYERS.yaml`.
3. Use the browser to open the live careers portal and collect currently listed roles.
4. Filter out obvious non-matches using `PREFERENCES.md`.
5. Score the remaining roles against `USER.md` and `PREFERENCES.md`.
6. Return the top 10 roles, or fewer if fewer strong matches exist.
7. Cite the URLs to the selected job listings used for each shortlisted role.

If the employer name is ambiguous, resolve it by exact key match first, then obvious alias match, then ask the user only if multiple plausible employers remain.

If the portal exposes fewer than 10 current roles, return the available strong matches and state the shortfall explicitly.

If the portal is blocked, JavaScript-heavy, or difficult to search, state the limitation and use the best available accessible listing pages or search results from the same employer site.

## Hard Filters

Exclude roles that clearly conflict with the user's stated preferences.

## Ranking Rules

Prioritize:
- Strong overlap with the user's background from `USER.md`
- Strong overlap with the user's stated priorities in `PREFERENCES.md`
- Roles that satisfy the user's preferred work model, location, seniority, and domain constraints in `PREFERENCES.md`
- Practical matches the user could plausibly interview for now based on `USER.md` and `PREFERENCES.md`

Deprioritize:
- Prestige-only stretches with weak skill overlap
- Roles that depend on a narrow domain specialty not supported by `USER.md`
- Roles whose location model materially conflicts with preferences

Use the rubric in [references/scoring-rubric.md](references/scoring-rubric.md).

Use the output contract in [references/output-format.md](references/output-format.md).

## Evidence Discipline

Base every recommendation on the live job description and the contents of `USER.md` plus `PREFERENCES.md`.

Do not invent:
- Location flexibility
- Visa support
- Compensation
- Team details
- Seniority calibration beyond what the posting implies

When inferring fit, label it as an inference and tie it to concrete signals from the job description and user profile.

## Output Requirements

Return a ranked shortlist with:
- Rank
- Employer
- Role title
- Location and work model
- Why it matches
- Risks or gaps
- Source link

Include a short section listing:
- Roles reviewed
- Roles excluded for policy reasons
- Any portal access limitations

If no strong matches exist, say so directly and list the main reasons.
